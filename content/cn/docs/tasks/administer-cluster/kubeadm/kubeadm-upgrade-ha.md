---
reviewers:
- jamiehannaford 
- luxas
- timothysc 
- jbeda
title: 升级kubeadm HA集群从 1.9.x 版本到 1.9.y 版本
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

本文主要讲升级 `kubeadm` HA 集群从1.9.x 版本到1.9.y 版本（ `y > x` ）。"`kubeadm` HA clusters" 术语是指安装了多个 `kubeadm` 主节点的集群。要为 kubernetes 1.9.x 版本 `kubeadm` 设置 HA 集群需要额外的手动操作。查看[安装 kubeadm HA 集群](/docs/setup/independent/high-availability/)其指导该怎么安装。此处描述升级过程以这些指导创建集群为目标。查看[升级/回退 kubeadm 集群 在 v1.8 版本 和 v1.9 版本之间](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/)更好的指导如何安装具有 `kubeadm`
的 HA 集群。

{{% /capture %}}
{{% capture prerequisites %}}
<!--

Before proceeding:

- You need to have a functional `kubeadm` HA cluster running version 1.9.0 or higher in order to use the process described here.
- Make sure you read the [release notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.9.md) carefully.
- Note that `kubeadm upgrade` will not touch any of your workloads, only Kubernetes-internal components. As a best-practice you should back up anything important to you. For example, any application-level state, such as a database and application might depend on (like MySQL or MongoDB) should be backed up beforehand.
- Read [Upgrading/downgrading kubeadm clusters between v1.8 to v1.9](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/) to learn about the relevant prerequisites.
--->

在升级之前的操作：

- 为了按照本文的操作文档，您需要安装一个运行 1.9.0 版本或者更高的版本，该版本具有 `kubeadm` HA 集群功能。
- 认真阅读[发布说明](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.9.md)。
- 请注意，`kubeadm upgrade` 不会涉及任何的工作负载，只有 kubernetes 内部组件。作为一个最佳的实践，您应当知道备份是相当重要的。
例如，任何应用程序级别的状态（如应用程序可能依赖的数据库：mysql 或 mongoDB）必须预先备份。
- 阅读[升级/回退 kubeadm 集群 在 v1.8 版本 和 v1.9 版本之间](/docs/tasks/administer-cluster/kubeadm-upgrade-1-9/)进行了解有关的先决条件。
{{% /capture %}}

{{% capture steps %}}

<!--

## Preparation

--->

##准备
<!--

Some preparation is needed prior to starting the upgrade. First download the version of `kubeadm` that matches the version of Kubernetes that you are upgrading to:

--->

