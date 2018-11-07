
---
title: Integrate the SDK
description: 
platform: Web
updatedAt: Wed Nov 07 2018 10:00:45 GMT+0000 (UTC)
---
# Integrate the SDK
This page contains information on how to prepare the development environment before enabling a video call with the Agora Web SDK.

## <a name = "pre"></a>Prerequisites

1. Install a browser supported by the Agora Web SDK as shown in the following table:
  <table>
  <tr>
    <th>Platform</th>
    <th>Chrome 58+</th>
    <th>Firefox 56+</th>
    <th>Safari 11+</th>
    <th>Opera 45+</th>
    <th>QQ Browser Latest</th>
    <th>360 Security  Browser</th>
    <th>Wechat Built-in Browser</th>
  </tr>
   <tr>
    <td>Android 4.1+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11+</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

> Agora Web SDK 2.5 also supports Chrome 49 on Windows XP.

2. Open the ports and whitelist the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).
3. Understand the limitations in [Known Issues](../../en/Interactive%20Broadcast/release_web_video.md) and [FAQ](https://docs.agora.io/en/2.4.1/faq/faq/web).

## Creating an Agora Account and Getting an App ID

1. Sign up for a developer account at [https://dashboard.agora.io/](https://dashboard.agora.io/).

2. Click **Add New Project** on the **Projects** page of the dashboard.

   <img alt="../_images/appid_1.jpg" src="https://web-cdn.agora.io/docs-files/en/appid_1.jpg" />

3. Fill in the **Project Name** and click **Submit**. You have created your first project at Agora.

4. Find the **App ID** under the created project.

   <img alt="../_images/appid_2.jpg" src="https://web-cdn.agora.io/docs-files/en/appid_2.jpg" />


## Importing the Agora Web SDK to Your Project

Choose one of the following methods to obtain the Agora Web SDK:

### Method 1: Get the SDK through npm

This method requires npm, see [Install npm](https://www.npmjs.com/get-npm) for details.

1. Run the following command to install the SDK.
  `npm install agora-rtc-sdk`

	
2. Add the following code to your project.

	```javascript
	import AgoraRTC from 'agora-rtc-sdk'
	```

### Method 2: Getting the SDK through the CDN

Add `<script src="http://cdn.agora.io/sdk/web/AgoraRTCSDK-2.4-latest.js”></script>` to the line above `</body>` in your project.

<img alt="../_images/web_sdk_cdn.png" src="https://web-cdn.agora.io/docs-files/en/web_sdk_cdn.png" />

### Method 3: Getting the SDK from the official Agora website

1. [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the latest Agora Web SDK.

   <img alt="../_images/web_sdk_download.png" src="https://web-cdn.agora.io/docs-files/en/web_sdk_download.png" style="width: 500px;"/>

2. Copy the `AgoraRTCSDK-2.4.js` file to your project.

3. Reference the `AgoraRTCSDK-2.4.js` file in your project.

   <img alt="../_images/web_sdk_reference.jpeg" src="https://web-cdn.agora.io/docs-files/en/web_sdk_reference.jpeg" />

> The screenshots are for reference only, please use the latest version of the SDK.

## Preparing the Web Server

1. Install a web server, such as Apache, Nginx, or Node.js.
2. Import the downloaded Agora Web SDK to your web server.
3. Set up your web server so that you can access the sample app page or your own app page on the supported browsers, see [Prerequisites](#pre).

The Web environment is now set to use the Agora SDK.
