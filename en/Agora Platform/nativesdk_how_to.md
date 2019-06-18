
---
title: Native SDK-related Issues
description: 
platform: Native SDK-related Issues
updatedAt: Tue Jun 18 2019 10:01:56 GMT+0800 (CST)
---
# Native SDK-related Issues
## RESTful API authentication

Before using the Agora RESTful API, you need to fill in the `Authorization` parameter in the HTTP request header for authentication. Refer to the following code samples to get the `Authorization` parameter.

```java
// Java
// Fill in your Customer ID and Customer Certificate.
String plainCredentials = "customerId: customerCertificate";
// The base64Credentials here is the Authotization parameter you want.
String base64Credentials = new String(Base64.encodeBase64(plainCredentials.getBytes()));
```

```swift
// Swift
let username = "customerId"
let password = "customerCertificate"
let loginString = String(format: "%@:%@", username, password)
let loginData = loginString.data(using: String.Encoding.utf8)!
// The base64LoginString here is the Authorization parameter you want.
let base64LoginString = loginData.base64EncodedString()
```
