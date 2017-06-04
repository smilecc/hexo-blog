---
title: 使用OpenSSL生成CSR
date: 2017-05-30 21:43:42
categories:
    - 工具
---

在申请SSL证书的时候，需要创建证书签发请求文件，一般称之为CSR（Certificate Signing Request）文件。
使用OpenSSL可以很方便的创建这个文件。

首先安装OpenSSL
``` bash
sudo apt install openssl
```

<!-- more -->

生成私钥
``` bash
openssl genrsa -out domain.com.key 2048 # 将domain.com换成你自己的域名
```
要说明的是，最后一个参数`2048`是RSA秘钥的bit，而很多较老的教程上使用的还是1024bit的RSA，一些证书销售商会拒绝低于2048bit的秘钥生成出的CSR。

生成请求
``` bash
openssl req -new -key domain.com.key -out domain.com.csr
```
接下来的命令行会让你填写一些信息，要注意的是在`Common Name`这项信息中填写你要申请证书的域名。
