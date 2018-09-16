<!--

---
reviewers:
- sig-cluster-lifecycle
title: Upgrading kubeadm clusters from v1.10 to v1.11
content_template: templates/task
---

--->

---
reviewers:
- sig-cluster-lifecycle
title: 将kubeadm集群从 v1.10 版本更新到 v1.11 版本
content_template: templates/task
---

{{% capture overview %}}
<!--
This page explains how to upgrade a Kubernetes cluster created with `kubeadm` from version 1.10.x to version 1.11.x, and from version 1.11.x to 1.11.y, where `y > x`.
--->
本文介绍如何升级具有 `kubeadm`  的kubernetes集群，从 1.10.x 版本到 1.11.x 版本和从 1.11.x 版本到 1.11.y 版本（`y > x`）
{{% /capture %}}

{{% capture prerequisites %}}

<!--
- You need to have a `kubeadm` Kubernetes cluster running version 1.10.0 or later. Swap must be disabled. The cluster should use a static control plane and etcd pods.
- Make sure you read the [release notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.11.md) carefully.
- Make sure to back up any important components, such as app-level state storespecd in a database. `kubeadm upgrade` does not touch your workloads, only components internal to Kubernetes, but backups are always a best practice.
--->

- 您需要先安装一个版本为 1.10.0 或更高版本的 `kubeadm` Kubernetes 集群。另外还需要禁用节点的交换分区。该集群需要使用一个静态的控制面板和 etcd pods。
- 一定要认真阅读[发布通知](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.11.md)
- 确保备份任何重要的组件，例如存储在数据库中的 app-level 状态。`kubeadm upgrade`不涉及到您的工作负载，只涉及kubernetes内部组件，但是备份始终是最好的方案。

<!--
### Additional information
--->
### 此外

