
---
title: 如何进行 HTTP 基本认证？
description: 
platform: All Platforms
updatedAt: Fri May 22 2020 11:58:20 GMT+0800 (CST)
---
# 如何进行 HTTP 基本认证？
## 功能描述

在使用 RESTful API 前，你需要在 HTTP 请求头部中填入 `Authorization` 字段，进行 HTTP 基本认证 。本文介绍如何使用 Agora 提供的 Customer ID 和 Customer Certificate 生成一个 Authorization 字段。该 Authorization 字段使用 Base64 算法编码生成。

## 操作步骤

1. 获取客户 ID（Customer ID）和客户证书（Customer Certificate）。登录 [Console](https://console.agora.io/)，点击右上角账户名，进入下拉菜单 RESTful API 页面，即可获取 Customer ID 和 Customer Certificate。
 
 ![](https://web-cdn.agora.io/docs-files/1571022863083)

 <div class="alert note">客户 ID和客户证书仅用于访问 RESTful API。</div>
 
2. 在请求认证的代码中，填入 Customer ID 和 Customer Certificate，并获取 Authorization 字段。请参考下列代码。

   - Java

     ```java
     // 填入 Customer ID 和 Customer Certificate，计算 plainCredentials。
     String plainCredentials = "customerId:customerCertificate";
     // 填入 plainCredentials（使用 Base64 算法编码的 Credential），计算 base64Credentials，即你要的 Authorization 字段。
     String base64Credentials = new String(Base64.encodeBase64(plainCredentials.getBytes()));
```

   - Swift

     ```swift
     // 填入 Customer ID 和 Customer Certificate，计算 loginString。
     let username = "customerId"
     let password = "customerCertificate"
     let loginString = String(format: "%@:%@", username, password)
     // 填入 loginString，计算 loginData。
     let loginData = loginString.data(using: String.Encoding.utf8)!
     // 填入 loginData（使用 Base64 算法编码的 LoginData），计算 base64LoginString，即你要的 Authorization 字段。
     let base64LoginString = loginData.base64EncodedString()
```

3. 进行 HTTP 认证。请参考下列代码。

  发送 HTTP 请求时，Authorization 字段的格式需为：`Basic <Authorization>`，**Basic 不能省**。

   - Java

     ```java
     Request request = new Request.Builder()
     ...
         // 在 HTTP 请求头部填入获取到的 Authorization 字段。
         .addHeader("Authorization", "Basic " + base64Credentials)
     ...
```

   - Swift

     ```swift
     // 在 HTTP 请求头部填入获取到的 Authorization 字段。
     let headers = ["Authorization", "Basic " + base64LoginString]
```

     
