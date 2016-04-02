---
---

* TOC
{:toc}

## 什么是 _replication controller_？


_replication controller_ 能保证指定数量pod副本同时运行。如果运行的副本数量太多，它会杀掉一些。如果数量太少，它会启动一些。与用户直接创建pod的场景不同，_replication controller_ 会替换掉那些不管因为什么原因而被删除或者终止的pod，例如node故障或者突然性的node维护，如升级内核。因此，我们推荐使用 _replication controller_ ，即使你的应用只需要一个pod。可以把它想象成进程的管理者，只是它管理的是跨node的多个pod，而不单一node上的某单一进程。_replication controller_ 会将本地的容器重启任务指派给node上的agent（如Kubelet或者Docker）。

在[pod的声明周期中](/docs/user-guide/pod-states)，`ReplicationController`仅仅使用于那些有`RestartPolicy = Always`条件的pod。（注：如`RestartPolicy`的值没有设置，默认值是`Always`。）`ReplicationController`应该拒绝实例化有不同重启策略的pod，我们期待有在未来其他类型的控制器添加到Kubernetes中，来处理其它类型的workload，如build/test和batch类型的workload。

_replication controller_ 永远不会自我终结，但是不要期待它如service一样寿命很长。Service可以由Pod组成，这些pod可以由不同的 _replication controller_ 来控制，并且在service的生命周期期间可能有很多的 _replication controller_ 被创建和被销毁（如来升级运行这些service的pod）。service它们自身和它们的客户端都对于维持这些服务的pod的 _replication controller_ 都是不知情的。


## _replication controller_ 的工作原理？

### Pod 模板