<!--
- All containers are restarted after upgrade, because the container spec hash value is changed.
- You can upgrade only from one minor version to the next minor version. That is, you cannot skip versions when you upgrade. For example, you can upgrade only from 1.10 to 1.11, not from 1.9 to 1.11.
- The default DNS provider in version 1.11 is [CoreDNS](https://coredns.io/) rather than [kube-dns](https://github.com/kubernetes/dns).
To keep `kube-dns`, pass `--feature-gates=CoreDNS=false` to `kubeadm upgrade apply`.
--->
- 在升级之后所有的容器要重新启动，因为容器规范的哈希值改变了。
- 您可以仅从一个小版本升级到下一个小版本。也就是说，当您升级的时候，您不能跨域版本来进行升级。比如您只能从 1.10 版本升级到 1.11 版本，不能直接从 1.9 版本升级到 1.11 版本。
- 在 1.11 版本中默认的 DNS 提供者是 [CoreDNS](https://coredns.io/) 而不是 [kube-dns](https://github.com/kubernetes/dns)。

为了保留 `kube-dns`，请传入 `--feature-gates=CoreDNS=false` 参数到 `kubeadm upgrade apply`。

{{% /capture %}}

{{% capture steps %}}

<!--
## Upgrade the control plane
-->
## 升级控制面板
<!--
1.  On your master node, run the following (as root):

        export VERSION=$(curl -sSL https://dl.k8s.io/release/stable.txt)  # or manually specify a released Kubernetes version
        export ARCH=amd64 # or: arm, arm64, ppc64le, s390x
        curl -sSL https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubeadm > /usr/bin/kubeadm
        chmod a+rx /usr/bin/kubeadm

    Note that upgrading the `kubeadm` package on your system prior to upgrading the control plane causes a failed upgrade. Even though `kubeadm` ships in the Kubernetes repositories, it's important to install it manually. The kubeadm team is working on fixing this limitation.
--->
1. 在您的 master 节点上，以 root 用户运行以下步骤：
                                            
        export VERSION=$(curl -sSL https://dl.k8s.io/release/stable.txt)  #或者手动的指定一个发布的 kubernetes 版本
        export ARCH=amd64 # or: arm, arm64, ppc64le, s390x
        curl -sSL https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubeadm > /usr/bin/kubeadm
        chmod a+rx /usr/bin/kubeadm
                                                                               
    请注意在您的系统升级控制平面之前升级的 `kubeadm` 包会造成升级失败。尽管 `kubeadm` ships 存储在 Kubernetes 仓库中，但是手动升级它是重要的。kubeadm 团队在努力解决这个限制。 

<!--   
1.  Verify that the download works and has the expected version:

    ```shell
    kubeadm version
    ```
-->
1.  确认 kubeadm 下载工作和达到预期的版本：
    ```shell
    kubeadm version
    ```

<!--
1.  On the master node, run:

    ```shell
    kubeadm upgrade plan
    ```

    You should see output similar to this:
-->
1. 在master节点上运行：
   ```shell
   kubeadm upgrade plan
   ```
  运行之后，可以看到类似下面的输出：
  <!-- TODO: copy-paste actual output once new version is stable -->

<!--
   ```shell
   [preflight] Running pre-flight checks.
   [upgrade] Making sure the cluster is healthy:
   [upgrade/config] Making sure the configuration is correct:
   [upgrade/config] Reading configuration from the cluster...
   [upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
   I0618 20:32:32.950358   15307 feature_gate.go:230] feature gates: &{map[]}
   [upgrade] Fetching available versions to upgrade to
   [upgrade/versions] Cluster version: v1.10.4
   [upgrade/versions] kubeadm version: v1.11.0-beta.2.78+e0b33dbc2bde88

   Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
   COMPONENT   CURRENT       AVAILABLE
   Kubelet     1 x v1.10.4   v1.11.0

   Upgrade to the latest version in the v1.10 series:
   COMPONENT            CURRENT   AVAILABLE
   API Server           v1.10.4   v1.11.0
   Controller Manager   v1.10.4   v1.11.0
   Scheduler            v1.10.4   v1.11.0
   Kube Proxy           v1.10.4   v1.11.0
   CoreDNS                        1.1.3
   Kube DNS             1.14.8
   Etcd                 3.1.12    3.2.18

   You can now apply the upgrade by executing the following command:

       kubeadm upgrade apply v1.11.0

   Note: Before you can perform this upgrade, you have to update kubeadm to v1.11.0. 
   _____________________________________________________________________
   ```
--->

   ```shell
   [preflight] Running pre-flight checks.
   [upgrade] Making sure the cluster is healthy:
   [upgrade/config] Making sure the configuration is correct:
   [upgrade/config] Reading configuration from the cluster...
   [upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
   I0618 20:32:32.950358   15307 feature_gate.go:230] feature gates: &{map[]}
   [upgrade] Fetching available versions to upgrade to
   [upgrade/versions] Cluster version: v1.10.4
   [upgrade/versions] kubeadm version: v1.11.0-beta.2.78+e0b33dbc2bde88

   在使用 'kubeadm upgrade apply' 升级控制面板之后，必须手动升级组件：
   COMPONENT   CURRENT       AVAILABLE
   Kubelet     1 x v1.10.4   v1.11.0

   在 v1.10 版本系列中升级到最新的版本：
   COMPONENT            CURRENT   AVAILABLE
   API Server           v1.10.4   v1.11.0
   Controller Manager   v1.10.4   v1.11.0
   Scheduler            v1.10.4   v1.11.0
   Kube Proxy           v1.10.4   v1.11.0
   CoreDNS                        1.1.3
   Kube DNS             1.14.8
   Etcd                 3.1.12    3.2.18

   现在您可以通过执行以下命令来进行升级：
                                                                                            
        kubeadm upgrade apply v1.11.0
                                                                                                            
   注意：在您执行升级之前，必须升级 kubeadm 到 v1.11.0 版本。   
   _____________________________________________________________________
   ```

<!--
    This command checks that your cluster can be upgraded, and fetches the versions you can upgrade to.
--->
    下面命令查看你的集群升级和获取您的集群所升级到的版本。

<!--
1.  Choose a version to upgrade to, and run the appropriate command. For example:

    ```shell
    kubeadm upgrade apply v1.11.0
    ```

    If you currently use `kube-dns` and wish to continue doing so, add `--feature-gates=CoreDNS=false`.

    You should see output similar to this:
--->
1. 选择一个版本来进行升级和运行正确的命令。例如：
    ```shell
    kubeadm upgrade apply v1.11.0
    ```
                                        
    如果您最近使用  `kube-dns` 和继续想这样使用的话，请添加 `--feature-gates=CoreDNS=false` 参数。
    可以看到下面类似的输出：
    <!-- TODO: output from stable -->

    ```shell
    [preflight] Running pre-flight checks.
    [upgrade] Making sure the cluster is healthy:
    [upgrade/config] Making sure the configuration is correct:
    [upgrade/config] Reading configuration from the cluster...
    [upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
    I0614 20:56:08.320369   30918 feature_gate.go:230] feature gates: &{map[]}
    [upgrade/apply] Respecting the --cri-socket flag that is set with higher priority than the config file.
    [upgrade/version] You have chosen to change the cluster version to "v1.11.0-beta.2.78+e0b33dbc2bde88"
    [upgrade/versions] Cluster version: v1.10.4
    [upgrade/versions] kubeadm version: v1.11.0-beta.2.78+e0b33dbc2bde88
    [upgrade/confirm] Are you sure you want to proceed with the upgrade? [y/N]: y
    [upgrade/prepull] Will prepull images for components [kube-apiserver kube-controller-manager kube-scheduler etcd]
    [upgrade/apply] Upgrading your Static Pod-hosted control plane to version "v1.11.0-beta.2.78+e0b33dbc2bde88"...
    Static pod: kube-apiserver-ip-172-31-85-18 hash: 7a329408b21bc0c44d7b3b78ff8187bf
    Static pod: kube-controller-manager-ip-172-31-85-18 hash: 24fd3157627c7567b687968967c6a5e8
    Static pod: kube-scheduler-ip-172-31-85-18 hash: 5179266fb24d4c1834814c4f69486371
    Static pod: etcd-ip-172-31-85-18 hash: 9dfc197f444be11fcc70ab1467b030b8
    [etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests089436939/etcd.yaml"
    [certificates] Using the existing etcd/ca certificate and key.
    [certificates] Using the existing etcd/server certificate and key.
    [certificates] Using the existing etcd/peer certificate and key.
    [certificates] Using the existing etcd/healthcheck-client certificate and key.
    [upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/etcd.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2018-06-14-20-56-11/etcd.yaml"
    [upgrade/staticpods] Waiting for the kubelet to restart the component
    Static pod: etcd-ip-172-31-85-18 hash: 9dfc197f444be11fcc70ab1467b030b8
    < snip >
    [apiclient] Found 1 Pods for label selector component=etcd
    [upgrade/staticpods] Component "etcd" upgraded successfully!
    [upgrade/etcd] Waiting for etcd to become available
    [util/etcd] Waiting 0s for initial delay
    [util/etcd] Attempting to see if all cluster endpoints are available 1/10
    [upgrade/staticpods] Writing new Static Pod manifests to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests089436939"
    [controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests089436939/kube-apiserver.yaml"
    [controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests089436939/kube-controller-manager.yaml"
    [controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests089436939/kube-scheduler.yaml"
    [certificates] Using the existing etcd/ca certificate and key.
    [certificates] Using the existing apiserver-etcd-client certificate and key.
    [upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-apiserver.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2018-06-14-20-56-11/kube-apiserver.yaml"
    [upgrade/staticpods] Waiting for the kubelet to restart the component
    Static pod: kube-apiserver-ip-172-31-85-18 hash: 7a329408b21bc0c44d7b3b78ff8187bf
    < snip >
    [apiclient] Found 1 Pods for label selector component=kube-apiserver
    [upgrade/staticpods] Component "kube-apiserver" upgraded successfully!
    [upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-controller-manager.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2018-06-14-20-56-11/kube-controller-manager.yaml"
    [upgrade/staticpods] Waiting for the kubelet to restart the component
    Static pod: kube-controller-manager-ip-172-31-85-18 hash: 24fd3157627c7567b687968967c6a5e8
    Static pod: kube-controller-manager-ip-172-31-85-18 hash: 63992ff14733dcb9dcfa6ac0a3b8031a
    [apiclient] Found 1 Pods for label selector component=kube-controller-manager
    [upgrade/staticpods] Component "kube-controller-manager" upgraded successfully!
    [upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-scheduler.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2018-06-14-20-56-11/kube-scheduler.yaml"
    [upgrade/staticpods] Waiting for the kubelet to restart the component
    Static pod: kube-scheduler-ip-172-31-85-18 hash: 5179266fb24d4c1834814c4f69486371
    Static pod: kube-scheduler-ip-172-31-85-18 hash: 831e4b9425f758e572392976311e56d9
    [apiclient] Found 1 Pods for label selector component=kube-scheduler
    [upgrade/staticpods] Component "kube-scheduler" upgraded successfully!
    [uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
    [kubelet] Creating a ConfigMap "kubelet-config-1.11" in namespace kube-system with the configuration for the kubelets in the cluster
    [kubelet] Downloading configuration for the kubelet from the "kubelet-config-1.11" ConfigMap in the kube-system namespace
    [kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
    [patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "ip-172-31-85-18" as an annotation
    [bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
    [bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
    [bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
    [addons] Applied essential addon: CoreDNS
    [addons] Applied essential addon: kube-proxy
    
    [upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.11.0-beta.2.78+e0b33dbc2bde88". Enjoy!
   
    [upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
    ```
<!--
1.  Manually upgrade your Software Defined Network (SDN).

    Your Container Network Interface (CNI) provider may have its own upgrade instructions to follow.
    Check the [addons](/docs/concepts/cluster-administration/addons/) page to
    find your CNI provider and see whether additional upgrade steps are required.
--->
1. 手动升级您定义的网络（SDN）软件。

您的容器网络接口（CNI）提供者可能有自己的升级说明指导。
检查这个[插件](/docs/concepts/cluster-administration/addons/)页面来找到 CNI 提供者和查看是否需要额外的升级步骤。
<!--   
## Upgrade master and node packages
--->
## 升级master和node包
<!--
1.  Prepare each host for maintenance, marking it unschedulable and evicting the workload:
    ```shell
    kubectl drain $HOST --ignore-daemonsets
    ```


    On the master host, you must add `--ignore-daemonsets`:
    
    ```shell
    kubectl drain ip-172-31-85-18
    node "ip-172-31-85-18" cordoned
    error: unable to drain node "ip-172-31-85-18", aborting command...
    
    There are pending nodes to be drained:
    ip-172-31-85-18
    error: DaemonSet-managed pods (use --ignore-daemonsets to ignore): calico-node-5798d, kube-proxy-thjp9
    ```
    
    ```
    kubectl drain ip-172-31-85-18 --ignore-daemonsets
    node "ip-172-31-85-18" already cordoned
    WARNING: Ignoring DaemonSet-managed pods: calico-node-5798d, kube-proxy-thjp9
    node "ip-172-31-85-18" drained
    ```
--->
1. 准备对于每个主机维护，标记为不可达和收回工作负载：
    ```shell
    kubectl drain $HOST --ignore-daemonsets
    ```
    在 master 主机上,添加参数 `--ignore-daemonsets`:
    ```shell
    kubectl drain ip-172-31-85-18
    node "ip-172-31-85-18" cordoned
    error: unable to drain node "ip-172-31-85-18", aborting command...

    挂起的节点将被耗近：
    ip-172-31-85-18
    error: DaemonSet-managed pods (use --ignore-daemonsets to ignore): calico-node-5798d,kube-proxy-thjp9
    ```
    ```
    kubectl drain ip-172-31-85-18 --ignore-daemonsets
    node "ip-172-31-85-18" already cordoned
    WARNING: Ignoring DaemonSet-managed pods: calico-node-5798d,kube-proxy-thjp9
    node "ip-172-31-85-18" drained
    ```
<!--    
1.  Upgrade the Kubernetes package version on each `$HOST` node by running the Linux package manager for your distribution:
--->
    通过分发运行linux包管理器，升级每个` $HOST` 节点上的 kubernetes 包版本：
    {{< tabs name="k8s_install" >}}
    {{% tab name="Ubuntu, Debian or HypriotOS" %}}
    apt-get update
    apt-get upgrade -y kubelet kubeadm
    {{% /tab %}}
    {{% tab name="CentOS, RHEL or Fedora" %}}
    yum upgrade -y kubelet kubeadm --disableexcludes=kubernetes
    {{% /tab %}}
    {{< /tabs >}}
<!--
## Upgrade kubelet on each node
--->
## 在每个 node 上升级 kubelet
<!--
1.  On each node except the master node, upgrade the kubelet config:
--->
1. 在每个 node 上除了 master node，升级 kubelet 配置：
    ```shell
    sudo kubeadm upgrade node config --kubelet-version $(kubelet --version | cut -d ' ' -f 2)
    ```
<!--
1.  Restart the kubectl process:
-->
1. 重新启动 kubectl 过程：
    ```shell
    sudo systemctl restart kubelet
    ```
<!--
1.  Verify that the new version of the `kubelet` is running on the host:
--->
1. 确认具有 `kubelet` 新的版本运行在该主机上：
    ```shell
    systemctl status kubelet
    ```
<!--
1.  Bring the host back online by marking it schedulable:
-->
1. 通过标记可计划将主机重新连接：
    ```shell
    kubectl uncordon $HOST
    ```
<!--
1.  After the kubelet is upgraded on all hosts, verify that all nodes are available again by running the following command from anywhere -- for example, from outside the cluster:
--->
1. 在所以主机升级kubelet后，通过从任意位置运行以下命令（例如从集群外部）来验证所有节点是否可用：
    ```shell
    kubectl get nodes
    ```
<!--
    The `STATUS` column should show `Ready` for all your hosts, and the version number should be updated.
--->
    在您所有的主机上 `STATUS` 列显示 `Ready` 状态和版本更新的数据。
{{% /capture %}}
<!--
## Recovering from a failure state
-->
## 从失败的状态恢复
<!--
If `kubeadm upgrade` fails and does not roll back, for example because of an unexpected shutdown during execution,
you can run `kubeadm upgrade` again. This command is idempotent and eventually makes sure that the actual state is the desired state you declare.

To recover from a bad state, you can also run `kubeadm upgrade --force` without changing the version that your cluster is running.
--->
如 `kubeadm upgrade` 失败并且不回滚，例如由于在执行过程中意外中断，您可以再次运行 `kubeadm upgrade`。此命令是幂等的，最终确保所声明的状态是所需要的状态。

为了从坏的状态中恢复过来，您可以在您的集群上不用改变版本直接用 `kubeadm upgrade --force` 来运行。
<!--
## How it works
-->
## 如何工作
<!--
`kubeadm upgrade apply` does the following:
--->
`kubeadm upgrade apply` 遵循以下步骤：
<!--
- Checks that your cluster is in an upgradeable state:
  - The API server is reachable,
  - All nodes are in the `Ready` state
  - The control plane is healthy
- Enforces the version skew policies.
- Makes sure the control plane images are available or available to pull to the machine.
- Upgrades the control plane components or rollbacks if any of them fails to come up.
- Applies the new `kube-dns` and `kube-proxy` manifests and enforces that all necessary RBAC rules are created.
- Creates new certificate and key files of the API server and backs up old files if they're about to expire in 180 days.
--->
- 检查集群是否处于可升级状态：
  -API服务器是否可达
  -所有的节点处于就绪状态
  -控制面板是健康的
- 强制执行版本倾斜策略
- 确保控制面板镜像可用或者可用于机器pull
- 升级控制面板组件或者回滚如果其中一个无法出现
- 应用新的“kube-dns”和“kube-proxy”清单并强制创建所必须的RBAC规则
-创建API服务器新的证书和秘钥文件，并备份旧文件（如果它们即将在180天到期）


