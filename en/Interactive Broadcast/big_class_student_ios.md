
---
title: Build a Client for the Students
description: 
platform: iOS
updatedAt: Thu May 14 2020 06:29:48 GMT+0800 (CST)
---
# Build a Client for the Students
This section describes how to implement an iOS client for the students.

## Flowchart

This flowchart shows the major logic of the students joining and leaving the classroom:

![](https://web-cdn.agora.io/docs-files/1589357458788)

## Integrate the SDK

Refer to the following table to download the SDKs, and integrate the SDKs into your project.


| Product | SDK download | Integration guide |
| ---------------- | ---------------- | ---------------- |
| [RTC (Real-time Communication) SDK](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [Agora SDK for iOS](https://download.agora.io/sdk/release/Agora_Native_SDK_for_iOS_v2_9_0_102_FULL_20200216_2115.zip)     | [Start Live Interactive Streaming](https://docs.agora.io/en/Interactive%20Broadcast/start_live_ios?platform=iOS) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/en/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Real-time messaging SDK](https://docs.agora.io/en/Real-time-Messaging/downloads) | [Peer-to-peer or Channel Messaging](https://docs.agora.io/en/Real-time-Messaging/messaging_ios?platform=iOS) |
| Agora Edu Cloud Service | / | [Agora Edu Cloud Service quickstart](https://github.com/AgoraIO-Usecase/eEducation/wiki/Agora-Edu-Cloud-Service) |
| [Whiteboard](https://developer-en.netless.link/docs/ios/overview/ios-introduction/) | [White SDK](https://developer-en.netless.link/docs/ios/quick-start/ios-prepare/) | [Whiteboard quickstart](https://developer-en.netless.link/docs/ios/quick-start/ios-init-sdk/) | 



## Core API call sequence

Refer to the following diagram to implement the basic real-time communication and messaging functions in your project with the [Agora RTC SDK](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#agora-rtc-sdk), [RTM SDK](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#agora-rtm-sdk), and Agora Edu Cloud Service.

- Joins and leaves a channel:

![](https://web-cdn.agora.io/docs-files/1589270816861)

- Sneds a request to speak up:

![](https://web-cdn.agora.io/docs-files/1589270836743)

## Core API reference

- Agora Edu Cloud Service

| API | Function |
| ---------------- | ---------------- |
| [entry](https://github.com/AgoraIO-Usecase/eEducation/wiki/Agora-Edu-%E4%BA%91%E6%9C%8D%E5%8A%A1#%E8%BF%9B%E5%85%A5%E6%95%99%E5%AE%A4) | Enters the classroom. |
| [get room info](https://github.com/AgoraIO-Usecase/eEducation/wiki/Agora-Edu-%E4%BA%91%E6%9C%8D%E5%8A%A1#%E5%88%9D%E5%A7%8B%E5%8C%96%E6%95%99%E5%AE%A4) | Gets the classroom information. |
| [change room info](https://github.com/AgoraIO-Usecase/eEducation/wiki/Agora-Edu-%E4%BA%91%E6%9C%8D%E5%8A%A1#change-room-info) | Changes the classroom information. |
| [change user info](https://github.com/AgoraIO-Usecase/eEducation/wiki/Agora-Edu-%E4%BA%91%E6%9C%8D%E5%8A%A1#change-user-info) | Changes the user information. |

- Agora RTM SDK

| API | Function |
| ---------------- | ---------------- |
| [initWithAppId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:)      | Creates an AgoraRtmKit instance.      |
| [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) | Logs into the Agora RTM system. |
| [createChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | Creates an Agora RTM channel. You can create multiple channels with an AgoraRtmKit object. |
| [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) | Joins an Agora RTM channel.|
| [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:) | Sends a channel message, which can be received by all the users in the channel. |
| [leaveWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/leaveWithCompletion:) | Leaves the RTM channel. |
| [logoutWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/logoutWithCompletion:) | Logs out of the RTM system. |


- Agora RTC SDK

| API | Function |
| ---------------- | ---------------- |
| [sharedEngineWithAppId](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithAppId:delegate:)      | Initializes the AgoraRtcEngineKit object.      |
| [setChannelProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setChannelProfile:) | Sets the channel profile. In a Lecture Hall, we set the channel profile as Live Broadcast.|
| [setClientRole](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setClientRole:) | Sets the user role in a live broadcast. In a Big Online Classroom, we set the role of students as audience before they join the channel. During the class, if the student gets the permission to speak up, we switch the user role to broadcaster. |
| [joinChannelByToken](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) | Joins an Agora RTC channel. |
| [setupRemoteVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupRemoteVideo:) | Sets the remote video view. Call this method after the students join the channel, to configure the the view of the teacher on the students' client. |
| [setupLocalVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupLocalVideo:) | ets the local video view. Call this method after the student switches the user role to broadcaster, to configure the local view on the client. |
| [leaveChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) | Leaves the RTC channel. |



## Additional functions

For more features and functions available for an  online class, you can refer to the following:


<details>
<summary>Monitor the network quality</summary>
Use the <code>networkQuality</code> callback of the Agora RTC SDK  to monitor the last-mile uplink and downlink network quality of every user in the channel. 
For more methods for reporting the real-time network quality, see the following guides:
<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/lastmile_quality_apple?platform=iOS">Lastmile tests</a></li>
<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/in-call_quality_apple?platform=iOS">In-call Stats</a></li>
</details>
<details>
<summary>Mute the local audio or video</summary>
Call the following methods provided by the Agora RTC SDK:
<li><code>muteLocalAudioStream</code>, to stop or resume sending the local audio stream.</li>
<li><code>muteLocalVideoStream</code>, to stop or resume sending the local video stream.</li>
</details>
<details>
<summary>Voice detection</summary>
For RTC SDKs later than v2.9.2, you can enable voice detection by calling <code>enableAudioVolumeInfication</code>, and setting the <code>report_vad</code> parameter as <code>true</code>.
Once enabled, the <code>reportAudioVolumeIndicationOfSpeakers</code> callback reports whether the local user is speaking in the <code>AgoraRtcAudioVolumeInfo</code> struct.
</details>
<details>
<summary>Whiteboard</summary>
Implement the following whiteboard functions in your project:
	<li><a href="https://developer-en.netless.link/docs/ios/guides/ios-document/">Document Conversion</a></li>
		<li><a href="https://developer-en.netless.link/docs/ios/guides/ios-state/">State Management</a></li>
	<li><a href="https://developer-en.netless.link/docs/ios/guides/ios-tools/">Tools</a></li>
	<li><a href="https://developer-en.netless.link/docs/ios/guides/ios-view/">Perspective operation</a></li>
	<li><a href="https://developer-en.netless.link/docs/ios/guides/ios-operation/">Whiteboard Operation</a></li>
	<li><a href="https://developer-en.netless.link/docs/ios/guides/ios-scenes/">Page (Scene) Management</a></li>
</details>


## Open-source demo project

Agora provides an open-source demo for [Lecture Hall](https://github.com/AgoraIO-Usecase/eEducation) on GitHub to download as a source code reference.
