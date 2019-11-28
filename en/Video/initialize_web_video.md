
---
title: Create and Initialize a Client
description: 
platform: Web
updatedAt: Tue Oct 22 2019 06:19:11 GMT+0800 (CST)
---
# Create and Initialize a Client
## Prerequisites

Before creating and initizing the client, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/web_prepare.md).

Create a project in Agora Console and get the App ID of the project. You need to pass in the App ID during initialization.

1. Sign up for a developer account at [Agora Console](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Video/sign_in_and_sign_up.md).

2. Click ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to enter the [**Project Management**](https://dashboard.agora.io/projects) page.

3. Click **Create**. 

![](https://web-cdn.agora.io/docs-files/1574924327108)

4.  Enter your project name and select your authentication mechanism ("App ID") in the dialog box.

![](https://web-cdn.agora.io/docs-files/1574924446798)
	
5. Click **Submit** and you can find the **App ID** of your newly created project.

![](https://web-cdn.agora.io/docs-files/1574924570426)

## Implementation

### Create a Client
Use the `AgoraRTC.createClient` method to create a client object. Set the mode and codec parameters. 

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

### Initialize the Client
After creating the client, pass the project App ID to the `client.init` method to initialize the client.

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```

> Ensure that you call the `create` and `client.init` methods to intiialize the AgoraRtcEngine before calling any other API. 

## Next Steps
You hava created the Client and can start a voice call with the following steps:
- [Join a Channel](../../en/Video/join_video_web.md)
- [Publish and Subscribe to Streams](../../en/Video/publish_web.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections.
- [Set the Stereo/High-fidelity Audio Profile](../../en/Video/audio_profile_web.md)
