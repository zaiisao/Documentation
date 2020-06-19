
---
title: Set up Authentication
description: 
platform: All Platforms
updatedAt: Fri Jun 19 2020 04:57:29 GMT+0800 (CST)
---
# Set up Authentication
## Introduction

The Agora RTM SDK provides different security keys for authentication: 

- App ID
- RTM token

The following figure shows the environment where the security keys are used:

![](https://web-cdn.agora.io/docs-files/1555490456944)

Where:

-   An App ID is easily obtained and is used in environments with low-security requirements, such as in a testing environment.

-   An RTM token adds security and is used in environments with high-security requirements, such as in a production environment.

-   An App Certificate is enabled for the sole purpose of generating an RTM token and cannot be used alone. Once an App Certificate is enabled, you can only use the RTM token for authentication.

<a name = "Get-an-App-ID"></a>

## Get and Use an App ID

Each Agora account can create multiple projects, and each project has a unique App ID. Anyone with your App ID can use it on any Agora SDK. Hence, please keep your App IDs safe.

1.  Sign up for a new account at [https://sso.agora.io/en/login](https://sso.agora.io/en/login).
2.  Click the **Create** button on the **Overview** page in Console.
3.  Fill in the **Project Name** and click **Submit**.
4.  Find the App ID under the created project.

You may need an App ID in the following situations: 

-   Setting the `appId` parameter as the App ID when initializing the client.
-   You are required to use an RTM token when calling the `login` method to log in the Agora RTM system, but under low-security requirements, you can pass your App ID as `token`.


## Get and Use an RTM Token

You need an RTM token to log in the Agora RTM system. Take the following steps to get and use an RTM token. 

### Step 1: Get an App ID

See [Get an App ID](#Get-an-App-ID).

### Step 2: Get an App Certificate

1.  Log in [Agora Console](https://dashboard.agora.io).
2.  Click **Project** in the left navigation menu to go to the **Projects** page in Console.
3.  Enable the App Certificate for the project.
	-   Click **Edit** under the created project.
	-   Click **Enable** to the right of the App Certificate. Read **About App Certificate** before confirming the operation.
	-  Click ![](https://web-cdn.agora.io/docs-files/1551778086037) to view the App Certificate. You can re-click this icon to hide the App Certificate.

> -   An App Certificate is enabled for the sole purpose of generating a token and cannot be used alone.
> 
> -   Keep the App Certificate on the server, never on any client.
> 
> -   The App Certificate takes about an hour to take effect after it is enabled.
> 
> -   Once the App Certificate is enabled for a project, an RTM token must be used. 
> 
> -   You do not need to toggle the **Signaling Token Debugging Switch**, because it does not affect your RTM project.
> 

### Step 3: Deploy an RTM Token Generator 

Agora's token scheme is based on a request-response model. Whenever a client needs token-specific services, it sends a request to the app server before the server processes the request and sends a new or updated token back to the client. Therefore, you first must deploy an RTM token generator on your app server. See the following sample code for generating an RTM token:

-   [RTM Token Builder for C++](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/sample/RtmTokenBuilderSample.cpp)
-   [RTM Token Builder for Java](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/java/src/main/java/io/agora/sample/RtmTokenBuilderSample.java)
-   [RTM Token Builder for Python](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/python/sample/RtmTokenBuilderSample.py)
-   [RTM Token Builder for PHP](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/php/sample/RtmTokenBuilderSample.php )
-   [RTM Token Builder for Node.js](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/nodejs/sample/RtmTokenBuilderSample.js)


### Step 4: Send an RTM Token Request

Whenever you need an RTM token, send an RTM token request from your client to your app server.

To build an RTM token, you need the following parameters:

- `appID`: the unique App ID of your project. See <a href="#getting-an-app-id">Getting an App ID</a>.
- `userId`: the ID of the user logging in the Agora RTM system.

> Your App Certificate is kept on your app server, not on your client. 

To set the RTM privilege, you need the following parameters:

- `privilege`: all Agora RTM clients have only one role. Set the value as 1000.
- `expireTimeStamp`: set the value as 0. This feature is still under development. 

When the app server processes the request, it sends a new RTM token to the client.

> You must use the RTM within 24 hours after it is generated. Otherwise, you need to regenerate the RTM token. 








