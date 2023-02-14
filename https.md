#acme.sh 申请证书

## 基于腾讯的dnspod.cn

*注意⚠️：犹豫腾讯升级接口到3.0 要区分开dnspod和两种调用接口的密钥区别*

犹豫acme.sh没有升级相关脚本，现在还是使用老接口。

DP_Id DP_Key 使用的是dnspod token，而不是腾讯云API密钥

## 安装

```shell
curl https://get.acme.sh | sh -s email=my@example.com
```

## 配置密钥

`DP_Id`
`DP_Key`

## 发布

```shell
 acme.sh --issue --dns dns_dp -d imjcker.com -d '*.imjcker.com'
```

