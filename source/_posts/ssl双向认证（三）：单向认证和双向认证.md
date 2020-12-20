---
title: ssl双向认证（三）：单向认证和双向认证
date: 2018-12-04 21:55:56
categories: 
- 网络
- java
tags:
- httpclient
- ssl
---
> SSL认证是指客户端到服务器端的认证。主要用来提供对用户和服务器的认证；对传送的数据进行加密和隐藏；确保数据在传送中不被改变，即数据的完整性，现已成为该领域中全球化的标准。

#### `HTTPS` 单向认证

```sequence
Client -> Server: ①Client Hello(随机数，支持的加密算法)
Server --> Client: ②Server Hello(随机数，选择一个client支持的算法)
Server --> Client: ②Server证书
note left of Client: ③验证证书
Client -> Server: ④Client key Exchange(用Server公钥加密)
Client -> Server: ④Change cipher Spec(通知Server参数协商完成)
Client -> Server: ④Finish handshake(Encrypted handshake message)
Server --> Client: ⑤cipher spec + Finish handshake
note left of Client: 开始传输
Client -> Server: ⑥开始使用协商密钥与算法进行加密通信

```

流程：

①	**`Client Hello`**:  **客户端**的浏览器向**服务器**传送客户端 `SSL` 协议的版本号，加密算法的种类，产生的随机数，以及其他服务器和客户端之间通讯所需要的各种讯息。

②	**`Server Hello`**:  **服务器**向**客户端**传送 `SSL` 协议的版本号，客户端支持的加密算法，随机数以及其他相关信息，同时服务器还将向客户端传送自己的证书。

③	**`验证证书`**:  **客户端**利用服务器传过来的信息验证服务器的合法性，服务器的合法性包括：证书是否过期，发行服务器证书的CA是否可靠，发行者证书的公钥能否正确解开服务器证书的“发行者的数字签名”，服务器证书上的域名是否和服务器的实际域名相匹配。如果合法性验证没有通过， 通讯将断开；如果合法性验证通过，将继续进行第四步。

④	**`Client key Exchange`**:  合法性校验通过之后，**客户端**计算产生随机数字`Pre-master`，并用证书公钥加密，发送给服务器。

​	**`Change cipher Spec`**:  **客户端**通知服务器后续的通信都采用协商的通信密钥和加密算法进行加密通信。

​	**`Encrypted handshake message`**:  **客户端**结合之前所有通信参数的hash值与其他相关信息生成一段数据，采用协商session secret 与算法进行加密，然后发送给服务器用于数据与握手验证。

⑤	**`cipher spec + Finish handshake`**:  验证通过之后，服务器接收到浏览器送过来的消息，用自己的私钥解密，获得通话密钥。**服务器**同样发送 `change_cipher_spec` 以告知客户端后续的通信都采用协商的密钥与算法进行加密通信。

⑥	开始使用协商密钥与算法进行加密通信。

#### `HTTPS` 双向认证

```sequence
Client -> Server: ①Client Hello(随机数，支持的加密算法)
Server --> Client: ②Server Hello(随机数，选择一个client支持的算法)
Server --> Client: ②Server证书
note left of Client: ③验证证书
Client -> Server: ④Client key Exchange(用Server公钥加密)
Client -> Server: ④Change cipher Spec(通知Server参数协商完成)
Client -> Server: ④Finish handshake(Encrypted handshake message)
Client -> Server: ④Client证书
note right of Server: ⑤验证证书
Server --> Client: ⑥Change cipher Spec(通知Server参数协商完成)
Server --> Client: ⑥Finish handshake(Encrypted handshake message)

note left of Client: 开始传输
Client -> Server: ⑦开始使用协商密钥与算法进行加密通信
```

流程：

①	**`Client Hello`**:  **客户端**的浏览器向**服务器**传送客户端 `SSL` 协议的版本号，加密算法的种类，产生的随机数，以及其他服务器和客户端之间通讯所需要的各种讯息。

②	**`Server Hello`**:  **服务器**向**客户端**传送 `SSL` 协议的版本号，客户端支持的加密算法，随机数以及其他相关信息，同时服务器还将向客户端传送自己的证书。

③	**`验证证书`**:  **客户端**利用服务器传过来的信息验证服务器的合法性，服务器的合法性包括：证书是否过期，发行服务器证书的CA是否可靠，发行者证书的公钥能否正确解开服务器证书的“发行者的数字签名”，服务器证书上的域名是否和服务器的实际域名相匹配。如果合法性验证没有通过， 通讯将断开；如果合法性验证通过，将继续进行第四步。

④	**`Client key Exchange`**:  合法性校验通过之后，**客户端**计算产生随机数字`Pre-master`，并用证书公钥加密，发送给服务器。

​	**`Change cipher Spec`**:  **客户端**通知服务器后续的通信都采用协商的通信密钥和加密算法进行加密通信。

​	**`Encrypted handshake message`**:  **客户端**结合之前所有通信参数的hash值与其他相关信息生成一段数据，采用协商session secret 与算法进行加密，然后发送给服务器用于数据与握手验证。

​	**`Client证书`**:  **服务器**要求客户发送客户自己的证书，客户端发送自己证书。

⑤	**`验证证书`**:  **服务器**验证客户的证书，如果没有通过验证，拒绝连接；如果通过验证，服务器获得用户的公钥。 服务器接收到浏览器送过来的消息，用自己的私钥解密，获得通话密钥。

⑥	**`Change cipher Spec`**:  **服务器**同样发送 `change_cipher_spec` 以告知客户端后续的通信都采用协商的密钥与算法进行加密通信。

⑦	开始使用协商密钥与算法进行加密通信。



参考：

- [ssl双向认证和单向认证原理](http://edison0663.iteye.com/blog/996526)

- [SSL/TLS 双向认证(一) -- SSL/TLS工作原理](https://blog.csdn.net/ustccw/article/details/76691248)