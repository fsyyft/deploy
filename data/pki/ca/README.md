# OpenSSL 证书配置

## 示例

### 示例规划

类型    | 编号          |  一级           | 二级              | 三级                        | 四级
---|---|---|---|---|---
机构    | 0100E3        | ca.ppno.net   |                       |
机构    | 020101        |               | ca.vpn.ppno.net       |
机构    | 02010101      |               |                       | ca.openvpn.vpn.ppno.net   |
机构    | 020201        |               | ca.internet.ppno.net  |
机构    | 020401        |               | ca.test.ppno.net

### 示例实验

#### 生成根证书并自签名

- 生成密钥（如果已经有，则将密钥放到 private 目录下）。
- 生成证书颁发请求。
- 自签名。

```
[fsyyft@kvm-centos7-openssl ca]# cd /data/pki/ca/
[fsyyft@kvm-centos7-openssl ca]# bin/ca.ppno.net genkey ca.ppno.net 4096 123456 123456
[fsyyft@kvm-centos7-openssl ca]# bin/ca.ppno.net req_ca
[fsyyft@kvm-centos7-openssl ca]# bin/ca.ppno.net selfsign_ca
```