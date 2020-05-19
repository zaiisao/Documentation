
---
title: Release Notes
description: 
platform: Windows
updatedAt: Tue May 19 2020 08:49:21 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Video SDK.

## Overview

The Video SDK for Windows supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) and [Video Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

The Windows Video SDK supports the X86 and X64 architecture.

## v3.0.0.2

v3.0.0.2 was released on Apr 22, 2020.

**New features**

#### Specify the area of connection

This release adds `areaCode` member in the `RtcEngineContext` struct for specifying the area of connection when creating an `IRtcEngine` instance. This advanced feature applies to scenarios that have regional restrictions. You can choose from areas including Mainland China, North America, Europe, Asia (excluding Mainland China), and global (default).

After specifying the area of connection:

- When the app that integrates the Agora SDK is used within the specified area, it connects to the Agora servers within the specified area under normal circumstances.
- When the app that integrates the Agora SDK is used out of the specified area, it connects to the Agora servers either in the specified area or in the area where the SDK is located.

**Issues fixed**

This release fixed the occasional black screen issue when a co-host shares the screen.

**API changes**

#### Added

`areaCode` member in the [`RtcEngineContext`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) struct

## v3.0.0

v3.0.0 was released on Mar 5, 2020.

In this release, Agora improves the user experience under poor network conditions for both the Communication and Live-broadcast profiles through the following measures:
- Adopting a new architecture for the Communication profile.
- Upgrading the last-mile network strategy for both the Communication and Live-broadcast profiles,  which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.0 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.0.

We also upgrade the On-premise Recording SDK to v3.0.0. Ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**Compatibility changes**

#### 1. Dual-stream mode not enabled in the Communication profile

As of v3.0.0, the native SDK does not enable the [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode) by default in the Communication profile. Call the `enableDualStreamMode(true)` method after joining the channel to enable it. In video scenarios with multiple users, we recommend enabling the dual-stream mode.

**New features**

#### 1. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the `IChannel` and `IChannelEventHandler` classes. By creating multiple `IChannel` objects, a user can join the corresponding channels at the same time.
After joining multiple channels, users can receive the audio and video streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams. See [Join multiple channels](../../en/Interactive%20Broadcast/multiple_channel_windows.md) for details.

#### 2. Raw video data

Adds the following C++ callbacks to the `IVideoFrameObserver` class to provide raw video data at different video transmission stages, and to accommodate more scenarios.
- `onPreEncodeVideo`: Gets the video data after pre-processing and prior to encoding. This method applies to the scenarios where you need to pre-process the video data.
- `getSmoothRenderingEnabled`: Sets whether to smooth the acquired video frames. The smoothed video frames are more evenly spaced, providing a better rendering experience.

#### 3. Adjusting the playback volume of the specified remote user

Adds `adjustUserPlaybackSignalVolume` for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or a live broadcast to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

#### 4. Image enhancement

Adds `setBeautyEffectOptions` for enabling image enhancement in scenarios such as video social networking, an online class, or an interactive broadcast. You can call this method to set parameters including contrast, brightness, smoothness, red saturation, and so on. See [Image enhancement](../../en/Interactive%20Broadcast/image_enhancement_windows.md) for details.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile of `AUDIO_PROFILE_DEFAULT (0)` in the Live-Broadcast profile.

| SDK   | AUDIO_PROFILE_DEFAULT (0)                                   |
| :--------- | :---------------------------------------------------------- |
| v3.0.0      | A sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 52 Kbps. |
| Earlier than v3.0.0 | 3A sample rate of 32 kHz, music encoding, mono, and a bitrate of up to 64 Kbps. |

#### 2. Mirror mode

This release enables you to set a mirror effect for the stream to be encoded or for the streams to be rendered.

- Setting a mirror mode for the stream to be encoded: This release adds the `mirrorMode` member to the `VideoEncoderConfiguration` struct for setting a mirror effect for the stream to be encoded and transmitted.
- Setting a mirror mode for the streams to be rendered: 
    - This release also adds the `setLocalRenderMode` and `setRemoteRenderMode` methods, both of which take an extra `mirrorMode` parameter. During a call, you can use `setLocalRenderMode` to update the mirror effect of the local view or `setRemoteRenderMode` to update the mirror effect of the remote view on the local device.
    - We also add the `mirrorMode` member to the `VideoCanvas` struct. You can use `setupLocalVideo` to set a mirror effect for the local view, or use `setupRemoteVideo` to set a mirror effect for the remote view on the local device.

#### 3. Quality statistics

Adds the following members in the `RtcStats` class for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`  

#### 4. Screen sharing

This release enables window sharing of [UWP](https://docs.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) (Universal Windows Platform) applications when you call [`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52).

#### 5. Others

This release enables interoperability between the RTC Native SDK and the RTC Web SDK by default, and deprecates the `enableWebSdkInteroperability` method.

**Issues fixed**

* Audio issues relating to audio mixing, audio encoding, and echoing.
* Video issues relating to the watermark, aspect ratio, video sharpness, black outline appearing while screen sharing and toggling to full-screen.
* Other issues relating to app crashes, log file, and unstable service during CDN live streaming.

**API changes**

#### Added

