---
reviewers:
- jamiehannaford 
- luxas
- timothysc 
- jbeda
cn-approvers:
- chentao1596
title: 将 kubeadm HA 集群从 1.9.x 版本到 1.9.y
content_template: templates/task
---

<!--
---
reviewers:
- jamiehannaford 
- luxas
- timothysc 
- jbeda
title: Upgrading kubeadm HA clusters from 1.9.x to 1.9.y
content_template: templates/task
---
--->

{{% capture overview %}}
<!--
This guide is for upgrading `kubeadm` HA clusters from version 1.9.x to 1.9.y where `y > x`. The term "`kubeadm` HA clusters" refers to clusters of more than one master node created with `kubeadm`. To set up an HA cluster for Kubernetes version 1.9.x `kubeadm` requires additional manual steps. See [Creating HA clusters with kubeadm](/docs/setup/independent/high-availability/) for instructions on how to do this. The upgrade procedure described here targets clusters created following
those very instructions. See [Upgrading/downgrading kubeadm clusters between v1.8 to v1.9](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/) for more instructions on how to create an HA cluster with `kubeadm`.
--->

本指南用于指导如何将 `kubeadm HA` 集群从 1.9.x 版本升级到 1.9.y 版本，其中 `y > x`。"`kubeadm` HA 集群" 是指使用 `kubeadm` 安装了多个主节点的集群。为了设置一个 kubernetes 1.9.x 版本的 HA 集群，`kubeadm` 需要额外的手动步骤。有关如何执行此操作的说明，请查看 [使用 kubeadm 创建 HA 集群](/docs/setup/independent/high-availability/)。此处描述升级过程以这些指导创建的集群为目标。有关如何使用 `kubeadm` 创建 HA 集群的更多说明，请查看 [在 v1.8 和 v1.9 之间升级和回滚 kubeadm 集群](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/)。

{{% /capture %}}
{{% capture prerequisites %}}
<!--
Before proceeding:

- You need to have a functional `kubeadm` HA cluster running version 1.9.0 or higher in order to use the process described here.
- Make sure you read the [release notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.9.md) carefully.
- Note that `kubeadm upgrade` will not touch any of your workloads, only Kubernetes-internal components. As a best-practice you should back up anything important to you. For example, any application-level state, such as a database and application might depend on (like MySQL or MongoDB) should be backed up beforehand.
- Read [Upgrading/downgrading kubeadm clusters between v1.8 to v1.9](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/) to learn about the relevant prerequisites.
--->

开始之前：

- 您需要有一个正常工作的 1.9.0 或更高版本的 `kubeadm` HA 集群，以进行此处描述的流程。
- 请确保已经仔细阅读 [版本更新](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.9.md)。
- 请注意，`kubeadm upgrade` 只会升级 Kubernetes 内建（Kubernetes-internal）组件，不会触及任何工作负载。作为最佳实践，您应该备份所有重要数据。例如任何应用层级的状态数据，如应用可能依赖的数据库（如 MySQL 或 MongoDB）等，在开始升级前必须对其进行备份。
- 阅读 [在 v1.8 和 v1.9 之间升级和回滚 kubeadm 集群](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/) 以了解有关的先决条件。
{{% /capture %}}

{{% capture steps %}}

<!--
## Preparation
--->
## 准备

<!--
Some preparation is needed prior to starting the upgrade. First download the version of `kubeadm` that matches the version of Kubernetes that you are upgrading to:
--->
在开始升级之前需要做一些准备工作。 首先下载与您要升级到的 Kubernetes 版本匹配的 `kubeadm` 版本：

<!--
```shell
# Use the latest stable release or manually specify a
# released Kubernetes version
export VERSION=$(curl -sSL https://dl.k8s.io/release/stable.txt) 
export ARCH=amd64 # or: arm, arm64, ppc64le, s390x
curl -sSL https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubeadm > /tmp/kubeadm
chmod a+rx /tmp/kubeadm
```
--->

```shell
# 使用最新稳定的发布版或者手动地指定一个
# 发布的 kubernetes 版本
export VERSION=$(curl -sSL https://dl.k8s.io/release/stable.txt) 
export ARCH=amd64 # or: arm, arm64, ppc64le, s390x
curl -sSL https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubeadm > /tmp/kubeadm
chmod a+rx /tmp/kubeadm
```

