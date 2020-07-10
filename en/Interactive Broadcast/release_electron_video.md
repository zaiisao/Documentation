
---
title: Release Notes
description: 
platform: Electron
updatedAt: Fri Jul 10 2020 08:29:03 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora SDK for Electron.

## Introduction

The Agora SDK for Electron is developed upon Agora SDK for macOS and Agora SDK for Windows, with the Node.js C++ plug-in units. The Electron SDK supports the following scenarios:

- Voice and Video Call
- Live Interactive Audio and Video Streaming

For key features included in each scenario, see [Agora Voice Call Overview](../../en/Interactive%20Broadcast/product_voice.md), [Agora Video Call Overview](../../en/Interactive%20Broadcast/product_video.md), [Agora Interactive Audio Streaming Overview](../../en/Interactive%20Broadcast/product_live_audio.md) and [Agora Interactive Video Streaming Overview](../../en/Interactive%20Broadcast/product_live.md).

## v3.0.0

v3.0.0 was released on April 7th, 2020.

In this release, Agora improves the user experience under poor network conditions for both the `COMMUNICATION` and `LIVE_BROADCASTING` profiles through the following measures:
- Adopting a new architecture for the `COMMUNICATION` profile.
- Upgrading the last-mile network strategy for both the `COMMUNICATION` and `LIVE_BROADCASTING` profiles,  which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.0 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.0.

We also upgrade the On-premise Recording SDK to v3.0.0. Ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**Compatibility changes**

#### 1. Dual-stream mode not enabled in the `COMMUNICATION` profile

