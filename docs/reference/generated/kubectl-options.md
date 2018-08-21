---
title: kubectl
---
<!--
kubectl controls the Kubernetes cluster manager.

Find more information at [https://github.com/kubernetes/kubernetes](https://github.com/kubernetes/kubernetes).
-->
kubectl 可以控制 Kubernetes 集群管理器。

获取更多信息，请访问 [https://github.com/kubernetes/kubernetes](https://github.com/kubernetes/kubernetes)。

```
kubectl
```
<!--
### Options

```
      --alsologtostderr                  log to standard error as well as files
      --as string                        Username to impersonate for the operation
      --certificate-authority string     Path to a cert file for the certificate authority
      --client-certificate string        Path to a client certificate file for TLS
      --client-key string                Path to a client key file for TLS
      --cluster string                   The name of the kubeconfig cluster to use
      --context string                   The name of the kubeconfig context to use
      --insecure-skip-tls-verify         If true, the server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kubeconfig string                Path to the kubeconfig file to use for CLI requests.
      --log-backtrace-at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log-dir string                   If non-empty, write log files in this directory
      --logtostderr                      log to standard error instead of files
      --match-server-version             Require server version to match client version
  -n, --namespace string                 If present, the namespace scope for this CLI request
      --password string                  Password for basic authentication to the API server
      --request-timeout string           The length of time to wait before giving up on a single server request. Non-zero values should contain a corresponding time unit (e.g. 1s, 2m, 3h). A value of zero means don't timeout requests. (default "0")
  -s, --server string                    The address and port of the Kubernetes API server
      --stderrthreshold severity         logs at or above this threshold go to stderr (default 2)
      --token string                     Bearer token for authentication to the API server
      --user string                      The name of the kubeconfig user to use
      --username string                  Username for basic authentication to the API server
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging
```
-->
### 选项
```
      --alsologtostderr                  同时输出日志到标准错误控制台和文件
      --as string                        以指定用户执行操作
      --certificate-authority string     用于进行认证授权的 .cert 文件路径
      --client-certificate string        TLS 使用的客户端证书路径
      --client-key string                TLS 使用的客户端密钥文件路径
      --cluster string                   指定要使用的 kubeconfig 文件中集群名
      --context string                   指定要使用的 kubeconfig 文件中上下文
      --insecure-skip-tls-verify         值为 true，则不会检查服务器的证书的有效性。 这将使您的HTTPS连接不安全
      --kubeconfig string                CLI 请求使用的 kubeconfig 配置文件路径。
      --log-backtrace-at traceLocation   当日志长度超出规定的行数时，忽略堆栈信息（默认值：0）
      --log-dir string                   如果不为空，则将日志文件写入此目录
      --logtostderr                      日志输出到标准错误控制台而不输出到文件
      --match-server-version             要求客户端版本和服务端版本相匹配
  -n, --namespace string                 如果存在，CLI 请求将使用此命名空间
      --password string                  API server 进行简单认证的密码
      --request-timeout string           放弃一个简单服务请求前的等待时间，非零值需要包含相应时间单位(例如：1s, 2m, 3h)。零值则认为不做超时请求。 (默认值 "0")
  -s, --server string                    Kubernetes API server 的地址和端口
      --stderrthreshold severity         等于或高于此阈值的日志将输出标准错误控制台（默认值2）
      --token string                     用于 API server 进行身份认证的承载令牌
      --user string                      指定使用的 kubeconfig 配置文件中的用户名
      --username string                  API server 进行简单认证的用户名
  -v, --v Level                          指定输出日志的日志级别
      --vmodule moduleSpec               指定输出日志的模块，格式如下：pattern=N，使用逗号分隔
```
    
<!--
### SEE ALSO

* [kubectl annotate](/docs/user-guide/kubectl/{{page.version}}/#annotate)     - Update the annotations on a resource
* [kubectl api-versions](/docs/user-guide/kubectl/{{page.version}}/#api-versions)     - Print the supported API versions on the server, in the form of "group/version"
* [kubectl apply](/docs/user-guide/kubectl/{{page.version}}/#apply)     - Apply a configuration to a resource by filename or stdin
* [kubectl attach](/docs/user-guide/kubectl/{{page.version}}/#attach)     - Attach to a running container
* [kubectl autoscale](/docs/user-guide/kubectl/{{page.version}}/#autoscale)     - Auto-scale a Deployment, ReplicaSet, or ReplicationController
* [kubectl certificate](/docs/user-guide/kubectl/{{page.version}}/#certificate)     - Modify certificate resources.
* [kubectl cluster-info](/docs/user-guide/kubectl/{{page.version}}/#cluster-info)     - Display cluster info
* [kubectl completion](/docs/user-guide/kubectl/{{page.version}}/#completion)     - Output shell completion code for the given shell (bash or zsh)
* [kubectl config](/docs/user-guide/kubectl/{{page.version}}/#config)     - Modify kubeconfig files
* [kubectl convert](/docs/user-guide/kubectl/{{page.version}}/#convert)     - Convert config files between different API versions
* [kubectl cordon](/docs/user-guide/kubectl/{{page.version}}/#cordon)     - Mark node as unschedulable
* [kubectl cp](/docs/user-guide/kubectl/{{page.version}}/#cp)     - Copy files and directories to and from containers.
* [kubectl create](/docs/user-guide/kubectl/{{page.version}}/#create)     - Create a resource by filename or stdin
* [kubectl delete](/docs/user-guide/kubectl/{{page.version}}/#delete)     - Delete resources by filenames, stdin, resources and names, or by resources and label selector
* [kubectl describe](/docs/user-guide/kubectl/{{page.version}}/#describe)     - Show details of a specific resource or group of resources
* [kubectl drain](/docs/user-guide/kubectl/{{page.version}}/#drain)     - Drain node in preparation for maintenance
* [kubectl edit](/docs/user-guide/kubectl/{{page.version}}/#edit)     - Edit a resource on the server
* [kubectl exec](/docs/user-guide/kubectl/{{page.version}}/#exec)     - Execute a command in a container
* [kubectl explain](/docs/user-guide/kubectl/{{page.version}}/#explain)     - Documentation of resources
* [kubectl expose](/docs/user-guide/kubectl/{{page.version}}/#expose)     - Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
* [kubectl get](/docs/user-guide/kubectl/{{page.version}}/#get)     - Display one or many resources
* [kubectl label](/docs/user-guide/kubectl/{{page.version}}/#label)     - Update the labels on a resource
* [kubectl logs](/docs/user-guide/kubectl/{{page.version}}/#logs)     - Print the logs for a container in a pod
* [kubectl options](/docs/user-guide/kubectl/{{page.version}}/#options)     -
* [kubectl patch](/docs/user-guide/kubectl/{{page.version}}/#patch)     - Update field(s) of a resource using strategic merge patch
* [kubectl port-forward](/docs/user-guide/kubectl/{{page.version}}/#port-forward)     - Forward one or more local ports to a pod
* [kubectl proxy](/docs/user-guide/kubectl/{{page.version}}/#proxy)     - Run a proxy to the Kubernetes API server
* [kubectl replace](/docs/user-guide/kubectl/{{page.version}}/#replace)     - Replace a resource by filename or stdin
* [kubectl rolling-update](/docs/user-guide/kubectl/{{page.version}}/#rolling-update)     - Perform a rolling update of the given ReplicationController
* [kubectl rollout](/docs/user-guide/kubectl/{{page.version}}/#rollout)     - Manage a deployment rollout
* [kubectl run](/docs/user-guide/kubectl/{{page.version}}/#run)     - Run a particular image on the cluster
* [kubectl scale](/docs/user-guide/kubectl/{{page.version}}/#scale)     - Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job
* [kubectl set](/docs/user-guide/kubectl/{{page.version}}/#set)     - Set specific features on objects
* [kubectl taint](/docs/user-guide/kubectl/{{page.version}}/#taint)     - Update the taints on one or more nodes
* [kubectl top](/docs/user-guide/kubectl/{{page.version}}/#top)     - Display Resource (CPU/Memory/Storage) usage
* [kubectl uncordon](/docs/user-guide/kubectl/{{page.version}}/#uncordon)     - Mark node as schedulable
* [kubectl version](/docs/user-guide/kubectl/{{page.version}}/#version)     - Print the client and server version information

###### Auto generated by spf13/cobra on 13-Dec-2016
-->
### 也可以看看

* [kubectl annotate](/docs/user-guide/kubectl/{{page.version}}/#annotate)     - 更新资源上注释
* [kubectl api-versions](/docs/user-guide/kubectl/{{page.version}}/#api-versions)     - 以 "group/version" 的形式在服务器上打印支持的 API 版本
* [kubectl apply](/docs/user-guide/kubectl/{{page.version}}/#apply)     - 通过文件名或标准输入将配置添加给资源
* [kubectl attach](/docs/user-guide/kubectl/{{page.version}}/#attach)     - 附加到正在运行的容器
* [kubectl autoscale](/docs/user-guide/kubectl/{{page.version}}/#autoscale)     - 自动扩展 Deployment, ReplicaSet 或 ReplicationController
* [kubectl certificate](/docs/user-guide/kubectl/{{page.version}}/#certificate)     - 修改证书资源
* [kubectl cluster-info](/docs/user-guide/kubectl/{{page.version}}/#cluster-info)     - 展示集群信息
* [kubectl completion](/docs/user-guide/kubectl/{{page.version}}/#completion)     - 为给定的 shell 输出完成代码（ bash 或 zsh）
* [kubectl config](/docs/user-guide/kubectl/{{page.version}}/#config)     - 修改 kubeconfig 配置文件
* [kubectl convert](/docs/user-guide/kubectl/{{page.version}}/#convert)     - 在不同的 API 版本之间转换配置文件
* [kubectl cordon](/docs/user-guide/kubectl/{{page.version}}/#cordon)     - 将 node 节点标记为不可调度
* [kubectl cp](/docs/user-guide/kubectl/{{page.version}}/#cp)     - 从容器复制文件和目录，也可将文件和目录复制到容器。
* [kubectl create](/docs/user-guide/kubectl/{{page.version}}/#create)     - 通过文件名或标准输入创建资源
* [kubectl delete](/docs/user-guide/kubectl/{{page.version}}/#delete)     - 通过文件名，标准输入，资源和名称或资源和标签选择器删除资源
* [kubectl describe](/docs/user-guide/kubectl/{{page.version}}/#describe)     - 显示特定资源或资源组的详细信息
* [kubectl drain](/docs/user-guide/kubectl/{{page.version}}/#drain)     - 为便于维护，需要提前驱逐node节点
* [kubectl edit](/docs/user-guide/kubectl/{{page.version}}/#edit)     - 在服务器编辑资源
* [kubectl exec](/docs/user-guide/kubectl/{{page.version}}/#exec)     - 容器内退出命令
* [kubectl explain](/docs/user-guide/kubectl/{{page.version}}/#explain)     - 资源文档
* [kubectl expose](/docs/user-guide/kubectl/{{page.version}}/#expose)     - 获取 replication controller, service, deployment 或 pod 资源，并作为新的 Kubernetes 服务暴露 
* [kubectl get](/docs/user-guide/kubectl/{{page.version}}/#get)     - 展示一个或多个资源
* [kubectl label](/docs/user-guide/kubectl/{{page.version}}/#label)     - 升级资源标签
* [kubectl logs](/docs/user-guide/kubectl/{{page.version}}/#logs)     - 为 pod 中的容器打印日志
* [kubectl options](/docs/user-guide/kubectl/{{page.version}}/#options)     -
* [kubectl patch](/docs/user-guide/kubectl/{{page.version}}/#patch)     - 使用战略性合并补丁更新资源字段
* [kubectl port-forward](/docs/user-guide/kubectl/{{page.version}}/#port-forward)     - 给 pod 开放一个或多个本地端口
* [kubectl proxy](/docs/user-guide/kubectl/{{page.version}}/#proxy)     - 为 Kubernetes API server 运行代理
* [kubectl replace](/docs/user-guide/kubectl/{{page.version}}/#replace)     - 通过文件或标准输入替换资源
* [kubectl rolling-update](/docs/user-guide/kubectl/{{page.version}}/#rolling-update)     - 对给定 ReplicationController 执行滚动更新
* [kubectl rollout](/docs/user-guide/kubectl/{{page.version}}/#rollout)     - 管理推出 deployment
* [kubectl run](/docs/user-guide/kubectl/{{page.version}}/#run)     - 在集群上运行指定镜像
* [kubectl scale](/docs/user-guide/kubectl/{{page.version}}/#scale)     - 给 Deployment, ReplicaSet, Replication Controller 或 Job 设置新大小
* [kubectl set](/docs/user-guide/kubectl/{{page.version}}/#set)     - 给对象设置特定功能
* [kubectl taint](/docs/user-guide/kubectl/{{page.version}}/#taint)     - 更新一个或多个 node 节点的污点信息
* [kubectl top](/docs/user-guide/kubectl/{{page.version}}/#top)     - 展示资源 (CPU/Memory/Storage) 使用信息
* [kubectl uncordon](/docs/user-guide/kubectl/{{page.version}}/#uncordon)     - 标记 node 节点为可调度
* [kubectl version](/docs/user-guide/kubectl/{{page.version}}/#version)     - 打印客户端和服务端版本信息

###### spf13/cobra自动生成于2016年12月13号

<!-- BEGIN MUNGE: GENERATED_ANALYTICS -->
[![Analytics](https://kubernetes-site.appspot.com/UA-36037335-10/GitHub/docs/user-guide/kubectl/kubectl.md?pixel)]()
<!-- END MUNGE: GENERATED_ANALYTICS -->
