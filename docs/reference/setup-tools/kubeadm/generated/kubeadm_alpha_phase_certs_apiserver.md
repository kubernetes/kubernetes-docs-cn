
Generates API server serving certificate and key

生成 Apiserver 的服务证书和密钥

### Synopsis

### 概要

Generates the API server serving certificate and key and saves them into apiserver.crt and apiserver.key files.

生成 Apiserver 服务证书和密钥，然后保存为 apiserver.crt 和 apiserver.key。

The certificate includes default subject alternative names and additional sans eventually provided by the user; default sans are: {node-name}, {apiserver-advertise-address}, kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc. {service-dns-domain}, {internalAPIServerVirtualIP}(that is the .10 address in {service-cidr} address space).

证书包含默认的 SANs（subject alternative names）和额外的最终提供给用户的 SANs；默认的 SANs 有: {node-name}, {apiserver-advertise-address}, kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.{service-dns-domain}, {internalAPIServerVirtualIP}（那就是在 {service-cidr} 地址空间里的 .10 地址）。

If both files already exist, kubeadm skips the generation step and existing files will be used.

如果证书和密钥都存在，那么kubeadm会跳过生成证书的步骤，然后使用已经存在的文件。

Alpha Disclaimer: this command is currently alpha.

Alpha版本免责声明: 该命令目前是 alpha 版本。

```
kubeadm alpha phase certs apiserver
```

### Options

```
      --apiserver-advertise-address string      The IP address the API server is accessible on, to use for the API server serving cert
      --apiserver-cert-extra-sans stringSlice   Optional extra altnames to use for the API server serving cert. Can be both IP addresses and dns names
      --cert-dir string                         The path where to save the certificates (default "/etc/kubernetes/pki")
      --config string                           Path to kubeadm config file (WARNING: Usage of a configuration file is experimental)
      --service-cidr string                     Alternative range of IP address for service VIPs, from which derives the internal API server VIP that will be added to the API Server serving cert (default "10.96.0.0/12")
      --service-dns-domain string               Alternative domain for services, to use for the API server serving cert (default "cluster.local")
```

### 选项

```
    --apiserver-advertise-address string: Apiserver 能够被访问到的IP地址，用于 Apiserver 服务证书。
    --apiserver-cert-extra-sans stringSlice: 可选的额外的给 Apiserver 的服务证书使用的 SANs。可以是IP地址和DNS名称。
    --cert-dir string: 存放证书的路径。（默认为 "/etc/kubernetes/pki"）
    --config string: kubeadm 的配置文件的路径。注意: 配置文件的用法还在试验阶段。
    --service-cidr string: Service 的 VIP 的备选IP地址范围，从中派生的内部的 Apiserver 的 VIP 会被添加到 Apiserver 的服务证书。（默认为 "10.96.0.0/12"）
    --service-dns-domain string: Service 的备选域名，用来给 Apiserver 的服务证书使用。（默认为 "cluster.local"）
```
