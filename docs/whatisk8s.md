---
---
Kubernetes一个用于容器集群的自动化部署、扩容以及运维的开源平台。

使用Kubernetes，你可以快速高效地响应客户需求：

 - 动态地对应用进行扩容。
 - 无缝地发布新特性。
 - 仅使用需要的资源以优化硬件使用。

我们希望培育出一个组件及工具的生态，帮助大家减轻在公有云及私有云上运行应用的负担。


## Kubernetes是：

* **简洁的**：轻量级，简单，易上手
* **可移植的**：公有，私有，混合，多重云（multi-cloud）
* **可扩展的**: 模块化, 插件化, 可挂载, 可组合
* **可自愈的**: 自动布置, 自动重启, 自动复制

Kubernetes项目是Google在2014年启动的。Kubernetes构建在[Google公司十几年的大规模高负载生产系统运维经验](https://research.google.com/pubs/pub43438.html)之上，同时结合了社区中各项最佳设计和实践。


## 为何要使用容器技术？

想了解为何要使用[容器](http://aucouranton.com/2014/06/13/linux-containers-parallels-lxc-openvz-docker-and-more/)技术？

下面是一些关键点:

* **以应用程序为中心的管理**：
    将抽象级别从在虚拟硬件上运行操作系统上升到了在使用特定逻辑资源的操作系统上运行应用程序。这在提供了Paas的简洁性的同时拥有IssS的灵活性，并且相对于运行[12-factor应用程序](http://12factor.net/)有过之而无不及。
* **开发和运维的关注点分离**:
    提供构建和部署的分离；这样也就将应用从基础设施中解耦。
* **敏捷的应用创建和部署**:
    相对使用虚拟机镜像，容器镜像的创建更加轻巧高效。
* **持续开发，持续集成以及持续部署**:
    提供频繁可靠地构建和部署容器镜像的能力，同时可以快速简单地回滚(因为镜像是固化的)。
* **松耦合，分布式，弹性，自由的[微服务](http://martinfowler.com/articles/microservices.html)**:
    应用被分割为若干独立的小型程序，可以被动态地部署和管理 -- 而不是一个运行在单机上的超级臃肿的大程序。
* **开发，测试，生产环境保持高度一致**:
    无论是再笔记本电脑还是服务器上，都采用相同方式运行。
* **兼容不同的云平台或操作系统上**:
    可运行与Ubuntu，RHEL，on-prem或者Google Container Engine，覆盖了开发，测试和生产的各种不同环境。
* **资源分离**:
    带来可预测的程序性能。
* **资源利用**:
    高性能，大容量。
    

## Kubernetes能做什么？

Kubernetes能够可以在运行于物理机或者虚拟机上的集群中运行和调度容器。

它可以做的事情当然还有更多。

Kubernetes能够满足在生产环境运行应用程序的常用需求，如：

* [主机代管进程](/docs/user-guide/pods/),
* [挂在卷](/docs/user-guide/volumes/),
* [分发密钥](/docs/user-guide/secrets/),
* [应用存活检测](/docs/user-guide/production-pods/#liveness-and-readiness-probes-aka-health-checks),
* [复制应用实例](/docs/user-guide/replication-controller/),
* [自动水平扩展](/docs/user-guide/horizontal-pod-autoscaler/),
* [负载均衡](/docs/user-guide/services/),
* [热更新](/docs/user-guide/update-demo/), 以及
* [资源监控](/docs/user-guide/monitoring/).

## Kubernetes不是：

Kubernetes不是PaaS（平台即服务）。

* Kubernetes并不对支持的应用程序类型有任何限制。 它并不指定应用框架，限制语言类型，也不仅仅迎合 [12-factor应用程序](http://12factor.net/)模式. Kubernetes旨在支持各种多种多样的负载类型：只要一个程序能够在容器中运行，它就可以在Kubernetes中运行。
* Kubernetes并不关注代码到镜像领域。它并不负责应用程序的构建。不同的用户和项目对持续集成流程都有不同的需求和偏好，所以我们分层支持持续集成但并不规定和限制它的工作方式。
* 另一方面， 确实有不少PaaS系统运行在Kubernetes*之上*，比如[Openshift](https://github.com/openshift/origin)和[Deis](http://deis.io/)。同样你也可以将定制的PaaS系统，结合一个持续集成系统再Kubernetes上进行实施：只需生成容器镜像并通过Kubernetes部署。
* 由于Kubernetes运行再应用层而不是硬件层，所以它提供了一些一般PaaS提供的功能，比如部署，扩容，负载均衡，日志，监控，等等。无论如何，Kubernetes不是一个单一应用，所以这些解决方案都是可选可插拔的。

Kubernetes并不是单单的"编排系统"；它排除了对编排的需要:

* “编排”的技术定义为按照指定流程执行一系列动作：执行A，然后B，然后C。相反，Kubernetes有一系列控制进程组成，持续地控制从当前状态到指定状态的流转。无需关注你是如何从A到C：只需结果如此。这样将使得系统更加易用，强大，健壮和弹性。


## *Kuberbetes*这个名字是什么意思？k8s又是什么？

Kubernetes这个名字源自希腊语，意思是“舵手”，也是“管理者”，“治理者”等词的源头。k8s是Kubernetes的简称（用数字『8』替代中间的8个字母『ubernete』）。