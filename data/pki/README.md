# OpenSSL 证书配置

## 示例

### 示例规划

类型    | 编号          |  一级 | 二级          | 三级                  | 四级
---|---|---|---|---|---
机构    | 0100E3        | ca    |               |
机构    | 020101        |       | ca.vpn        |
机构    | 03010101      |       |               | ca.openvpn.vpn        |
机构    | 020201        |       | ca.internet   |
机构    | 03020101      |       |               | ca.service.internet   |
机构    | 03020102      |       |               | ca.personal.internet
机构    | 020401        |       | ca.test

### 示例实验

#### 生成根证书并自签名

- 生成密钥（如果已经有，则将密钥放到 private 目录下）。
- 生成证书颁发请求。
- 自签名。

```
[fsyyft@kvm-centos7-openssl pki]# cd /data/pki/
[fsyyft@kvm-centos7-openssl pki]# ca_root/bin/ca.ppno.net genkey ca.ppno.net 4096 123456 123456
[fsyyft@kvm-centos7-openssl pki]# ca_root/bin/ca.ppno.net req_ca
[fsyyft@kvm-centos7-openssl pki]# ca_root/bin/ca.ppno.net selfsign_ca
```

#### 生成并签名 Internet 相关证书

##### 生成并签名 Service 相关证书

- 生成密钥（如果已经有，则将密钥放到 private 目录下）。
- 生成证书颁发请求。
- 签名。

```
[fsyyft@kvm-centos7-openssl pki]# ca_root/bin/ca.ppno.net genkey ca.internet.ppno.net 4096 123456 123456
[fsyyft@kvm-centos7-openssl pki]# ca_root/bin/ca.ppno.net req_ca_internet
[fsyyft@kvm-centos7-openssl pki]# ca_root/bin/ca.ppno.net sign_ca_internet
```

##### 生成并签名 Personal 相关证书