在您开始升级之前，一些准备是必要的。首先下载 `kubeadm` 版本，该版本匹配您将升级到的 kubernetes 版本。

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
#发布的 kubernetes 版本
export VERSION=$(curl -sSL https://dl.k8s.io/release/stable.txt) 
export ARCH=amd64 # or: arm, arm64, ppc64le, s390x
curl -sSL https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubeadm > /tmp/kubeadm
chmod a+rx /tmp/kubeadm
```

<!--

Copy this file to `/tmp` on your primary master if necessary. Run this command for checking prerequisites and determining the versions you will receive:

--->
如有必要在主要的 master 节点上 拷贝文件到 `/tmp` 目录下。运行此命令以检查先决条件是否满足并决定你将升级哪个版本。
```shell
/tmp/kubeadm upgrade plan
```
<!--
If the prerequisites are met you'll get a summary of the software versions kubeadm will upgrade to, like this:

    Upgrade to the latest stable version:
--->

如果满足了先决条件，您将会获得 kubeadm 将升级到的那个软件版本的摘要信息，如下所述：
    升级到最新稳定的版本：
    COMPONENT            CURRENT   AVAILABLE
    API Server           v1.9.0    v1.9.2
    Controller Manager   v1.9.0    v1.9.2
    Scheduler            v1.9.0    v1.9.2
    Kube Proxy           v1.9.0    v1.9.2
    Kube DNS             1.14.5    1.14.7
    Etcd                 3.2.7     3.1.11


{{< caution >}}

<!--

**Caution:** Currently the only supported configuration for kubeadm HA clusters requires the use of an externally managed etcd cluster. Upgrading etcd is not supported as a part of the upgrade. If necessary you will have to upgrade the etcd cluster according to [etcd's upgrade instructions](/docs/tasks/administer-cluster/configure-upgrade-etcd/), which is beyond the scope of these instructions.

--->

**注意:** 目前，kubeadm HA 集群仅仅支持的配置要求使用外载管理的 etcd 集群。升级 etcd 不是升级的一部分。如果需要升级 etcd，您需要参考[etcd 升级指导](/docs/tasks/administer-cluster/configure-upgrade-etcd/)，该指导不在升级 kubeadm HA 集群等这些指导的范围。

{{< /caution >}}

<!--
## Upgrading your control plane
--->
## 升级控制面板

<!--
The following procedure must be applied on a single master node and repeated for each subsequent master node sequentially.
--->

下面的程序在一个单独的主节点上和为每个后续的重复的主节点上应用。

<!--

Before initiating the upgrade with `kubeadm` `configmap/kubeadm-config` needs to be modified for the current master host. Replace any hard reference to a master host name with the current master hosts' name:

--->

在启动升级时，需要为当前 master 主机修改 `kubeadm` `configmap/kubeadm-config`。用当前 master 主机名替换引用到 master 主机名的硬件：
```shell
kubectl get configmap -n kube-system kubeadm-config -o yaml >/tmp/kubeadm-config-cm.yaml
sed -i 's/^\([ \t]*nodeName:\).*/\1 <CURRENT-MASTER-NAME>/' /tmp/kubeadm-config-cm.yaml
kubectl apply -f /tmp/kubeadm-config-cm.yaml --force
```
<!--

Now the upgrade process can start. Use the target version determined in the preparation step and run the following command (press “y” when prompted):

--->

现在升级过程开始。使用事先准备好步骤的确定版本和运行以下命令（当出现提示时按 “y”）

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
为了升级具有 coreDNS 集群作为默认内部 DNS，调用具有 `--feature-gates=CoreDNS=true` 标记的 `kubeadm upgrade apply` 命令。

接下来，手动升级您的 CNI 提供者

您的容器网络接口(CNI)提供者应该有自己的更新指导来进行。检查这个[插件](/docs/concepts/cluster-administration/addons/)页面来找到 CNI 提供者和查看是否需要额外的升级步骤。
{{< note >}}
<!--
**Note:** The `kubeadm upgrade apply` step has been known to fail when run initially on the secondary masters (timed out waiting for the restarted static pods to come up). It should succeed if retried after a minute or two.
--->
**注意:** 在最初辅助的主节点上运行时，`kubeadm upgrade apply` 步骤已知失败（如：等待重新启动的静态 pods 出现超时）。如果在一到两分钟后重试，它将成功。
{{< /note >}}
<!--
## Upgrade base software packages
--->
## 升级基本的软件包

<!--

At this point all the static pod manifests in your cluster, for example API Server, Controller Manager, Scheduler, Kube Proxy have been upgraded, however the base software, for example `kubelet`, `kubectl`, `kubeadm` installed on your nodes’ OS are still of the old version. For upgrading the base software packages we will upgrade them and restart services on all nodes one by one:

--->
此时，集群中的所有静态 pod 都将显示出来，例如 API服务器，控制器管理器，调度器，kube代理，这些都已经完成升级，但是基本软件安装在您的 nodes 节点的 OS的 `kubelet`, `kubectl`, `kubeadm` 仍是旧的版本。为了升级基本软件包，我们将升级它们，并将所有节点上的服务一个个重新启动：

<!--

```shell
# use your distro's package manager, e.g. 'yum' on RH-based systems
# for the versions stick to kubeadm's output (see above)
yum install -y kubelet-<NEW-K8S-VERSION> kubectl-<NEW-K8S-VERSION> kubeadm-<NEW-K8S-VERSION> kubernetes-cni-<NEW-CNI-VERSION>
systemctl restart kubelet
```
--->

```shell
# 使用您的发行版本的包管理器，例如，在 RH-based 系统上的 'yum'
#  为了坚持 kubeadm 的输出版本(参考上文)
yum install -y kubelet-<NEW-K8S-VERSION> kubectl-<NEW-K8S-VERSION> kubeadm-<NEW-K8S-VERSION> kubernetes-cni-<NEW-CNI-VERSION>
systemctl restart kubelet
```

<!--
In this example an _rpm_-based system is assumed and `yum` is used for installing the upgraded software. On _deb_-based systems it will be `apt-get update` and then `apt-get install <PACKAGE>=<NEW-K8S-VERSION>` for all packages.

Now the new version of the `kubelet` should be running on the host. Verify this using the following command on the respective host:
--->
在该例子中假设一个 _rpm_-based 系统和使用 `yum` 来安装升级的软件。在 _deb_-based 系统上，通过使用 `apt-get update` 然后 `apt-get install <PACKAGE>=<NEW-K8S-VERSION>` 来安装所有的软件包。

现在新的 `kubelet` 的版本将运行在主机上。确认在各个主机上运行下面的命令：

```shell
systemctl status kubelet
```

<!--
Verify that the upgraded node is available again by executing the following from wherever you run `kubectl` commands:
--->

再次确定升级的节点通过在主机任何地方可以执行 `kubectl` 命令：
```shell
kubectl get nodes
```
<!--
If the `STATUS` column of the above command shows `Ready` for the upgraded host, you can continue (you may have to repeat this for a couple of time before the node gets `Ready`).
--->
如果上面命令的 `STATUS` 列显示所有升级的主机 `Ready` 状态，您可以继续（必须在节点处于 `Ready` 状态之前时您可能要重复几个时间段）。
<!--
## If something goes wrong

If the upgrade fails the situation afterwards depends on the phase in which things went wrong:
--->
## 如果出错

如果升级失败，之后需要确定：故障发生在哪个阶段：
<!----
1. If `/tmp/kubeadm upgrade apply` failed to upgrade the cluster it will try to perform a rollback. Hence if that happened on the first master, chances are pretty good that the cluster is still intact.

   You can run `/tmp/kubeadm upgrade apply` again as it is idempotent and should eventually make sure the actual state is the desired state you are declaring. You can use `/tmp/kubeadm upgrade apply` to change a running cluster with `x.x.x --> x.x.x` with `--force`, which can be used to recover from a bad state.
>
1. 如果 `/tmp/kubeadm upgrade apply` 没有升级集群，它将尝试执行回滚。因此，如果第一个master发生这种情况，集群仍然完好无损的可能性很大。

   您可以再次运行 `/tmp/kubeadm upgrade apply` 命令，因为它是幂等的，并且最终确保实际状态是声明的期望得到的状态。您可以使用 `/tmp/kubeadm upgrade apply` 命令来更改运行的使用 `--force` 参数的 `x.x.x --> x.x.x` 集群，它是用来恢复坏的状态的。
<!--
2. If `/tmp/kubeadm upgrade apply` on one of the secondary masters failed you still have a working, upgraded cluster, but with the secondary masters in a somewhat undefined condition. You will have to find out what went wrong and join the secondaries manually. As mentioned above, sometimes upgrading one of the secondary masters fails waiting for the restarted static pods first, but succeeds when the operation is simply repeated after a little pause of one or two minutes. 
--->

2.如果某个辅助的主机上使用 `/tmp/kubeadm upgrade apply` 命令运行失败，您仍然有一个可以工作的，升级的集群，除了在处于某种没有定义情况下的辅助的主节点没有作用。您必须找出是什么问题，并手动加入辅助机器。综上所述，有时升级其中一个辅助的主节点机器失败，原因是首先一直等待重新启动所有的静态 pods，但是在等待一到两分钟停顿片刻后重复操作却成功了。
{{% /capture %}}


