
---
title: Generate a Token from Your Server
description: 
platform: Go
updatedAt: Thu Jul 02 2020 10:58:19 GMT+0800 (CST)
---
# Generate a Token from Your Server
This page provides Agora RTC SDK v2.1+, Agora Web SDK v2.4+, Agora Recording SDK v2.1+, and Agora RTSA SDK users with  a quick guide on generating a sample token using the **RtcTokenBuilderSample** demos we provide, as well as token-generating API references in Go. 

## An introduction to Agora's token repository

Your token needs to be generated on your own server, hence you are required to first deploy a token generator on the server. In our [GitHub Repository](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey), we provide source codes and token generator demos in the following programming languages:

- CPP
- Java
- Python
- PHP
- Node.js
- Go
- Ruby

The <b>./\<language\>/src</b> folder of each language holds source codes for generating different types of dynamic keys and tokens. Note that both **AccessToken** and **SimpleTokenBuilder** can generate a token for the following SDKs:

- Agora RTC SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 
- Agora RTSA SDK

However, we recommend using **RtcTokenBuilder** instead of **AccessToken**.  **AccessToken** implements all the core algorithms for generating a token, while **RtcTokenBuilder** is a wrapper of **AccessToken** and provides much more simplified Interfaces. 

The **./\<language\>/sample** folder of each language holds token generator demos we create for demonstration purposes. **RtcTokenBuilderSample** is  a demo for generating a token for the Agora RTC SDK, Agora Web SDK, Agora Recording SDK or Agora RTSA SDK. You can customize it based on your real business needs. 

## Generate a token using **RtcTokenBuilderSample**

<div class="alert note">To quickly test generating a token on the server, you can follow the steps in <a href="https://www.agora.io/en/blog/2-click-setup-testing-token-server/">Click Setup: Testing Token Server</a>.</div>

We take **RtcTokenBuilderSample.go** as an example:

1. Log onto the Golang official site to download the latest Stable version of Golang.
2. Synchronize the GitHub repository to your local drive.
3. Navigate to the **/go/sample/RtcTokenBuilder/** folder and open **sample.go**. 
> Our demo provides sample-App ID, appCertificate, channelName, uid, and userAccount for demonstration purposes.
4. Replace the sample-App ID, appCertificate, uid, and channelName with your own. For information about getting an App ID and an App certificate, see [Token Security](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#app-id).
    - If you use an int uid to join a channel, comment out the following code block:
```Go
	result, err = rtctokenbuilder.BuildTokenWithUserAccount(appID, appCertificate, channelName, uidStr, rtctokenbuilder.RoleAttendee, expireTimestamp)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("Token with userAccount: %s\n", result)
	}
```    
    - If you use a string userAccount to join a channel, comment out the following code block:
```Go
	result, err := rtctokenbuilder.BuildTokenWithUID(appID, appCertificate, channelName, uid, rtctokenbuilder.RoleAttendee, expireTimestamp)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("Token with uid: %s\n", result)
	}
```
> Skip this step if you just want to take a quick look at how a token is generated.
5. Open your terminal and navigate to the local folder holding **sample.go**.
6. Run the following command:
    `go build`
  *Your token is printed in your terminal window.*



## API Reference

Source code:  [../go/src/RtcTokenBuilder/RtcTokenBuilder.go](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/go/src/RtcTokenBuilder/RtcTokenBuilder.go)

You can create your own token generator using the public methods that **RtcTokenBuilder.go** provides. Note that **RtcTokenBuilder.go** supports both int uid and string userAccount. Ensure that you choose the right method. 

### buildTokenWithUid



```Go
   func BuildTokenWithUID(appID string, appCertificate string, channelName string, uid uint32, role Role, privilegeExpiredTs uint32) (string, error)
```

This method builds a token with your int uid.

| **Parameter**    | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | The App ID issued to you by Agora. Apply for a new App ID from Agora Console if it is missing from your kit. See [Get an App ID](https://docs.agora.io/en/Agora%20Platform/token/#app-id). |
| `appCertificate` | Certificate of the application that you registered in the Agora Console. See [Get an App Certificate](https://docs.agora.io/en/Agora%20Platform/token/#app-certificate). |
| `channelName`    | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `uid`            | User ID. A 32-bit unsigned integer with a value ranging from 1 to (2<sup>32</sup>-1). Must be unique. |
| `role`          | The user role. Both roles enjoy the same privilege. <li> `Role_Publisher = 1`: (Recommended) A broadcaster.<li>`Role_Subscriber = 2`: An audience. |
| `privilegeExpiredTs`      | Expiration time of the token. A Unix timestamp, represented by the number of seconds elapsed since 1/1/1970. For example, if you set this parameter as the current timestamp + 600 (seconds), the token expires 10 minutes after it is generated. A token is valid for 24 hours at most. If you set this parameter as 0 or a period longer than 24 hours, the token is valid for 24 hours. |


### buildTokenWithUserAccount



```Go
  func BuildTokenWithUserAccount(appID string, appCertificate string, channelName string, userAccount string, role Role, privilegeExpiredTs uint32) (string, error)
```

This method builds a token with your string userAccount.

| **Parameter**    | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | The App ID issued to you by Agora. Apply for a new App ID from Agora Console if it is missing from your kit. See [Get an App ID](https://docs.agora.io/en/Agora%20Platform/token/#app-id). |
| `appCertificate` | Certificate of the application that you registered in the Agora Console. See [Get an App Certificate](https://docs.agora.io/en/Agora%20Platform/token/#app-certificate). |
| `channelName`    | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `userAccount`    | The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `role`          | The user role. Both roles enjoy the same privilege. <li> `Role_Publisher = 1`: (Recommended) A broadcaster.<li>`Role_Subscriber = 2`: An audience. |
| `privilegeExpiredTs`      | Expiration time of the token. A Unix timestamp, represented by the number of seconds elapsed since 1/1/1970. For example, if you set this parameter as the current timestamp + 600 (seconds), the token expires 10 minutes after it is generated. A token is valid for 24 hours at most. If you set this parameter as 0 or a period longer than 24 hours, the token is valid for 24 hours. |

## Reference
	
When generating a token, you can also refer to the following articles:

- [Troubleshooting token-related errors for Agora RTC SDK](https://docs.agora.io/en/faq/token_error)
- [Troubleshooting 101 error for Cloud Recording](https://docs.agora.io/en/faq/101_error)
