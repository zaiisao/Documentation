
---
title: Build a Client
description: 
platform: iOS
updatedAt: Mon Jul 20 2020 06:24:43 GMT+0800 (CST)
---
# Build a Client
This article describes how to implement the basic features of multi-hosted interactive streaming.

## Flowchart

Refer to the flowcharts for the following functions:

- Joins and leaves the room.

![](https://web-cdn.agora.io/docs-files/1594377181071)

- Controls host seats.

![](https://web-cdn.agora.io/docs-files/1595225449884)

## Integrate the SDK

Refer to the following table to integrate the SDKs into your project:


| Product | SDK downloads | Integration guide |
| ---------------- | ---------------- | ---------------- |
| [Agora RTC (Real-time Communication) SDK](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [Agora SDK for iOS](https://docs.agora.io/en/Agora%20Platform/downloads)      | [Start Live Interactive Video Streaming](https://docs.agora.io/en/Interactive%20Broadcast/start_live_ios?platform=iOS) |
| [Agora RTM (Real-time Messaging) SDK](https://docs.agora.io/en/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Real-time messaging SDK](https://docs.agora.io/en/Real-time-Messaging/downloads) | [Peer-to-peer or Channel Messaging](https://docs.agora.io/en/Real-time-Messaging/messaging_ios?platform=iOS) |
| [Third-party Image Enhancement SDK](https://www.faceunity.com/#/developindex) | N/A | [iOS SDK Integration Guide](https://www.faceunity.com/docs_develop_en/#/nama_api_docs/iOS/docs/iOS_Nama_SDK_Integration_Guide) |


## Core API call sequence

The following diagrams show the core APIs that the Agora Live Demo app uses to implement a multi-hosted interactive streaming scenario. Refer to them to implement the various functions in your project.

<div class="alert note">The Cloud Service in the demo app is implemented by Agora, and you need to deploy your own cloud service for the same purposes.</div>

- The host joins the room and starts the interactive streaming.

   ![](https://web-cdn.agora.io/docs-files/1595225702765)
	 
- The host invites the audience for co-hosting, stops co-hosting, mutes and unmutes a co-host, and closes and reopens a host seat.

  ![](https://web-cdn.agora.io/docs-files/1595225727493)
	
- The audience applies for co-hosting and switches back to an audience member.

   ![](https://web-cdn.agora.io/docs-files/1595225740510)

## Core API reference

- Agora RTM SDK

| API | Function |
| ---------------- | ---------------- |
| [initWithAppId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:)       | Creates an RTM Client object.      |
| [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) | Logs into the Agora RTM system.|
| [createChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | Creates an Agora RTM channel. |
| [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) | Joins the RTM channel.|
| [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:) | Sends a channel message, which can be received by all users in the channel. |
| [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) | Sends a peer message. The host uses this method to send a co-hosting invitation to an audience member; an audience member uses this method to send a co-hosting application to the host. |
| [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:) | Occurs when receiving a channel message. |
| [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:messageReceived:fromPeer:) | Occurs when receiving a peer-to-peer message. |
| [leaveWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/leaveWithCompletion:) | Leaves the RTM channel. |
| [logoutWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/logoutWithCompletion:) | Logs out of the Agora RTM system. |

- Agora RTC SDK

| API | Function |
| ---------------- | ---------------- |
| [sharedEngineWithAppId](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithAppId:delegate:)      | Creates an RtcEngine object. |
| [setChannelProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setChannelProfile:) | Sets the channel profile. In multi-hosted interactive streaming, the channel profile is set as `Broadcasting`.|
| [setClientRole](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setClientRole:) | Sets the user role in interactive streaming. In multi-hosted interactive streaming, use this method to switch a user between a co-host and an audience member.|
| [enableVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableVideo) | Enables video.|
| [setupLocalVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupLocalVideo:) | Sets the local video view. Call this method on the clients of the host and co-hosts to enable the host and co-hosts to see their own video view. |
| [joinChannelByToken](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) | Joins the RTC channel. |
| [setupRemoteVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupRemoteVideo:) | Sets the remote video view. Call this method on the clients of the host and co-hosts to enable the audience to see the video view of the host and co-hosts. |
| [muteLocalAudioStream](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/muteLocalAudioStream:) | Stops or resumes sending the local audio stream. Use this method with `sendMessage` to revoke or restore a co-host's privilege of sending audio.|
| [muteLocalVideoStream](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/muteLocalVideoStream:) | Stops or resumes sending the local video stream. Use this method with `sendMessage` to revoke or restore a co-host's privilege of sending video. |
| [leaveChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) | Leaves the channel. |

## Additional functions

**Image enhancement**

The Agora Live Demo app uses a third-party SDK for image enhancement. You can refer to the source code in the faceunity file to implement the function.

**Network quality detection**

Use the `reportRtcStats` callback to detect and report the network quality in real-time. Triggered once every two seconds during a live broadcast, this callback reports statistics, including the sending and receiving bitrate and packet loss rate.

**In-ear monitoring**

Call `enableInEarMonitoring` to enable the in-ear monitor function on the host's client.

**Audio mixing**

After joining the channel, call `startAudioMixing` on the host's client to play background music.

## Open-source sample project

Agora provides an open-source sample project for [Multi-hosted Interactive Streaming](https://github.com/AgoraIO-Usecase/AgoraLive) on GitHub for you to download as a source code reference.
