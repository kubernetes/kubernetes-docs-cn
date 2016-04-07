---
layout: docwithnav
title: "kubectl config set-credentials"
---
## kubectl config set-credentials

在kubeconfig配置文件中设置一个用户项。

### 摘要

在kubeconfig配置文件中设置一个用户项。
如果指定了一个已存在的名字，将合并新字段并覆盖旧字段。

  客户端证书设置：
    --client-certificate=certfile --client-key=keyfile

  不记名令牌设置：
    --token=bearer_token

  基础认证设置：
    --username=basic_user --password=basic_password

  不记名令牌和基础认证不能同时使用。

```
{% raw %}
kubectl config set-credentials NAME [--client-certificate=path/to/certfile] [--client-key=path/to/keyfile] [--token=bearer_token] [--username=basic_user] [--password=basic_password]
{% endraw %}
```

### 示例

```
{% raw %}
# 仅设置cluster-admin用户项下的client-key字段，不影响其他值
$ kubectl config set-credentials cluster-admin --client-key=~/.kube/admin.key

# 为cluster-admin用户项设置基础认证选项
$ kubectl config set-credentials cluster-admin --username=admin --password=uXFGweU9l35qcif

# 为cluster-admin用户项开启证书验证并设置证书文件路径
$ kubectl config set-credentials cluster-admin --client-certificate=~/.kube/admin.crt --embed-certs=true
{% endraw %}
```

### 选项

```
{% raw %}
      --client-certificate="": 设置kuebconfig配置文件中用户选项中的证书文件路径。
      --client-key="": 设置kuebconfig配置文件中用户选项中的证书密钥路径。
      --embed-certs=false: 设置kuebconfig配置文件中用户选项中的embed-certs开关。
      --password="": 设置kuebconfig配置文件中用户选项中的密码。
      --token="": 设置kuebconfig配置文件中用户选项中的令牌。
      --username="": 设置kuebconfig配置文件中用户选项中的用户名。
{% endraw %}
```

### 继承自父命令的选项

```
{% raw %}
      --alsologtostderr[=false]: 同时输出日志到标准错误控制台和文件。
      --api-version="": 和服务端交互使用的API版本。
      --certificate-authority="": 用以进行证书权威性认证的.cert文件路径。
      --client-certificate="": TLS使用的客户端证书路径。
      --client-key="": TLS使用的客户端密钥路径。
      --cluster="": 指定使用的kubeconfig配置文件中的集群名。
      --context="": 指定使用的kubeconfig配置文件中的环境名。
      --insecure-skip-tls-verify[=false]: 如果为true，将不会检查服务器凭证的有效性，这会导致你的HTTPS链接变得不安全。
      --kubeconfig="": 命令行请求使用的配置文件路径。
      --log-backtrace-at=:0: 当日志长度超过定义的行数时，忽略堆栈信息。
      --log-dir="": 如果不为空，将日志文件写入此目录。
      --log-flush-frequency=5s: 刷新日志的最大时间间隔。
      --logtostderr[=true]: 输出日志到标准错误控制台，不输出到文件。
      --match-server-version[=false]: 要求服务端和客户端版本匹配。
      --namespace="": 如果不为空，命令将使用此namespace。
      --password="": API Server进行简单认证使用的密码。
  -s, --server="": Kubernetes API Server的地址和端口号。
      --stderrthreshold=2: 高于此级别的日志将被输出到错误控制台。
      --token="": 认证到API Server使用的令牌。
      --user="": 指定使用的kubeconfig配置文件中的用户名。
      --username="": API Server进行简单认证使用的用户名。
      --v=0: 指定输出日志的级别。
      --vmodule=: 指定输出日志的模块，格式如下：pattern=N，使用逗号分隔。
{% endraw %}
```

### 参见

* [kubectl config](kubectl_config.html)	 - 修改kubeconfig配置文件。