- [`setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5899cc462e5250028c9afada4df98d48)
- [`onPreEncodeVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a2be41cdde19fcc0f365d4eb14a963e1c)
- [`getSmoothRenderingEnabled`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#aaa6c67373bb237a067318015749e8e51)
- [`setLocalRenderMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac433e6e88da91f87c107012cbaf8bb5c)
- [`setRemoteRenderMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aaaf8535dcab7ab0d12bdc351b88d09c2)
- The [`mirrorMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#a1ca537cb6966901330bce3681194ceb5) member in the [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) struct
- The [`mirrorMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_canvas.html#a7af6b90428fdcbd226f94df3cf6df9fc) and [`channelId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_canvas.html#ac16654ea954794b0e3ec8b962b4807a6) members in the [`VideoCanvas`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_canvas.html) struct
- The [`channelId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html#ab67471def88118f010e6d5add7d83f64) member in the [`AudioVolumeInfo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) struct
- [`createChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine2.html#a9cabefe84d3a52400f941f1bd8c0f486)
- [`IChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel.html) class
- [`IChannelEventHandler`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel_event_handler.html) class
- The [`gatewayRtt`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a30cc889ef34f63e23950b54591218cb5), [`memoryAppUsageRatio`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aab879ec9c52f2dd8d662c26c4939a7d3), [`memoryTotalUsageRatio`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aae6d57e709b08258be372960f9e19fd6) and [`memoryAppUsageInKbytes`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a17258e12476bb9106ccc248af4cfe734) members in the [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) class

#### Deprecated

* `RtcEngineParameters` class
* `enableWebSdkInteroperability`
* `setLocalRenderMode¹`
* `setRemoteRenderMode¹`
* `setLocalVideoMirrorMode`
* `onFirstRemoteVideoFrame`, replaced by [`onRemoteVideoStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) 
* `onUserMuteAudio`, `onFirstRemoteAudioDecoded`, and `onFirstRemoteAudioFrame`, replaced by [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
* `onStreamPublished` and `onStreamUnpublished`, replaced by [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)

## v2.9.3

v2.9.3 was released on Feb 10, 2020.

This release fixed the following issues:
- The `setRemoteSubscribeFallbackOption` method, which should work in the Live-broadcast profile only, also works in the Communication profile.
- In some one-to-one communication, the downlink media stream falls back to audio-only under poor network conditions.

## v2.9.1
v2.9.1 is released on Sep 19, 2019.

**New features**

#### 1. Detecting local voice activity

This release adds the `report_vad(bool)` parameter to the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb) method to enable local voice activity detection. Once it is enabled, you can check the [`AudioVolumeInfo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) struct of the [`onAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aab1184a2b276f509870c055a9ff8fac4) callback for the voice activity status of the local user.

#### 2. Supporting RGBA raw video data

This release supports RGBA raw video data. Use the [`getVideoFormatPreference`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a440e2a33140c25dfd047d1b8f7239369) method to set the format of the raw video data format.

You can also rotate or mirror the RGBA raw data using the [`getRotationApplied`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afd5bb439a9951a83f08d8c0a81468dcb) or [`getMirrorApplied`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afc5cce81bf1c008e9335a0423ca45991) methods respectively.

**Improvements**

#### 1. Improving the watermark function in Live Broadcasts

This release adds a new [`addVideoWatermark`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a36e51346ec3bd5a7370d7c70d58d9394) method with the following settings:

- The `visibleInPreview` member sets whether the watermark is visible in the local preview.
- The `positionInLandscapeMode`/`positionInPortraitMode` member sets the watermark position when the encoding video is in landscape/portrait mode.

This release optimizes the watermark function, reducing the CPU usage by 5% to 20%.

The original `addVideoWatermark` method is deprecated.

#### 2. Supporting more audio sample rates for recording

To enable more audio sample rate options for recording, this release adds a new [`startAudioRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz. The original method supports only a fixed sample rate of 32 kHz and is deprecated.

**Issues fixed**

#### Miscellaneous

A typo in the IAgoraRtcEngine.h file.

**API changes**

To improve the user experience, we made the following changes in v2.9.1:

#### Added

- [`startAudioRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f)
- [`addVideoWatermark`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a36e51346ec3bd5a7370d7c70d58d9394)
- [`getVideoFormatPreference`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a440e2a33140c25dfd047d1b8f7239369)
- [`getRotationApplied`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afd5bb439a9951a83f08d8c0a81468dcb)
- [`getMirrorApplied`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afc5cce81bf1c008e9335a0423ca45991)
- The `report_vad` parameter in the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb) method
- The `vad` member in the [`AudioVolumeInfo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) class

#### Deprecated

- `startAudioRecording`
- `addVideoWatermark`

## v2.9.0

v2.9.0 is released on Aug 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`

If your app implements RTMP streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)

For how to implement the new methods, see [Push Streams to the CDN](../../en/Interactive%20Broadcast/cdn_streaming_windows.md).

#### 2. Reporting the state of the remote video

This release extends the [`onRemoteVideoStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) callback with more states of the remote video: STOPPED(0), STARTING(1), DECODING(2), FROZEN(3), and FAILED(4). It adds a reason parameter to the callback to indicate why the remote video state changes. The original `onRemoteVideoStateChanged` callback is deleted. If you upgrade your Native SDK to the latest version, ensure that you re-implement the `onRemoteVideoStateChanged` callback.

The new callback reports most of the remote video states, and therefore deprecates the following callbacks. You can still use them, but we do not recommend doing so.

- [`onUserEnableVideo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a91bef59a3659b6e6bcbe43eb203d0732)
- [`onUserEnableLocalVideo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a23189a2a10fb8b06b774543ac6bb322b)
- [`onFirstRemoteVideoDecoded`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a345a8441861b9dbdc7ffc36a6c6ba186)

<div class="alert note">The triggering timing of the new callback is different from the old one. The new <code>onRemoteVideoStateChanged</code> callback is triggered only when the remote video state has changed.</div>

#### 3. Disabling/enabling the local audio

To improve the audio quality in the Communication profile, this release sets the system volume to the media volume after you call the `enableLocalAudio`(true) method. Calling `enableLocalAudio`(false) switches the system volume back to the in-call volume.

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab) method to enable the audience in a Live Broadcast channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the `leaveChannel` and `joinChannel` methods. After the audience successfully switches to another channel by calling the `switchChannel` method, the SDK triggers the `onLeaveChannel` and `onJoinChannelSuccess` callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different Live Broadcast channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)

During the media stream relay, the SDK reports the states and events of the relay with the [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22) and [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host across Channels](../../en/Interactive%20Broadcast/media_relay_windows.md).

#### 3. Reporting the local and remote audio state

