---
reviewers:
- sig-cluster-lifecycle
cn-approvers:
- chentao1596
title: 将 kubeadm 集群从 v1.10 升级到 v1.11
content_template: templates/task
---

<!--
---
reviewers:
- sig-cluster-lifecycle
title: Upgrading kubeadm clusters from v1.10 to v1.11
content_template: templates/task
---
--->

{{% capture overview %}}
<!--
This page explains how to upgrade a Kubernetes cluster created with `kubeadm` from version 1.10.x to version 1.11.x, and from version 1.11.x to 1.11.y, where `y > x`.
--->
本指南用于指导如何将 `kubeadm` 创建的集群从 1.10.x 版本升级到 1.11.x 版本，以及从 1.11.x 版本升级到 1.11.y 版本，其中 `y > x`。
{{% /capture %}}

{{% capture prerequisites %}}

<!--
- You need to have a `kubeadm` Kubernetes cluster running version 1.10.0 or later. Swap must be disabled. The cluster should use a static control plane and etcd pods.
- Make sure you read the [release notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.11.md) carefully.
- Make sure to back up any important components, such as app-level state storespecd in a database. `kubeadm upgrade` does not touch your workloads, only components internal to Kubernetes, but backups are always a best practice.
--->

- 您需要有一个正常运行中的 1.10.0 或更高版本的 `kubeadm` 集群。此外，还需要禁用节点的交换分区。该集群需要使用一个静态控制面板和 etcd pod。
- 请确保已经仔细阅读 [版本更新](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.11.md)。
- 请备份重要的组件，例如存储在数据库中应用级别的状态。`kubeadm upgrade` 只会升级 Kubernetes 内建（Kubernetes-internal）组件，不会触及任何工作负载，备份始终是最好的方案。

<!--
### Additional information
--->
### 补充信息

