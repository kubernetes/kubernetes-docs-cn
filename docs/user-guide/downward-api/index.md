---
---

通过下面的例子，你将创建一个pod, 它包含了一个通过访问[downward API](/docs/user-guide/downward-api/)来使用这个pod的name和namespace的容器。

## 步骤 0: 前提条件

这个例子会假设你有一个被安装好并正在工作的Kubernetes集群，也已经在PATH环境变量中的某个目录下安装好了kubectl命令行工具。具体安装步骤请在[getting-started](/docs/getting-started-guides/)中找到与你平台对应的安装说明。

## 步骤 1: 创建一个pod

容器可以通过环境变量来使用downward API，而且downward API允许容器通过被注入的方式来使用所在pod的name和namespace信息。

下面我们使用[`dapi-pod.yaml`](/docs/user-guide/downward-api/dapi-pod.yaml)来创建一个pod,并让它其中的容器通过downward API获取了所属pod的信息。

```shell
$ kubectl create -f docs/user-guide/downward-api/dapi-pod.yaml
```

### 检查日志
这个pod将在使用downward API的容器中运行`env`命令。你可以使用grep命令过滤pod的日志来检查pod被注入了正确的值。

```shell
$ kubectl logs dapi-test-pod | grep POD_
2015-04-30T20:22:18.568024817Z MY_POD_NAME=dapi-test-pod
2015-04-30T20:22:18.568087688Z MY_POD_NAMESPACE=default
2015-04-30T20:22:18.568092435Z MY_POD_IP=10.0.1.6
```