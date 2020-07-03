
---
title: Set up Authentication
description: 
platform: All Platforms
updatedAt: Thu Jul 02 2020 06:03:57 GMT+0800 (CST)
---
# Set up Authentication
## Introduction

The Agora RTM SDK provides two authentication methods:

- App ID
- RTM token

The following figure shows scenarios for using security keys:

![](https://web-cdn.agora.io/docs-files/1555490456944)

<a name = "Get-an-App-ID"></a>

## Use an App ID for authentication

After signing up for a developer account in [Agora Console](https://console.agora.io/?_ga=2.18229183.782459552.1593311578-73063204.1585890674), you can create multiple projects. Each project has an App ID, which is the unique identity of the project. If others steal your App ID, they can use it in their own projects. Therefore, using an App ID for authentication is less secure. Agora recommends using an App ID for authentication only in a test environment, or if your project has low security requirements.

1.  Sign up for a new account at [https://sso.agora.io/en/login](https://sso.agora.io/en/login).
2.  Click **Create** on the **Overview** page in Console.
3.  Enter the project name in **Project Name** and click **Submit**.
4.  Find the App ID under the created project.

You must set the `appId` parameter as the App ID when initializing the client.

## Use an RTM token for authentication

Complete the following steps to get and use an RTM token. 

### Step 1: Get an App ID

See [Get an App ID](#Get-an-App-ID).

### Step 2: Get an App Certificate

1.  Log in [Agora Console](https://dashboard.agora.io).
2.  Click **Overview** in the left navigation menu to go to the Projects page in Console.
3.  Enable the App Certificate for the project.
	-  Click ![](https://web-cdn.agora.io/docs-files/1593666964813) to the right of the created project.
	-  Click **Enable** to the right of the App Certificate. Read **About App Certificate** before confirming the operation.
	-  Click ![](https://web-cdn.agora.io/docs-files/1551778086037) to view the App Certificate. You can re-click this icon to hide the App Certificate.

<div class="alert note"><ul><li>The App Certificate takes about an hour to take effect after it is enabled. Once the App Certificate is enabled for a project, you can use only a token for authentication.</li><li>You do not have to set <b>Signaling token debugging switch</b> because it does not affect RTM projects.</li></ul></div>

### Step 3: Create an RTM token generator 

Agora provides an open source [Agora Dynamic Key](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) repository on GitHub. The `./<language>/src` folder of each language holds source codes for generating different types of dynamic keys and tokens. You can use `RtmTokenBuilder` to generate an RTM Token. The `./<language>/sample` folder of each language holds token generator examples that Agora creates for demonstration purposes. `RtmTokenBuilderSample` is a demo for generating an RTM token.

<div class="alert note">The token encoding uses the standard HMAC/SHA1 approach, and the libraries are available on common server-side development platforms, such as Node.js, Java, PHP, Python, and C++. For more information, see <a href="http://en.wikipedia.org/wiki/Hash-based_message_authentication_code">Authentication code</a>.</div>

`RtmTokenBuilder` parameter description (C++)

```c++
static std::string buildToken(const std::string& appId,
                                const std::string& appCertificate,
                                const std::string& userAccount,
                                RtmUserRole userRole,
                                uint32_t privilegeExpiredTs = 0);
```

| Parameter             | Description                                                       |
| :----------------- | :----------------------------------------------------------- |
| appId              | The App ID of your project.|
| appCertificate     | The App Certificate of your project.                                          |
| userAccount        | The user ID of the RTM system.                                   |
| userRole           | The user role. Agora supports only one user role. Set the value as the default value  `Rtm_User`. |
| privilegeExpiredTs | This parameter is currently invalid. You can ignore this parameter. |

<div class="alert note">An RTM token is valid for 24 hours.</div>

### Step 4: Deploy an RTM token generator to production

Complete the following steps to deploy an RTM token generator to production:
1. Implement an RTM generator on the server.
2. The client sends a request to get an RTM token from the server whenever a token is required.
3. The server receives the request, generates an RTM token, and sends the token to the client.
4. The client calls <a href="https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2c33be67bfec02d69041f1e8978f4559">`renewToken`</a> to update the RTM token.