This release adds the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) and [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: STOPPED(0), RECORDING(1), ENCODING(2), or FAILED(3). When the state is FAILED(3), see the `error` parameter for troubleshooting.
- The remote audio: STOPPED(0), STARTING(1), DECODING(2), FROZEN(3), or FAILED(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`onLocalAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the `RtcStats`, `LocalVideoStats`, and `RemoteVideoStats` classes:

- [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html): The total number of the sent audio bytes, sent video bytes,  received audio bytes, and received video bytes during a session.
- [`LocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html): The encoding bitrate, the width and height of the encoding frame, the number of frames, and the codec type of the local video.
- [`RemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html): The packet loss rate of the remote video.

#### 2. Improving the live broadcast video quality

This release minimizes the video freeze rate under poor network conditions, improves the video sharpness, and optimizes the video smoothness when the packet loss rate is high.

#### 3. Improving the screen sharing quality

This release improves the sharpness of text during screen sharing in the Communication profile, particularly when the network condition is poor. Note that this improvement takes effect only when you set ContentHint as Details(2).

#### 4. Supporting hot-swappable devices

To improve the experience in using video devices on Windows, this release adds support for hot-swappable devices with the `context` member in the [`RtcEngineContext`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) class. Setting this member also achieves the following functions:

- Supports plugging or unplugging video devices before or after joining the channel.
- Supports plugging or unplugging video devices when the app uses the external video renderer.
- Fixes crashes caused by plugging or unplugging the camera when the device is powered on.
- Minimizes the time to join a channel.

#### 5. Other Improvements

- Improves the audio quality when the audio scenario is set to Game Streaming.
- Improves the audio quality after the user disables the microphone in the Communication profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Invalid call of the `muteRemoteAudioStream` method.
- Occasionally no audio. 

#### Video

- The `onRemoteVideoStateChanged` callback behaves unexpectedly. 

#### Miscellaneous

- Occasionally mixed streams in RTMP streaming. 
- Occasional crashes occur.
- Failure to join the channel.

**API changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added

- [`onLocalAudioStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a)
- [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
- [`onRemoteVideoStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e)
- [`onLocalAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3)
- [`switchChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab)
- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429)
- [`RtcEngineContext`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html): context
- [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html): `txAudioBytes`, `txVideoBytes`, `rxAudioBytes`, and `rxVideoBytes`
- [`LocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html): `encodedBitrate`, `encodedFrameWidth`, `encodedFrameHeight`, `encodedFrameCount`, and `codedType`
- [`RemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html): `packetLossRate`

#### Deprecated

- `onMicrophoneEnabled`. Use LOCAL_AUDIO_STREAM_STATE_CHANGED(0) or LOCAL_AUDIO_STREAM_STATE_RECORDING(1) in the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) callback instead. 
- `onRemoteAudioTransportStats`. Use the [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a) callback instead.
- `onRemoteVideoTransportStats`. Use the [`onRemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7163ffb650852be270ba0215b596d968) callback instead.
- `onUserEnableVideo`. Use the [`onRemoteVideoStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) callback with the following parameters instead:
	- REMOTE_VIDEO_STATE_STOPPED(0) and REMOTE_VIDEO_STATE_REASON_REMOTE_MUTED(5).
	- REMOTE_VIDEO_STATE_DECODING(2) and REMOTE_VIDEO_STATE_REASON_REMOTE_UNMUTED(6).

- `onUserEnableLocalVideo`. Use the [`onRemoteVideoStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) callback with the following parameters instead:
	- REMOTE_VIDEO_STATE_STOPPED(0) and REMOTE_VIDEO_STATE_REASON_REMOTE_MUTED(5).
	- REMOTE_VIDEO_STATE_DECODING(2) and REMOTE_VIDEO_STATE_REASON_REMOTE_UNMUTED(6).

- `onFirstRemoteVideoDecoded`. Use REMOTE_VIDEO_STATE_STARTING(1) or REMOTE_VIDEO_STATE_DECODING(2) in the [`onRemoteVideoStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae69799238c6a6cd9f017274dd630b74e) callback instead.

#### Deleted

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`
- `onRemoteVideoStateChanged`

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string user IDs

Many apps use string user IDs. This release adds the following methods to enable apps to join an Agora channel directly with string user IDs as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. 

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your user IDs into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote audio and video statistics

To monitor the audio and video transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) and [RemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) classes, to report the audio and video freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [RemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class.

**Improvements**

This release adds a `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` member to the `reason` parameter of the [onConnectionStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [getUserInfoByUid](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [onLocalUserRegistered](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [onUserInfoUpdated](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class
- The `totalFrozenTime` and `frozenRate` members in the [RemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) class

#### Deprecated

- The `lowLatency` member in the [LiveTranscoding](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) class


## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

**Compatibility changes**

Ensure that you read the following SDK behavior changes if you migrate from an earlier SDK version.

#### 1. Publishing streams to the RTMP

To improve the usability of the RTMP streaming service, v2.4.1 defines the following parameter limits:

| Class / Interface  | Parameter Limit                                              |
| ---------------------- | ------------------------------------------------------------ |
| [LiveTranscoding](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html)        | <li>[videoFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a8086eda68ed5b9d106cf830fd4e24f9c): Frame rate (fps) of the CDN live output video stream. The value range is [0, 30], and the default value is 15. Agora adjusts all values over 30 to 30.<li>[videoBitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#ab5ff17deab2e3149f149987cc0d83433): Bitrate (Kbps) of the RTMP live output video stream. The default value is 400. Set this parameter according to the [Video Bitrate Table](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#af10ca07d888e2f33b34feb431300da69). If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.<li>[videoCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a72fbfdd2c0d4f0cc438d5ca052aa7531): The video codec profile. Set it as **BASELINE**, **MAIN**, or **HIGH** (default). If you set this parameter to other values, Agora adjusts it to the default value of **HIGH**.<li>[width](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#ad0a505490500917dd246510471ef88be) and [height](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a340ee10a10bf0137fb996ca77729002e): Pixel of the video. The minimum value of width x height is 16 x 16.</li> |
| [RtcImage](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_image.html)             | url: The maximum length of this parameter is **1024** bytes. |
| [addPublishStreamUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)    | `url`: The maximum length of this parameter is **1024** bytes. |
| [removePublishStreamUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7) | `url`: The maximum length of this parameter is **1024** bytes. |

This release also adds the [audioCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a76987efee0f6d507ef648083507bd935) parameter in the `LiveTranscoding` class to set the audio codec profile type. The default type is LC-AAC, which means the low-complexity audio codec profile.

v2.4.1 also adds five error codes to the `error` parameter in the  [onStreamPublished](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a85cc73e9f53766b8a0a7fa8c96f2ea98) method for quick troubleshooting.

#### 2. Renaming the receivedFrameRate parameter in the RemoteVideoStats class

v2.4.1 renames the `receivedFrameRate` parameter to [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0) in the  [RemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) class to more accurately describe the statistics of the remote video stream.

**New features**

#### 1. Adding media metadata

In live broadcast scenarios, the host can send shopping links, digital coupons, and online quizzes to the audience for more diversified live broadcast interactions. v2.4.1 adds the [registerMediaMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a84dbf06de1d769b63200d7ec0289cca0) interface and the [IMediaMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_metadata_observer.html) class, allowing the host to add metadata to the output video and to send media attached information.

#### 2. Optimized screen sharing

To avoid image cropping and distortion in screen sharing, v2.4.1 optimizes the encoding algorithms. In this release Agora applies the following encoding algorithms:

Suppose the value of **dimensions** is 1920 x 1080 pixels, that is, 2073600 pixels:

- If the value of the screen `dimensions` is lower than that of the encoding dimensions, for example, 1000 x 1000 pixels, the SDK uses 1000 x 1000 pixels for encoding.
- If the value of the screen `dimensions` is higher than that of the encoding dimensions, for example, 2000 x 2000 pixels, the SDK uses the maximum value under 1920 x 1080 pixels with the aspect ratio of the screen dimension (1:1) for encoding, that is, 1440 x 1440 pixels.

Agora uses the [dimensions](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#a9ee8c96bbfe031e8c614d7b2e49ddbb8) value in the[ScreenCaptureParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html) class to calculate the charges. If you do not set the value of **dimensions**, the SDK uses the default value of 1920 x 1080 to calculate the charges.

You can also choose whether or not to capture the mouse cursor when sharing the screen. v2.4.1 adds the [captureMouseCursor](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#aa68157203616423bad85f852f3816c1f) parameter in the `ScreenCaptureParameters` class and captures the mouse by default.

#### 3. State of the local video

v2.4.1 adds the [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) callback to indicate the local video state. In this callback, the SDK returns the `STOPPED`,` CAPTURING`, `ENCODING`, or `FAILED` state. When the state is `FAILED`, you can use the error code for troubleshooting. This callback indicates whether or not the interruption is caused by capturing or encoding. This release deprecates the `onCameraReady` and `onVideoStopped` callbacks.

#### 4. State of the RTMP streaming

v2.4.1 adds the [onRtmpStreamingStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d) callback to indicate the state of the RTMP streaming and help you troubleshoot issues when exceptions occur. In this callback, the SDK returns the IDLE, `CONNECTING`, `RUNNING`, `RECOVERING`, or `FAILURE` state. When the state is `FAILURE`, you can use the error code for troubleshooting. You can still use the `onStreamPublished` and `onStreamUnpublished` callbacks, but we do not recommend using them.

#### 5. More reasons for a network connection state change

In the [onConnectionStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback, v2.4.1 adds error codes to the `reason` parameter to help you troubleshoot issues when exceptions occur. The SDK returns the `onConnectionStateChanged` callback whenever the connection state changes. This release also deprecates `WARN_LOOK_UP_CHANNEL_REJECTED(105)`, `ERR_TOKEN_EXPIRED(109)`, and `ERR_INVALID_TOKEN(110)`.

#### 6. State of the local network type 

v2.4.1 adds the [onNetworkTypeChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4) callback to indicate the local network type. In this callback, the SDK returns the `UNKNOWN`, `DISCONNECTED`, `LAN`, `WIFI`, `2G`, `3G`, or `4G` type. When the network connection is interrupted, this callback indicates whether or not the interruption is caused by a network type change or poor network conditions.

#### 7. Getting the audio mixing volume

v2.4.1 adds the [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aed9dda5a7b2683776f41f6ba0e1f281c) and [getAudioMixingPublishVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027) methods, which respectively gets the audio mixing volume for local playback and remote playback, to help you troubleshoot audio volume related issues.

#### 8. Reporting when the first remote audio frame is received and decoded

To get the more accurate time of the first audio frame from a specified remote user, v2.4.1 adds the [onFirstRemoteAudioDecoded](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7d375e7467bd099ad2d4c9cfc0a6c242) callback to report to the app that the SDK decodes first remote audio. This callback is triggered in either of the following scenarios:

- The remote user joins the channel and sends the audio stream.
- The remote user stops sending the audio stream and re-sends it after 15 seconds.

The difference between the `onFirstRemoteAudioDecoded` and `onFirstRemoteAudioFrame` callbacks is that the `onFirstRemoteAudioFrame` callback occurs when the SDK receives the first audio packet. It occurs before the `onFirstRemoteAudioDecoded` callback.

#### 9. Miscellaneous

- v2.4.1 supports 64-bit operation systems.

**Improvements**

#### 1. Reporting more statistics

- v2.4.1 adds the [txPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a52232c2b3af1573b3783f74f7ca75c1c) and [rxPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a1882c369c5d1bc04b85fb9dcb15700b6) parameters in the [RtcStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) class. These parameters return the packet loss rate from the local client to the server and vice versa.

- To provide more accurate statistics of the local and remote video, v2.4.1 makes the following changes to the following classes:
  - [LocalVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html): Adds the [encoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ab5df1607ac79acbc51941797189b8dba) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#aa91d7d73f3d2c2933658a9dced6ec3ed) parameters
  - [RemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html): Adds the [decoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a21776c26b256d836a90944c1051c9322) parameter, and renames the receivedFrameRate parameter to the [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0) parameter

#### 2. Miscellaneous

- Improved the sound quality of the GAME_STREAMING audio scenario.
- Reduced the audio and video latency.
- Reduced the SDK package size by 0.5 M.
- Improved the accuracy of the network quality after users change the video bitrate.
- Enabled the audio quality notification callback by default, that is, enabled the [onRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a) callback without calling the `enableAudioQualityIndication` method.
- Improved the stability of video services.
- Improved the stability of RTMP streaming services.
- Improved the compatibility of the SDK with video devices.

**Issues fixed**

#### Audio

- The app cannot play multiple local audio effect files simultaneously. - windows

#### Video

- The sending client sends the low stream unexpectedly.
- The user cannot switch between the screen sharing stream and the camera stream. 

#### Miscellaneous

- Users still receive the `onNetworkQuality` callback after leaving the channel.
- Occasional crashes. 

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Unified the C++ interface for all platforms

v2.4.1 unifies the behavior of the C++ interfaces across different platforms so that you can apply the same code logic on different platforms. v2.4.1 implements the methods of the `RtcEngineParameters` class in the `IRtcEngine` class. Refer to [Agora C++ API Reference for All Platforms home page](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/index.html) for the applicable platforms and considerations of each interface.

#### Added

- [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aed9dda5a7b2683776f41f6ba0e1f281c)
- [getAudioMixingPublishVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027)
- [onFirstRemoteAudioDecoded](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7d375e7467bd099ad2d4c9cfc0a6c242)
- [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6)
- [onNetworkTypeChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4)
- [onRtmpStreamingStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4)
- [registerMediaMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a84dbf06de1d769b63200d7ec0289cca0)
- The [IMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_metadata_observer.html) class
- The [audioCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a76987efee0f6d507ef648083507bd935) parameter in the `LiveTranscoding` class
- The [captureMouseCursor](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#aa68157203616423bad85f852f3816c1f) parameter in the `ScreenCaptureParameters` class
- The [txPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a52232c2b3af1573b3783f74f7ca75c1c) and [rxPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a1882c369c5d1bc04b85fb9dcb15700b6) parameters in the `RtcStats` class
- The [encoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ab5df1607ac79acbc51941797189b8dba) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#aa91d7d73f3d2c2933658a9dced6ec3ed) parameters in the `LocalVideoStats` class
- The [decoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a21776c26b256d836a90944c1051c9322) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0) (to replace `receivedRemoteRate`) parameters in the `RemoteVideoStats` class

#### Deprecated

- `enableAudioQualityIndication`
- `onCameraReady`. Use LOCAL_VIDEO_STREAM_STATE_CAPTURING(1) in the [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) callback instead.
- `onVideoStopped`. Use LOCAL_VIDEO_STREAM_STATE_STOPPED(0) in the [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) callback instead.
- The `WARN_LOOKUP_CHANNEL_REJECTED(105)` warning code. Use CONNECTION_CHANGED_REJECTED_BY_SERVER(10) in the [onConnectionStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback instead.
- The `ERR_TOKEN_EXPIRED(109)` error code. Use CONNECTION_CHANGED_TOKEN_EXPIRED(9) in the [onConnectionStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback instead.
- The `ERR_INVALID_TOKEN(110)` error code. Use CONNECTION_CHANGED_INVALID_TOKEN(8) in the [onConnectionStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback instead.
- The `ERR_START_CAMERA(1003)` error code. Use LOCAL_VIDEO_STREAM_ERROR_CAPTURE_FAILURE(4) in the [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) callback instead.
- The `ERR_VDM_WIM_DEVICE_IN_USE(1502)` error code. Use LOCAL_VIDEO_STREAM_ERROR_DEVICE_BUSY(3) in the  in the [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) callback instead.

## v2.4.0 and earlier

**v2.4.0**

v2.4.0 is released on April 1, 2019.

#### New features

##### 1. Advanced screen sharing

v2.4.0 upgrades screen sharing and provides the following advanced functions:

- Shares the whole or part of a specified screen in a multi-display environment ([`startScreenCaptureByScreenRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)).
- Shares the whole or part of a specified window ([`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)) .
- Sets the content hint of the screen sharing to prioritize motion or detail ([`setScreenCaptureContentHint`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)).
- Sets the dimensions, frame rate and bitrate for screen sharing ([`updateScreenCaptureParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)).

v2.4.0 deprecates the `startScreenCapture` method. We recommend using the new methods for screen sharing. With the new methods, developers need to design the code logic to obtain the `screenRect` and `windowId`. For more information, see [Share the Screen](../../en/Interactive%20Broadcast/screensharing_windows.md).

##### 2. Voice changer and voice reverberation

Adding voice changer and reverberation effects in an audio chat room brings much more fun. v2.4.0 adds the [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda) and [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b) methods, allowing you to change your voice or reverberation by choosing from the preset options.

##### 3. Tracking the sound position of a remote user 

v2.4.0 adds the [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d) and [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20). Call the [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d) method before joining a channel to enable stereo panning for the remote users, and then you can call the [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20) method to track the position of a remote user.

##### 4. Pre-call last-mile network probe test

Conducting a last-mile probe test before joining the channel helps the local user to evaluate or predict the uplink network conditions. v2.4.0 adds the [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b), [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593), and [`onLastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5) APIs, allowing you to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

##### 5. Audio device loopback test

v2.4.0 adds the [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac78c08f3212dc3efa000e197207dec53) and [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aad01da1e0bacd3f2fd355483f9e3befb) methods for testing whether the local audio devices are working properly. The test involves only the local audio devices and does not report the network condition.

##### 6. Setting the priority of a remote user's stream

v2.4.0 adds the [`setRemoteUserPriority`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aecb5d85e9b3a60947d569b88253da710) method for setting the priority of a remote user. You can use this method with the [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) method. If the fallback function is enabled for a remote stream, the SDK ensures the high-priority user gets the best possible stream quality.

##### 7. State of an audio mixing file 

v2.4.0 adds the [`onAudioMixingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22) callback to report any change of the audio-mixing file playback state (playback succeeds or fails) and the corresponding reason. This release also adds the warning code 701, which is triggered if the local audio-mixing file does not exist, or if the SDK does not support the file format or cannot access the music file URL when playing the audio-mixing file.

##### 8. Setting the log file size

The SDK has two log files, each with a default size of 512 KB. In case some customers require more than the default size, v2.4.0 adds the [`setLogFileSize`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a6fb256cb165856a4412bb30b098408a1) method for setting the log file size (KB).

##### 9. Setting the background image for LiveTranscoding

v2.4.0 adds the [`backgroundImage`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a729037c7cf31b57efd1e8c9fadeab6eb) parameter in the [`LiveTranscoding`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) Class in Windows to set the background image in the combined video of a live broadcast.

#### Improvements

##### 1. Accuracy of call quality statistics

- v2.4.0 adds the intervalInSeconds parameter to the startEchoTest method, allowing you to set the interval between when you speak and when the recording plays back.
- v2.4.0 adds three parameters to the [`LocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html) class: [`targetBitrate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#a3e8d46c9b67c9fe62487ec56bc587fd6) for setting the target bitrate of the current encoder, [`targetFrameRate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ac06928c5bf18c5db7eb4661dd783ba0a) for setting the target frame rate, and [`qualityAdaptIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#a902fc5b33956471b7a28f43399fa8b20) for reporting the quality of the local video since last count.

##### 2. Video encoder preferences

v2.4.0 provides the following options for setting video encoder preferences:

- Setting preferences under limited bandwidth. v2.4.0 adds two parameters to the [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) class: minFrameRate and degradationPrefer. You can use these parameters together to set the minimum video encoder frame rate and the video encoding degradation preference under limited bandwidth. 
- Setting the camera capture preference. v2.4.0 adds the [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af) method, allowing you to set the camera capture preference. You can choose system performance over video quality or vice versa as needed. For more information, see the [API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af).

##### 3. Core quality improvements

- Reduces the audio delay.
- Improves the video quality and stability.
- Shortens the time to render the first remote video frame. 
- Improves the video smoothness and reduces the time delay when sharing a screen under poor network conditions. 

#### Issues fixed

##### Audio

- Calling the `enableLocalAudio` method disconnects all connected Bluetooth devices.
- The SDK does not support audio mixing URLs with Chinese characters.
- Volume levels of the high-pitch sound are lowered.
- Sounds are occasionally played fast.

##### Video

- If you skip the `renderMode` setting, the video stretches due to a mismatch with the display.
- Video freezes on some lower-end devices.
- It takes too long to render the first received video frame.

##### Miscellaneous

- Occasional crashes.
- The stream for screen sharing is cropped when lowering the screen resolution. We highly recommend using the new methods in v2.4.0 for screen sharing. With the new screen sharing parameters, these methods separate the stream for screen sharing from the camera-captured stream. See [Share the Screen](../../en/Interactive%20Broadcast/screensharing_windows.md) for more information.
- A region cannot be shared if one of its coordinates is negative.
- Occasional echoes.
- The SEI information does not synchronize with the media stream when publishing transcoded streams to the CDN.

#### API changes

To improve your experience, we made the following changes to the APIs:

##### Added

- [`setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5899cc462e5250028c9afada4df98d48)
- [`startScreenCaptureByScreenRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)
- [`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)
- [`updateScreenCaptureParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)
- [`setScreenCaptureContentHint`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b)
- [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d)
- [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20)
- [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593)
- [`setRemoteUserPriority`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aecb5d85e9b3a60947d569b88253da710)
- [`startEchoTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a842ed126b6e21a39059adaa4caa6d021)
- [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac78c08f3212dc3efa000e197207dec53)
- [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aad01da1e0bacd3f2fd355483f9e3befb)
- [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)
- [`setLogFileSize`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a6fb256cb165856a4412bb30b098408a1)
- [`onAudioMixingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22)
- [`onLastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5) 

##### Deprecated

- `startEchoTest`
- `startScreenCapture`
- `setVideoQualityParameters`

**v2.3.3**

v2.3.3 is released on January 24, 2019. 

#### Improvements

v2.3.3 optimizes the screen-sharing algorithm for different scenarios. The video smoothness and quality are enhanced when a user presents slides or browses websites. v2.3.3 also improves the initial image quality in the Communication profile.

#### Issues fixed

Occasional inaccurate statistics returned in the `onNetworkQuality` callback.

**v2.3.2**

v2.3.2 is released on January 16, 2019. 

#### Compatibility changes

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the Live Broadcast profile.

Before upgrading your SDK, ensure that your version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

If you using an Agora SDK version v2.0.8 and wish to migrate the v2.3.2, refer to the [Migration Guide](../../en/Interactive%20Broadcast/migration_windows.md) for major API changes.

#### New features

##### 1. External video data (Push Mode)

v2.3.2 adds the following methods, allowing you to use the external video during a call or in a live broadcast:

- [`setExternalVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7): Configures the external video source.
- [`pushVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985): Pushes the external video frames.

The application can encapsulate the external video frames in the `AgoraVideoFrame` format and push them to the SDK for encoding and transmitting.

This feature applies to scenarios where a user captures and renders the external video and then pushes the video frames to the SDK for transmission (the application processes the rendering).

##### 2. External audio sink (Pull Mode)

v2.3.2 adds the following methods to improve the user experience in playing the external audio:

- [`setExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638): Sets the external audio sink.
- [`pullAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940): Pulls the external audio frame.

The application pulls the decoded audio frames (mixed from the remote users) from the media engine for external playback.

The difference between `pullAudioFrame` and [`onPlaybackAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac): 

- `pullAudioFrame`: The application pulls the audio frames and:

	- Specifies the number of audio samples for playback.
	- Adjusts the frame buffer. 
	- Allows for a delay in processing the audio frame. 
	
	This method avoids problems caused by jitter in the external audio, such as an unsynchronized audio playback.
	
- `onPlaybackAudioFrame`: The SDK pushes the audio frames to the application once every 10 ms. Any delay in processing the audio frames may result in audio jitter.

##### 3. Video quality in a live broadcast

v2.3.2 adds the [minBitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#ab3249ca88227aaf36f21681b9de7ced2) parameter (minimum encoding bitrate) in the [setVideoEncoderConfiguration](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

##### 4. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5e117be71d38d813208198f4064aa964) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing.

This release also changes the behavior of the [adjustPlaybackSignalVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a98919705c8b2346811f91f9ce5e97a79) method to control only the voice volume. Therefore, to mute the local audio playback, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods.

See [Adjust the Volume](../../en/Interactive%20Broadcast/volume_windows.md) for the scenarios and corresponding APIs.

##### 5. Fallback options for a live broadcast under unreliable network conditions

Unreliable network conditions affect the overall quality of a live broadcast. v2.3.2 adds the [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec) and [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) methods to allow the SDK to:

- Automatically disable the video stream when the network conditions cannot support both audio and video, or
- Enable the video when the network conditions improve. 

The SDK triggers the [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513) or [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74) callback when the stream falls back to audio-only or switches back to the video.

##### 6. Upstream and Downstream statistics of each remote user/host

v2.3.2 adds the [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d) and [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)  callbacks to provide the upstream and downstream statistics of each remote user/host. During a call or live broadcast, the SDK triggers these callbacks once every two seconds after the local user receives audio/video packets from a remote user. The callbacks return the user ID, received audio/video bitrate, packet loss rate, and network time delay (ms).

##### 7. New video encoder configuration

To support video rotation scenarios and improve the quality of the custom video source, v2.3.2 deprecates the [`setVideoProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb) method and replaces it with the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) method to set the video encoder configurations. The [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. You can still use the `setVideoProfil`e method, but Agora recommends using the `setVideoEncoderConfiguration` method to set the video profile.

##### 8. Virtual sound card

v2.3.2 adds the `deviceName` parameter in the [`enableLoopbackRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a065f485fd23b8c24a593680a47d754aa) method, allowing you to use a virtual sound card for audio recording:

- To use the current sound card, set deviceName as NULL.
- To use a virtual card, set `deviceName` as the name of the virtual card.

#### Improvements

##### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `onAudioQuality` callback and replaces it with the `onRemoteAudioStats` callback to improve the accuracy of the call quality statistics. The `onRemoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `onNetworkQuality` callback for the uplink and downlink network qualities.

- [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`onNetworkQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80003ae8cce02039f3aa0e8ffad7deed): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`onLastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33): Reports the last mile network quality of the local user before the user joins a channel.

##### 2. New network connection policy 

v2.3.2 adds the following API method and callback to get the current network connection state and reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362): Gets the connection state of the SDK.
- [`onConnectionStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`onConnectionInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483) and [`onConnectionBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `onConnectionStateChanged` callback when the network connection state changes. The SDK also triggers the `onConnectionInterrupted` and `onConnectionBanned` callbacks under certain circumstances, but Agora does not recommend using them.

##### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. Application developers can use this feedback for future product improvement. Agora strongly recommends integrating this method in your application.


##### 4. Improves the audio device usability

v2.3.2 optimizes the audio device selection to fix the no audio issue when a user switches the audio device during a call or live broadcast.

##### 5. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Improves the stability in pushing streams.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.
- Optimizes the screen-sharing function.

#### Issues fixed

The following issues are fixed in v2.3.2:

##### SDK

- Flashes when detecting a device.

##### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 
- Crashes in the audio module.

##### Video

- The users on the Web client cannot see the video sent from the Native client due to codec bugs.
- Hardware encoder issues when using an external video source on x86 devices.
- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.
- Occasional long intervals between joining a channel and seeing the first remote video frame.
- The remote user's video is mirrored when sharing part of the desktop and when the application calls the `setVideoProfile` method to specify the video resolution.
- Blurry video when sharing a high-resolution screen.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

#### API changes

To improve the user experience, Agora has made the following changes to the APIs:

##### Added

- [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
- [`setExternalVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pushVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)
- [`setExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)
- [`pullAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7)
- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734)
- [`onConnectionStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74)
- [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)
- [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d)
- [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)

##### Deprecated

- [`setVideoProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb)
- [`onAudioQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36ad42975f3545382de07875016fb7fa)
- [`onConnectionInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483)
- [`onConnectionBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404)

**v2.2.2**

v2.2.2 is released on June 21, 2018.

#### Issues fixed

- Fixed occasional online statistics crashes.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of occasional video freeze after a view size change.

**v2.2.1**

v2.2.0 is released on May 30, 2018 and improves the internal code implementation. 

**v2.2.0**

v2.2.0 is released on May 4, 2018. 

#### New features

##### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method to enable the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

##### 2. Deploy the Proxy at the server

Agora provides a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. 

##### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

##### 4. Add watermarks on the broadcasting video

Adds the watermark function for users to add a PNG file to the local or RTMP broadcast as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast. Adds the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface to add watermarks in RTMP broadcasts. 

#### Improvements

##### 1. Audio volume indication

Improves the <code>enableAudioVolumeIndication</code> method. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

##### 2. Network quality detection during a session

To meet the need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> callback improves its data accuracy. 

##### 3. Last mile network quality detection before joining a channel

To test if the network condition can support audio or video calls before joining a channel, the <code>onLastmileQuality</code> callback changes its detection base from a fixed bitrate to the bitrate set in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK still triggers this callback once every 2 seconds. 

##### 4. Audio quality enhancement

Improves the audio quality in scenarios that involve music playback.

#### Issues fixed

-   Occasional screen display abnormalities when a large number of audience members join as the host in a live-broadcast channel.
-   Occasional audio issues during a live broadcast.


**v2.1.3**

v2.1.3 is released on April 19, 2018. 

In v2.1.3, Agora updates the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

#### Issues fixed

Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.

#### Improvements

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to a normal communication or live-broadcast profile.

**v2.1.2**

v2.1.2 is released on April 2, 2018. 

>  If you upgrade the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth.

#### Issues fixed

-   The co-host is not seen by his/her counterpart after leaving and rejoining the channel.
-   The video resolution of the shared screen is worse in the communication profile than in the live broadcast profile.


**v2.1.1**

v2.1.1 is released on March 16, 2018. 

Agora has identified a critical bug in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

**v2.1.0**

v2.1.0 is released on March 7, 2018. 

#### New features

##### 1. Voice Optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

##### 2. Enhance the audio effect input from the built-in microphone

In an Interactive Broadcast scenario, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods to implement the expected voice equalization and reverberation effects.

##### 3. Online statistics query

Adds RESTful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host. See:

- Voice or Video Call scenario: [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md)
- Interactive Broadcast scenario: [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md)


##### 4. 17-way video

Adds the support of 17-way video in an Interactive Broadcast scenario


##### 5. Injecting an external video stream

Adds the function of injecting an external video stream to an ongoing Live Broadcast. See [Injecting an External Stream to a Live Broadcast](../../en/Interactive%20Broadcast/inject_stream_windows.md).


##### 6. Screen sharing for Interactive Broadcast

-   Before v2.1.0: The Agora SDK only supports the screen sharing function in a video call scenario.
-   From v2.1.0: The Agora SDK adds the screen-sharing function in interactive broadcasts.


##### 7. Loopback recording

Adds the <code>enableLoopbackRecording</code> method to collect all local sounds.

#### Improvements

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Video Freeze Rate</td>
<td>Reduces the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, host-in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are charged according to the voice-only mode. For example, 16 &times; 16.</td>
</tr>
</tbody>
</table>



#### Issues fixed

-   Occasional issues of the camera unable to capture the video source.
-   Occasional video freeze issues.
-   Occasional crashes.

**v2.0**

v2.0 is released on November 21, 2017. 

#### Compatibility changes

Developers,

Due to the upgrade of Agora products, Windows SDK 2.0 no longer supports the Agora Recording Server version 1.8.2 and before, or relevant APIs.

**This may affect:**

-   Users of both the Windows SDK and Agora Recording Server v1.8.2 or before.
-   The following API methods are no longer supported: startRecordingService, stopRecordingService, and refreshRecordingServiceStatus.

**Two solutions are available:**

1.  Migrate from the Agora Recording Server v1.8.2 or before to the Agora Recording SDK v1.12 or later. The Agora Recording SDK does not require recording on the client side and does not affect subsequent upgrades of the Windows SDK (recommended). For how to use the Agora Recording SDK, refer to:

-   [Integrate the SDK](../../en/Recording/recording_integrate_cpp.md)
-   [Record a Call](../../en/Recording/recording_cmd_cpp.md)
-   [API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/recording_cpp/index.html)

2.  If you want to continue using the Agora Recording Server, you can use your current Windows SDK version (v1.14 and before). Contact Technical Support for any questions.


#### New features

-   Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication profile to support dual streams.
-   Supports the external audio source in the communication and live broadcast profiles by adding the following API methods:

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
<tr><td><code>setExternalAudioSource</code></td>
<td>Sets the external audio source parameters, including the sample rate and channel number.</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>Pushes the external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>



- Provides a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast profiles. Contact [support@agora.io](mailto:support@agora.io) to enable the function, if required.
- Supports Windows (x64).
- Adds API methods such as setting the volume, mute states, and system volume settings.

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
<tr><td><code>setPlaybackDeviceVolume</code></td>
<td>Sets the volume of the playback device.</td>
</tr>
<tr><td><code>getPlaybackDeviceVolume</code></td>
<td>Gets the volume of the playback device.</td>
</tr>
<tr><td><code>setRecordingDeviceVolume</code></td>
<td>Sets the volume of the microphone.</td>
</tr>
<tr><td><code>getRecordingDeviceVolume</code></td>
<td>Gets the volume of the microphone.</td>
</tr>
<tr><td><code>setPlaybackDeviceMute</code></td>
<td>Mutes the playback device.</td>
</tr>
<tr><td><code>getPlaybackDeviceMute</code></td>
<td>Gets the mute status of the playback device.</td>
</tr>
<tr><td><code>setRecordingDeviceMute</code></td>
<td>Mutes the microphone.</td>
</tr>
<tr><td><code>getRecordingDeviceMute</code></td>
<td>Gets the mute status of the microphone.</td>
</tr>
<tr><td><code>onAudioDeviceVolumeChanged</code></td>
<td>Notifies the application when the volume of the playback device, recording device, or application changes.</td>
</tr>
</tbody>
</table>




**v1.14**

v1.14 is released on October 20, 2017. 

#### New features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.

-   Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimizes the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions).
-   Adds the ability to reduce the bandwidth:
    -   Before v1.14: If you muted the audio or disabled the video of a specific user, the network still sent the streams.
    -   Starting from v1.14: If you mute the audio or disable the video of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues fixed:

-   Unable to hear any audio on certain Windows systems.
-   Camera-related issues on certain Windows systems.


**v1.13.1**

v1.13.1 is released on September 28, 2017 and optimizes the echo issue under certain circumstances.

**v1.13**

v1.13 is released on September 4, 2017. 

#### New features:

-   Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Adds the function to disable the audio playback.
-   Supports the profile configuration for stream-pushing on the client side.
-   Adds the <code>onClientRoleChanged</code> callback to report on a user role switch between the host and audience in a live broadcast.
-   Supports the push-stream failure callback on the server side.


#### Improvements:

-   Screen Sharing: Enhances the video definition and fluency.
-   Screen Sharing: Supports updating the captured region dynamically.
-   The video profile is controllable by the software codec.


#### Issues Fixed:

Occasional crashes.

**v1.12**

v1.12 is released on July 25, 2017. 

#### New features:

-   Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in the Live-broadcast profile.
-   Adds the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Adds a set of APIs to manage the audio effect.
-   Adds the <code>ActiveSpeaker</code> method to report on the active speaker in the current channel.
-   Removes the <code>setScreenCaptureWindow</code> method, and updated the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the Communication profile.
-   Adds displaying the mouse function when the screen-sharing function is enabled in the Communication profile.
-   Web: Adds and updates a series of APIs to enable interoperability between the web browsers and native clients for the Communication or Live-broadcast profiles. See [Release Notes](../../en/Interactive%20Broadcast/release_web_video.md).


Recording: Adds APIs for real-time video mixing and web recording. See [Release Notes for the Recording SDK](../../en/Product%20Overview/release_recording.md).

#### Improvements:

Android/iOS/macOS/Windows: In the Communication profile, an improvement for the 320 &times; 180 resolution profile:

- Keeps the video smooth under poor network and equipment conditions.
- Enhances the image quality to be better than 180p under good network and equipment conditions.


#### Issues fixed:

- Android: Bluetooth issues related to audio routing.
- Android/iOS/macOS/Windows: Occasional crashes.




