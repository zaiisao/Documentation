
---
title: Generate a Token from Your Server
description: Guide on how to generate tokens on the server side
platform: Server
updatedAt: Fri Jul 19 2019 09:27:17 GMT+0800 (CST)
---
# Generate a Token from Your Server
This page provides Agora RTC SDK v2.1+, Agora Web SDK v2.4+, and Agora Recording SDK v2.1+ users with  a quick guide on generating a pseudo-token using the **sample_builder** demos we provide, as well as token-generating API references in C++. 

## An introduction to Agora's token repository

Your token needs to be generated on your own server, hence you are required to first deploy a token generator on the server. In our [GitHub Repository](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey), we provide source codes and token generator demos in the following programming languages:

- CPP
- Java
- Python
- Ruby
- Node.js
- Go

The **./\<language\>/src** folder of each language holds source codes for generating different types of dynamic keys and tokens. Note that both AccessToken and SimpleTokenBuilder can generate a token for the following SDKs:

- Agora RTC SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 

However, we recommend using **SimpleTokenBuilder** instead of **AccessToken**.  **AccessToken** implements all the core algorithms for generating a token, whilst **SimpleTokenBuilder** is a wrapper of **AccessToken** and provides much more simplified Interfaces. 

The **./\<language\>/sample** folder of each language holds token generator demos we create for demonstration purposes.  Built upon **SimpleTokenBuilder**, **Sample_builder** is  a demo for generating a token for the Agora RTC SDK, Agora Web SDK, or Agora Recording SDK. You can customize it based on your real business needs. 

## Generate a token using **sample_builder**

We take **sample_builder.cpp** as an example:

1. Ensure that you have installed **openssl**. If you are using macOS, try the following command:
    `brew install openssl`
2. Synchronize the GitHub repository to your local drive.
3. Navigate to the **/cpp/sample/** folder and open **sample_builder.cpp**. 
> Our demo provides pseudo-App ID, appCertificate, channelName, uid, and userAccount for demonstration purposes.
4. Replace the pseudo-App ID, appCertificate, and channelName with your own. For information about getting an App ID and an App certificate, see [Token Security](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#app-id).
    - If you use an int uid to join a channel, comment out the following code block:
```C++
  result = SimpleTokenBuilder::buildTokenWithUserAccount(
      appID, appCertificate, channelName, userAccount, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With UserAccount:" << result << std::endl;
```    
    - If you use a string userAccount to join a channel, comment out the following code block:
```C++
  result = SimpleTokenBuilder::buildTokenWithUid(
      appID, appCertificate, channelName, uid, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With Int Uid:" << result << std::endl;
```
> Skip this step if you just want to take a quick look at how a token is generated.
5. Open your terminal and navigate to the local folder holding **sample_builder.cpp**.
6. Run the following command:
    `g++ -std=c++0x -O0 -I../../ sample_builder.cpp -lz -lcrypto -o sample_builder`
		*An executable file <b>sample_builder</b> appears in the folder.*
7. In your terminal, run `./sample_builder` to generate a Token. 
    *Your token is printed in your terminal window.*
		
> Ensure that you have correctly set the environment variant of **openssl**. Suppose you are using macOS and if `fatal error: 'openssl/hmac.h' file not found` appears, try the following command to debug:
> 1. Run the `which openssl` command in your terminal
>     *The terminal prints: `/usr/bin/openssl`*
>  2. `cd /usr/local/include`
>  3. `ln -s ../opt/openssl/include/openssl .`   
