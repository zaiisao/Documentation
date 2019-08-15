
---
title: Use Security Keys
description: 
platform: All Platforms
updatedAt: Thu Aug 15 2019 06:00:41 GMT+0800 (CST)
---
# Use Security Keys
We understand that security is a vital consideration when you integrate real-time communications into your application. To help you build an application that meets your security requirements, the Agora SDK provides two security mechanisms:

* For low-security requirements, use an App ID for authentication.
* For high-security requirements, use a dynamic key for authentication (recommended).

This page introduces Agora's two authentication mechanisms in details.


## Scope of application
We have two types of dynamic keys: Channel Key and Token. Different versions of our SDK use different dynamic keys for authentication. This page mainly deals with the Token. So before you start, see the following table to check which type of dynamic key that your SDK version supports:

| Agora SDK | Versions supporting Token | Versions supporting Channel Key | How to check SDK version |
| --------- | -------------------- | ------------------------- | ------------------------ |
| Native SDK   | 2.1.0 or later               | Earlier than 2.1.0        | `getSdkVersion`          |
| Web SDK      | 2.4.0 or later              | Earlier than 2.4.0        | `AgoraRTC.VERSION`       |
| Gaming SDK   | 2.2.0 or later               | Earlier than 2.2.0        | `getSdkVersion`          |

>-   If you use an Agora SDK that supports the Channel Key, see [Channel Keys](../../en/null/channel_key.md).
>-   If you wish to upgrade your SDK to a version that supports Token, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).
>-   For the Agora Signaling SDK, see [Signaling Security Keys](../../en/Agora%20Platform/key_signaling.md).
>-   For the Agora RTM SDK, see [Use Security Keys](https://docs.agora.io/en/Real-time-Messaging/RTM_key?platform=All%20Platforms).


## Use an App ID only for authentication

Each project you create in [Agora Dashboard](http://dashboard.agora.io) has a unique App ID.

### Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Agora%20Platform/sign_in_and_sign_up.md).

2. Click **Get Started** under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523371446)

3. Input your project name in the pop-up window and click **Create**. Follow the on-screen instructions to get to know the basic steps to start a video call. Once the project is created, you can find it under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523478084)
	
4. Click the **Edit** button behind the new project, or the **Project Management** button ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to go to the **Project Management** page.

 ![](https://web-cdn.agora.io/docs-files/1563523678240)

5. On the **Project Management** panel, find the **App ID** of your project.

 ![](https://web-cdn.agora.io/docs-files/1563523737158)

### Apply your App ID

When initializing the client, set the `appId` parameter as the App ID that you get to authenticate your application.

>  When joining a channel, set the `token` parameter as NULL.

## Use a token for authentication

The Token is a more secure and sophisticated authentication mechanism than the App ID.  You need to use an App ID and an App Certificate to generate a token for authentication. 

<a id="appcertificate"></a>

### Enable the App Certificate

If you choose **APP ID + APP certificate + Token (recommended)**  when you create a project in the Dashboard,  the App Certificate is enabled by default.

![](https://web-cdn.agora.io/docs-files/1563114012279)

If you did not choose  **APP ID + APP certificate + Token (recommended)**, follow the steps to enable the certificate.

1. Find your project on the **Project Management** page at the [Agora Dashboard](https://dashboard.agora.io/) and click the **Edit** button.

	![](https://web-cdn.agora.io/docs-files/1563112238811)
	
2. On the **Edit Project** page, click **Enable** to switch on the App Certificate and click **Save** to confirm your setting. 

	![](https://web-cdn.agora.io/docs-files/1563112280018)
	
3. Agora sends your account a confirmation Email. Follow the instruction to enable the App Certificate. 
4. Go back to the  **Project Management** page, your can see App Certificate appears enabled. 

	![](https://web-cdn.agora.io/docs-files/1563113154996)

**Note**: Check the Spam Email or Junk Email, if there's no confirmation Email in your inbox.

<a id = "temptoken"></a>
### Get a temporary token

When working on a test version of your application, you can generate a temporary token at the [Agora Dashboard](https://dashboard.agora.io/) to join a channel. 

On the **Project Details** page, click **Generate a Temp Token**, enter a channel name, and you will get a temporary token on the **Token** page. 

![](https://web-cdn.agora.io/docs-files/1563113619615)

![](https://web-cdn.agora.io/docs-files/1563113643411)

> - Ensure that you have enabled the App Certificate of the project before clicking **Generate a Temp Token** . See [Enable the App Certificate](#appcertificate).
> - A temp token applies to scenarios with low security requirements. For the production environment, we recommend using a token generated at your server.

### Get a token

When building the final production version of your application, you should generate a token on your server. See [Generate a Token on Your Server](../../en/Agora%20Platform/token_server.md).

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
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>iOS/macOS</td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:"><span>Join a Channel (joinChannelByToken)</span></a></td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>Windows</td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739"><span>Renew the Token (renewtoken)</span></a></td>
</tr>
<tr><td>Web</td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#join"><span>Join an AgoraRTC Channel (join)</span></a></td>
<td><a href="https://docs.agora.io/en/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#renewtoken"><span>Renew the Token (renewToken)</span></a></td>
</tr>
</tbody>
</table>



