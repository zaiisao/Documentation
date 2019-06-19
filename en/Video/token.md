
---
title: Use Security Keys
description: 
platform: All Platforms
updatedAt: Wed Jun 19 2019 02:53:48 GMT+0800 (CST)
---
# Use Security Keys
Agora knows that security is a vital consideration when you integrate real time communications into your application. To help you build an application that meets your security requirements, the Agora SDK provides two security mechanisms:

* For low security requirements, use an App ID for authentication.
* For high security requirements, use a dynamic key for authentication (recommended).

This page introduces Agora's two authentication mechanisms in details.


## Scope of application
We have two types of dynamic keys: Channel Key and Token. Different versions of our SDK use different dynamic keys for authentication. This page mainly deals with the Token. So before you start, see the following table to check which type of dynamic key that your SDK version supports:

| Agora SDK | Versions supporting Token | Versions supporting Channel Key | How to check SDK version |
| --------- | -------------------- | ------------------------- | ------------------------ |
| Native SDK   | 2.1.0 or later               | Earlier than 2.1.0        | `getSdkVersion`          |
| Web SDK      | 2.4.0 or later              | Earlier than 2.4.0        | `AgoraRtc.VERSION`       |
| Gaming SDK   | 2.2.0 or later               | Earlier than 2.2.0        | `getSdkVersion`          |

>-   If you use an Agora SDK that supports the Channel Key, see [Channel Keys](../../en/null/channel_key.md).
>-   If your wish to upgrade your SDK to a version that supports Token, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).
>-   For the Agora Signaling SDK, see [Signaling Security Keys](../../en/Agora%20Platform/key_signaling.md).
>-   For the Agora RTM SDK, see [Use Security Keys](https://docs.agora.io/en/Video/%3Chttps://docs-preview.agoralab.co/en/Real-time-Messaging/RTM_key?platform=All%20Platforms%3E).

## Prerequisites

Ensure that you have signed up for a developer account at the [Agora Dashboard](https://dashboard.agora.io/) and follow the on-screen instructions to create your first project.

## Use an App ID only for authentication

Each project you create at the [Agora Dashboard](http://dashboard.agora.io) has a unique App ID.

### Get an App ID

To get an App ID, follow these steps:

1. Click ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to go to the **Project Management** page.
2. Find the **App ID** that corresponds to your project.


### Apply your App ID

When initializing the client, set the `appId` parameter as the App ID you get to authenticate your application.

>  When joining a channel, set the `token` parameter as NULL.

## Use a token for authentication

The Token is a securer and more sophisticated authentication mechanism than the App ID.  You need to use an App ID and an App Certificate to generate a token for authentication. 

### Enable the App Certificate

For your first Agora project, take the following steps to enable the App Certificate:

1. Find your project on the **Project Management** page at the [Agora Dashboard](https://dashboard.agora.io/) and click the **Edit** button.

2. On the **Edit Project** page, click **Enable** to switch on the App Certificate and click **Save** to confirm your setting. 

3. Agora sends your account a confirmation Email. Follow the instruction to enable the App Certificate. 

>Your App Certificate appears enabled on the **Project Management** page. 

### Get a temporary token

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/). Use either of the following ways to generate a temporary token:

- On the **Project Details** page, click **Generate a Temp Token**, enter a channel name, and you will get a temporary token on the **Token** page. 
- When creating a project, choose **APP ID + APP certificate + Token (recommended)** to have the Dashboard enable the App Certificate for you, and click **Generate a Temp Token** to get your temporary token. 

### Get a token

When building the final production version of your application, you should generate a token on your server.

#### 1. Deploy a token generator on your server

First, use one of the Agora [sample codes](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) (C++, Go, Java, Node.js, Python, PHP, and Perl) to deploy a token generator on your server.

Or you can write your own code in a programming language that is not mentioned above to deploy a token generator. 

If you implement a token generator in a different language, you can file propose a pull request on [GitHub](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey). We will merge any implementation that proves valid.

#### 2. Generate a token

The process of generating a token is as follows: 

1.  The client sends a request for a token to your server.
2.  The server uses the token generator you deploy to create a token and sends it back to the client.

The application client needs to send the following parameters to the server to generate a token:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
	<tr><td><code>appID</code> <sup>[1]</sup></td>
<td>The App ID of the user’s project in the Agora Dashboard.</td>
</tr>
	<tr><td><code>appCertificate</code></td>
<td>The App Certificate of the user’s project in the Agora Dashboard.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></a></td>
<td>The privilege expiration time. The default value is 0, where the token never expires. A user can join a channel indefinitely within the designated expiration time and will be removed from the channel after the expiration time.</td>
</tr>
</tbody>
</table>

>[1] Agora does not support signing Token with a non-zero string uid for the time being.
>[2] `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. If, for example, you want to access the Agora Service within 10 minutes after the token is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The expiration time for each token is independent, and you can set it through the `setPrivilege` method.


For the methods and parameters involved, see [Generat a Token on Your Server](../../en/null/token_server.md).

### Apply your token or temporary token

When calling the `join` method to join a channel, you pass your token (or temporary token).

> - Ensure that the channel ID and user name you use to join a channel are the same as the channel ID and user name you use to create a token (or a temporary token).
> - After a token (or a temporary token) is generated, the client should use the token to join a channel within 24 hours. Otherwise, you need to generate a new token (or temporary token).
> - A token (or a temporary token) expires after a certain period of time. When the SDK notifies the client that the token is about to expire or has expired by the `onTokenPrivilegeWillExpire` or `onTokenExpired` callbacks, you need to generate a new token and call the `renewToken` method.
> - The token encoding uses the standard HMAC/SHA1 approach and the libraries are available on common server-side development platforms, such as Node.js, Java, PHP, Python, and C++. For more information, see  [Authentication code](http://en.wikipedia.org/wiki/Hash-based\_message\_authentication\_code).

## References

The following table lists the API methods that require a token as a parameter:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Platform</th>
<th>Join a Channel</th>
<th>Renew the Token</th>
</tr>
</thead>
<tbody>
<tr><td>Android</td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>iOS/macOS</td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:"><span>Join a Channel (joinChannelByToken)</span></a></td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>Windows</td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739"><span>Renew the Token (renewtoken)</span></a></td>
</tr>
<tr><td>Web</td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#join"><span>Join an AgoraRTC Channel (join)</span></a></td>
<td><a href="https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#renewtoken"><span>Renew the Token (renewToken)</span></a></td>
</tr>
</tbody>
</table>



