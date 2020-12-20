---
title: ssl双向认证（二）：证书
date: 2018-11-27 21:59:49
categories: 
- 网络
- java
tags:
- httpclient
- ssl
---

## 证书

> 数字证书（或简称证书）是在 `Internet` 上唯一地标识人员和资源的电子文件。证书使两个实体之间能够进行安全、保密的通信。证书有很多种类型，例如个人证书（由个人使用）和服务器证书（用于通过安全套接字层  `SSL`  技术在服务器和客户机之间建立安全会话）。

### 证书介绍

#### `SSL` 简介

**`wiki`** : **传输层安全性协议**（`Transport Layer Security`，简称 `TLS`），前身**安全套接层**（`Secure Sockets Layer`，简称 `SSL`），是一种安全协议，目的是为互联网通信提供安全及数据完整性保障。

**`Https`** : **超文本传输安全协议**（`Hypertext Transfer Protocol Secure`，简称 `HTTPS`，常称为`HTTP over TLS`，`HTTP over SSL`），是一种透过计算机网络进行安全通信的传输协议。`HTTPS` 经由 `HTTP` 进行通信，但利用 `SSL/TLS` 来加密数据包。`HTTPS` 开发的主要目的，是提供对网站服务器的身份认证，保护交换数据的隐私与完整性。简而言之，`HTTPS` 就是带加密的 `HTTP` 协议。

**`OpenSSL`** : 一个开放源代码的软件库包，应用程序可以使用这个包来进行安全通信，避免窃听，同时确认另一端连线者的身份。其主要库是 `C语言` 所写成，实现了基本的加密功能，实现了 `SSL/TLS` 协议。也就是说 `OpenSSL` 是 `SSL/TLS` 的一种实现。

#### 证书标准

**`X.509`** : 它是密码学里公钥证书的格式标准。`X.509` 证书里含有**公钥**、**身份信息**（比如网络主机名，组织的名称或个体名称等）和**签名信息**（可以是证书签发机构CA的签名，也可以是自签名）。

#### 证书文件扩展名

`X.509` 有多种常用的扩展名。不过其中的一些还用于其它用途，就是说具有这个扩展名的文件可能并不是证书，比如说可能只是保存了私钥。

- **`.pem`** :   ` Privacy Enhanced Mai` ，`DER` 编码的证书再进行 `BASE64` 编码的数据存放在 `----BEGIN CERTIFICATE---- `和 `----END CERTIFICATE----`之中。

  查看 `pem` 格式证书信息：

  ```shell
  openssl x509 -in own.pem -text -noout
  ```

  `Apache` 和 `*NIX`服务器偏向于使用这种编码格式。

- **`.cer`、`.crt`、`.der`** : `Distinguished Encoding Rules`， 通常是 `DER` 二进制格式。

  查看 `der` 格式证书信息：

  ```shell
  openssl x509 -in own.der -inform der -text -noout
  ```

  `Java` 和 `Windows` 服务器偏向于使用这种编码格式。

- **`.key`** : 通常用来存放一个公钥或者私钥，并非`X.509`证书，编码同样的，可能是`PEM`，也可能是`DER`。

- **`PFX/P12`** : `predecessor of PKCS#12` ，对 `*nix` 服务器来说，一般 `CRT` 和 `KEY` 是分开放在不同文件夹中，但 `Windows` 的 `IIS` 则将它们存放在一个 `PFX` 文件中，通常会有一个**提取密码**。

- **`JKS`** :  `Java Key Storage` ，利用Java的`keytool`工具可以将 `PFX` 转为 `JKS`，供 `Java` 调用。

上面只是对自己常用的证书后缀的一个简单解释，具体可参照 `维基百科`。

#### 证书转换

> Java自带的 `keytool` 工具是个密钥和证书管理工具。它使用户能够管理自己的公钥/私钥对及相关证书，用于（通过数字签名）自我认证（用户向别的用户/服务认证自己）或数据完整性以及认证服务。它还允许用户储存他们的通信对等者的公钥（以证书形式）。

由于业务需要，需要 `java` 实现 `https` 双向认证，需要使用 `keytool` 和 `openssl` 将 `pem` 证书转换成 `jks` 证书。

文件列表：

1. `own.pem`: `BASE64` 编码的证书
2. `per.key`: 密钥

生成`JKS`证书：

1. 生成 `pk12` 密钥文件

   ```shell
   openssl pkcs12 -export -in own.pem -inkey per.key -out test.pk12 -name test
   ```

2. 将 `test.pk12` 生成 `test.jks`

   ```shell
   keytool -importkeystore -deststorepass changeit -destkeypass changeit -destkeystore test.jks -srckeystore test.pk12 -srcstoretype PKCS12 -srcstorepass changeit -alias test
   ```

1. 除了生成 `keystore` 之外，还可以生成 `truststore`。

参考：

- [从pem文件生成keystore和truststore文件](https://chenguangqi.github.io/blog/keystore.html)
- [证书转换：pem 转keystore](http://sky425509.iteye.com/blog/1994891http://sky425509.iteye.com/blog/1994891)
