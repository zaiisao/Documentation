
---
title: Generate a Token from Your Server
description: 
platform: Java
updatedAt: Tue Aug 13 2019 03:01:00 GMT+0800 (CST)
---
# Generate a Token from Your Server
This page provides Agora RTC SDK v2.1+, Agora Web SDK v2.4+, Agora Recording SDK v2.1+, and Agora RTSA SDK users with  a quick guide on generating a sample token using the **RtcTokenBuilderSample** demos we provide, as well as token-generating API references in Java. 

~35ce7bc0-a9da-11e9-9e5e-256c0a74561a~


## API Reference

Source code:  [../java/src/io/agora/media/RtcTokenBuilder.java](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/java/src/io/agora/media/RtcTokenBuilder.java)

You can create your own token generator using the public methods that **RtcTokenBuilder.h** provides. Note that **RtcTokenBuilder.h** supports both int uid and string userAccount. Ensure that you choose the right method. 

### buildTokenWithUid



```Java
   public String buildTokenWithUid(String appId, String appCertificate, 
    		String channelName, int uid, Role role, int privilegeTs)
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



```Java
  public String buildTokenWithUserAccount(String appId, String appCertificate, 
    		String channelName, String account, Role role, int privilegeTs) 
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






