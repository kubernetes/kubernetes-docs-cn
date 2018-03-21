---
<<<<<<< HEAD
title: 生成kubeconfig文件给调度器使用
=======
title: 生成kubeconfig文件以便调度器使用
>>>>>>> babea19203d7b3d09c4f4414a74ed219464f3560
approvers:
cn-approvers:
- okzhchy
cn-approvers-update:
cn-reviewers:
<<<<<<< HEAD
---

<!-- 
=======

---

<!--
>>>>>>> babea19203d7b3d09c4f4414a74ed219464f3560
Generates a kubeconfig file for the scheduler to use

### Synopsis


Generates the kubeconfig file for the scheduler to use and saves it to /etc/kubernetes/scheduler.conf file. 

Alpha Disclaimer: this command is currently alpha.
<<<<<<< HEAD
 -->

生成kubeconfig文件给调度器使用

### 概要


生成kubeconfig文件给调度器使用且把它保存至/etc/kubernetes/scheduler.conf。

Alpha Disclaimer: 此命令目前为alpha阶段。
=======
-->


>>>>>>> babea19203d7b3d09c4f4414a74ed219464f3560

<!-- 
```
kubeadm alpha phase kubeconfig scheduler
```
 -->
```
kubeadm alpha阶段kubeconfig调度器
```

<!-- 
### Options

```
      --apiserver-advertise-address string   The IP address the API server is accessible on
      --apiserver-bind-port int32            The port the API server is accessible on (default 6443)
      --cert-dir string                      The path where certificates are stored (default "/etc/kubernetes/pki")
      --config string                        Path to kubeadm config file (WARNING: Usage of a configuration file is experimental)
      --kubeconfig-dir string                The port where to save the kubeconfig file (default "/etc/kubernetes")
```

 -->

### 选择项

```
      --apiserver-advertise-address string   API server可访问的IP地址
      --apiserver-bind-port int32            API server可访问的端口 (默认值 6443)
      --cert-dir string                      证书存储路径 (默认值 "/etc/kubernetes/pki")
      --config string                        kubeadm配置文件存储路径 (警告: 配置文件使用是实验性的)
      --kubeconfig-dir string                kubeconfig文件存储路径 (默认值 "/etc/kubernetes")
```