---
---

接下来的例子，你会创建一个pod, 它包含了一个通过访问[downward API](/docs/user-guide/downward-api/)来使用这个pod名字和命名空的容器

#步骤 0: 前提条件

这个例子会假设你有一个安装好并正在运行的Kubernetes集群，也已经在系统的某个路径下安装好了kubectl命令行工具。具体安装步骤请在[安装入门](http://kubernetes.io/v1.0/docs/getting-started-guides/)中找到与你平台对应的安装说明。

#步骤 1: 创建一个pod

容器可以通过环境变量来消费downward API，而且downward API允许容器通过被注入的方来式使用所在pod的name和namespace等信息

下面我们使用examples/downward-api/dapi-pod.yaml来创建一个pod,并让它其中的容器通过downward API获取了所属pod的信息:

```
$ kubectl create -f docs/user-guide/downward-api/dapi-pod.yaml
```

#检查日志
这个pod在一个容器中运行env命令，来调用downward API。 接下来你就能通过过滤pod的日志来看到这个pod被注入的确切的值：

```
$ kubectl logs dapi-test-pod | grep POD_
2015-04-30T20:22:18.568024817Z POD_NAME=dapi-test-pod
2015-04-30T20:22:18.568087688Z POD_NAMESPACE=default
2015-04-30T20:22:18.568092435Z MY_POD_IP=10.0.1.6
```