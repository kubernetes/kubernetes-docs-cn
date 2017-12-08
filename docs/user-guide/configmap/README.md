---
title: README
cn-approvers:
- WinSon-XiaoYin
---
<!--
# ConfigMap example
-->
# ConfigMap 示例

<!--
## Step Zero: Prerequisites

This example assumes you have a Kubernetes cluster installed and running, and that you have
installed the `kubectl` command line tool somewhere in your path. Please see [pick the right solution
started](/docs/setup/pick-right-solution/) for installation instructions for your platform.
-->

## 第0步：前提条件

这个例子假定你有一个安装好的并正在运行的 Kubernetes 集群，以及在你的路径某处已经安装了 ‘kubectl’ 命令行工具。请查看[选择正确的解决方案开始](/docs/setup/pick-right-solution/)为你的平台提供了安装说明。

<!--
## Step One: Create the ConfigMap

A ConfigMap contains a set of named strings.

Use the [`configmap.yaml`](configmap.yaml) file to create a ConfigMap:

```shell
$ kubectl create -f docs/user-guide/configmap/configmap.yaml
```

You can use `kubectl` to see information about the ConfigMap:

```shell
$ kubectl get configmap
NAME                   DATA      AGE
test-configmap         2         6s

$ kubectl describe configMap test-configmap
Name:          test-configmap
Labels:        <none>
Annotations:   <none>

Data
====
data-1: 7 bytes
data-2: 7 bytes
```

View the values of the keys with `kubectl get`:

```shell
$ kubectl get configmaps test-configmap -o yaml
apiVersion: v1
data:
  data-1: value-1
  data-2: value-2
kind: ConfigMap
metadata:
  creationTimestamp: 2016-02-18T20:28:50Z
  name: test-configmap
  namespace: default
  resourceVersion: "1090"
  selfLink: /api/v1/namespaces/default/configmaps/test-configmap
  uid: 384bd365-d67e-11e5-8cd0-68f728db1985
```
-->

## 第一步：创建ConfigMap

一个ConfigMap包含一组命名字符串。

使用[`configmap.yaml`](configmap.yaml)文件来创建ConfigMap：

```shell
$ kubectl create -f docs/user-guide/configmap/configmap.yaml
```

你可以使用`kubectl`来查看ConfigMap的相关信息：

```shell
$ kubectl get configmap
NAME                   DATA      AGE
test-configmap         2         6s

$ kubectl describe configMap test-configmap
Name:          test-configmap
Labels:        <none>
Annotations:   <none>

Data
====
data-1: 7 bytes
data-2: 7 bytes
```

查看 `kubectl get`获取到的值：

```shell
$ kubectl get configmaps test-configmap -o yaml
apiVersion: v1
data:
  data-1: value-1
  data-2: value-2
kind: ConfigMap
metadata:
  creationTimestamp: 2016-02-18T20:28:50Z
  name: test-configmap
  namespace: default
  resourceVersion: "1090"
  selfLink: /api/v1/namespaces/default/configmaps/test-configmap
  uid: 384bd365-d67e-11e5-8cd0-68f728db1985
```

<!--
## Step Two: Create a pod that consumes a configMap in environment variables

Use the [`env-pod.yaml`](env-pod.yaml) file to create a Pod that consumes the
ConfigMap in environment variables.

```shell
$ kubectl create -f docs/user-guide/configmap/env-pod.yaml
```

This pod runs the `env` command to display the environment of the container:

```shell
$ kubectl logs config-env-test-pod | grep KUBE_CONFIG
KUBE_CONFIG_1=value-1
KUBE_CONFIG_2=value-2
```
-->

## 第二步：创建一个在环境变量中使用configMap的pod

使用[`env-pod.yaml`](env-pod.yaml)文件来创建一个在环境变量中使用configMap的pod

```shell
$ kubectl create -f docs/user-guide/configmap/env-pod.yaml
```

这个pod运行`env`来展示容器的环境变量：

```shell
$ kubectl logs config-env-test-pod | grep KUBE_CONFIG
KUBE_CONFIG_1=value-1
KUBE_CONFIG_2=value-2
```


<!--
## Step Three: Create a pod that sets the command line using ConfigMap

Use the [`command-pod.yaml`](command-pod.yaml) file to create a Pod with a container
whose command is injected with the keys of a ConfigMap:

```shell
$ kubectl create -f docs/user-guide/configmap/command-pod.yaml
```

This pod runs an `echo` command to display the keys:

```shell
$ kubectl logs config-cmd-test-pod
value-1 value-2
```
-->

## 第三步：创建一个使用ConfigMap设置命令行的pod

使用[`command-pod.yaml`](command-pod.yaml)文件来创建一个命令被注入到ConfigMap键中的容器pod：

```shell
$ kubectl create -f docs/user-guide/configmap/command-pod.yaml
```

这个pod运行`echo`命令显示这些键：

```shell
$ kubectl logs config-cmd-test-pod
value-1 value-2
```



<!--
## Step Four: Create a pod that consumes a configMap in a volume

Pods can also consume ConfigMaps in volumes.  Use the [`volume-pod.yaml`](volume-pod.yaml) file to create a Pod that consumes the ConfigMap in a volume.

```shell
$ kubectl create -f docs/user-guide/configmap/volume-pod.yaml
```

This pod runs a `cat` command to print the value of one of the keys in the volume:

```shell
$ kubectl logs config-volume-test-pod
value-1
```

Alternatively you can use [`mount-file-pod.yaml`](mount-file-pod.yaml) file to mount
only a file from ConfigMap, preserving original content of /etc directory.
-->

## 第四步：创建一个在卷中使用configMap的pod

Pods同样可以在卷中使用ConfigMaps。使用[`volume-pod.yaml`](volume-pod.yaml)文件来创建一个在卷中使用ConfigMap的pod。

```shell
$ kubectl create -f docs/user-guide/configmap/volume-pod.yaml
```

或者你可以使用[`mount-file-pod.yaml`](mount-file-pod.yaml)文件去挂载一个来自ConfigMap的文件，保留/etc目录的原始内容。