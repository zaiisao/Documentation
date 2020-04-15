
---
title: How can I pass the basic HTTP authentication?
description: How to pass the HTTP authentication?
platform: All Platforms
updatedAt: Wed Mar 25 2020 22:50:33 GMT+0800 (CST)
---
# How can I pass the basic HTTP authentication?
## Introduction

Before using the Agora RESTful API, you need to type the `Authorization` parameter in the HTTP request header for authentication. This page shows you how to generate the `Authorization` parameter with the Customer ID and Customer Certificate provided by Agora. 

## Implementation

1. Get the Customer ID and Customer Certificate: log in to [Agora Console](https://console.agora.io/), click the account name on the top right of Agora Console, and enter the RESTful API page from the drop-down list to get the Customer ID and Customer Certificate.
   
	 ![](https://web-cdn.agora.io/docs-files/1571023473774)

   <div class="alert note">The Customer ID and Customer Certificate are used for RESTful API access only.</div>

2. Get the `Authorization` parameter: type the Customer ID and Customer Certificate in the following sample code to obtain the `Authorization` parameter.

   - Java

     ```java
     // Fill in the Customer ID and Customer Certificate to obtain the plainCredentials value.
     String plainCredentials = "customerId:customerCertificate";
     // Fill in the plainCredentials (the base64 encoding of the credential) to obtain the base64Credentials value which is the Authorization parameter you want.
     String base64Credentials = new String(Base64.encodeBase64(plainCredentials.getBytes()));
```

   - Swift

     ```swift
     // Fill in the Customer ID and Customer Certificate to obtain the loginString value.
     let username = "customerId"
     let password = "customerCertificate"
     let loginString = String(format: "%@:%@", username, password)
     // Fill in the loginString to obtain the loginData value.
     let loginData = loginString.data(using: String.Encoding.utf8)!
     // Fill in the loginData (the base64 encoding of the loginData) to obtain the base64LoginString value which is the Authorization parameter you want.
     let base64LoginString = loginData.base64EncodedString()
```

3. Pass the basic HTTP authentication: fill in the `Authorization` parameter in the following sample code to pass the authentication.

   <div class="alert note">When sending the HTTP request, ensure that the format of Authorization is Basic base64Credentials or Basic base64LoginString.</div>

   - Java

     ```java
     Request request = new Request.Builder()
     ...
         // Pass in the Authorization parameter in the HTTP request header.
         .addHeader("Authorization", "Basic base64Credentials")
     ...
```

   - Swift

     ```swift
     // Pass in the Authorization parameter in the HTTP request header.
     let headers = ["Authorization", "Basic base64LoginString"]
```
