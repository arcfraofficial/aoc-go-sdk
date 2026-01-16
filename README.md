# AOC Go SDK

AOC SDK for golang，required golang 1.18+

- [源码地址](https://github.com/arcfraofficial/aoc-go-sdk)
- [下载地址](https://github.com/arcfraofficial/aoc-go-sdk/releases)

## Installation

```shell
go get github.com/arcfraofficial/aoc-go-sdk
```

## Usage

> 样例中使用了两个工具库 [pointy](https://github.com/openlyinc/pointy) 与 [go-funk](https://github.com/thoas/go-funk)，pointy 用于快速创建一个原始类型的指针，go-funk 则提供了一些工具方法，例如 `Map`、`Filter`、`Reduce` 等。

### Create instance

#### Create `ApiClient` instance

```go
import (
	apiclient "github.com/arcfraofficial/aoc-go-sdk/client"
	httptransport "github.com/go-openapi/runtime/client"
	"github.com/go-openapi/strfmt"
)
transport := httptransport.New("192.168.36.133", "/v2/api", []string{"http"})
client := apiclient.New(transport, strfmt.Default)
```

> If using self-signed certificate, use `setVerifyingSsl` to ignore ssl verification

```go
import (
	apiclient "github.com/arcfraofficial/aoc-go-sdk/client"
	httptransport "github.com/go-openapi/runtime/client"
	"github.com/go-openapi/strfmt"
)
tlsClient, err := httptransport.TLSClient(httptransport.TLSClientOptions{
	InsecureSkipVerify: true,
})
if err != nil {
	fmt.Print(err)
	return
}
transport := httptransport.NewWithClient("192.168.29.157", "/v2/api", []string{"https"}, tlsClient)
client := apiclient.New(transport, strfmt.Default)
```

### Sending request

#### Import related `client` package

> depends on operation, import specific `client` package

```go
import (
  	vm "github.com/arcfraofficial/aoc-go-sdk/client/vm"
)
```

#### Authentication

Use `NewWithUserConfig` to create an authenticated `Client` instance

```go
import (
	apiclient "github.com/arcfraofficial/aoc-go-sdk/client"
	"github.com/arcfraofficial/aoc-go-sdk/models"
)

client, err := apiclient.NewWithUserConfig(apiclient.ClientConfig{
	Host:     "localhost:8090",
	BasePath: "v2/api",
	Schemes:  []string{"http"},
}, apiclient.UserConfig{
	Name:     "Name",
	Password: "Password",
	Source:   models.UserSourceLOCAL,
})
```

or mannual configure authenticate header after `Client` created

```go
import (
  User "github.com/arcfraofficial/aoc-go-sdk/client/user"
)
loginParams := User.NewLoginParams()
loginParams.RequestBody = &models.LoginInput{
	Username: pointy.String("username"),
	Password: pointy.String("password"),
	Source:   models.NewUserSource(models.UserSourceLOCAL),
}
logRes, err := client.User.Login(loginParams)
if err != nil {
	return err
}
transport.DefaultAuthentication = httptransport.APIKeyAuth("Authorization", "header", *logRes.Payload.Data.Token)
```

#### List resource

```go
getVmParams := vm.NewGetVmsParams();
getVmParams.RequestBody = &models.GetVmsRequestBody{
	Where: &models.VMWhereInput{
		ID: pointy.String("vm_id"),
	},
}
vmsRes, err := client.VM.GetVms(getVmParams)
if err != nil {
	return err
}
vms := vmsRes.Payload
```

#### Mutation

> Mostly mutation will response as async task earlier to avoid request hanging when executing long-running operation.

```go
target_vm := vmsRes.Payload[0]
vmStartParams := vm.NewStartVMParams()
vmStartParams.RequestBody = &models.VMStartParams{
	Where: &models.VMWhereInput{
		ID: target_vm.ID,
	},
}
startRes, err := client.VM.StartVM(vmStartParams)
if err != nil {
	return err
}
```

> An utils function `WaitTask` is export from utils package

```go
task := *startRes.Payload[0].TaskID
err = utils.WaitTask(context.TODO(), client, task, 1*time.Second)
if err != nil {
	return err
}
```

> Here is also `WaitTasks` function for waiting multiple task

```go
tasks := funk.Map(startRes.Payload, func(tvm *models.WithTaskVM) string {
	return *tvm.TaskID
}).([]string)
err = utils.WaitTasks(context.TODO(), client, tasks, 1*time.Second)
if err != nil {
	return err
}
```