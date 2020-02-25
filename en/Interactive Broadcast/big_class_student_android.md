
---
title: Build a Client for the Students
description: 
platform: Android
updatedAt: Tue Feb 25 2020 05:39:16 GMT+0800 (CST)
---
# Build a Client for the Students
This section describes how to implement an Android client for the students.

## Flowchart

This flowchart shows the major logic of the students joining and leaving the classroom:

![](https://web-cdn.agora.io/docs-files/1582536817886)

## Integrate the SDK

Refer to the following table to download the SDKs, and integrate the SDKs into your project.


| Product | SDK download | Integration guide |
| ---------------- | ---------------- | ---------------- |
| [RTC (Real-time Communication) SDK](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)      | [Agora SDK for Android](https://download.agora.io/sdk/release/Agora_Native_SDK_for_Android_v2_9_0_102_FULL_20200216_1288.zip)      | [Start a Live Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/start_live_android?platform=Android) |
| [RTM (Real-time Messaging) SDK](https://docs.agora.io/en/Real-time-Messaging/product_rtm?platform=All%20Platforms) | [Real-time Messaging SDK](https://docs.agora.io/en/Real-time-Messaging/downloads) | [Peer-to-peer or Channel Messaging](https://docs.agora.io/en/Real-time-Messaging/messaging_android?platform=Android) |
| [Whiteboard](https://developer-en.netless.link/docs/android/overview/android-introduction/) | [White SDK](https://developer-en.netless.link/docs/android/quick-start/android-prepare/) | [Whiteboard quickstart](https://developer-en.netless.link/docs/android/quick-start/android-init-sdk/) |



## Core API call sequence

Refer to the following diagram to implement the basic real-time communication and messaging functions in your project with the Agora RTC SDK and RTM SDK.

![](https://web-cdn.agora.io/docs-files/1582602524115)

## Core API reference

- RTM SDK

| API | Function |
| ---------------- | ---------------- |
| [createInstance](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6411640143c4d0d0cd9481937b754dbf)      | Creates an RtmClient object.      |
| [login](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) | Logs into the Agora RTM system. |
| [createChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a) | Creates an Agora RTM channel. You can create multiple channels with an RtmClient object. |
| [join](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) | Joins an Agora RTM channel. |
| [getChannelAttributes](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6) | Gets the information of a specified channel. |
| [queryPeersOnlineStatus](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) | Queries the online status of a specified user. |
| [ceateMessage](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3) | Creates an RtmMessage object.  |
| [sendMessageToPeer](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a) | Sends a peer-to-peer message. Use this method on the students' client to send the hands-up message. |
| [setEnableNotificationToChannelMembers](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_channel_attribute_options.html#a2f240727791b3ad1af97f4a399ce1579) | Notifies all users in the RTM channel of the current channel information update. When the local channel information changes, all remote users receive the onAttributesUpdated callback. |
| [addOrUpdateChannelAttributes](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23) | Adds or Updates the information of a specified channel.  |
| [sendMessage](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a6e16eb0e062953980a92e10b0baec235) | Sends a channel message, which can be received by all the users in the channel. |
| [leave](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a9e0b6aad17bfceb3c9c939351a467d14) | Leaves the RTM channel. |
| [logout](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6f5695854e251ddd4ba05547ab47b317) | Logs out of the RTM systtem. |

- RTC SDK

| API | Function |
| ---------------- | ---------------- |
| [create](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35466f690d0a9332f24ea8280021d5ed)      | Creates an RtcEngine object.      |
| [setChannelProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1bfb76eb4365b8b97648c3d1b69f2bd6) | Sets the channel profile. In a Big Online Classroom, we set the channel profile as Live Broadcast. |
| [setClientRole](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa2affa28a23d44d18b6889fba03f47ec) | Sets the user role in a live broadcast. In a Big Online Classroom, we set the role of students as audience before they join the channel. During the class, if the student gets the permission to speak up, we switch the user role to broadcaster.  |
| [joinChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c) | Joins an Agora RTC channel. |
| [setupRemoteVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0e9f693c9bc2ccb91554c2c7dc6b7140) | Sets the remote video view. Call this method after the students join the channel, to configure the the view of the teacher on the students' client.|
| [setupLocalVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1fa43a5ce24196e840bcb1062cadbf23) | Sets the local video view. Call this method after the student switches the user role to broadcaster, to configure the local view on the client. |
| [leaveChannel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a2929e4a46d5342b68d0deb552c29d597) | Leaves the RTC channel. |


## Additional functions

For more features and functions available for an  online class, you can refer to the following:


<details>
<summary>Monitor the network quality</summary>
Use the <code>onNetworkQuality</code> callback of the Agora RTC SDK  to monitor the last-mile uplink and downlink network quality of every user in the channel. 
For more methods for reporting the real-time network quality, see the following guides:
<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/lastmile_quality_android?platform=Android">Lastmile Tests</a></li>
<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/in-call_quality_android?platform=Android">In-call Stats</a></li>
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
Once enabled, the <code>onAudioVolumeIndication</code> callback reports whether the local user is speaking in the <code>AudioVolumeInfo</code> struct.
</details>
<details>
<summary>Whiteboard</summary>
Implement the following whiteboard functions in your project:
	<li><a href="https://developer-en.netless.link/docs/android/guides/android-document/">Document Conversion</a></li>
		<li><a href="https://developer-en.netless.link/docs/android/guides/android-state/">State Managment</a></li>
	<li><a href="https://developer-en.netless.link/docs/android/guides/android-tools/">Tools</a></li>
	<li><a href="https://developer-en.netless.link/docs/android/guides/android-view/">Perspective Operation</a></li>
	<li><a href="https://developer-en.netless.link/docs/android/guides/android-operation/">Whiteboard Operation</a></li>
	<li><a href="https://developer-en.netless.link/docs/android/guides/android-scenes/">Page (Scene) Management</a></li>
</details>


## Open-source demo project

Agora provides an open-source demo for [Big Online Clasroom](https://github.com/AgoraIO-Usecase/eEducation) on GitHub to download as a source code reference.