_replication controller_ 从模板中生成pod，目前是仅仅是`ReplicationController`对象的一个字段，但是我们计划将其抽象它自己的resource[#170](http://issue.k8s.io/170)。

与指定所有的副本的预想状态不同，pod模板就像一种饼干成型刀（cookie cutter）。一旦饼干成型，饼干自身与成型刀之间没有关联。这里没有量子纠缠态。对模板接下来的改动甚至切换到新的模板不会对已经创建的pod有直接影响。类似的，由一个　_replication controller_　创建的pod可以被后续直接更新。这与pod有明显的不同，pod会指定所有属于pod的人哦拿起当前预想的状态。这个方法从根本上简化了系统的语义并且提供了原语的灵活性，正如下面的用户示例展示的一样。

由 _repllication controller_　创建的pod是可以被替换的且语义相同，尽管它们的配置随着时间的推移可能会变得异构。这对于复制的无状态的服务器来说显然很合适，但是 _repllication controller_ 也可用来获取应用的高可用性，不论它们是选主型（masater-elected），分片型（sharded），worker池（worker-pool）或者其他什么类型的应用。这些应用应该使用动态的work分配机制，例如[etcd lock module](https://coreos.com/docs/distributed-configuration/etcd-modules/) or [RabbitMQ work queues](https://www.rabbitmq.com/tutorials/tutorial-two-python), 而不是静止的，一次性的为每一个pod进行差异化的配置，这被认为是一种反模式。任何的pod的自定义化操作，例如资源的竖向自动伸缩（例如：CPU或者内存），应该由另一个在线的控制器进程完成，而不是由 _repllication controller_ 自己来完成。

### Labels

一个 _repllication controller_ 监控的pod是使用[label selector](/docs/user-guide/labels/#label-selectors)来完成的，这会在控制器和被控制的pod之间建立一种松耦合的关系，这与pod和它们的定义之间的紧密耦合关系有很大的差异。我们故意选择不用一个定长的pod定义组成的数组来代表一组受控的pod，因为根据我们的经验这种做法会增加管理运维的成本，不论客户端或者系统都这样。

 _repllication controller_ 应该验证从指定的模板创建出来的pod中的标签是否与标签选择器匹配。尽管尚未被验证，你也应该确保只有一个 _repllication controller_ 控制着某一个pod，通过确保 _repllication controller_ 的标签的目标pod集合不会有交集。如果确实你遇到了多个控制器有重叠的选择器的话，你应该通过--cascade=false来进行删除，直到没有没有任何一个控制器有一个重叠选择器的超集。

注意 _repllication controller_ 自身通常有一些标签会会和它们的pod一样的标签，但是这些标签不会影响 _repllication controller_ 的行为。

可以通过改变pod的标签来将一个pod从 _repllication controller_ 的目标集合中删除。这个技术可以被用来从服务中删除pod，从而达到调试，数据恢复等等目的。通过这个方式移除的pod会被自动替换（假设副本额你的数量没有被改变）。

类似的，通过API删除一个 _repllication controller_ 不会影响该控制器创建的pod。它的`replica`字段必须首先设置为`0`，然后才能删除受控制的pod。（注意`kubectl`这个客户端工具提供了一个单一的操作，[delete](/docs/user-guide/kubectl/kubectl_delete)，可以同时删除 _repllication controller_ 和它控制的pod。如果你想让pod在 _repllication controller_ 被删除的时候让pod继续保持运行，可以指定`--cascade=false`。但是，目前没有相应的API操作。）

##  _repllication controller_ 的责任

 _repllication controller_ 仅仅保证预想的pod数量和它的标签选择器选择的pod数量匹配，且都处理运行的状态。目前，只有终止的pod是从这个数量中排除的。在以后， [readiness](http://issue.k8s.io/620)和其他的系统中可用的信息也可能被纳入考虑范围，我们可能在替代策略中添加更多的控制，并且我们计划生成事以让外部的客户端可以实现任意复杂的替代和/或向下伸缩的策略。

 _repllication controller_ 永远都会受限于它狭窄的责任范围。它自身不会进行就绪状态或者存活状态的探侦。与执行自动伸缩不同，它意在被一个外部的自动伸缩器来控制（如[#492](http://issue.k8s.io/492)中讨论的），并且`replicas`字段也会因此修改。我们不会向 _repllication controller_ 添加调度的策略（如：[spreading](http://issue.k8s.io/367#issuecomment-48428019)）。同时 _repllication controller_ 不会验证当前控制的pod是否与目前指定的模板匹配，因为这会阻碍容量自动调整和其他自动化的进程。类似的，完成的最后期限，依赖的排序，配置的自动替换，和其他的特性都不属于这里。我们甚至计划将批量的pod创建的特性给优化出去([#170](http://issue.k8s.io/170))。

 _repllication controller_ 的目的是作为一种可以组合的，构建块式的原语。我们期望有更高层次的API或者工具可以在它和其他原语的基础上构建出来。kubectl目前支持的一些微小的操作(如：run，stop，scale，rolling-update)就是这种理念的例证。例如，我们可以想象有一些如[Spinnaker](http://spinnaker.io/) 一样的东西可以管理 _repllication controller_ ，自动伸缩器，服务，调度策略，金丝雀等等。

## 常见的使用模式

###  重新调度

正如上边提到的，不管想要运行的pod是一个还是1000个， _repllication controller_ 都会保证指定数量的pod存在，不管是在node出现故障或者pod中止的时候（如：因为一个其他的控制代理而中止）。

### 伸缩

 _repllication controller_ 让把副本的数量往上或者往下伸缩变得容易，不管是通过手动控制，或者是通过一个自动的伸缩空置代理，只需要调整`replicas`字段。

### 滚动更新

 _repllication controller_ 被设计来通过一个个的替换pod来推动服务的自动更新.

正如在[#1353](http://issue.k8s.io/1353)中解释的一样，推荐的方式是创建一个新的 _repllication controller_ 把副本数量设置为1,然后将新的控制器逐步加一，老的控制器逐步减一，然后在老的控制器的副本数量为０的时候将其删除。这种方式可以确保在出现没有预想到的错误的时候以一种可以预测的方式进行更新。

理想的状态是，滚动更新的空置器可以将应用的就绪状态考虑在内，并且保证在一个指定的时间，有足够数量的pod能以生产状态就行服务。

这两个 _repllication controller_ 需要至少有一个进行区别的标签，例如pod中主要容器的镜像的tag，因为通常是镜像的更新驱使着滚动的更新。

滚动更新是在客户端工具[kubectl](/docs/user-guide/kubectl/kubectl_rolling-update)中实现的。

### 多个发布线路

除了在滚动更新尚未完成的过程中运行一个应用的多个发布版本之外，以更长时间运行一个应用的多个版本也是常见的，甚至是持久的，使用多个发行路线。这些路线是通过标签来进行区分。

例如，一个服务可能把将所有满足`tier in (frontend), environment in (prod)`的pod纳入了目标范围。现在，你想要有10个这样的pod副本来组成这样一个tier。但是，你想能用使用这个组件的一个新版本进行金丝雀部署。你可以设置一个 _repllication controller_ 把`replicas`设置为9，他们的标签为`tier=frontend, environment=prod, track=stable`，然后另外一个一个 _repllication controller_ 的`replicas`设置为1，然后标签为 `tier=frontend, environment=prod, track=canary`。现在，服务同时包含金丝雀和非金丝雀的pod。但是你可以用 _repllication controller_ 单独地来进行测试，监控结果等等。

## API 对象

 _repllication controller_ 在kubernetes的REST API中是一个顶层的资源。关于这个API对象的更多详情可以在这里找到：[ReplicationController API
object](http://kubernetes.io/v1.1/docs/api-reference/v1/definitions/#_v1_replicationcontroller)。
