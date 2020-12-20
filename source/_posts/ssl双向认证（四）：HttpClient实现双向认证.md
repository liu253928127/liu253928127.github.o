---
title: ssl双向认证（四）：HttpClient实现双向认证
date: 2018-12-09 12:03:35
categories: 
- 网络
- java
tags:
- httpclient
- ssl
---

> `keystore` 和 `truststore`是 `JSSE` 中使用的两种文件。这两种文件都使用 `java` 的 `keytool` 来管理，他们的不同主要在于用途和相应用途决定的内容的不同。

#### `keystore` 和 `truststore`

这两种文件在一个SSL认证场景中，`keystore` 用于服务器认证客户端，而 `truststore` 用于客户端认证服务器。

比如在客户端（服务请求方）对服务器（服务提供方）发起一次 `HTTPS` 请求时，服务器需要向客户端提供认证以便客户端确认这个服务器是否可信。 这里，服务器向客户端提供的认证信息就是自身的证书和公钥，而这些信息，包括对应的私钥，服务器就是通过 `keystore` 来保存的。 当服务器提供的证书和公钥到了客户端，客户端就要生成一 个 `truststore` 文件保存这些来自服务器证书和公钥。

因为 `keystore` 文件既可以存储敏感信息，比如密码和私钥，也可以存储公开信息比如公钥，证书之类，所有实际上来讲，可以将 `keystore` 文件同样用做 `truststore` 文件。

#### `httpclient` 实现双向认证

由于公司给了密钥以及证书，所以直接可以用 `keytool`生成 `keystore`，由于服务器和客户端用的是一套证书，所以，生成的`keystore`既可以用来做`keystore`，也能做 `truststore`，从而实现双向认证的证书互相认证。上一篇介绍了如何生成 `jks` 文件，这里就不细说了，直接拿过来用。

maven import

```xml
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.5</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.4</version>
</dependency>
```

设置证书的方法

```java
/**
    * 设置证书
    *
    * @return
    * @throws Exception
    */
private SSLConnectionSocketFactory getSslConnectionSocketFactory() throws Exception {
    String client_key_pass = "changeit";

    // keystore
    KeyStore clientKeyStore = KeyStore.getInstance("JKS");
    InputStream clientKeyStoreInput = new FileInputStream(new File("C:/test/keystore.jks"));
    clientKeyStore.load(clientKeyStoreInput, client_key_pass.toCharArray());

    SSLContext sslContext = SSLContexts.custom()
            .loadTrustMaterial(clientKeyStore, new TrustSelfSignedStrategy())   // 设置truststore
            .loadKeyMaterial(clientKeyStore, client_key_pass.toCharArray())     // 设置keystore
            .setSecureRandom(new SecureRandom())
            .build();

    // Allow TLSv1 protocol only
    return new SSLConnectionSocketFactory(
            sslContext,
            new String[]{"TLSv1.2"},
            null,
            SSLConnectionSocketFactory.BROWSER_COMPATIBLE_HOSTNAME_VERIFIER);
}
```

实例化httpclient

```java
// 实例化httpClient
httpClient = HttpClients.custom()
        .setSSLSocketFactory(getSslConnectionSocketFactory())   // 设置SSLSocket
        .build();
```

通过以上的 `httpclient` 配置，即可实现 `httpclient`双向认证。

在访问有些 HTTPS 站点时，你还可能会遇到下面的异常：

> javax.net.ssl.SSLHandshakeException:

> sun.security.validator.ValidatorException: PKIX path building failed:

> sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target

> In order to use the self-signed client certificate which was issued by a non public CA the public server certificate must be imported into a truststore.

在之前有介绍道关于 `SSL`、`HTTPS` 的相关联性。我碰到的这个异常是因为只设置了 `keystore` 而没有设置 `truststore`。

之前在网上查到的资料是通过设置系统环境变量，将其添加到 `truststore`。

```
System.setProperty("javax.net.ssl.trustStore", "path/to/truststore.jks");
System.setProperty("javax.net.ssl.trustStorePassword", "changeit");
System.setProperty("javax.net.ssl.trustStoreType", "JKS");
```

然后发现并不管用，于是直接在初始化 `httpclient` 的时候将 `keystore`、`truststore` 设置进去：

```java
SSLContext sslContext = SSLContexts.custom()
            .loadTrustMaterial(clientKeyStore, new TrustSelfSignedStrategy())   // 设置truststore
            .loadKeyMaterial(clientKeyStore, client_key_pass.toCharArray())     // 设置keystore
            .setSecureRandom(new SecureRandom())
            .build();
```

