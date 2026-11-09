## release date 2026-11-06

v1.1.0 release (aoc version 4.10.0)

### features

- Adds [BackupPlanApi]: create, update, delete, execute, suspend and resume backup plans, and query backup plans / plan executions (including the without-executions variants)
- Adds [BackupRestorePointApi], [BackupRestoreExecutionApi], [BackupTargetExecutionApi], [BackupServiceApi] and [BackupStoreRepositoryApi] for querying backup resources, and [BackupRestoreVMInPlace] / [BackupRebuildVM] for restoring VMs from restore points
- Adds [ReplicationPlanApi], [ReplicaVmApi], [ReplicationServiceApi] and [SyncReplicationPlanApi] for querying replication plans, replica VMs, replication services and sync replication plans
- [EverouteLicenseApi]: adds [GetV2EverouteLicensesConnection]
- [VmApi]: disk parameters of VM creation, cloning, rebuilding, importing, disk addition/update and CD-ROM addition APIs now support the `StorageConfig` field to specify the storage cluster (`StorageClusterID`) or VCFS datastore (`DatastoreID`) where the disk resides; `MountNewCreateDisks` adds the `ProvisionPolicy` field to specify the provisioning policy of datastore volumes (`thin` / `thick_lazy_zeroed` / `thick_eager_zeroed`)
- [VmApi]: [MigrateVMAcrossCluster] supports configuring `StorageConfig` and `ProvisionPolicy` for disks, and adds the `MigrateMode` option
- [VmVolumeApi]: [CreateVMVolume], [ImportVMVolume] and [CloneVMVolume] add `StorageConfig` and `ProvisionPolicy` fields, supporting creating, importing and cloning VM volumes on datastores with specified provisioning policies
- [VMVolume]: adds fields `ProvisionPolicy` and `ElfStoragePolicyStripeNum`; [VMVolumeType] adds `KVM_VOLUME_VCFS`
- [VMVolumeSnapshot]: adds fields `ProvisionPolicy` and `ElfStoragePolicyStripeNum`; [VMVolumeSnapshotType] adds `KVM_VOLUME_VCFS_SNAPSHOT`
- [Bus]: adds enum value `SATA`; [EncryptMethod]: adds enum value `SM4_CTR`
- [ContentLibraryImageApi]: adds [ImportContentLibraryImageV2] API, supporting importing images to multiple clusters or datastores; the original [ImportContentLibraryImage] is deprecated
- [ContentLibraryImageApi]: [UploadContentLibraryImage] adds `Datastores` form field, supporting distribution to datastores during upload
- [ContentLibraryImageApi]: [DistributeContentLibraryImageClusters] and [RemoveContentLibraryImageClusters] add `Datastores` field, supporting distributing to and removing images by datastore
- [ContentLibraryImage]: adds field `Datastores`
- [ContentLibraryImageApi]: [ImportContentLibraryImage] and image update parameters now accept `Datastore`/`Datastores` as an alternative to `Cluster`/`Clusters` (no longer required)
- [ContentLibraryVmTemplateApi]: in [CloneContentLibraryVMTemplateFromVM], `Clusters` is no longer required when `StorageConfigs` is provided
- [ContentLibraryVmTemplateApi]: [CloneContentLibraryVMTemplateFromVM] adds `StorageConfigs` field to specify the datastores for template configuration files and volume files (VCFS template placement)
- [ContentLibraryVmTemplateApi]: [ImportContentLibraryVMTemplate] adds `StorageConfig` and `TemplateArchitecture` fields
- [ContentLibraryVMTemplate]: adds fields `Datastores`, `Usage` and `VMTemplateConfigs`
- [Host]: adds memory overcommit related fields `MemoryOvercommitEnabled`, `AllocatedMemoryBytes`, `MemoryAllocationRate`, `OvercommitMemoryRatio`, `ReservedMemoryBytes`, `RunningPauseReservedMemoryBytes`, `OvercommitMemoryBytes`, `RunningPauseOvercommitMemoryBytes`, `OvercommitActiveMemoryBytes`, `TotalPhysicalMemoryBytes`
- [Host]: adds fields `DirtyCacheSpace`, `DirtyCacheUsage`, `FailureCacheSpace`, `TotalLogicalCpuCores`
- [Cluster]: adds memory overcommit related fields `AllocatedMemoryBytes`, `MemoryAllocationRate`, `OvercommitMemoryRatio`
- [Cluster]: adds fields `DirtyCacheSpace`, `DirtyCacheUsage`, `FailureCacheSpace`, `OverprovisionRatio`, `CapSnapshotUniqueSize`, `CapVolumeUniqueSize`
- [VmNic]: adds fields `GuestInfoIpAddresses`, `GuestInfoIpAddressesV6` and `DpiEnabled`
- [Vm]: adds fields `DpiEnabled`, `AntiMalwareEnabled`, `InternalProduct`, `InternalProductName` and `BackupPlans`; [VmUsage] adds `CLOUDTOWER_PASSIVE`, `CLOUDTOWER_WITNESS`, `EVEROUTE_LB`, `EVEROUTE_VPC_EDGE`
- [VmFolderApi]: [CreateVMFolder] and [UpdateVMFolder] support the `ParentID` field for creating and adjusting nested VM folders; [VmFolder] adds fields `Parent`, `Path` and `TotalVmNum`
- [VmPlacementGroup]: adds field `Usage`
- [IscsiLun]: adds field `ReplicationPlans`
- [NetworkType]: adds enum values `CUSTOM`, `ERSPAN`, `INTERNAL`
- [UserApi]: [Login] supports logging in via LDAP login sources (`AuthConfigID`); [MfaType] adds `Totp` and `TotpRecoveryCode`
- [LabelApi]: adds [GetLabels] API to query the label list
- [LabelApi]: [GetLabels], [UpdateLabel], [AddLabelsToResources] and [RemoveLabelsFromResources] support the `x-skip-resources` request header to skip loading associated resources for better query performance
- [UserRolePreset]: adds enum value `WORKLOAD_CLUSTER_REGISTRY_USER`
- [VirtualPrivateCloudEdgeGateway]: adds fields `EdgeGatewayGroup` and `VpcService`; [VirtualPrivateCloudEdgeGatewayGroup]: adds field `VpcService`
- [EverouteLicensePricingType]: adds enum values `CORE_NUM`, `VCPU_NUM`; [ConnectState]: adds enum value `CONNECTED_ERROR`
- [NestedEverouteControllerInstance]: fields `IpAddr` and `Vlan` become optional; [EverouteCluster]: field `ControllerTemplate` becomes optional

### bugfix

- [LabelApi]: fix label queries carrying redundant parameters
