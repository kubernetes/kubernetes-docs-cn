---
title: kube-controller-manager
notitle: true
---
## kube-controller-manager



### Synopsis


#The Kubernetes controller manager is a daemon that embeds
#the core control loops shipped with Kubernetes. In applications of robotics and
#automation, a control loop is a non-terminating loop that regulates the state of
#the system. In Kubernetes, a controller is a control loop that watches the shared
#state of the cluster through the apiserver and makes changes attempting to move the
#current state towards the desired state. Examples of controllers that ship with
#Kubernetes today are the replication controller, endpoints controller, namespace
#controller, and serviceaccounts controller.


### 概要


Kubernetes controller manager 是一个嵌入了 Kubernetes 核心控制循环的守护进程。在机器人和自动化的应用中，
控制循环是一个用于调节系统状态的非终止循环。在 Kubernetes 中，controller 是一个控制循环，它通过 apiserver 监视集群
的共享状态，并进行更改以尝试将当前状态移向所需状态。如今与 Kubernetes 一起提供的 controller 有 replication controller，
endpoints controller，namespace controller和serviceaccounts controller。

```
kube-controller-manager [flags]
```

### Options
### 选项

<table style="width: 100%; table-layout: fixed;">
  <colgroup>
    <col span="1" style="width: 10px;" />
    <col span="1" />
  </colgroup>
  <tbody>

    <tr>
    <!--
      <td colspan="2">--address ip&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 0.0.0.0</td>
    -->
      <td colspan="2">--address ip&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 0.0.0.0</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">DEPRECATED: the IP address on which to listen for the --port port (set to 0.0.0.0 for all IPv4 interfaces and :: for all IPv6 interfaces). See --bind-address instead.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">弃用: 要监听 --por t端口的IP地址(对于所有IPv4接口设置为0.0.0.0，对于所有IPv6接口设置为::）。请参阅 --bind-address。</td>
    </tr>

    <tr>
      <td colspan="2">--allocate-node-cidrs</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Should CIDRs for Pods be allocated and set on the cloud provider.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">是否应在云提供商上分配和设置 Pod 的 CIDR。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--attach-detach-reconcile-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 1m0s</td>
    -->
      <td colspan="2">--attach-detach-reconcile-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 1m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The reconciler sync wait time between volume attach detach. This duration must be larger than one second, and increasing this value from the default may allow for volumes to be mismatched with pods.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在 volume 连接和分离时协调同步等待时间。这个持续时间必须大于1秒，并且从默认值增加这个值可能导致 volumes 与pod不匹配。</td>
    </tr>

    <tr>
      <td colspan="2">--azure-container-registry-config string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Path to the file containing Azure container registry configuration information.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">包含 Azure 容器注册表配置信息的文件的路径。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--bind-address ip&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 0.0.0.0</td>
    -->
      <td colspan="2">--bind-address ip&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 0.0.0.0</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The IP address on which to listen for the --secure-port port. The associated interface(s) must be reachable by the rest of the cluster, and by CLI/web clients. If blank, all interfaces will be used (0.0.0.0 for all IPv4 interfaces and :: for all IPv6 interfaces).</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">要监听 --secure-port 端口的 IP 地址。关联的接口必须可以被群集的其余部分以及 CLI/Web 客户端访问。如果为空，则将使用所有接口（所有 IPv4 接口均为0.0.0.0，所有 IPv6 接口为::）。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--cert-dir string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "/var/run/kubernetes"</td>
    -->
      <td colspan="2">--cert-dir string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "/var/run/kubernetes"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The directory where the TLS certs are located. If --tls-cert-file and --tls-private-key-file are provided, this flag will be ignored.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">TLS 证书所在的目录。如果提供了 --tls-cert-file 和 --tls-private-key-file，则将忽略此标志。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--cidr-allocator-type string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "RangeAllocator"</td>
    -->
      <td colspan="2">--cidr-allocator-type string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "RangeAllocator"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Type of CIDR allocator to use</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">要使用的 CIDR 分配器的类型</td>
    </tr>

    <tr>
      <td colspan="2">--cloud-config string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The path to the cloud provider configuration file. Empty string for no configuration file.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">云提供商配置文件的路径。如果没有配置文件则设置为空。</td>
    </tr>

    <tr>
      <td colspan="2">--cloud-provider string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The provider for cloud services. Empty string for no provider.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">云服务提供商。如果没有云服务供应商，则设置为空。</td>
    </tr>

    <tr>
      <td colspan="2">--cluster-cidr string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">CIDR Range for Pods in cluster. Requires --allocate-node-cidrs to be true</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">群集中 Pod 的 CIDR 范围。要求 --allocate-node-cidrs 为 true</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--cluster-name string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "kubernetes"</td>
    -->
      <td colspan="2">--cluster-name string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "kubernetes"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The instance prefix for the cluster.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">集群的实例前缀。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--cluster-signing-cert-file string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "/etc/kubernetes/ca/ca.pem"</td>
    -->
      <td colspan="2">--cluster-signing-cert-file string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "/etc/kubernetes/ca/ca.pem"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Filename containing a PEM-encoded X509 CA certificate used to issue cluster-scoped certificates</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">文件名包含 PEM 编码的 X509 CA 证书，用于颁发群集范围的证书</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--cluster-signing-key-file string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "/etc/kubernetes/ca/ca.key"</td>
    -->
      <td colspan="2">--cluster-signing-key-file string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "/etc/kubernetes/ca/ca.key"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Filename containing a PEM-encoded RSA or ECDSA private key used to sign cluster-scoped certificates</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">文件名包含 PEM 编码的 RSA 或 ECDSA 私钥，用于对集群范围的证书进行签名</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-deployment-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5</td>
    -->
      <td colspan="2">--concurrent-deployment-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of deployment objects that are allowed to sync concurrently. Larger number = more responsive deployments, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的部署对象的数量。更大的数量=响应更快的部署，但 CPU（和网络）负载更多。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-endpoint-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5</td>
    -->
      <td colspan="2">--concurrent-endpoint-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of endpoint syncing operations that will be done concurrently. Larger number = faster endpoint updating, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">同步操作多个 endpoint,并且将会同时完成。更大的数字=更快的端点更新,但更多的 CPU（和网络）负载。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-gc-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 20</td>
    -->
      <td colspan="2">--concurrent-gc-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 20</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of garbage collector workers that are allowed to sync concurrently.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的垃圾收集器数量。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-namespace-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 10</td>
    -->
      <td colspan="2">--concurrent-namespace-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 10</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of namespace objects that are allowed to sync concurrently. Larger number = more responsive namespace termination, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许并发同步的命名空间对象的数量。更大的数字=响应更快的命名空间终止，但更多的CPU（和网络）负载</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-replicaset-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5</td>
    -->
      <td colspan="2">--concurrent-replicaset-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of replica sets that are allowed to sync concurrently. Larger number = more responsive replica management, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的 replica 集数。 数字越大=响应更快的 replica management，但 CPU（和网络）负载越多</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-resource-quota-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5</td>
    -->
      <td colspan="2">--concurrent-resource-quota-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of resource quotas that are allowed to sync concurrently. Larger number = more responsive quota management, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的 resource quotas。 数字越大=响应更快的 quota management，但 CPU负载（和网络）负载越多</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-service-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 1</td>
    -->
      <td colspan="2">--concurrent-service-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 1</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of services that are allowed to sync concurrently. Larger number = more responsive service management, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的 services。更大的数字=响应更快的 service management，但更多的 CPU（和网络）负载</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent-serviceaccount-token-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5</td>
    -->
      <td colspan="2">--concurrent-serviceaccount-token-syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of service account token objects that are allowed to sync concurrently. Larger number = more responsive token generation, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的服务帐户令牌对象的数量。 数字越大=响应式令牌生成越多，但CPU（和网络）负载越多</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--concurrent_rc_syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5</td>
    -->
      <td colspan="2">--concurrent_rc_syncs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The number of replication controllers that are allowed to sync concurrently. Larger number = more responsive replica management, but more CPU (and network) load</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">允许同时同步的 replication controllers的数量。数字越大=响应更快的副本管理，但 CPU（和网络）负载越多。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--configure-cloud-routes&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: true</td>
    -->
      <td colspan="2">--configure-cloud-routes&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: true</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Should CIDRs allocated by allocate-node-cidrs be configured on the cloud provider.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">是否应在云提供商上配置 allocate-node-cidrs 分配的 CIDR。</td>
    </tr>

    <tr>
      <td colspan="2">--contention-profiling</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Enable lock contention profiling, if profiling is enabled</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">如果启用了性能分析，则启用锁竞争分析</td>
    </tr>

    <tr>
      <td colspan="2">--controller-start-interval duration</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Interval between starting controller managers.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">启动 controller managers 之间的间隔。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--controllers stringSlice&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: [*]</td>
    -->
      <td colspan="2">--controllers stringSlice&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: [*]</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">A list of controllers to enable.  '*' enables all on-by-default controllers, 'foo' enables the controller named 'foo', '-foo' disables the controller named 'foo'.<br/>All controllers: attachdetach, bootstrapsigner, clusterrole-aggregation, cronjob, csrapproving, csrcleaner, csrsigning, daemonset, deployment, disruption, endpoint, garbagecollector, horizontalpodautoscaling, job, namespace, nodeipam, nodelifecycle, persistentvolume-binder, persistentvolume-expander, podgc, pv-protection, pvc-protection, replicaset, replicationcontroller, resourcequota, route, service, serviceaccount, serviceaccount-token, statefulset, tokencleaner, ttl<br/>Disabled-by-default controllers: bootstrapsigner, tokencleaner</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">启用所有的 controller。'*'代表启用所有默认的 controller，'foo' 代表启用名为 'foo' 的 controller，' - foo'代表禁用名为'foo'的 controller 。<br/>所有的 controller: attachdetach, bootstrapsigner, clusterrole-aggregation, cronjob, csrapproving, csrcleaner, csrsigning, daemonset, deployment, disruption, endpoint, garbagecollector, horizontalpodautoscaling, job, namespace, nodeipam, nodelifecycle, persistentvolume-binder, persistentvolume-expander, podgc, pv-protection, pvc-protection, replicaset, replicationcontroller, resourcequota, route, service, serviceaccount, serviceaccount-token, statefulset, tokencleaner, ttl<br/>默认禁用的controllers: bootstrapsigner, tokencleaner</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--deployment-controller-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 30s</td>
    -->
      <td colspan="2">--deployment-controller-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 30s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Period for syncing the deployments.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">同步部署的周期。</td>
    </tr>

    <tr>
      <td colspan="2">--disable-attach-detach-reconcile-sync</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Disable volume attach detach reconciler sync. Disabling this may cause volumes to be mismatched with pods. Use wisely.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">禁用 volume 的 attach 和 detach 同步功能。禁用此功能可能会导致 volumes 与 pod 不匹配。理智地使用。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--enable-dynamic-provisioning&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: true</td>
    -->
      <td colspan="2">--enable-dynamic-provisioning&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: true</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Enable dynamic provisioning for environments that support it.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">为支持它的环境启用动态配置。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--enable-garbage-collector&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: true</td>
    -->
      <td colspan="2">--enable-garbage-collector&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: true</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Enables the generic garbage collector. MUST be synced with the corresponding flag of the kube-apiserver.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">启用通用垃圾收集器。必须与 kube-apiserver 的相应标志同步。</td>
    </tr>

    <tr>
      <td colspan="2">--enable-hostpath-provisioner</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Enable HostPath PV provisioning when running without a cloud provider. This allows testing and development of provisioning features.  HostPath provisioning is not supported in any way, won't work in a multi-node cluster, and should not be used for anything other than testing or development.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在没有云提供商的情况下启用 HostPath PV 配置。这允许测试和开发配置该功能。不支持 HostPath 配置，不能在多节点群集中工作，也不应用于测试或开发之外的任何其他设置。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--enable-taint-manager&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: true</td>
    -->
      <td colspan="2">--enable-taint-manager&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: true</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">WARNING: Beta feature. If set to true enables NoExecute Taints and will evict all not-tolerating Pod running on Nodes tainted with this kind of Taints.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">警告：测试版功能。如果设置为 true，则启用 NoExecute Taints，并驱逐在被这种污点污染的节点上运行的所有不能容忍的 Pod。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--experimental-cluster-signing-duration duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 8760h0m0s</td>
    -->
      <td colspan="2">--experimental-cluster-signing-duration duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 8760h0m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The length of duration signed certificates will be given.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">将给出签署证书的持续时间长度。</td>
    </tr>

    <tr>
      <td colspan="2">--external-cloud-volume-plugin string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The plugin to use when cloud provider is set to external. Can be empty, should only be set when cloud-provider is external. Currently used to allow node and volume controllers to work for in tree cloud providers.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">云提供商设置为外部时使用的插件。可以为空，只应在云提供商为外部时设置。目前用于允许 node 和 volume controllers 为内置的云提供商工作。</td>
    </tr>

    <tr>
      <td colspan="2">--feature-gates mapStringBool</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">A set of key=value pairs that describe feature gates for alpha/experimental features. Options are:<br/>APIListChunking=true|false (BETA - default=true)<br/>APIResponseCompression=true|false (ALPHA - default=false)<br/>AdvancedAuditing=true|false (BETA - default=true)<br/>AllAlpha=true|false (ALPHA - default=false)<br/>AppArmor=true|false (BETA - default=true)<br/>AttachVolumeLimit=true|false (ALPHA - default=false)<br/>BalanceAttachedNodeVolumes=true|false (ALPHA - default=false)<br/>BlockVolume=true|false (ALPHA - default=false)<br/>CPUManager=true|false (BETA - default=true)<br/>CRIContainerLogRotation=true|false (BETA - default=true)<br/>CSIBlockVolume=true|false (ALPHA - default=false)<br/>CSIPersistentVolume=true|false (BETA - default=true)<br/>CustomPodDNS=true|false (BETA - default=true)<br/>CustomResourceSubresources=true|false (BETA - default=true)<br/>CustomResourceValidation=true|false (BETA - default=true)<br/>DebugContainers=true|false (ALPHA - default=false)<br/>DevicePlugins=true|false (BETA - default=true)<br/>DynamicKubeletConfig=true|false (BETA - default=true)<br/>DynamicProvisioningScheduling=true|false (ALPHA - default=false)<br/>EnableEquivalenceClassCache=true|false (ALPHA - default=false)<br/>ExpandInUsePersistentVolumes=true|false (ALPHA - default=false)<br/>ExpandPersistentVolumes=true|false (BETA - default=true)<br/>ExperimentalCriticalPodAnnotation=true|false (ALPHA - default=false)<br/>ExperimentalHostUserNamespaceDefaulting=true|false (BETA - default=false)<br/>GCERegionalPersistentDisk=true|false (BETA - default=true)<br/>HugePages=true|false (BETA - default=true)<br/>HyperVContainer=true|false (ALPHA - default=false)<br/>Initializers=true|false (ALPHA - default=false)<br/>KubeletPluginsWatcher=true|false (ALPHA - default=false)<br/>LocalStorageCapacityIsolation=true|false (BETA - default=true)<br/>MountContainers=true|false (ALPHA - default=false)<br/>MountPropagation=true|false (BETA - default=true)<br/>PersistentLocalVolumes=true|false (BETA - default=true)<br/>PodPriority=true|false (BETA - default=true)<br/>PodReadinessGates=true|false (BETA - default=false)<br/>PodShareProcessNamespace=true|false (ALPHA - default=false)<br/>QOSReserved=true|false (ALPHA - default=false)<br/>ReadOnlyAPIDataVolumes=true|false (DEPRECATED - default=true)<br/>ResourceLimitsPriorityFunction=true|false (ALPHA - default=false)<br/>ResourceQuotaScopeSelectors=true|false (ALPHA - default=false)<br/>RotateKubeletClientCertificate=true|false (BETA - default=true)<br/>RotateKubeletServerCertificate=true|false (ALPHA - default=false)<br/>RunAsGroup=true|false (ALPHA - default=false)<br/>ScheduleDaemonSetPods=true|false (ALPHA - default=false)<br/>ServiceNodeExclusion=true|false (ALPHA - default=false)<br/>ServiceProxyAllowExternalIPs=true|false (DEPRECATED - default=false)<br/>StorageObjectInUseProtection=true|false (default=true)<br/>StreamingProxyRedirects=true|false (BETA - default=true)<br/>SupportIPVSProxyMode=true|false (default=true)<br/>SupportPodPidsLimit=true|false (ALPHA - default=false)<br/>Sysctls=true|false (BETA - default=true)<br/>TaintBasedEvictions=true|false (ALPHA - default=false)<br/>TaintNodesByCondition=true|false (ALPHA - default=false)<br/>TokenRequest=true|false (ALPHA - default=false)<br/>TokenRequestProjection=true|false (ALPHA - default=false)<br/>VolumeScheduling=true|false (BETA - default=true)<br/>VolumeSubpath=true|false (default=true)<br/>VolumeSubpathEnvExpansion=true|false (ALPHA - default=false)</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">一组 key=value 键值对，用于描述 alpha/experimental 的 feature gates 特征。选项包括：<br/>APIListChunking=true|false (BETA - 默认等于 true)<br/>APIResponseCompression=true|false (ALPHA - 默认等于 false)<br/>AdvancedAuditing=true|false (BETA - 默认等于 true)<br/>AllAlpha=true|false (ALPHA - 默认等于 false)<br/>AppArmor=true|false (BETA - 默认等于 true)<br/>AttachVolumeLimit=true|false (ALPHA - 默认等于 false)<br/>BalanceAttachedNodeVolumes=true|false (ALPHA - 默认等于 false)<br/>BlockVolume=true|false (ALPHA - 默认等于 false)<br/>CPUManager=true|false (BETA - 默认等于 true)<br/>CRIContainerLogRotation=true|false (BETA - 默认等于 true)<br/>CSIBlockVolume=true|false (ALPHA - 默认等于 false)<br/>CSIPersistentVolume=true|false (BETA - 默认等于 true)<br/>CustomPodDNS=true|false (BETA - 默认等于 true)<br/>CustomResourceSubresources=true|false (BETA - 默认等于 true)<br/>CustomResourceValidation=true|false (BETA - 默认等于 true)<br/>DebugContainers=true|false (ALPHA - 默认等于 false)<br/>DevicePlugins=true|false (BETA - 默认等于true)<br/>DynamicKubeletConfig=true|false (BETA - 默认等于true)<br/>DynamicProvisioningScheduling=true|false (ALPHA - 默认等于false)<br/>EnableEquivalenceClassCache=true|false (ALPHA - 默认等于false)<br/>ExpandInUsePersistentVolumes=true|false (ALPHA - 默认等于false)<br/>ExpandPersistentVolumes=true|false (BETA - 默认等于true)<br/>ExperimentalCriticalPodAnnotation=true|false (ALPHA - 默认等于false)<br/>ExperimentalHostUserNamespaceDefaulting=true|false (BETA - 默认等于false)<br/>GCERegionalPersistentDisk=true|false (BETA - 默认等于true)<br/>HugePages=true|false (BETA - 默认等于true)<br/>HyperVContainer=true|false (ALPHA - 默认等于false)<br/>Initializers=true|false (ALPHA - 默认等于false)<br/>KubeletPluginsWatcher=true|false (ALPHA - 默认等于false)<br/>LocalStorageCapacityIsolation=true|false (BETA - 默认等于true)<br/>MountContainers=true|false (ALPHA - 默认等于false)<br/>MountPropagation=true|false (BETA - 默认等于true)<br/>PersistentLocalVolumes=true|false (BETA - 默认等于true)<br/>PodPriority=true|false (BETA - 默认等于true)<br/>PodReadinessGates=true|false (BETA - 默认等于false)<br/>PodShareProcessNamespace=true|false (ALPHA - 默认等于false)<br/>QOSReserved=true|false (ALPHA - 默认等于false)<br/>ReadOnlyAPIDataVolumes=true|false (DEPRECATED - 默认等于true)<br/>ResourceLimitsPriorityFunction=true|false (ALPHA - 默认等于false)<br/>ResourceQuotaScopeSelectors=true|false (ALPHA - 默认等于false)<br/>RotateKubeletClientCertificate=true|false (BETA - 默认等于true)<br/>RotateKubeletServerCertificate=true|false (ALPHA - 默认等于false)<br/>RunAsGroup=true|false (ALPHA - 默认等于false)<br/>ScheduleDaemonSetPods=true|false (ALPHA - 默认等于false)<br/>ServiceNodeExclusion=true|false (ALPHA - 默认等于false)<br/>ServiceProxyAllowExternalIPs=true|false (DEPRECATED - 默认等于false)<br/>StorageObjectInUseProtection=true|false (默认等于true)<br/>StreamingProxyRedirects=true|false (BETA - 默认等于true)<br/>SupportIPVSProxyMode=true|false (默认等于true)<br/>SupportPodPidsLimit=true|false (ALPHA - 默认等于false)<br/>Sysctls=true|false (BETA - 默认等于true)<br/>TaintBasedEvictions=true|false (ALPHA - 默认等于false)<br/>TaintNodesByCondition=true|false (ALPHA - 默认等于false)<br/>TokenRequest=true|false (ALPHA - 默认等于false)<br/>TokenRequestProjection=true|false (ALPHA - 默认等于false)<br/>VolumeScheduling=true|false (BETA - 默认等于true)<br/>VolumeSubpath=true|false (默认等于true)<br/>VolumeSubpathEnvExpansion=true|false (ALPHA - 默认等于false)</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--flex-volume-plugin-dir string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "/usr/libexec/kubernetes/kubelet-plugins/volume/exec/"</td>
    -->
      <td colspan="2">--flex-volume-plugin-dir string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "/usr/libexec/kubernetes/kubelet-plugins/volume/exec/"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Full path of the directory in which the flex volume plugin should search for additional third party volume plugins.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">flex 卷插件应搜索其他第三方卷插件的目录的完整路径。</td>
    </tr>

    <tr>
      <td colspan="2">-h, --help</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">help for kube-controller-manager</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">kube-controller-manager 帮助</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--horizontal-pod-autoscaler-downscale-delay duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5m0s</td>
    -->
      <td colspan="2">--horizontal-pod-autoscaler-downscale-delay duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period since last downscale, before another downscale can be performed in horizontal pod autoscaler.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在 HPA(horizontal pod autoscaler)过程中，自上一次 pod 缩容到下一次 pod 缩容的时间段</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--horizontal-pod-autoscaler-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 30s</td>
    -->
      <td colspan="2">--horizontal-pod-autoscaler-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 30s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period for syncing the number of pods in horizontal pod autoscaler.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在 HPA(horizontal pod autoscaler)过程中,同步 pods 的时间段</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--horizontal-pod-autoscaler-tolerance float&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 0.1</td>
    -->
      <td colspan="2">--horizontal-pod-autoscaler-tolerance float&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 0.1</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The minimum change (from 1.0) in the desired-to-actual metrics ratio for the horizontal pod autoscaler to consider scaling.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">HPA(horizontal pod autoscaler)考虑扩缩容时从期望到实际度量比的最小变化（从1开始）。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--horizontal-pod-autoscaler-upscale-delay duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 3m0s</td>
    -->
      <td colspan="2">--horizontal-pod-autoscaler-upscale-delay duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 3m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period since last upscale, before another upscale can be performed in horizontal pod autoscaler.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在 HPA(horizontal pod autoscaler)过程中，自上一次 pod 扩容到下一次 pod 扩容的时间段</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--horizontal-pod-autoscaler-use-rest-clients&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: true</td>
    -->
      <td colspan="2">--horizontal-pod-autoscaler-use-rest-clients&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: true</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">If set to true, causes the horizontal pod autoscaler controller to use REST clients through the kube-aggregator, instead of using the legacy metrics client through the API server proxy.  This is required for custom metrics support in the horizontal pod autoscaler.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">如果设置为 true，HPA(horizontal pod autoscaler controller)通过 kube-aggregator 使用 REST clients,而不是通过 API server proxy 使用旧的 metrics clients,这是 HPA(horizontal pod autoscaler)中自定义所支持的指标所必需的。</td>
    </tr>

    <tr>
      <td colspan="2">--http2-max-streams-per-connection int</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The limit that the server gives to clients for the maximum number of streams in an HTTP/2 connection. Zero means to use golang's default.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">服务器为在 HTTP/2 连接中,服务器为客户端提供最大 streams 数量的限制。零意味着使用 golang 的默认值。</td>
    </tr>

    <tr>
      <td colspan="2">--insecure-experimental-approve-all-kubelet-csrs-for-group string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">This flag does nothing.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">这个标志什么也没做。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--kube-api-burst int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 30</td>
    -->
      <td colspan="2">--kube-api-burst int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 30</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Burst to use while talking with kubernetes apiserver.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在与 kubernetes apiserver 交互时使用。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--kube-api-content-type string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "application/vnd.kubernetes.protobuf"</td>
    -->
      <td colspan="2">--kube-api-content-type string&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "application/vnd.kubernetes.protobuf"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Content type of requests sent to apiserver.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">发送到 apiserver 的请求的内容类型。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--kube-api-qps float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 20</td>
    -->
      <td colspan="2">--kube-api-qps float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 20</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">QPS to use while talking with kubernetes apiserver.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">与 kubernetes apiserver 交谈时使用的 QPS。</td>
    </tr>

    <tr>
      <td colspan="2">--kubeconfig string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Path to kubeconfig file with authorization and master location information.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">具有 authorization 和 master 位置信息的 kubeconfig 文件的路径。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--large-cluster-size-threshold int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 50</td>
    -->
      <td colspan="2">--large-cluster-size-threshold int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 50</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Number of nodes from which NodeController treats the cluster as large for the eviction logic purposes. --secondary-node-eviction-rate is implicitly overridden to 0 for clusters this size or smaller.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">NodeController 将群集视为驱逐逻辑目的的 nodes。对于此大小或更小的群集，--secondary-node-eviction-rate 被隐式重写为0。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--leader-elect&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: true</td>
    -->
      <td colspan="2">--leader-elect&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: true</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Start a leader election client and gain leadership before executing the main loop. Enable this when running replicated components for high availability.</td>
    -->
    <td></td><td style="line-height: 130%; word-wrap: break-word;">在执行主循环之前，启动 leader 选举 client 并获得领导地位。在运行 replicated 组件以实现高可用性时启用此选项。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--leader-elect-lease-duration duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 15s</td>
    -->
      <td colspan="2">--leader-elect-lease-duration duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 15s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The duration that non-leader candidates will wait after observing a leadership renewal until attempting to acquire leadership of a led but unrenewed leader slot. This is effectively the maximum duration that a leader can be stopped before it is replaced by another candidate. This is only applicable if leader election is enabled.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;"> 一个非 leader 的候选人，在观察到领导权更新并尝试获取领导权的时间间隔，该时间间隔没有领导权的更新。这实际上是 leader 在被另一个候选人替换之前停止的最长持续时间。这仅适用于启用 leader 选举的情况。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--leader-elect-renew-deadline duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 10s</td>
    -->
      <td colspan="2">--leader-elect-renew-deadline duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 10s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The interval between attempts by the acting master to renew a leadership slot before it stops leading. This must be less than or equal to the lease duration. This is only applicable if leader election is enabled.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">一个活跃的 master 在停止 lead 之前更新 leadership 的时间间隔，这必须小于或等于租约期限。这仅适用于启用领导者选举的情况。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--leader-elect-resource-lock endpoints&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: "endpoints"</td>
    -->
      <td colspan="2">--leader-elect-resource-lock endpoints&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: "endpoints"</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The type of resource object that is used for locking during leader election. Supported options are endpoints (default) and `configmaps`.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在 leader 选举期间用于锁定的资源对象的类型。支持的选项是 endpoints（默认）和 `configmaps`。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--leader-elect-retry-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 2s</td>
    -->
      <td colspan="2">--leader-elect-retry-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 2s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The duration the clients should wait between attempting acquisition and renewal of a leadership. This is only applicable if leader election is enabled.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">客户在尝试获取和更新领导之间应该等待的持续时间。这仅适用于启用 leader 选举的情况。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--log-flush-frequency duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5s</td>
    -->
      <td colspan="2">--log-flush-frequency duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Maximum number of seconds between log flushes</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">日志刷新之间的最大秒数</td>
    </tr>

    <tr>
      <td colspan="2">--master string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The address of the Kubernetes API server (overrides any value in kubeconfig).</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Kubernetes API 服务器的地址（覆盖 kubeconfig 中的任何值）。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--min-resync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 12h0m0s</td>
    -->
      <td colspan="2">--min-resync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 12h0m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The resync period in reflectors will be random between MinResyncPeriod and 2*MinResyncPeriod.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">反射器中重新同步的周期随机在 MinResyncPeriod 和 2 * MinResyncPeriod 之间。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--namespace-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5m0s</td>
    -->
      <td colspan="2">--namespace-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period for syncing namespace life-cycle updates</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">同步命名空间生命周期更新的周期</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--node-cidr-mask-size int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 24</td>
    -->
      <td colspan="2">--node-cidr-mask-size int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 24</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Mask size for node cidr in cluster.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">集群中节点 CIDR 的掩码大小。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--node-eviction-rate float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 0.1</td>
    -->
      <td colspan="2">--node-eviction-rate float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 0.1</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Number of nodes per second on which pods are deleted in case of node failure when a zone is healthy (see --unhealthy-zone-threshold for definition of healthy/unhealthy). Zone refers to entire cluster in non-multizone clusters.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">当区域健康时，如果节点发生故障，每秒删除 pod 的节点数（请参阅 --unhealthy-zone-threshold 健康/不健康的定义）。 区域是指非多区域群集中的整个群集。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--node-monitor-grace-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 40s</td>
    -->
      <td colspan="2">--node-monitor-grace-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 40s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Amount of time which we allow running Node to be unresponsive before marking it unhealthy. Must be N times more than kubelet's nodeStatusUpdateFrequency, where N means number of retries allowed for kubelet to post node status.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在标记为不健康之前,我们允许运行 Node 无响应的时间。必须是 kubelet 的 nodeStatusUpdateFrequency 的 N 倍，其中N表示允许 kubelet 发布节点状态的重试次数。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--node-monitor-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5s</td>
    -->
      <td colspan="2">--node-monitor-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period for syncing NodeStatus in NodeController.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在 NodeController 中同步 NodeStatus 的时间段。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--node-startup-grace-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 1m0s</td>
    -->
      <td colspan="2">--node-startup-grace-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 1m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Amount of time which we allow starting Node to be unresponsive before marking it unhealthy.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在标记为不健康之前，我们允许启动 Node 无响应的时间。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--pod-eviction-timeout duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5m0s</td>
    -->
      <td colspan="2">--pod-eviction-timeout duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The grace period for deleting pods on failed nodes.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">删除故障节点上 pod 的宽限期。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--port int&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 10252</td>
    -->
      <td colspan="2">--port int&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 10252</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">DEPRECATED: the port on which to serve HTTP insecurely without authentication and authorization. If 0, don't serve HTTPS at all. See --secure-port instead.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">弃用：不经身份验证和授权就可以安全地提供 HTTP 服务的端口。如果0，根本不不提供 HTTPS 服务。请参见安全端口。</td>
    </tr>

    <tr>
      <td colspan="2">--profiling</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Enable profiling via web interface host:port/debug/pprof/</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">通过 Web 接口主机实现配置文件： port/debug/pprof/</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--pv-recycler-increment-timeout-nfs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 30</td>
    -->
      <td colspan="2">--pv-recycler-increment-timeout-nfs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 30</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">the increment of time added per Gi to ActiveDeadlineSeconds for an NFS scrubber pod</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">为 NFS scrubber pod 添加 Gi 到 ActiveDeadlineSeconds 的时间增量</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--pv-recycler-minimum-timeout-hostpath int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 60</td>
    -->
      <td colspan="2">--pv-recycler-minimum-timeout-hostpath int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 60</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The minimum ActiveDeadlineSeconds to use for a HostPath Recycler pod.  This is for development and testing only and will not work in a multi-node cluster.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">用于 HostPath Recycler pod 的最小 ActiveDeadlineSeconds。这仅用于开发和测试，不适用于多节点群集。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--pv-recycler-minimum-timeout-nfs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 300</td>
    -->
      <td colspan="2">--pv-recycler-minimum-timeout-nfs int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 300</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The minimum ActiveDeadlineSeconds to use for an NFS Recycler pod</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">用于 NFS Recycler pod 的最小 ActiveDeadlineSeconds</td>
    </tr>

    <tr>
      <td colspan="2">--pv-recycler-pod-template-filepath-hostpath string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The file path to a pod definition used as a template for HostPath persistent volume recycling. This is for development and testing only and will not work in a multi-node cluster.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">pod 定义的文件路径，用作 HostPath 持久卷回收的模板。这仅用于开发和测试，不适用于多节点群集。</td>
    </tr>

    <tr>
      <td colspan="2">--pv-recycler-pod-template-filepath-nfs string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The file path to a pod definition used as a template for NFS persistent volume recycling</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">pod 定义的文件路径，用作 NFS 持久卷回收的模板</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--pv-recycler-timeout-increment-hostpath int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 30</td>
    -->
      <td colspan="2">--pv-recycler-timeout-increment-hostpath int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 30</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">the increment of time added per Gi to ActiveDeadlineSeconds for a HostPath scrubber pod.  This is for development and testing only and will not work in a multi-node cluster.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">HostPath scrubber pod 添加每个 Gi 到 ActiveDeadlineSeconds 的时间增量。这仅用于开发和测试，不适用于多节点群集。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--pvclaimbinder-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 15s</td>
    -->
      <td colspan="2">--pvclaimbinder-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 15s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period for syncing persistent volumes and persistent volume claims</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">同步持久卷和持久卷声明的时间段</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--resource-quota-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 5m0s</td>
    -->
      <td colspan="2">--resource-quota-sync-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 5m0s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period for syncing quota usage status in the system</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在系统中同步配额使用状态的时间段</td>
    </tr>

    <tr>
      <td colspan="2">--root-ca-file string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">If set, this root certificate authority will be included in service account's token secret. This must be a valid PEM-encoded CA bundle.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">如果设置，则此根证书颁发机构将包含在服务帐户的令牌机密中。这必须是有效的 PEM 编码的 CA 捆绑包。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--route-reconciliation-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 10s</td>
    -->
      <td colspan="2">--route-reconciliation-period duration&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 10s</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The period for reconciling routes created for Nodes by cloud provider.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">协调由云提供商为节点创建路由的时间段。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--secondary-node-eviction-rate float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 0.01</td>
    -->
      <td colspan="2">--secondary-node-eviction-rate float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 0.01</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Number of nodes per second on which pods are deleted in case of node failure when a zone is unhealthy (see --unhealthy-zone-threshold for definition of healthy/unhealthy). Zone refers to entire cluster in non-multizone clusters. This value is implicitly overridden to 0 if the cluster size is smaller than --large-cluster-size-threshold.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">当区域不健康时,节点发生故障时,删除 pod 的每秒节点数（请参阅--unhealthy-zone-threshold定义健康/不健康）。 区域是指非多区域群集中的整个群集。 如果簇大小小于--large-cluster-size-threshold，则会将此值隐式重写为0。</td>
    </tr>

    <tr>
      <td colspan="2">--secure-port int</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">The port on which to serve HTTPS with authentication and authorization. If 0, don't serve HTTPS at all.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">通过身份验证和授权为 HTTPS 提供服务的端口。如果为0，则根本不提供 HTTPS。</td>
    </tr>

    <tr>
      <td colspan="2">--service-cluster-ip-range string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">CIDR Range for Services in cluster. Requires --allocate-node-cidrs to be true</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">集群中服务的 CIDR 范围。 要求 --allocate-node-cidrs 为 true</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--terminated-pod-gc-threshold int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 12500</td>
    -->
      <td colspan="2">--terminated-pod-gc-threshold int32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 12500</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Number of terminated pods that can exist before the terminated pod garbage collector starts deleting terminated pods. If &lt;= 0, the terminated pod garbage collector is disabled.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">在终止的 pod 垃圾收集器开始删除已终止的 pod 之前可以存在的已终止 pods。如果 ＆lt;=0，则禁用终止的 pod 垃圾收集器。</td>
    </tr>

    <tr>
      <td colspan="2">--tls-cert-file string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">File containing the default x509 Certificate for HTTPS. (CA cert, if any, concatenated after server cert). If HTTPS serving is enabled, and --tls-cert-file and --tls-private-key-file are not provided, a self-signed certificate and key are generated for the public address and saved to the directory specified by --cert-dir.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">包含 HTTPS 的默认 x509 证书的文件。（CA 证书，如果有的话，在服务器证书之后连接）。如果启用了 HTTPS 服务，并且未提供 --tls-cert-file和--tls-private-key-file，则会为公共地址生成自签名证书和密钥，并将其保存到指定的 --cert-dir 目录中。</td>
    </tr>

    <tr>
      <td colspan="2">--tls-cipher-suites stringSlice</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Comma-separated list of cipher suites for the server. If omitted, the default Go cipher suites will be use.  Possible values: TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305,TLS_ECDHE_ECDSA_WITH_RC4_128_SHA,TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305,TLS_ECDHE_RSA_WITH_RC4_128_SHA,TLS_RSA_WITH_3DES_EDE_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_GCM_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_256_GCM_SHA384,TLS_RSA_WITH_RC4_128_SHA</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">用于服务器的以逗号分隔的密码套件列表。如果省略，将使用默认的 Go 密码套件。可能的值：TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA，TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256，TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256，TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA，TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384，TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305，TLS_ECDHE_ECDSA_WITH_RC4_128_SHA，TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA，TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA，TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256，TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256，TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA，TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384，TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305，TLS_ECDHE_RSA_WITH_RC4_128_SHA，TLS_RSA_WITH_3DES_EDE_CBC_SHA，TLS_RSA_WITH_AES_128_CBC_SHA，TLS_RSA_WITH_AES_128_CBC_SHA256，TLS_RSA_WITH_AES_128_GCM_SHA256，TLS_RSA_WITH_AES_256_CBC_SHA，TLS_RSA_WITH_AES_256_GCM_SHA384，TLS_RSA_WITH_RC4_128_SHA</td>
    <tr>

    <tr>
      <td colspan="2">--tls-min-version string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Minimum TLS version supported. Possible values: VersionTLS10, VersionTLS11, VersionTLS12</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">支持最低 TLS 版本。可能的值：VersionTLS10，VersionTLS11，VersionTLS12</td>
    </tr>

    <tr>
      <td colspan="2">--tls-private-key-file string</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">File containing the default x509 private key matching --tls-cert-file.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">包含与 --tls-cert-file 匹配的默认 x509 私钥文件。</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--tls-sni-cert-key namedCertKey&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: []</td>
    -->
      <td colspan="2">--tls-sni-cert-key namedCertKey&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: []</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">A pair of x509 certificate and private key file paths, optionally suffixed with a list of domain patterns which are fully qualified domain names, possibly with prefixed wildcard segments. If no domain patterns are provided, the names of the certificate are extracted. Non-wildcard matches trump over wildcard matches, explicit domain patterns trump over extracted names. For multiple key/certificate pairs, use the --tls-sni-cert-key multiple times. Examples: "example.crt,example.key" or "foo.crt,foo.key:*.foo.com,foo.com".</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">一对 x509 证书和私钥文件路径，</td>
    </tr>

    <tr>
    <!--
      <td colspan="2">--unhealthy-zone-threshold float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Default: 0.55</td>
    -->
      <td colspan="2">--unhealthy-zone-threshold float32&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认: 0.55</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Fraction of Nodes in a zone which needs to be not Ready (minimum 3) for zone to be treated as unhealthy. </td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">一个区域中部分 Nodes为notReady 状态（最小3），便被视为不健康的区域。</td>
    </tr>

    <tr>
      <td colspan="2">--use-service-account-credentials</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">If true, use individual service account credentials for each controller.</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">如果为 true，请为每个 controller 使用单独的服务帐户凭据。</td>
    </tr>

    <tr>
      <td colspan="2">--version version[=true]</td>
    </tr>
    <tr>
    <!--
      <td></td><td style="line-height: 130%; word-wrap: break-word;">Print version information and quit</td>
    -->
      <td></td><td style="line-height: 130%; word-wrap: break-word;">打印版本信息并退出</td>
    </tr>

  </tbody>
</table>