<!--
- All containers are restarted after upgrade, because the container spec hash value is changed.
- You can upgrade only from one minor version to the next minor version. That is, you cannot skip versions when you upgrade. For example, you can upgrade only from 1.10 to 1.11, not from 1.9 to 1.11.
- The default DNS provider in version 1.11 is [CoreDNS](https://coredns.io/) rather than [kube-dns](https://github.com/kubernetes/dns).
To keep `kube-dns`, pass `--feature-gates=CoreDNS=false` to `kubeadm upgrade apply`.
--->
- 升级之后，所有的容器要重新启动，因为容器规范的哈希值已经变更了。
- 您仅可以从一个小版本升级到下一个小版本。也就是说，当您升级的时候，您不能跨版本来进行升级。比如，您只能从 1.10 版本升级到 1.11 版本，不能直接从 1.9 版本升级到 1.11 版本。
- 在 1.11 版本中默认，的 DNS 提供者是 [CoreDNS](https://coredns.io/) 而不是 [kube-dns](https://github.com/kubernetes/dns)。如果想要保持使用 `kube-dns`，执行 `kubeadm upgrade apply` 时请带上 `--feature-gates=CoreDNS=false`。

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
1.  在您的 master 节点上，以 root 用户运行以下步骤：
                                            
        export VERSION=$(curl -sSL https://dl.k8s.io/release/stable.txt)  #或者手动的指定一个发布的 kubernetes 版本
        export ARCH=amd64 # or: arm, arm64, ppc64le, s390x
        curl -sSL https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubeadm > /usr/bin/kubeadm
        chmod a+rx /usr/bin/kubeadm
                                                                               
    请注意，在升级控制平面前，升级系统上的 `kubeadm` 包将导致升级失败。即使 `kubeadm` 已经放入 Kubernetes 仓库中，您仍应该手动安装它。Kubeadm 团队正在修复这个限制。

<!--   
1.  Verify that the download works and has the expected version:

    ```shell
    kubeadm version
    ```
-->
1.  验证下载的 kubeadm 是否工作正常，是否为预期的版本：
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
1.  在 master 节点上运行：
   ```shell
   kubeadm upgrade plan
   ```
  运行之后，可以看到类似下面的输出：
  <!-- TODO: copy-paste actual output once new version is stable -->

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

<!--
    This command checks that your cluster can be upgraded, and fetches the versions you can upgrade to.
--->
    此命令检查您的群集是否可以升级，并获取可以升级到的版本。

<!--
1.  Choose a version to upgrade to, and run the appropriate command. For example:

    ```shell
    kubeadm upgrade apply v1.11.0
    ```

    If you currently use `kube-dns` and wish to continue doing so, add `--feature-gates=CoreDNS=false`.

    You should see output similar to this:
--->
1.  选择一个版本来进行升级，并运行正确的命令。例如：
    ```shell
    kubeadm upgrade apply v1.11.0
    ```
                                        
    如果您最近使用 `kube-dns`，并想继续这样使用的话，请添加 `--feature-gates=CoreDNS=false` 参数。
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
1.  手动升级您定义的网络（SDN）软件。

	您的容器网络接口（CNI）提供程序可能有自己的升级说明。检查 [addons](/docs/concepts/cluster-administration/addons/) 页面找到 CNI 提供程序，并查看是否需要额外的升级步骤。
	
<!--   
## Upgrade master and node packages
--->
## 升级 master 和 node 包
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
1.  对于需要进行维护的所有主机，将其标记为不可调度并逐出工作负载：
    ```shell
    kubectl drain $HOST --ignore-daemonsets
    ```
	
    在 master 主机上，您必须添加参数 `--ignore-daemonsets`:
	
    ```shell
    kubectl drain ip-172-31-85-18
    node "ip-172-31-85-18" cordoned
    error: unable to drain node "ip-172-31-85-18", aborting command...

    There are pending nodes to be drained:
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
    通过运行适用于您的发行版的 Linux 软件包管理器，在每个 `$HOST` 节点上升级 Kubernetes 包版本：
	
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
1.  除了 master，在每个 node 上升级 kubelet 配置：
    ```shell
    sudo kubeadm upgrade node config --kubelet-version $(kubelet --version | cut -d ' ' -f 2)
    ```
<!--
1.  Restart the kubectl process:
-->
1.  重新启动 kubelet 进程：
    ```shell
    sudo systemctl restart kubelet
    ```
<!--
1.  Verify that the new version of the `kubelet` is running on the host:
--->
1.  验证运行在该主机上的 `kubelet` 是新版本的：
    ```shell
    systemctl status kubelet
    ```
<!--
1.  Bring the host back online by marking it schedulable:
-->
1.  通过将主机标记为可调度来使主机重新联机：
    ```shell
    kubectl uncordon $HOST
    ```
<!--
1.  After the kubelet is upgraded on all hosts, verify that all nodes are available again by running the following command from anywhere -- for example, from outside the cluster:
--->
1.  在所有主机上升级 kubelet 后，通过从任何位置运行以下命令来验证所有节点是否可用 - 例如，从集群外部：
    ```shell
    kubectl get nodes
    ```
<!--
    The `STATUS` column should show `Ready` for all your hosts, and the version number should be updated.
--->
    对于所有主机，`STATUS` 列应该显示为 `Ready`，而且版本号是更新后的值。
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
如果`kubeadm upgrade` 失败并且不能回滚，例如在执行过程中意外中断，您可以再次运行 `kubeadm upgrade`。此命令是幂等的，最终能够确保所声明的状态是所需要的状态。

要从错误状态恢复，您还可以运行 `kubeadm upgrade --force` 而不更改集群正在运行的版本。

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
  - API 服务器是否可达
  - 所有的节点处于 `Ready` 状态
  - 控制面板是健康的
- 强制执行版本倾斜策略。
- 确保控制平面可用或可拉取到机器上。
- 升级控制平面组件或回滚（如果其中任何一个组件无法启动）。
- 应用新的 `kube-dns` 和 `kube-proxy`，并强制创建所必须的 RBAC 规则。
- 创建 API 服务器新的证书和秘钥文件，并备份旧文件（如果它们将在180天后到期）。