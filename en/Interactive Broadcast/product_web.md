
---
title: Product Overview
description: Overview of Agora Web SDK
platform: Web
updatedAt: Fri Nov 02 2018 04:03:07 GMT+0000 (UTC)
---
# Product Overview
Agora Web SDK is a JavaScript library loaded by an HTML web page. The Agora Web SDK library uses APIs in the web browser to establish connections and control the communication and live broadcast services.

Agora Web SDK provides the same functionality as the Agora Native SDK on the Web.

Agora Web SDK is interoperable with other Agora SDKs:

- Android SDK
- iOS SDK
- macOS SDK
- Windows SDK
- Miniapp SDK for WeChat

## Browser Support

Agora Web SDK currently supports the following browsers:

- Google Chrome 58+
- Firefox 56+
- Safari 11+
- Opera 45+
- QQ 10+

## Enable Audio/Video Communication or Live Broadcast on the Web

After [integrating the SDK](../../en/Interactive%20Broadcast/web_prepare.md), follow these steps to enable audio/video communication or live broadcast on the Web:

1. Create a client
2. Initialize the client
3. Join a channel
4. Create a stream
5. Initialize the stream
6. Publish the local stream
7. Subscribe to a remote stream
8. Play the stream
9. Leave the channel

See the image below for the key steps in the process:

<img alt="../_images/web-sdk-workflow.jpg" src="https://web-cdn.agora.io/docs-files/en/web-sdk-workflow.jpg" style="width: 486.2px; height: 423.5px;"/>

For the detailed instructions, check out the **Quickstart Guide** section on the left navigation bar.

Once you understand the basic functions of the Agora Web SDK, you can explore more complicated functions with the following advanced guides:

- [Hosting In](../../en/Interactive%20Broadcast/hostin_web.md)
- [Pushing Streams to the CDN](../../en/Interactive%20Broadcast/push_stream_web.md)
- [Screen Sharing on the Web](../../en/Interactive%20Broadcast/screensharing_web.md)
- [17-Way Live Video Broadcast](../../en/Interactive%20Broadcast/seventeen_web.md)
- [Deploying the Enterprise Proxy](../../en/Interactive%20Broadcast/proxy_web.md)

For specific API classes and methods, see the [Agora Web SDK API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/index.html).

## Core Concepts

The following terms are the important concepts in Agora Web SDK. Agora recommends you read this section to get familiar with these concepts before developing your App with our Web SDK. You can learn more about the basic concepts of Agora SDKs on the [Agora Key Terms](../../en/Agora%20Platform/terms.md) page.

If you want to directly start integration, check out [Integrate SDK](../../en/Interactive%20Broadcast/web_prepare.md).

### Client

A client is a representation of a local or remote user in the call.

The client methods provide much of the core functionality in Agora Web SDK, including: 

- Initializaition (`Client.init`) 
- Join a channel (`Client.join`)
- Publish a stream (`Client.publish`)
- Susbcribe to a stream (`Client.subscribe`)
- Leave the channel (`Client.leave`)

### Stream

A stream usually refers to an object that contains audio/video data.

By calling the Stream methods, you can manage and set the audio and video data you send and receive in the channel. For example:

- Play the audio or video (`Stream.play`)
- Get the current audio volume (`Stream.getAudioLevel`)
- Set the video profile (`Stream.setVideoProfile`)

### Publish

After joining the channel, the user can send the local audio/video data to other users in the channel, namely publishing the stream.
One client can only publish one stream. If you need to publish multiple streams, you need to create multiple clients.

For example, in the case of screen sharing, if you want to enable the video call and share the screen at the same time, you need to create two clients, one for publishing the video stream, the other for publishing the screen stream.

### Subscribe

After joining the channel, the user can receive the audio/video stream published by other users in the channel, namely subscribing to the stream.

## FAQ

Check out questions related to Web SDK on the [FAQ](../../en/Agora%20Platform/websdk_related_faq.md) page.
