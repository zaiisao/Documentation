
---
title: Generate a Token from Your Server
description: 
platform: Python
updatedAt: Mon Sep 23 2019 08:05:24 GMT+0800 (CST)
---
# Generate a Token from Your Server
This page provides Agora RTC SDK v2.1+, Agora Web SDK v2.4+, Agora Recording SDK v2.1+, and Agora RTSA SDK users with  a quick guide on generating a pseudo-token using the **RtcTokenBuilderSample** demos we provide, as well as token-generating API references in Python. 

## An introduction to Agora's token repository

Your token needs to be generated on your own server, hence you are required to first deploy a token generator on the server. In our [GitHub Repository](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey), we provide source codes and token generator demos in the following programming languages:

- CPP
- Java
- Python
- PHP
- Node.js
- Go
- Ruby

The **./\<language\>/src** folder of each language holds source codes for generating different types of dynamic keys and tokens. Note that both **AccessToken** and **SimpleTokenBuilder** can generate a token for the following SDKs:

- Agora RTC SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 
- Agora RTSA SDK

However, we recommend using **RtcTokenBuilder** instead of **AccessToken**.  **AccessToken** implements all the core algorithms for generating a token, while **RtcTokenBuilder** is a wrapper of **AccessToken** and provides much more simplified Interfaces. 

The **./\<language\>/sample** folder of each language holds token generator demos we create for demonstration purposes. **RtcTokenBuilderSample** is  a demo for generating a token for the Agora RTC SDK, Agora Web SDK, Agora Recording SDK or Agora RTSA SDK. You can customize it based on your real business needs. 

## Generate a token using **RtcTokenBuilderSample**

We take **RtcTokenBuilderSample.py** as an example:

1. Ensure that you have installed **Python**. If you are using macOS, try the following command:
    `brew install Python`
2. Synchronize the GitHub repository to your local drive.
3. Navigate to the **/python/sample/** folder and open **RtcTokenBuilderSample.py**. 
> Our demo provides sample-App ID, appCertificate, channelName, uid, and userAccount for demonstration purposes.
4. Replace the sample-App ID, appCertificate, and channelName with your own. For information about getting an App ID and an App certificate, see [Token Security](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#app-id).
    - If you use an int uid to join a channel, comment out the following code block:
```Python
    token = RtcTokenBuilder.buildTokenWithAccount(appID, appCertificate, channelName, userAccount, Role_Attendee, privilegeExpiredTs)
    print("Token with user account: {}".format(token))
```    
    - If you use a string userAccount to join a channel, comment out the following code block:
```Python
    token = RtcTokenBuilder.buildTokenWithUid(appID, appCertificate, channelName, uid, Role_Attendee, privilegeExpiredTs)
    print("Token with int uid: {}".format(token))
```
> Skip this step if you just want to take a quick look at how a token is generated.
5. Open your terminal and navigate to the local folder holding **RtcTokenBuilderSample.py**.
6. Run the following command:
    `Python RtcTokenBuilderSample.py`
  *Your token is printed in your terminal window.*


## API Reference

Source code:  [../python/src/RtcTokenBuilder.py](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/python/src/RtcTokenBuilder.py)

You can create your own token generator using the public methods that **RtcTokenBuilder.py** provides. Note that **RtcTokenBuilder.py** supports both int uid and string userAccount. Ensure that you choose the right method. 

### buildTokenWithUid



```Python
   def buildTokenWithUid(appId, appCertificate, channelName, uid, role, privilegeExpiredTs):
```

This method build a token with your int uid.

| **Parameter**    | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | The App ID issued to you by Agora. Apply for a new App ID from Agora Dashboard if it is missing from your kit. See [Get an App ID](https://docs.agora.io/en/Agora%20Platform/token/#app-id). |
| `appCertificate` | Certificate of the application that you registered in the Agora Dashboard. See [Get an App Certificate](https://docs.agora.io/en/Agora%20Platform/token/#app-certificate). |
| `channelName`    | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `uid`            | User ID. A 32-bit unsigned integer with a value ranging from 1 to (2<sup>32</sup>-1). optionalUid must be unique. |
| `role`           | <li> `Role_Publisher = 1`: (Recommended) A broadcaster (host) in a live-broadcast profile.<li>`Role_Subscriber = 2`: An audience in a live-broadcast profile. |
| `privilegeExpiredTs`      | Time represented by the number of seconds elapsed since 1/1/1970. If, for example, you want to access the Agora Service within 10 minutes after the token is generated, set expireTimestamp as the current timestamp + 600 (seconds). Set it as 0 if the privilege never expires. |


### buildTokenWithUserAccount



```Python
  def buildTokenWithAccount(appId, appCertificate, channelName, account, role, privilegeExpiredTs):
```

This method build a token with your string userAccount.

| **Parameter**    | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | The App ID issued to you by Agora. Apply for a new App ID from Agora Dashboard if it is missing from your kit. See [Get an App ID](https://docs.agora.io/en/Agora%20Platform/token/#app-id). |
| `appCertificate` | Certificate of the application that you registered in the Agora Dashboard. See [Get an App Certificate](https://docs.agora.io/en/Agora%20Platform/token/#app-certificate). |
| `channelName`    | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `userAccount`    | The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `role`           | <li> `Role_Publisher = 1`: (Recommended) A broadcaster (host) in a live-broadcast profile.<li>`Role_Subscriber = 2`: An audience in a live-broadcast profile.  |
| `privilegeExpiredTs`      | Time represented by the number of seconds elapsed since 1/1/1970. If, for example, you want to access the Agora Service within 10 minutes after the token is generated, set expireTimestamp as the current timestamp + 600 (seconds). Set it as 0 if the privilege never expires. |





