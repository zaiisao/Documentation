
---
title: Channel Keys
description: Guide on how to use channel key
platform: All Platform
updatedAt: Fri Nov 02 2018 04:16:09 GMT+0800 (CST)
---
# Channel Keys
## Introduction

This page describes how to use channel keys with the Agora SDK.

### App ID

After signing up at [Dashboard](http://dashboard.agora.io/), multiple projects can be created. Each project will be assigned a unique App ID. Anyone with your App ID can use it on any Agora SDK. Hence, it is prudent to safeguard the App IDs.

## Get a Channel Key

### Step 1: Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/).

2. Click the **Project Management** icon ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu and click **Create**.

3. Fill in the **Project Name** and click **Submit**. You have created your first project at Agora.

4. Find the **App ID** under the created project.

#### Use an App ID

Access the Agora services by using your unique App ID:

1. Enter the App ID in the start window to enable voice or video communication in the demo.

2. Add the App ID to the code when developing the application.

3. Set the `appId` parameter as the App ID when calling the following APIs:

  <table>
  <tr>
    <th>Platform</th>
    <th>API</th>
  </tr>
  <tr>
    <td>Android/Windows</td>
    <td>create</td>
  </tr>
  <tr>
    <td>iOS/macOS</td>
    <td>sharedEngineWithappId</td>
  </tr>
  <tr>
    <td>Web</td>
    <td>client.init</td>
  </tr>
</table>

4. Set the `channelKey` parameter to NULL when calling the following APIs:

	<table>
		<tr>
			<th>Platform</th>
			<th>API</th>
		</tr>
		<tr>
			<td>Android/Windows</td>
			<td>joinChannel</td>
		</tr>
		<tr>
			<td>iOS/macOS</td>
			<td>joinChannelByKey</td>
		</tr>
		<tr>
			<td>Web</td>
			<td>client.join</td>
		</tr>
	</table>

### Step 2: Get an App Certificate

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

### Step 3: Integrate the Schema

Use the `generateMediaChannelKey` method and sample code provided by Agora to acquire the Channel Key. Agora provides server-side sample code in programming languages, such as Java, C++, Python, and Node.js.

Go to <https://github.com/AgoraIO/AgoraDynamicKey> to download the corresponding code and integrate it directly into your application.

Enter the following parameters into your application. The field names vary according to different programming languages:

<table>
  <tr>
    <th>Field Name</th>
    <th>C++</th>
    <th>Java</th>
    <th>Python</th>
    <th>Node.js</th>
    <th>Go</th>
  </tr>
  <tr>
    <td>App ID</td>
    <td>appID</td>
    <td>appID</td>
    <td>appID</td>
    <td>appID</td>
    <td>appID</td>
  </tr>
  <tr>
    <td>App Certificate</td>
    <td>appCertificate</td>
    <td>appCertificate</td>
    <td>appCertificate</td>
    <td>appCertificate</td>
    <td>appCertificate</td>
  </tr>
  <tr>
    <td>Channel</td>
    <td>channelName</td>
    <td>channel</td>
    <td>channelName</td>
    <td>channel</td>
    <td>channelName</td>
  </tr>
  <tr>
    <td>Timestamp [1]</td>
    <td>unixTs</td>
    <td>ts</td>
    <td>unixTs</td>
    <td>ts</td>
    <td>unixTs</td>
  </tr>
  <tr>
    <td>Random Number</td>
    <td>randomInt</td>
    <td>r</td>
    <td>randomInt</td>
    <td>r</td>
    <td>randomInt</td>
  </tr>
  <tr>
    <td>User ID</td>
    <td>uid</td>
    <td>uid</td>
    <td>uid</td>
    <td>uid</td>
    <td>uid</td>
  </tr>
  <tr>
    <td>Call Expiration Timestamp [2]</td>
    <td>expiredTs</td>
    <td>expiredTs</td>
    <td>expiredTs</td>
    <td>expiredTs</td>
    <td>expiredTs</td>
  </tr>
</table>

> [1]: The timestamp, represented by the number of seconds elapsed since 1/1/1970. The user can use the Dynamic Key to access the Agora service within 5 minutes after the Dynamic Key is generated. If the user does not access the Agora service after 5 minutes, the Dynamic Key is no longer valid.
>
> [2]: Set the value to 0 for no time limit. Indicates the exact time when a user can no longer use the Agora service (for example, when a user is forced to leave an ongoing call). When the value is set for Call Expiration Timestamp, it does not mean that the Dynamic Key will be expired, but means that the user will be kicked out of the channel. 

To verify the user ID (uid), check the following requirements:

<table>
  <tr>
    <th>DynamicKey Version</th>
    <th>User ID</th>
    <th>SDK Version</th>
  </tr>
  <tr>
    <td>DynamicKey4</td>
    <td>uid of the specific user</td>
    <td>1.3 or later</td>
  </tr>
  <tr>
    <td>DynamicKey3</td>
    <td>uid of the specific user</td>
    <td>1.2.3 or later</td>
  </tr>
  <tr>
    <td>DynamicKey</td>
    <td>N/A</td>
    <td>N/A</td>
  </tr>
</table>

### Step 4: Use a Channel Key

Before a user joins a channel (start a call or receive an invitation), the following sequence occurs：

1. The client application requests authentication from your organization’s business server.

2. The server, upon receiving the request, uses the algorithm provided by Agora to generate a Channel Key and then passes the Channel Key back to the client application.

   > The Channel Key is based on the App Certificate, App ID, Channel Name, Current Timestamp, Client User ID, and Lifespan Timestamp

3. The client application calls the following API to join a channel, which requires the Channel Key as the first parameter.

 <table>
  <tr>
    <th>Platform</th>
    <th>API</th>
  </tr>
  <tr>
    <td>Android/Windows</td>
    <td>joinChannel</td>
  </tr>
  <tr>
    <td>iOS/macOS</td>
    <td>joinChannelByKey</td>
  </tr>
  <tr>
    <td>Web</td>
    <td>join</td>
  </tr>
</table>

4. The Agora server receives the Channel Key and confirms that the call comes from a legitimate user, and then allows the user to access the Agora SD-RTN™ (Software Defined Real-time Network).

> - When you deploy the Channel Key, it replaces the original App ID when someone joins a channel.
> - The Channel Key expires after a certain period of time. Your application must call `renewChannelKey` when a timeout occurs. The onError or didOccurError callback returns ERR_CHANNEL_KEY_EXPIRED (109).
> - The Channel Key encoding uses the industry-standard HMAC/SHA1 approach and the libraries are available on most server-side development platforms, such as Node.js, PHP, Python, and Ruby. For more information, see: <http://en.wikipedia.org/wiki/Hash-based_message_authentication_code>


