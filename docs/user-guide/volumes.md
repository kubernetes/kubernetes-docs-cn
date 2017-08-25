---
---

在容器中的磁盘文件是非持久的，当运行在容器中时这对于一些重要的应用会造成一些问题。首先，当容器崩溃的时候，kubelet会重启容器，但是文件丢失了 - 容器会重新以一个白板的状态开始。其次，当在`Pod`中一起运行容器的时候，通常需要在这些容器之间分享文件。Kubernetes的`Volume`的抽象同时解决了这些问题。

建议在读这一部分时候，已经对[pods](/docs/user-guide/pods)有一定的熟悉度。

* TOC
{:toc}


## 背景


在Docker中也有[volumes](https://docs.docker.com/userguide/dockervolumes/)的概念，然而它更松散一些，管理更少一点。在Docker中，volume只是简单的一个磁盘上或者另外一个容器中的目录。生命周期没有管理，而且直到最近只有本地磁盘类型的volume。Docker现在提供了volume的驱动，但是暂时这个功能还比较有限（比如，对于Docker 1.7，一个容器只能指定一个volume驱动，而且没有什么办法可以向volume传递参数）。


而Kubernetes中的Volumne，有着明确的生命周期 - 正如环绕着它的pod一样。结果，volumne的可以活的比Pod中的容器更加长久，即使容器反复重启数据仍然存在。当然，当一个Pod消失的时候，volumne也会随之消失。可能更加重要的一点是，Kubernetes支持很多类型的Volume，并且Pod可以同时使用任意的数量。

从本质上说，一个Volume只是一个目录，里面可能有一些数据，这些数据对于Pod中的容器可以访问。至于该目录是如何存在的，后面有什么媒介支撑着它，里面有什么内容是决定于使用的volume类型。

要使用一个volume，pod需要指定要给该pod提供什么volume（[`spec.volumes`](http://kubernetes.io/third_party/swagger-ui/#!/v1/createPod)字段）和从该挂载的位置（[`spec.containers.volumeMounts`](http://kubernetes.io/third_party/swagger-ui/#!/v1/createPod)字段）。

容器中的进程看到的文件系统视图包含Docker镜像和Volume。[Docker镜像](https://docs.docker.com/userguide/dockerimages/) 位于文件系统层级的根部，任何volume都是挂载在该镜像中的指定路径。Volume不能挂载到其他的Volume上，或者有到其他Volume的硬连接。每一个Pod中的容器必须独立地指定该在什么地方挂载每一个volume。

## Volume的类型

Kubernetes支持很多类型的Volume：

   * `emptyDir`
   * `hostPath`
   * `gcePersistentDisk`
   * `awsElasticBlockStore`
   * `nfs`
   * `iscsi`
   * `flocker`
   * `glusterfs`
   * `rbd`
   * `gitRepo`
   * `secret`
   * `persistentVolumeClaim`

我们欢迎贡献其他类型的Volume。

### emptyDir


一个`emptyDir`的volume首先在一个Pod指派给一个Node时候被创建，只要该Pod运行在该node上就会一直存在。正如名字说的那样，开始的时候她是空的。pod中的容器都可以对`emptyDir`中相同的文件进行读或者写，尽管这个volume可以在每一个容器中挂载在相同或者不同的路径下面。当一个Pod从node中因为任意原因被删除的时候，`emptyDir`中的数据就会被永久删除。注意：容器崩溃不会把pod从node中删除掉，所有在`emptyDir`Volume中的数据在容器崩溃的情况下是安全的。

一些`emptyDir`的用途有：

* 暂存空间（scratch space），例如基于磁盘的归并排序
* 用于实现一个长耗时计算的检查点，方便在崩溃后恢复
* 用于保存一个内容管理器容器要获取的文件，而另外一个web服务器容器用来对外服务这些文件

默认情况下，`emptyDir` volume可以保存在任意支撑机器的媒介上 - 可以是磁盘或者是SSD或者是网络存储，这取决于你的环境。然而，你可以把`emptyDir.medium`字段设置为`"Memory"`来告诉Kubernetes为你挂载一个tmpfs（RAM文件系统）。尽管tmpfs非常快，要留意与磁盘不同，tmpfs在机器重启的时候会清空数据，你写的任意文件都要算在你的容器的内存限制内。


### hostPath

一个`hostPath`的volume会挂载来自host node的文件系统中的一个文件或者目录。这不是一个绝大多数Pod需要的功能，但是它为一些应用提供了一个强大的应急出口。

例如，`hostPath`可以用来：
* 运行一个需要访问Docker内部的的容器；使用`/var/lib/docker`作为`hostPath`
* 在容器中运行cAdvisor；使用`/dev/cgroups`作为`hostPath`

要当心这种类型的volume,因为：

* 在不同的node上的pod虽然有一样配置(如一些从podTemplate创建的Pod)，可能会因为nodes上不同的文件而表现出不同的行为
* 当Kubernetes添加把资源考虑在内（resource-aware）的调度的时候，正如计划的一样，它可能不会考虑被`hostPath`使用的资源

### gcePersistentDisk

`gcePersistentDisk`类型的volume会挂载一个把一个Google Compute Engine (GCE)的[Persistent Disk（持久化磁盘）](http://cloud.google.com/compute/docs/disks)挂载进你的pod里面。不同于会在Pod被删除的时候会被抹去数据的`emptyDir`，PD的内容会被被保留，并且该volume仅仅是被卸载了而已。这意味着一个PD可以预先填充数据，而且这些数据可以在pod间过手。

__重要提示：你必须用`gcloud`或者GCE API或者UI创建一个PD先，然后才可以使用它__

在使用`gcePersistentDisk`的时候有一些限制：

* pod运行所在的node必须是GCE VM
* 这些VM需要跟PD位于相同的GCE项目和zone

一个PD的特性是他们可以被不同的用户（消费者）以只读的方式挂载。这意味着，你可以用你的数据预先填充PD，然后根据你的需要从任意多的pod中提供这些文件。糟糕的是，PD只能同时被一个消费者以读写的模式挂载 - 不支持同时多个reader。

在一个由ReplicationController控制的pod上使用PD将会失败，除非这个PD是只读的或者副本的数量是0或者1。

#### 创建一个PD

要在Pod中使用一个GCE PD，需要首先创建它：

```shell
gcloud compute disks create --size=500GB --zone=us-central1-a my-data-disk
```

#### Example pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: gcr.io/google_containers/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    # This GCE PD must already exist.
    gcePersistentDisk:
      pdName: my-data-disk
      fsType: ext4
```

### awsElasticBlockStore

一个`awsElasticBlockStore`的volume会挂载一个Amazon Web Services (AWS)[EBSVolume](http://aws.amazon.com/ebs/)到你的pod中。不同于会在Pod被删除的时候会被抹去数据的`emptyDir`，PD的内容会被被保留，并且该volume仅仅是被卸载了而已。这意味着一个PD可以预先填充数据，而且这些数据可以在pod间过手。


__重要：你必须用`aws ec2 create-volume`或者AWS API先创建一个EBS volume，然后才可以使用它__

在使用`awsElasticBlockStore`的时候有一些限制：

* pod运行所在的node必须是一个AWS EC2的实例
* 这些实例必须与该EBS volume处于同一个region和availability-zone
* EBS支持一个EC2实例挂载一个volume


#### Creating an EBS volume

在pod中使用EBS卷之前，你需要创建它：

```shell
aws ec2 create-volume --availability-zone eu-west-1a --size 10 --volume-type gp2
```

确保该zone和你要操作的集群所在zone匹配。（同时也要检查size和EBS volume的类型符合你的用途）。

#### AWS EBS的示例配置

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-ebs
spec:
  containers:
  - image: gcr.io/google_containers/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-ebs
      name: test-volume
  volumes:
  - name: test-volume
    # This AWS EBS volume must already exist.
    awsElasticBlockStore:
      volumeID: aws://<availability-zone>/<volume-id>
      fsType: ext4
```

(提示: volumeID的语法目前很怪异; #10181修复了这个问题)

### nfs

`nfs`的volume允许一个已经存在的NFS share挂载进你的POD里面。不同于会在Pod被删除的时候会被抹去数据的`emptyDir`，PD的内容会被被保留，并且该volume仅仅是被卸载了而已。这意味着一个PD可以预先填充数据，而且这些数据可以在pod间过手。NFS可以被不同的writer同时挂载。

__重要: 你必须保证你的NFS服务器正常运行，并且share已经导出，然后才能使用__

更多细节请查看[NFS example](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/nfs/) 。

### iscsi

一个`iscsi` volume能让你把一个现有的iSCSI (SCSI over IP)volume挂载进你的Pod里面。不同于会在Pod被删除的时候会被抹去数据的`emptyDir`，PD的内容会被被保留，并且该volume仅仅是被卸载了而已。这意味着一个PD可以预先填充数据，而且这些数据可以在pod间过手。

__重要: 在使用前请确保你的iSCSI服务器正常运行，并且volume已经创建好__

一个iSCSI的特性是他们可以被不同的用户（消费者）以只读的方式挂载。这意味着，你可以用你的数据预先填充PD，然后根据你的需要从任意多的pod中提供这些文件。糟糕的是，iSCSI类型的Volume只能同时被一个消费者以读写的模式挂载 - 不支持同时多个reader。

更多细节参见[iSCSI示例](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/iscsi/).

### flocker


[Flocker](https://clusterhq.com/flocker) 是一个开源的集群容器数据卷管理器。它提供了数据卷的管理和编排功能，支持很多类型的后端存储。

`flocker`的卷能让一个Flocker的dataset（数据集）挂载到pod中。如果该dataset尚未在Flocker中存在，需要先使用Flocker CLI或者使用Flocker API创建。如果dataset已经存在，它会重新被Flocker附加到pod被调度到的node上。这意味着数据可以根据需要在pod之间传递。

__重要: 使用前请确保安装的Flocker正常运行__


更多内容请查看[Flocker示例](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/flocker/).

### glusterfs

一个`glusterfs` volume能让你把一个[Glusterfs](http://www.gluster.org)（一个开源的网络文件系统）挂载进你的Pod里面。不同于会在Pod被删除的时候会被抹去数据的`emptyDir`，`glusterfs` volume 的内容会被被保留，并且该volume仅仅是被卸载了而已。这意味着一个glusterfs volume可以预先填充数据，而且这些数据可以在pod间过手。GlusterFS被多个写入者同时挂载。

__重要: 使用前请确保你自己的GlusterFS正常运行__

更多内容请查看[GlusterFS示例](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/glusterfs/) 。

### rbd

一个`rbd`类型的volume能让你把一个[Rados Block Device](http://ceph.com/docs/master/rbd/rbd/)的卷挂载进你的Pod里面。不同于会在Pod被删除的时候数据会被抹去的`emptyDir`，`rbd`卷的内容会被被保留，并且该volume仅仅是被卸载了而已。这意味着一个glusterfs volume可以预先填充数据，而且这些数据可以在pod间传递。

__重要: 使用RBD前请确保你的Ceph正常运行__

一个RBD的特性是他们可以被不同的使用者以只读的方式挂载。这意味着，你可以用你的数据预先填充它，然后根据你的需要从任意多的pod中提供这些文件。但是，iSCSI类型的Volume只能同时被一个使用者以读-写的模式挂载 - 不支持同时多个写入者。

更多细节请查看[RBD示例](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/rbd/) 。

### gitRepo

一个`gitRepo`卷是一个很好例子来说明一个volume插件可以做些什么。它会挂载一个空的目录，然后把一个git仓库克隆进去，方便你的pod使用。在以后，这样的volume可能会被移到一个更加低耦合的模型中，而不是对于每一个这样的使用场景，都要扩展一下Kubernetes API。

这里是一个gitRepo卷的例子：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: server
spec:
  containers:
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /mypath
      name: git-volume
  volumes:
  - name: git-volume
    gitRepo:
      repository: "git@somewhere:me/my-git-repository.git"
      revision: "22f1d8406d464b0c0874075539c1f2e96c253775"
```

### secret

一个`secret`类型的卷可以用来传递敏感信息，比如密码，给pod。你可以在Kubernetes API中保存一些秘密的信息，然后把它们作为文件挂载而不需要要与Kubernetes直接产生耦合。`secret`卷后边是tmpfs（一个内存文件系统）因此它们永远不会被写入到非易失的存储中。

__重要: 在使用之前请确保你已经通过Kubernetes API创建了一个sercret__

关于Secret的更多细节请参见[这里](/docs/user-guide/secrets).

### persistentVolumeClaim

`persistentVolumeClaim`的卷是用来挂载一个[PersistentVolume](/docs/user-guide/persistent-volumes)到pod里面。PersistentVolumes可以用户声明持久化的存储（例如一个GCE PersistentDisk或者iSCSI卷），而不需要知道具体特定云环境的细节。

### downwardAPI

`downwardAPI`卷是用来让downward API的数据可以被应用使用。它挂载一个目录然后把请求的数据写入明文的文本文件中。、

更多细节请参考[`downwardAPI` volume 实例](/docs/user-guide/downward-api/volume/)。

## Resources

一个`emptyDir`卷的存储媒介（磁盘，SSD等）是由存放kubelet根目录的文件系统的媒介决定的（通常是`/var/lib/kubelet`）。`emptyDir`或者`hostPath`卷能消耗多少空间是没有限制的，并且在容器或者pod之间也没有隔离。

在以后，我们希望`emptyDir`和`hostPath`的卷可以使用[resource](/docs/user-guide/compute-resources)的规范请求定量的空间，并且对于那些有多种媒介类型的集群来说，可以选择使用哪种媒介。
