
---
title: HTTP 基本认证
description: 
platform: All Platforms
updatedAt: Fri Aug 16 2019 12:16:11 GMT+0800 (CST)
---
# HTTP 基本认证
在使用 RESTful API 前，你需要在 HTTP 请求头部中填入 `Authorization` 字段，进行认证。本文提供 Java 和 Swift 语言生成 Authorization 字段的示例代码，请参考代码获取相应的 `Authorization` 值。

你需要在代码中填入 Customer ID 和  Customer Certificate。登录 [Dashboard](https://dashboard.agora.io)，点击右上角账户名，进入下拉菜单 RESTful API 页面，即可获取 Customer ID 和 Customer Certificate。

> Customer ID 和 Customer Certificate 仅用于访问 Restful API。

```java
// Java
// 填入你获取到的 Customer ID 和 Customer Certificate
String plainCredentials = "customerId:customerCertificate";
// 这里的 base64Credentials 就是你要的 Authorization 值
String base64Credentials = new String(Base64.encodeBase64(plainCredentials.getBytes()));
```

```swift
// Swift
let username = "customerId"
let password = "customerCertificate"
let loginString = String(format: "%@:%@", username, password)
let loginData = loginString.data(using: String.Encoding.utf8)!
// 这里的 base64LoginString 就是你要的 Authorization 值
let base64LoginString = loginData.base64EncodedString()
```
