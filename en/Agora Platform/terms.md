
---
title: Agora Key Terms
description: 
platform: All Platforms
updatedAt: Thu Sep 12 2019 02:29:24 GMT+0800 (CST)
---
# Agora Key Terms
Learn about the key terms of the Agora platform.

## Basics

Things you need to know before using the Agora SDK.

### RTC SDK
We refer to all the Agora SDKs that enable real-time communication as the RTC SDK. You can use the RTC SDK to enable a  [Voice Call](https://docs.agora.io/en/Voice/product_voice), [Video Call](https://docs.agora.io/en/Video/product_video), [Audio Broadcast](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio), and [Video Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live). We categorize the RTC SDKs by the supported platforms:

- The Agora Native SDK, including the Android, iOS, macOS, Windows, and Electron SDK.
- The Agora Web SDK

### Agora Dashboard

[Agora Dashboard](https://dashboard.agora.io/) is a platform provided by Agora for you to create and manage your projects. After signing up at [Agora Dashboard](https://dashboard.agora.io/), you can create projects, get [App IDs](#appid), view your usage statistics, analyze the quality of your calls, and check your bills.

### <a name="appid"></a>App ID

App IDs are issued to app developers by Agora to identify the projects and organizations. After signing up at [Agora Dashboard](https://dashboard.agora.io/), you can create multiple projects, and each project will have a unique App ID. See [Getting an App ID](../../en/Agora%20Platform/token.md).

All communication sessions created across the Agora SD-RTN™ (Software Defined Real-time Network) with one App ID are isolated from all other sessions with different App IDs. Communication sessions with different App IDs are not connected or related. Statistics, management, and billing are separately associated based on each App ID. If an organization develops independently different apps by different teams, the apps should use different App IDs.  However, if the apps need to communicate with each other, a single App ID should be used.

### App Certificate

Agora provides an App Certificate for generating [dynamic keys](#key). You can enable the App Certificate in the [Agora Dashboard](https://dashboard.agora.io). See [Getting an App Certificate](../../en/Agora%20Platform/token.md).

### <a name="key"></a>Dynamic Key

Using your App ID is simple and sufficient for initial app development. However, a person can illicitly obtain your App ID to develop apps and join your sessions billed to you. To prevent this and to secure your apps, Agora recommends you use Dynamic Keys for large-scale production apps.

Dynamic Keys are generated within the server-side code using an App Certificate. The App Certificate is not accessible in any client code, which makes using the Dynamic Key more secure than the static App ID.

Dynamic Keys have expiry dates and contain client permissions, such as different role privileges (host and audience).

Based on the SDK version, the Dynamic Key can be a Channel Key or a Token, see [Set up Authentication](../../en/Agora%20Platform/token.md) for details.

### SD-RTN™

Agora’s audio and video transmissions rely on Agora's self-built SD-RTN™ (Software Defined Real-time Network), a virtual and UDP (User Datagram Protocol)-based network architecture designed specifically for real-time communications. By deploying software networking units, which work in synergy with one another, at different data centers across the Internet, Agora managed to add a virtual layer. To ensure stable transmission and low latency, particularly on weak networks, the SD-RTN™ automatically assigns an optimal path according to the following node conditions in real-time:

- Transmission status
- Load conditions
- Distance to the users
- Response time

## SDK core concepts

The Agora SDKs are a set of API methods (engine interfaces) and callbacks (events).

- Method: The client calls the API methods to implement the features provided by the Agora SDKs.
- Callback: Feedback sent from the Agora SDKs to the client on a local or remote event that has happened. A remote event is a callback of a remote user in the channel and transmits through the UDP channel that is not 100% reliable.

For details on specific methods and callbacks, see the Agora API Reference:
- [Android](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/index.html)
- [iOS/macOS](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/index.html)
- [Web](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/index.html)
- [Windows](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/index.html)


### Channel

As an analogy, if we imagine an app being a building, a channel will be a room in the building. A channel is created when the first user joins the channel and is automatically destroyed when the last user leaves the channel. When entering a room in a building, you need a key to open the door. Similarly, when joining a channel, you need an [App ID](#appid) or [Dynamic Key](#key) for authentication.

### Channel profile

The SDK applies different optimization methods according to the channel profile. Users in the same channel must use the same channel profile. Agora supports the following channel  profiles:


| Channel Profile | Description | 
| ---------------- | ---------------- | 
| Communication     | One-on-one or group calls, where all users in the channel can talk freely.    | 
| Live Broadcast     | In a live broadcast channel, users have two client roles: [Host](#host) and [audience](#audience). The host sends and receives audio/video, and the audience receives audio/video with the sending function disabled.   | 
| Gaming    | Any user in the channel can talk freely. This profile uses the codec with low-power consumption and low bitrate by default.   | 

> The gaming profile applies to the Agora Gaming SDK only.


### <a name="username"></a>Username

A username identifies a user in the channel. Each user in the same channel should have a unique username.

Agora supports two types of usernames, integer (UID) and string (User Account). You can choose either type to identify the users. Ensure that all the users in a channel use the same type of username.

### Stream

A stream is an object that contains audio/video data. Users in a channel can [publish](#pub) the local stream and [subscribe](#sub) to the remote streams from other users.

### <a name ="pub"></a>Publish

Publishing a stream describes the action of a user sending the local audio/video data to other users in the channel after joining a channel.

In a live broadcast channel, only hosts can publish streams.

### <a name ="sub"></a>Subscribe

Subscribing to the streams describes the action of the user receiving the audio/video streams published (sent) by other users in the channel after joining a channel.

### <a name ="dual"></a>Dual-stream mode

Dual streams are a hybrid of a high-video stream and a low-video stream. The publisher can choose to enable the dual-stream mode to send both video streams at the same time.

High video and low video are relative concepts. A low-video stream consumes less bandwidth and is suitable for poor network conditions.

| Stream Type | Description |
| ---------------- | ---------------- | 
| High-video     | High bitrate, and high-resolution video stream.      | 
| Low-video     | Low bitrate, and low-resolution video stream.      | 


### Stream fallback

The publisher/subscriber can enable stream fallback to send/receive the low-video stream or audio-only stream in poor network conditions. Stream fallback only works when the [dual-stream mode](#dual) is enabled.

## Live broadcast core concepts

A live broadcast is an Internet broadcast of a live performance through an app, where the viewers are called the audience and the performer is called the host.

Agora’s products allow you to implement the live broadcast feature on an app:

- To give a live broadcast, create a channel and join the channel as the host role.
- To view a live broadcast, join the channel created by the host as the audience role.

### <a name = "host"></a>Broadcaster/Host

In a live broadcast channel, the broadcasters or hosts are users who can publish and subscribe to streams.

### <a name = "audience"></a>Audience

In a live broadcast channel, an audience is a group of users who can only subscribe to streams. The audience cannot publish streams. 

### Hosting-in

An audience can apply to become a host to interact directly with the existing hosts, namely hosting-in.

### CDN live streaming

The process of publishing streams into the CDN (Content Delivery Network) is called CDN live streaming, where users can view the live broadcast through a web browser.

### Transcoding

Transcoding is used in CDN live streaming when multiple hosts are in the channel.

In CDN live streaming, the audio and video streams sent to the SD-RTN™ are transferred into RTMP (Real-Time Messaging Protocol) protocol and pushed to the CDN. If there are multiple hosts, their streams need to be combined into a single stream by transcoding. 

Transcoding sets the audio/video profiles and the picture-in-picture layout for the stream to be pushed to the CDN.

> Agora does not recommend transcoding in the case of a single host.