<!--
Copy this file to `/tmp` on your primary master if necessary. Run this command for checking prerequisites and determining the versions you will receive:
--->
如有必要，请将文件复制到主 master 节点的 `/tmp` 目录下。运行此命令以检查先决条件并确定将收到的版本：
```shell
/tmp/kubeadm upgrade plan
```
<!--
If the prerequisites are met you'll get a summary of the software versions kubeadm will upgrade to, like this:
-->
如果满足先决条件，您将获得 kubeadm 可以升级到的软件版本的摘要，如下所示：

    Upgrade to the latest stable version:
    COMPONENT            CURRENT   AVAILABLE
    API Server           v1.9.0    v1.9.2
    Controller Manager   v1.9.0    v1.9.2
    Scheduler            v1.9.0    v1.9.2
    Kube Proxy           v1.9.0    v1.9.2
    Kube DNS             1.14.5    1.14.7
    Etcd                 3.2.7     3.1.11


<!--{{< caution >}}
**Caution:** Currently the only supported configuration for kubeadm HA clusters requires the use of an externally managed etcd cluster. Upgrading etcd is not supported as a part of the upgrade. If necessary you will have to upgrade the etcd cluster according to [etcd's upgrade instructions](/docs/tasks/administer-cluster/configure-upgrade-etcd/), which is beyond the scope of these instructions.
--->
{{< caution >}}**注意：** 目前，kubeadm HA 集群唯一受支持的配置需要使用外部管理的 etcd 集群。升级 etcd 并没有作为升级过程的一个部分进行支持。如有必要，您必须根据 [etcd 升级指导](/docs/tasks/administer-cluster/configure-upgrade-etcd/) 升级etcd群集，这超出了本说明的范围。
{{< /caution >}}

<!--
## Upgrading your control plane
--->
## 升级控制面板

<!--
The following procedure must be applied on a single master node and repeated for each subsequent master node sequentially.
--->
接下来的过程必须在单个主节点上应用，并按顺序在后续每个主节点上重复。

<!--
Before initiating the upgrade with `kubeadm` `configmap/kubeadm-config` needs to be modified for the current master host. Replace any hard reference to a master host name with the current master hosts' name:
--->
在使用 `kubeadm` 启动升级时，需要修改当前 master 主机的 `configmap/kubeadm-config`。使用当前 master 主机名替换对 master 主机名的任何硬引用：
```shell
kubectl get configmap -n kube-system kubeadm-config -o yaml >/tmp/kubeadm-config-cm.yaml
sed -i 's/^\([ \t]*nodeName:\).*/\1 <CURRENT-MASTER-NAME>/' /tmp/kubeadm-config-cm.yaml
kubectl apply -f /tmp/kubeadm-config-cm.yaml --force
```
<!--
Now the upgrade process can start. Use the target version determined in the preparation step and run the following command (press “y” when prompted):
--->
现在升级过程可以开始了。使用在准备步骤中确定的目标版本并运行以下命令（在出现提示时按 “y”）：

```shell
/tmp/kubeadm upgrade apply v<YOUR-CHOSEN-VERSION-HERE>
```
<!--
If the operation was successful you’ll get a message like this:
--->
如果操作成功，您将得到以下信息：

    [upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.9.2". Enjoy!

<!--
To upgrade the cluster with CoreDNS as the default internal DNS, invoke `kubeadm upgrade apply` with the `--feature-gates=CoreDNS=true` flag.

Next, manually upgrade your CNI provider

Your Container Network Interface (CNI) provider may have its own upgrade instructions to follow. Check the [addons](/docs/concepts/cluster-administration/addons/) page to find your CNI provider and see if there are additional upgrade steps necessary.
--->
如果要使用 CoreDNS 作为默认内部 DNS 来升级集群，请调用 `kubeadm upgrade apply` 时带上 `--feature-gates=CoreDNS=true`。

接下来，手动升级您的 CNI 提供程序。

您的容器网络接口（CNI）提供程序可能有自己的升级说明。检查 [addons](/docs/concepts/cluster-administration/addons/) 页面找到 CNI 提供程序，并查看是否需要额外的升级步骤。

<!--{{< note >}}
**Note:** The `kubeadm upgrade apply` step has been known to fail when run initially on the secondary masters (timed out waiting for the restarted static pods to come up). It should succeed if retried after a minute or two.
--->
{{< note >}}**注意：** 我们已经知道，第一次在辅助 master 上运行 `kubeadm upgrade apply` 时是会失败的（等待重新启动静态 pod 超时）。如果在一两分钟后重试，它应该会成功。
{{< /note >}}

<!--
## Upgrade base software packages
--->
## 升级基本的软件包

<!--
At this point all the static pod manifests in your cluster, for example API Server, Controller Manager, Scheduler, Kube Proxy have been upgraded, however the base software, for example `kubelet`, `kubectl`, `kubeadm` installed on your nodes’ OS are still of the old version. For upgrading the base software packages we will upgrade them and restart services on all nodes one by one:
--->
此时，集群中的所有静态 pod 都已经升级，例如 API Server，控制器管理器（Controller Manager），调度器（Scheduler），Kube 代理（Kube Proxy），但是，安装在您节点的 OS 上面的基本软件，例如 `kubelet`，`kubectl`，`kubeadm` 仍是旧版本。为了升级基本软件包，我们将逐个升级它们并在所有节点上重新启动服务：

<!--
```shell
# use your distro's package manager, e.g. 'yum' on RH-based systems
# for the versions stick to kubeadm's output (see above)
yum install -y kubelet-<NEW-K8S-VERSION> kubectl-<NEW-K8S-VERSION> kubeadm-<NEW-K8S-VERSION> kubernetes-cni-<NEW-CNI-VERSION>
systemctl restart kubelet
```
--->

```shell
# 使用您的发行版本的包管理器，例如基于 RH 的系统上的 'yum'，获取跟 kubeadm 输出的版本号一致的版本（见上文）
yum install -y kubelet-<NEW-K8S-VERSION> kubectl-<NEW-K8S-VERSION> kubeadm-<NEW-K8S-VERSION> kubernetes-cni-<NEW-CNI-VERSION>
systemctl restart kubelet
```

<!--
In this example an _rpm_-based system is assumed and `yum` is used for installing the upgraded software. On _deb_-based systems it will be `apt-get update` and then `apt-get install <PACKAGE>=<NEW-K8S-VERSION>` for all packages.

Now the new version of the `kubelet` should be running on the host. Verify this using the following command on the respective host:
--->
在本例中，是假设一个基于 _rpm_ 的系统使用 `yum` 来安装升级的软件。在基于 _deb 的系统上，则可以通过使用 `apt-get update` 和 `apt-get install <PACKAGE>=<NEW-K8S-VERSION>` 来安装所有的软件包。

现在新版本的 `kubelet` 将运行在主机上。在相应主机上使用以下命令验证这一点：

```shell
systemctl status kubelet
```

<!--
Verify that the upgraded node is available again by executing the following from wherever you run `kubectl` commands:
--->
在任何位置运行 `kubectl` 命令再次验证升级后的节点是可用的：
```shell
kubectl get nodes
```
<!--
If the `STATUS` column of the above command shows `Ready` for the upgraded host, you can continue (you may have to repeat this for a couple of time before the node gets `Ready`).
--->
如果上面命令的 `STATUS` 列显示所有升级的主机是 `Ready` 状态，您可以继续（节点处于 `Ready` 状态之前时您可能要多尝试几次）。

<!--
## If something goes wrong

If the upgrade fails the situation afterwards depends on the phase in which things went wrong:
--->
## 如果出错

如果升级失败，之后的情况取决于出现问题的阶段：
<!--
1. If `/tmp/kubeadm upgrade apply` failed to upgrade the cluster it will try to perform a rollback. Hence if that happened on the first master, chances are pretty good that the cluster is still intact.

   You can run `/tmp/kubeadm upgrade apply` again as it is idempotent and should eventually make sure the actual state is the desired state you are declaring. You can use `/tmp/kubeadm upgrade apply` to change a running cluster with `x.x.x -> x.x.x` with `--force`, which can be used to recover from a bad state.
--->
1. 如果是执行 `/tmp/kubeadm upgrade apply` 的时候集群升级失败，它将尝试执行回滚。因此，如果是第一个 master 发生这种情况，集群仍然完好无损的可能性很大。

   您可以再次运行 `/tmp/kubeadm upgrade apply` 命令，因为它是幂等的，并且最终确保实际状态是声明的期望得到的状态。您可以使用参数 `x.x.x -> x.x.x` 时带上 `--force` 来运行 `/tmp/kubeadm upgrade apply` 命令，它通常用于从坏状态进行恢复。
<!--
2. If `/tmp/kubeadm upgrade apply` on one of the secondary masters failed you still have a working, upgraded cluster, but with the secondary masters in a somewhat undefined condition. You will have to find out what went wrong and join the secondaries manually. As mentioned above, sometimes upgrading one of the secondary masters fails waiting for the restarted static pods first, but succeeds when the operation is simply repeated after a little pause of one or two minutes. 
--->

2. 如果某个辅助主机上运行 `/tmp/kubeadm upgrade apply` 失败，您升级的集群仍然是可以正常工作的，但辅助主服务器处于稍微不确定的状态。您必须找出出错的地方并手动加入辅助设备。 如上所述，有时升级其中一个辅助主机在第一次等待重新启动的静态容器时可能出现超时，但是在稍微暂停一两分钟后简单地重复操作时会成功。

{{% /capture %}}