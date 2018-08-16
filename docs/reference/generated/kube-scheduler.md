---
title: kube-scheduler
notitle: true
---
## kube-scheduler


<!--
### Synopsis

The Kubernetes scheduler is a policy-rich, topology-aware,
workload-specific function that significantly impacts availability, performance,
and capacity. The scheduler needs to take into account individual and collective
resource requirements, quality of service requirements, hardware/software/policy
constraints, affinity and anti-affinity specifications, data locality, inter-workload
interference, deadlines, and so on. Workload-specific requirements will be exposed
through the API as necessary.
-->
### 概要

Kubernetes 调度器是一个策略丰富、拓扑感知、工作负载特定的功能，显著影响可用性、性能和容量。调度器需要考虑个人和集体
的资源要求、服务质量要求、硬件/软件/政策约束、亲和力和反亲和力规范、数据局部性、工作量间干扰，截止日期等。 
工作负载特定的要求必要时将通过 API 暴露。

```
kube-scheduler [flags]
```

<!--
### Options
-->
### 选项

<!--
```
      --address string                           The IP address to serve on (set to 0.0.0.0 for all IPv4 interfaces and :: for all IPv6 interfaces).
      --algorithm-provider string                The scheduling algorithm provider to use, one of: ClusterAutoscalerProvider | DefaultProvider
      --azure-container-registry-config string   Path to the file containing Azure container registry configuration information.
      --config string                            The path to the configuration file.
      --contention-profiling                     Enable lock contention profiling, if profiling is enabled
      --feature-gates mapStringBool              A set of key=value pairs that describe feature gates for alpha/experimental features. Options are:
APIListChunking=true|false (BETA - default=true)
APIResponseCompression=true|false (ALPHA - default=false)
Accelerators=true|false (ALPHA - default=false)
AdvancedAuditing=true|false (BETA - default=true)
AllAlpha=true|false (ALPHA - default=false)
AppArmor=true|false (BETA - default=true)
BlockVolume=true|false (ALPHA - default=false)
CPUManager=true|false (BETA - default=true)
CRIContainerLogRotation=true|false (ALPHA - default=false)
CSIPersistentVolume=true|false (BETA - default=true)
CustomPodDNS=true|false (BETA - default=true)
CustomResourceSubresources=true|false (ALPHA - default=false)
CustomResourceValidation=true|false (BETA - default=true)
DebugContainers=true|false (ALPHA - default=false)
DevicePlugins=true|false (BETA - default=true)
DynamicKubeletConfig=true|false (ALPHA - default=false)
EnableEquivalenceClassCache=true|false (ALPHA - default=false)
ExpandPersistentVolumes=true|false (ALPHA - default=false)
ExperimentalCriticalPodAnnotation=true|false (ALPHA - default=false)
ExperimentalHostUserNamespaceDefaulting=true|false (BETA - default=false)
GCERegionalPersistentDisk=true|false (BETA - default=true)
HugePages=true|false (BETA - default=true)
HyperVContainer=true|false (ALPHA - default=false)
Initializers=true|false (ALPHA - default=false)
LocalStorageCapacityIsolation=true|false (BETA - default=true)
MountContainers=true|false (ALPHA - default=false)
MountPropagation=true|false (BETA - default=true)
PersistentLocalVolumes=true|false (BETA - default=true)
PodPriority=true|false (ALPHA - default=false)
PodShareProcessNamespace=true|false (ALPHA - default=false)
ReadOnlyAPIDataVolumes=true|false (DEPRECATED - default=true)
ResourceLimitsPriorityFunction=true|false (ALPHA - default=false)
RotateKubeletClientCertificate=true|false (BETA - default=true)
RotateKubeletServerCertificate=true|false (ALPHA - default=false)
RunAsGroup=true|false (ALPHA - default=false)
ScheduleDaemonSetPods=true|false (ALPHA - default=false)
ServiceNodeExclusion=true|false (ALPHA - default=false)
ServiceProxyAllowExternalIPs=true|false (DEPRECATED - default=false)
StorageObjectInUseProtection=true|false (BETA - default=true)
StreamingProxyRedirects=true|false (BETA - default=true)
SupportIPVSProxyMode=true|false (BETA - default=true)
SupportPodPidsLimit=true|false (ALPHA - default=false)
TaintBasedEvictions=true|false (ALPHA - default=false)
TaintNodesByCondition=true|false (ALPHA - default=false)
TokenRequest=true|false (ALPHA - default=false)
VolumeScheduling=true|false (BETA - default=true)
VolumeSubpath=true|false (default=true)
  -h, --help                                     help for kube-scheduler
      --kube-api-burst int32                     Burst to use while talking with kubernetes apiserver (default 100)
      --kube-api-content-type string             Content type of requests sent to apiserver. (default "application/vnd.kubernetes.protobuf")
      --kube-api-qps float32                     QPS to use while talking with kubernetes apiserver (default 50)
      --kubeconfig string                        Path to kubeconfig file with authorization and master location information.
      --leader-elect                             Start a leader election client and gain leadership before executing the main loop. Enable this when running replicated components for high availability. (default true)
      --leader-elect-lease-duration duration     The duration that non-leader candidates will wait after observing a leadership renewal until attempting to acquire leadership of a led but unrenewed leader slot. This is effectively the maximum duration that a leader can be stopped before it is replaced by another candidate. This is only applicable if leader election is enabled. (default 15s)
      --leader-elect-renew-deadline duration     The interval between attempts by the acting master to renew a leadership slot before it stops leading. This must be less than or equal to the lease duration. This is only applicable if leader election is enabled. (default 10s)
      --leader-elect-resource-lock endpoints     The type of resource object that is used for locking during leader election. Supported options are endpoints (default) and `configmaps`. (default "endpoints")
      --leader-elect-retry-period duration       The duration the clients should wait between attempting acquisition and renewal of a leadership. This is only applicable if leader election is enabled. (default 2s)
      --lock-object-name string                  Define the name of the lock object. (default "kube-scheduler")
      --lock-object-namespace string             Define the namespace of the lock object. (default "kube-system")
      --log-flush-frequency duration             Maximum number of seconds between log flushes (default 5s)
      --master string                            The address of the Kubernetes API server (overrides any value in kubeconfig)
      --policy-config-file string                File with scheduler policy configuration. This file is used if policy ConfigMap is not provided or --use-legacy-policy-config==true
      --policy-configmap string                  Name of the ConfigMap object that contains scheduler's policy configuration. It must exist in the system namespace before scheduler initialization if --use-legacy-policy-config==false. The config must be provided as the value of an element in 'Data' map with the key='policy.cfg'
      --policy-configmap-namespace string        The namespace where policy ConfigMap is located. The system namespace will be used if this is not provided or is empty.
      --port int32                               The port that the scheduler's http service runs on (default 10251)
      --profiling                                Enable profiling via web interface host:port/debug/pprof/
      --scheduler-name string                    Name of the scheduler, used to select which pods will be processed by this scheduler, based on pod's "spec.SchedulerName". (default "default-scheduler")
      --use-legacy-policy-config                 When set to true, scheduler will ignore policy ConfigMap and uses policy config file
      --version version[=true]                   Print version information and quit
```
-->
```
      --address string                           要服务的 IP 地址（所有 IPv4 接口设置为 0.0.0.0，所有 IPv6 接口设置为 ::）
      --algorithm-provider string                要使用的调度算法，其中之一：ClusterAutoscalerProvider |DefaultProvider
      --azure-container-registry-config string   包含 Azure 容器注册表配置信息的文件的路径
      --config string                            配置文件的路径
      --contention-profiling                     如果启用了性能分析，则启用锁竞争分析
      --feature-gates mapStringBool              一组 key=value 对，用于描述 alpha/experimental 特征的特征门。 选项包括：
APIListChunking=true|false (BETA - 默认=true)
APIResponseCompression=true|false (ALPHA - 默认=false)
Accelerators=true|false (ALPHA - 默认=false)
AdvancedAuditing=true|false (BETA - 默认=true)
AllAlpha=true|false (ALPHA - 默认=false)
AppArmor=true|false (BETA - 默认=true)
BlockVolume=true|false (ALPHA - 默认=false)
CPUManager=true|false (BETA - 默认=true)
CRIContainerLogRotation=true|false (ALPHA - 默认=false)
CSIPersistentVolume=true|false (BETA - 默认=true)
CustomPodDNS=true|false (BETA - 默认=true)
CustomResourceSubresources=true|false (ALPHA - 默认=false)
CustomResourceValidation=true|false (BETA - 默认=true)
DebugContainers=true|false (ALPHA - 默认=false)
DevicePlugins=true|false (BETA - 默认=true)
DynamicKubeletConfig=true|false (ALPHA - 默认=false)
EnableEquivalenceClassCache=true|false (ALPHA - 默认=false)
ExpandPersistentVolumes=true|false (ALPHA - 默认=false)
ExperimentalCriticalPodAnnotation=true|false (ALPHA - 默认=false)
ExperimentalHostUserNamespaceDefaulting=true|false (BETA - 默认=false)
GCERegionalPersistentDisk=true|false (BETA - 默认=true)
HugePages=true|false (BETA - 默认=true)
HyperVContainer=true|false (ALPHA - 默认=false)
Initializers=true|false (ALPHA - 默认=false)
LocalStorageCapacityIsolation=true|false (BETA - 默认=true)
MountContainers=true|false (ALPHA - 默认=false)
MountPropagation=true|false (BETA - 默认=true)
PersistentLocalVolumes=true|false (BETA - 默认=true)
PodPriority=true|false (ALPHA - 默认=false)
PodShareProcessNamespace=true|false (ALPHA - 默认=false)
ReadOnlyAPIDataVolumes=true|false (DEPRECATED - 默认=true)
ResourceLimitsPriorityFunction=true|false (ALPHA - 默认=false)
RotateKubeletClientCertificate=true|false (BETA - 默认=true)
RotateKubeletServerCertificate=true|false (ALPHA - 默认=false)
RunAsGroup=true|false (ALPHA - 默认=false)
ScheduleDaemonSetPods=true|false (ALPHA - 默认=false)
ServiceNodeExclusion=true|false (ALPHA - 默认=false)
ServiceProxyAllowExternalIPs=true|false (DEPRECATED - 默认=false)
StorageObjectInUseProtection=true|false (BETA - 默认=true)
StreamingProxyRedirects=true|false (BETA - 默认=true)
SupportIPVSProxyMode=true|false (BETA - 默认=true)
SupportPodPidsLimit=true|false (ALPHA - 默认=false)
TaintBasedEvictions=true|false (ALPHA - 默认=false)
TaintNodesByCondition=true|false (ALPHA - 默认=false)
TokenRequest=true|false (ALPHA - 默认=false)
VolumeScheduling=true|false (BETA - 默认=true)
VolumeSubpath=true|false (默认=true)
  -h, --help                                     kube-scheduler 帮助信息
      --kube-api-burst int32                     每秒与 kubernetes apiserver 交互的数量 (默认 100)
      --kube-api-content-type string             发送到 apiserver 的请求的内容类型 (默认 "application/vnd.kubernetes.protobuf")
      --kube-api-qps float32                     与 kubernetes apiserver 交互时使用的QPS (默认 50)
      --kubeconfig string                        具有授权和 master 位置信息的 kubeconfig 文件的路径
      --leader-elect                             在执行主循环之前，启动 leader 选举客户端并获得领导能力。 在运行复制组件以实现高可用性时启用此选项。 (默认 true)
      --leader-elect-lease-duration duration     非 leader 候选人在观察领导层续约之后将等待的时间，直到试图获得领导但尚未更新的 leader 位置。 这实际上是 leader 在被另一个候选人替换之前可以停止的最长持续时间。 这仅适用于启用 leader 选举的情况 (默认 15s)
      --leader-elect-renew-deadline duration     代理 master 在停止领导之前更新领导位置的时间间隔。这必须小于或等于租约期限。 这仅适用于启用 leader 选举的情况 (默认 10s)
      --leader-elect-resource-lock endpoints     在 leader 选举期间用于锁定的资源对象的类型。 支持的选项是 endpoints (默认) 和 `configmaps`. (默认 "endpoints")
      --leader-elect-retry-period duration       客户端在尝试获取和更新领导之间应该等待的持续时间。 这仅适用于启用leader选举的情况。 (默认 2s)
      --lock-object-name string                  定义锁对象的名称 (默认 "kube-scheduler")
      --lock-object-namespace string             定义锁对象的命名空间 (默认 "kube-system")
      --log-flush-frequency duration             日志刷新最大间隔 (默认 5s)
      --master string                            Kubernetes API 服务器的地址（覆盖 kubeconfig 中的任何值）
      --policy-config-file string                具有调度器策略配置的文件。 如果未提供策略 ConfigMap 或 --use-legacy-policy-config==true，则使用此文件
      --policy-configmap string                  包含调度器策略配置的 ConfigMap 对象的名称。 如果 --use-legacy-policy-config==false，它必须在调度器初始化之前存在于系统命名空间中。 配置必须作为 'Data' 映射中元素的值提供，其中 key='policy.cfg'
      --policy-configmap-namespace string        策略 ConfigMap 所在的命名空间。 如果未提供此命名空间或为空，则将使用系统命名空间
      --port int32                               调度器的 http 服务运行的端口 (默认 10251)
      --profiling                                通过 web 接口 host:port/debug/pprof/ 启动性能分析
      --scheduler-name string                    调度器名称，用于根据 pod 的 "spec.SchedulerName" 选择哪些 pod 将被此调度器处理 (默认 "default-scheduler")
      --use-legacy-policy-config                 当设置为 true 时，调度器将忽略策略 ConfigMap 并使用策略配置文件
      --version version[=true]                   打印版本信息并退出
```


<!--
###### Auto generated by spf13/cobra on 25-Mar-2018
-->
###### spf13/cobra 自动生成于 2018/03/25
