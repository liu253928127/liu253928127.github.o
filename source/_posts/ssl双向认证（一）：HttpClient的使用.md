---
title: ssl双向认证（一）：HttpClient的使用
date: 2018-11-27 20:52:31
categories: 
- 网络
- java
tags:
- httpclient
- ssl
---

> HttpClient 是 Apache Jakarta Common 下的子项目，用来提供高效的、最新的、功能丰富的支持 HTTP 协议的客户端编程工具包。它相比传统的 HttpURLConnection，增加了易用性和灵活性，它不仅让客户端发送 HTTP 请求变得更容易，而且也方便了开发人员测试接口（基于 HTTP 协议的），即提高了开发的效率，也方便提高代码的健壮性。

这里对`HttpClient`做简单的使用，目的是利用它来解决双向认证的问题。

### 使用 HttpClient 发送 HTTP 请求

#### GET请求&POST请求

`HttpClient`对每一种`Http`方法都准备了一个类，`Get`请求使用`HttpGet`类，`Post`请求使用`HttpPost`类

> 模拟Http Get请求

```java
// httpclient 4.3版本后使用HttpClients来实例化
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet request = new HttpGet(url);
CloseableHttpResponse response = httpclient.execute(request);
 
// read response
```

> 模拟Http Post请求

```java
String parameter = "key=value";
HttpPost request = new HttpPost(url);
request.setEntity(
    new StringEntity(parameter, ContentType.create("application/x-www-form-urlencoded"))
);
```

> 读取响应

`HttpClient`的输入是一个`Entity`，输出也是一个`Entity`。对于`Entity`，`HttpClient`提供给我们一个工具类`EntityUtils`，使用它可以很方便的将其转换为字符串。

```java
CloseableHttpResponse response = httpclient.execute(request);
String responseBody = EntityUtils.toString(response.getEntity());
```
