
---
title: Use Security Keys
description: 
platform: All Platforms
updatedAt: Fri Jul 19 2019 08:42:47 GMT+0800 (CST)
---
# Use Security Keys
We understand that security is a vital consideration when you integrate real-time communications into your application. To help you build an application that meets your security requirements, the Agora SDK provides two security mechanisms:

* For low security requirements, use an App ID for authentication.
* For high security requirements, use a dynamic key for authentication (recommended).

This page introduces Agora's two authentication mechanisms in details.


## Scope of application
We have two types of dynamic keys: Channel Key and Token. Different versions of our SDK use different dynamic keys for authentication. This page mainly deals with the Token. So before you start, see the following table to check which type of dynamic key that your SDK version supports:

| Agora SDK | Versions supporting Token | Versions supporting Channel Key | How to check SDK version |
| --------- | -------------------- | ------------------------- | ------------------------ |
| Native SDK   | 2.1.0 or later               | Earlier than 2.1.0        | `getSdkVersion`          |
| Web SDK      | 2.4.0 or later              | Earlier than 2.4.0        | `AgoraRTC.VERSION`       |
| Gaming SDK   | 2.2.0 or later               | Earlier than 2.2.0        | `getSdkVersion`          |

>-   If you use an Agora SDK that supports the Channel Key, see [Channel Keys](../../en/null/channel_key.md).
>-   If your wish to upgrade your SDK to a version that supports Token, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).
>-   For the Agora Signaling SDK, see [Signaling Security Keys](../../en/Agora%20Platform/key_signaling.md).
>-   For the Agora RTM SDK, see [Use Security Keys](https://docs.agora.io/en/Interactive%20Gaming/%3Chttps://docs-preview.agoralab.co/en/Real-time-Messaging/RTM_key?platform=All%20Platforms%3E).


## Use an App ID only for authentication

Each project you create at the [Agora Dashboard](http://dashboard.agora.io) has a unique App ID.

### Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Interactive%20Gaming/sign_in_and_sign_up.md).

2. Click **Get Started** under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523371446)

3. Input your project name in the pop-up window and click **Create**. Follow the on-screen instructions to get to know the basic steps to start a video call. Once the project is created, you can find it under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523478084)
	
4. Click the **Edit** button behind the new project, or the **Project Management** button ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to go to the **Project Management** page.

 ![](https://web-cdn.agora.io/docs-files/1563523678240)

5. On the **Project Management** panel, find the **App ID** of your project.

 ![](https://web-cdn.agora.io/docs-files/1563523737158)

### Apply your App ID

When initializing the client, set the `appId` parameter as the App ID you get to authenticate your application.

>  When joining a channel, set the `token` parameter as NULL.

## Use a token for authentication

The Token is a securer and more sophisticated authentication mechanism than the App ID.  You need to use an App ID and an App Certificate to generate a token for authentication. 

<a id="appcertificate"></a>

### Enable the App Certificate

Each Agora account can create multiple projects, and each project has a unique App ID and App Certificate.

To get an App Certificate:

1.  Login to [https://dashboard.agora.io](https://dashboard.agora.io).

2.  Click the **Edit** button of the corresponding project on the **Project Management** page.

![](https://web-cdn.agora.io/docs-files/1558943085209)

3. Click the **Enable** button next to the **App Certificate**. 

4. Read the pop-up description of the App Certificate and click **Save** as promped. 

![](https://web-cdn.agora.io/docs-files/1558943467945)

5.  The system sends your mail account a confirmation Email. Please follow the instruction to enable the App Certificate. 

6. On the **Project Management** page, click the 'eye' icon to view and copy the App Certificate. You can re-click this icon to hide the App Certificate. 

![](https://web-cdn.agora.io/docs-files/1558943748601)


> -   Keep the App Certificate on the server, never on any client machine.
> 
> -   The App Certificate takes about five minutes to take effect after it is enabled.
> 
> -   Once the App Certificate is enabled for a project, a token must be used. For example, before enabling the App Certificate, an App ID can be used to join a channel; but once an App Certificate is enabled, a token or a Channel Key must be used to join a channel.

### Get a temporary token

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/). Use either of the following ways to generate a temporary token:

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/) to join a channel. Follow the steps to generate a temporary token:

1. Enable the [App Certificate](../../en/Interactive%20Gaming/token.md). 

2. On the **Project Details** page, click **Generate a Temp Token**, enter a channel name, and you will get a temporary token on the **Token** page. 

	![](https://web-cdn.agora.io/docs-files/1563113619615)

	![](https://web-cdn.agora.io/docs-files/1563113643411)

### Get a token

When building the final production version of your application, you should generate a token on your server.

#### 1. Deploy a token generator on your server

First, use one of the Agora [sample codes](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) (C++, Go, Java, Node.js, Python, PHP, and Perl) to deploy a token generator on your server.

Or you can write your own code in a programming language that is not mentioned above to deploy a token generator. 

If you implement a token generator in a different language, you can propose a pull request on [GitHub](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey). We will merge any implementation that proves valid.

#### 2. Generate a token

The process of generating a token is as follows: 

1.  The client sends a request for a token to your server.
2.  The server uses the token generator you deploy to create a token and sends it back to the client.

The application client needs to send the following parameters to the server to generate a token. See [Generate a Token on Your Server](../../en/Interactive%20Gaming/token_server.md).

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
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>iOS/macOS</td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:"><span>Join a Channel (joinChannelByToken)</span></a></td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>Windows</td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739"><span>Renew the Token (renewtoken)</span></a></td>
</tr>
<tr><td>Web</td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/web/interfaces/agorartc.client.html#join"><span>Join an AgoraRTC Channel (join)</span></a></td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/API%20Reference/web/interfaces/agorartc.client.html#renewtoken"><span>Renew the Token (renewToken)</span></a></td>
</tr>
</tbody>
</table>



