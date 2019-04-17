
---
title: Use Security Keys
description: 
platform: All Platforms
updatedAt: Wed Apr 17 2019 08:51:07 GMT+0800 (CST)
---
# Use Security Keys
This page describes the token (Agora’s authentication mechanism). Before you start, check if your SDK version supports token:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Agora SDK</th>
<th>Version that Supports the Token</th>
</tr>
</thead>
<tbody>
<tr><td>Native</td>
<td>2.1.0+</td>
</tr>
<tr><td>Web</td>
<td>2.4.0+</td>
</tr>
<tr><td>Gaming</td>
<td>2.2.0+</td>
</tr>
</tbody>
</table>

To get the SDK version, call the following API methods:
- Native SDK: `getSdkVersion`
- Web SDK: `AgoraRTC.VERSION`
- Gaming SDK: `getSdkVersion`




>-   For the Agora Signaling SDK, see [Signaling Security Keys](../../en/Agora%20Platform/key_signaling.md).
-   If you are using Agora SDKs that do not support the token, see [Channel Keys](../../en/null/channel_key.md).


## Agora’s Authentication Mechanisms

The `joinChannel` method requires a security key as an essential parameter. The Agora SDK provides two different security key mechanisms based on your security requirements: 

1. For low-security requirements, such as for testing: [App ID](#APPID).
2. For high-security requirements, such as for production: App ID + App Certificate + [Token](#Token). Note that an App Certificate is enabled solely for the purposes of generating a Channel Key and cannot be used alone.

![](https://web-cdn.agora.io/docs-files/1555491048103)


<a name = "APPID"></a>

## Authenticate using an App ID only

After signing up at [Dashboard](http://dashboard.agora.io), you can create multiple projects and each project will have a unique App ID.

Anyone with your App ID can use it on any Agora SDK. Hence, it is prudent to safeguard the App IDs.

<a id ="getting-an-app-id"></a>

### Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/).

2. Click the **Project Management** icon ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu and click **Create**.

3. Fill in the **Project Name** and click **Submit**. You have created your first project at Agora.

4. Find the **App ID** under the created project.


### Use an App ID

You can access the Agora services with a unique App ID:

1.  Enter the App ID in the start window to enable communications.
2.  Add the App ID to the code when developing the application.
3.  Set the `appId` parameter as the App ID when initializing the client.
4.  Set the `token` parameter as NULL when joining the channel.

<a name = "Token"></a>

## Authenticate using Token

Agora recommends using a combination of App ID, App certificate, and token for authentication in production scenarios. 

The following process generates a token:

1.  Deploy a token generator on your server.
2.  The client sends a request for a token to the server.
3.  The server uses the token generator to create a token and sends the token back to the client.
4.  The client passes the token when joining a channel.
5.  When the token is about to expire or has expired, repeat Steps 2 to 4.
6.  The application client calls `renewToken` to use the new token.


### Deploy a Token Generator

To use token for authentication, you need to deploy a token generator on your server to generate a token.

Agora provides the server-side [sample codes](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) supporting the following languages:

- C++
- Go
- Java
- Node.js
- Python
- PHP
- Perl

You can deploy the corresponding sample code on your server, or write your own code in a different programming language.

If you have implemented Agora’s algorithm in other languages, you can file a pull request on [GitHub](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey). Agora will merge any valid implementations and test cases.

<a name = "Generate_Token"></a>
### Generate a Token

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
<td>The App ID of the user’s project in the Agora Dashboard, see <a href="#getting-an-app-id">Getting an App ID</a>.</td>
</tr>
	<tr><td><code>appCertificate</code></td>
<td>The App Certificate of the user’s project in the Agora Dashboard, see <a href="#getting-an-app-certificate">Getting an App Certificate</a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></a></td>
<td>The token expiration time. The default value is 0, where the token never expires. A user can join a channel indefinitely within the designated expiration time and will be removed from the channel after the expiration time.</td>
</tr>
</tbody>
</table>

>[1] Agora does not support signing Token with a non-zero string uid for the time being.
>[2] `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. If, for example, you want to access the Agora Service within 10 minutes after the token is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The expiration time for each token is independent, and you can set it through the `setPrivilege` method.

<a id ="getting-an-app-certificate"></a>

### Get an App Certificate

Each Agora account can create multiple projects, and each project has a unique App ID and App Certificate.

To get an App Certificate:

1.  Login to [https://dashboard.agora.io](https://dashboard.agora.io).

2.  Click **Add New Project** on the **Projects** page of  [Dashboard](https://dashboard.agora.io).

3.  Fill in the **Project Name** and click **Submit**. Find the App ID under the created project.

    <img alt="../_images/create_project.png" src="https://web-cdn.agora.io/docs-files/en/create_project.png" />

4.  Enable the App Certificate for the project.

    - Click **Edit** on the top-right of the project.

    - Click **Enable** to the right of the App Certificate. Read **About App Certificate** before confirming the operation.

      <img alt="../_images/enable_app_cert.png" src="https://web-cdn.agora.io/docs-files/en/enable_app_cert.png" />

    - Click the ‘eye’ icon to view the App Certificate. You can re-click this icon to hide the App Certificate.
    
      <img alt="../_images/view_app_certificate.png" src="https://web-cdn.agora.io/docs-files/en/view_app_certificate.png" />


> -   Keep the App Certificate on the server, never on any client machine.
> 
> -   The App Certificate takes about an hour to take effect after it is enabled.
> 
> -   Once the App Certificate is enabled for a project, a token must be used. For example, before enabling the App Certificate, an App ID can be used to join a channel; but once an App Certificate is enabled, a token or a Channel Key must be used to join a channel.

<a id ="Role-privilege Model"></a>


### Use a Token

Before a user joins a channel from the client：

1.  The client requests authentication from your organization’s business server.

2.  The server, upon receiving the request, generates a token using the token generator and sends the token back to the client.

3.  To join a channel, the client calls the `join` method, which requires the token as the first parameter.

4.  The Agora server receives the token and confirms that the call comes from a legitimate user, and then allows the user to access the Agora SD-RTN™ \(Software Defined Real-time Network\).

> -   When you deploy the token, the token replaces the original App ID when a user joins a channel.
> 
> -   The token expires after a certain period of time. The application must call `renewToken` when notified by the `onTokenPrivilegeWillExpire` callback that the token is about to expire or has expired.
> 
> -   The token encoding uses the industry-standard HMAC/SHA1 approach and the libraries are available on most server-side development platforms, such as Node.js, Java, PHP, Python, and C++. For more information, see [http://en.wikipedia.org/wiki/Hash-based\_message\_authentication\_code](http://en.wikipedia.org/wiki/Hash-based_message_authentication_code).


### References

If your SDK version is earlier than v2.1.0 and you wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

Learn how to generate a token on the server on the [Generating a Token](../../en/null/token_server.md) page.

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
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>iOS/macOS</td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:"><span>Join a Channel (joinChannelByToken)</span></a></td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:"><span>Renew the Token (renewToken)</span></a></td>
</tr>
<tr><td>Windows</td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c"><span>Join a Channel (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739"><span>Renew the Token (renewtoken)</span></a></td>
</tr>
<tr><td>Web</td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/web/interfaces/agorartc.client.html#join"><span>Join an AgoraRTC Channel (join)</span></a></td>
<td><a href="https://docs.agora.io/en/Recording/API%20Reference/web/interfaces/agorartc.client.html#renewtoken"><span>Renew the Token (renewToken)</span></a></td>
</tr>
</tbody>
</table>




