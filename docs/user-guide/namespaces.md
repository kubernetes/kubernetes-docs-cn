---
---

Kubernetes支持一个集群支撑多个虚拟的集群.这些虚拟的集群被称作namespaces(命名空间).

## 何时使用多个Namespace

Namespace意在用在有分布多个团队或者项目的用户的的环境中.对于那些只有几个或者几十个用户的集群来说,你压根不必创建或者考虑Namespace.只有在当你需要Namespace提供的功能的时候再考虑使用Namespace.

Namespace为name提供了一个作用域.Resouce的名称需要子Namespace中唯一,当时不需要在Namespace之间唯一.

Namespace是一个将集群的Resource在多个使用场景钟分割的方式(通过[resouce quota(资源配额)](/docs/admin/resource-quota)).

在以后的Kubernetes版本中,在同一个namespace的对象默认有一样的访问控制策略.

如果只是分割只是差别不大的资源,那没有必要使用多个namespace,例如一个软件的不同版本:使用[labels](/docs/user-guide/labels)来区分在同一个命名空间的资源.


## 使用Namespaces

关于namespace的创建和删除请查看[Admin Guide documentation for namespaces](/docs/admin/namespaces).

### 查看namespaces

要列出集群中目前有哪些namespace,可以使用:

```shell
$ kubectl get namespaces
NAME          LABELS    STATUS
default       <none>    Active
kube-system   <none>    Active
```

Kubernetes starts with two initial namespaces:

   * `default` The default namespace for objects with no other namespace
   * `kube-system` The namespace for objects created by the Kubernetes system

Kubernetes默认有两个初始的namespace:

  * `default` 对与没有其他namespace的对象的默认namespace
  * `kube-system` 由Kubernetes系统创建的对象的namespace

### 设置一个请求的namespace

要临时为一个请求设置一个namespace,可以使用`--namespace`的标记.

例如:

```shell
$ kubectl --namespace=<insert-namespace-name-here> run nginx --image=nginx
$ kubectl --namespace=<insert-namespace-name-here> get pods
```

### 设置namespace的偏好


你可以为该场景的后续的kubectl命令永久地保存namespace.

首先,获取当前的场景:

```shell
$ export CONTEXT=$(kubectl config view | grep current-context | awk '{print $2}')
```

然后更新默认的namespace:

```shell
$ kubectl config set-context $(CONTEXT) --namespace=<insert-namespace-name-here>
```

## Namespace和DNS

当你创建一个[Service](/docs/user-guide/services)的时候,它会创建一个响应的 [DNS entry](/docs/admin/dns).这个条目的格式如`<service-name>.<namespace-name>.svc.cluster.local`,这意味着仅仅使用`<service-name>`就会解析到位于namespace本地的服务.这对于在多个namespace中使用相同的配置很有用,例如Development, Staging and Production.如果你想跨namespace访问,你应该使用完全限定域名(FQDN).

## 不是所有的对象都在Namespace中

绝大多数的kubernetes资源(如:pod,service,replication controller,和其他)都位于某一个namespace中.然后,namespace资源自身却不是位于某一个namespac中.并且,一些低层次的资源,如[nodes](/docs/admin/node)和persistentVolumes也不属于某一个namespace.Event是一个例外:它们可以有也可以没有一个namespace,这取决于改event的对象是关乎什么.