As of v3.0.0, the native SDK does not enable the [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode) by default in the `COMMUNICATION` profile. Call the [`enableDualStreamMode (true)`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#enabledualstreammode) method after joining the channel to enable it. In video scenarios with multiple users, we recommend enabling the dual-stream mode.

**New features**

#### 1. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the [`AgoraRtcChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcchannel.html) class. By creating multiple `AgoraRtcChannel` objects, a user can join the corresponding channels at the same time.

After joining multiple channels, users can receive the audio and video streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams.

#### 2. Adjusting the playback volume of the specified remote user

Adds [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#adjustuserplaybacksignalvolume) for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or live interactive streaming to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

#### 3. Detecting local voice activity

This release adds the `report_vad(boolean)` parameter to the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#enableaudiovolumeindication) method to enable local voice activity detection. Once it is enabled, you can check the `groupAudioVolumeIndication` callback for the voice activity status of the local user.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile (`profile(0)`) of [`setAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#setaudioprofile) in the `LIVE_BROADCASTING` profile.

| SDK                 | profile(0)                                                  |
| :------------------ | :----------------------------------------------------------- |
| 3.0.0               | A sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 52 Kbps. |
| Earlier than v3.0.0 | <li>macOS: A sample rate of 32 kHz, music encoding, mono, and a bitrate of up to 44 Kbps.</li><li>Windows: A sample rate of 32 kHz, music encoding, mono, and a bitrate of up to 64 Kbps.</li> |

#### 2. Mirror mode

This release enables you to set a mirror effect for the stream to be encoded:

Setting a mirror mode for the stream to be encoded: This release adds the `mirrorMode` property to the [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/videoencoderconfiguration.html) interface for setting a mirror effect for the stream to be encoded and transmitted.

#### 3. Quality statistics

Adds the following properties in the [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html) interface for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`

#### 4. Screen sharing

This release enables window sharing of [UWP (Universal Windows Platform)](https://docs.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) applications when you sharing a screen by the windows ID.

#### 5. Watermark in Live Streaming

This release enables the watermark function for users to add a PNG file to the local live streaming as a watermark. The [`addVideoWatermark`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#addvideowatermark) and [`clearVideoWatermark`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#clearvideowatermarks) methods can add and delete watermarks in live interactive streaming.

#### 6. Supporting more audio sample rates for recording

To enable more audio sample rate options for recording, this release adds a new [`startAudioRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startaudiorecording) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz.

#### 7. Others

This release enables interoperability between the Electron SDK and the Web SDK by default, and deprecates the `enableWebSdkInteroperability` and `videoSourceEnableWebSdkInteroperability` method.

**Issues fixed**

- Audio issues relating to audio mixing, audio encoding, and echoing.
- Video issues relating to the watermark, aspect ratio, video sharpness, black outline appearing while screen sharing and toggling to full-screen.
- Other issues relating to app crashes, log file, and unstable service during CDN live streaming.
- The [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#setremotesubscribefallbackoption) method, which should work in the `LIVE_BROADCASTING` profile only, also works in the `COMMUNICATION` profile.
- In some one-to-one call, the downlink media stream falls back to audio-only under poor network conditions.
- Occasionally, the UI of the system window is abnormal on macOS 10.15.

**API changes**

#### Behavior change

When connected to a headset or Bluetooth, the macOS device changes its audio route to be uniform as the audio route shown in the device manager.

#### Added

- The `mirrorMode` property in the [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/videoencoderconfiguration.html) interface
- [`createChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#createchannel)
- [`AgoraRtcChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcchannel.html) class
- The `gatewayRtt`, `memoryAppUsageRatio`, `memoryTotalUsageRatio` and `memoryAppUsageInKbytes` properties in the [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html) interface
- [`startAudioRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startaudiorecording)
- [`addVideoWatermark`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#addvideowatermark)
- [`clearVideoWatermark`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#clearvideowatermarks)
- The `report_vad` parameter in the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#enableaudiovolumeindication) method

#### Deprecated

- `enableWebSdkInteroperability`
- `videoSourceEnableWebSdkInteroperability`
- `firstRemoteVideoFrame`, replaced by `remoteVideoStateChanged`
-  `userMuteAudio`, `firstRemoteAudioDecoded`, and `firstRemoteAudioFrame`, replaced by `remoteAudioStateChanged`
- `streamPublished` and `streamUnpublished`, replaced by `rtmpStreamingStateChanged`

## v2.9.0

v2.9.0 is released on Aug 30, 2019.

**Compatibility changes**


####  Reporting the state of the remote video

This release extends the `remoteVideoStateChanged` callback with more states of the remote video: `STOPPED(0)`, `STARTING(1)`, `DECODING(2)`, `FROZEN(3)`, and `FAILED(4)`. It adds a reason parameter to the callback to indicate why the remote video state changes. The original `remoteVideoStateChanged` callback is deleted. If you upgrade your Native SDK to the latest version, ensure that you re-implement the `remoteVideoStateChanged` callback.

The new callback reports most of the remote video states, and therefore deprecates the following callbacks. You can still use them, but we do not recommend doing so.

- `userEnableVideo`
- `userEnableLocalVideo`
- `addStream`

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#switchchannel) method to enable the audience in a `LIVE_BROADCASTING` channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the `leaveChannel` and `joinChannel` methods. After the audience successfully switches to another channel by calling the `switchChannel` method, the SDK triggers the `leaveChannel` and `joinedChannel` callbacks to indicate that the audience has left the original channel and joined a new one.

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different `LIVE_BROADCASTING` channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startchannelmediarelay)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#updatechannelmediarelay)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#stopchannelmediarelay)

During the media stream relay, the SDK reports the states and events of the relay with the `channelMediaRelayState` and `channelMediaRelayEvent` callbacks.

#### 3. Reporting the local and remote audio state

This release adds the `localAudioStateChanged` and `remoteAudioStateChanged` callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: `STOPPED(0)`, `RECORDING(1)`, `ENCODING(2)`, or `FAILED(3)`. When the state is `FAILED(3)`, see the `error` parameter for troubleshooting.
- The remote audio: `STOPPED(0)`, `STARTING(1)`, `DECODING(2)`, `FROZEN(3)`, or `FAILED(4)`. See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the `localAudioStats` callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html), [`LocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/localvideostats.html), and [`RemoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/remotevideostats.html) classes:

- `RtcStats`: The total number of the sent audio bytes, sent video bytes,  received audio bytes, and received video bytes during a session.
- `LocalVideoStats`: The encoding bitrate, the width and height of the encoding frame, the number of frames, and the codec type of the local video.
- `RemoteVideoStats`: The packet loss rate of the remote video.

#### 2. Improving the video quality of live interactive streaming

This release minimizes the video freeze rate under poor network conditions, improves the video sharpness, and optimizes the video smoothness when the packet loss rate is high.

#### 3. Improving the screen sharing quality

This release improves the sharpness of text during screen sharing in the `COMMUNICATION` profile, particularly when the network condition is poor. Note that this improvement takes effect only when you set `ContentHint` as `Details(2)`.

#### 4. Other Improvements

- Improves the audio quality when the audio scenario is set to Game Streaming.
- Improves the audio quality after the user disables the microphone in the `COMMUNICATION` profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Invalid call of the [`muteRemoteAudioStream`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#muteremoteaudiostream) method.
- Occasionally no audio.
- Crashes occur when testing the microphone.

#### Video

- Video freezes.
- The `remoteVideoStateChanged` callback behaves unexpectedly.

#### Miscellaneous

- Occasionally mixed streams in RTMP streaming.
- Occasional crashes occur.
- Failure to join the channel.

**API changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added

- `localAudioStats`
- `localAudioStateChanged`
- `remoteAudioStateChanged`
- [`switchChannel`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#switchchannel)
- [`startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#startchannelmediarelay)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#updatechannelmediarelay)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/classes/agorartcengine.html#stopchannelmediarelay)
- `channelMediaRelayState`
- `channelMediaRelayEvent`
- `remoteVideoStateChanged`
- [`RtcStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/rtcstats.html): `txAudioBytes`, `txVideoBytes`, `rxAudioBytes` and `rxVideoBytes`
- [`localVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/localvideostats.html): `encodedBitrate`,`encodedFrameWidth`,`encodedFrameHeight`, `encodedFrameCount` and `codecType`
- [`remoteVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/interfaces/remotevideostats.html): `packetLossRate`

#### Deprecated

- `microphoneEnabled`. Use the `localAudioStateChanged` callback instead.
- `remoteVideoTransportStats`. Use the `remoteVideoStats` callback instead.
- `remoteAudioTransportStats`. Use the `remoteAudioStats` callback instead.
- `userEnableVideo`. Use the `remoteVideoStateChanged` callback instead.
- `userEnableLocalVideo`. Use the `remoteVideoStateChanged` callback instead.
- `addStream`. Use the `remoteVideoStateChanged` callback instead.

## v2.8.0

v2.8.0 is released on Jul 8, 2019.

This is the first release of the Agora SDK for Electron. Refer to the following documentation to quickly integrate the SDK and enable real-time communication in your project.

- Integrate the SDK: [Start a Call](../../en/Interactive%20Broadcast/start_call_electron.md) or [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_electron.md)
- [API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/electron/index.html)

Agora also provides the open-source [Electron GitHub Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) for you to download.
