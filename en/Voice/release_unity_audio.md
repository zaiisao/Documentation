
---
title: Release Notes
description: 
platform: Unity
updatedAt: Sat Sep 19 2020 14:30:01 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Unity SDK.

## Overview

The Agora Unity SDK supports the following scenarios:

-   Voice call
-   Live interactive audio streaming

For the key features included in each scenario, see [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Agora Live Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

## v3.0.1

v3.0.1 was released on September 16, 2020.

In this release, Agora improves the user experience under poor network conditions for both the `COMMUNICATION` and `LIVE_BROADCASTING` profiles through the following measures:

- Adopting a new architecture for the `COMMUNICATION` profile.
- Upgrading the last-mile network strategy for both the `COMMUNICATION` and `LIVE_BROADCASTING` profiles, which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.1 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.1.

If you also use the On-premise Recording SDK, ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**Compatibility changes**

#### 1. Dynamic library replaces the static library (for macOS and iOS)

This release adds support for a dynamic library and removes the static library. This release renames the library from `AgoraRtcEngineKit.framework` to `AgoraRtcKit.framework`. If you upgrade your SDK to v3.0.1, you must re-import the `AgoraRtcKit` class in Xcode, and set the **Embed** attribute as **Embed & Sign**.

If you integrated the encryption library `AgoraRtcCryptoLoader.framework`, you must re-import the `AgoraRtcCryptoLoader` class in Xcode, and set the **Embed** attribute as **Embed & Sign**.

![](https://web-cdn.agora.io/docs-files/1600240476016)

#### 2. Enumerator names change

This release changes `CLIENT_ROLE` to `CLIENT_ROLE_TYPE`, and changes the name of the following members:

- `BROADCASTER` to `CLIENT_ROLE_BROADCASTER`
- `AUDIENCE` to `CLIENT_ROLE_AUDIENCE`

**New features**

#### 1. Specify the area of connection

This release adds `GetEngine2` for specifying the area of connection when creating an `IRtcEngine` instance. This advanced feature applies to scenarios that have regional restrictions. You can choose from areas including Mainland China, North America, Europe, Asia (excluding Mainland China), and global (default).

#### 2. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the `AgoraChannel` class. By creating multiple `AgoraChannel` objects, a user can join the corresponding channels at the same time. Besides, this release adds `SetMultiChannelWant` for enabling the multi-channel mode.

After joining multiple channels, users can receive the audio streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams. See [Join multiple channels](../../en/Voice/multiple_channel_unity.md) for details.

#### 3. Audio mixing pitch

To set the pitch of the local music file during audio mixing, this release adds `SetAudioMixingPitch`. You can set the `pitch` parameter to increase or decrease the pitch of the music file. This method sets the pitch of the local music file only. It does not affect the pitch of a human voice.

#### 4. Adjusting the playback volume of the specified remote user

Adds `AdjustUserPlaybackSignalVolume` for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or the live interactive streaming to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

#### 5. Voice beautifier and audio effects

To improve the audio quality, this release adds the following enumerate elements in `SetLocalVoiceChanger` and `SetLocalVoiceReverbPreset`:

- `VOICE_CHANGER_PRESET` adds several enumerations that have the prefixes `VOICE_BEAUTY` and `GENERAL_BEAUTY_VOICE`. The `VOICE_BEAUTY`enumerations implement voice timbre transformation, and the `GENERAL_BEAUTY_VOICE` enumerations beautify chat voice.
- `AUDIO_REVERB_PRESET` adds the enumeration `AUDIO_VIRTUAL_STEREO` and several enumerations that have the prefix `AUDIO_REVERB_FX`. The `AUDIO_VIRTUAL_STEREO` enumeration implements reverberation in the virtual stereo, and the `AUDIO_REVERB_FX` enumerations implement voice changer effects, new style transformation effects, and new room acoustics effects.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile of `AUDIO_PROFILE_DEFAULT(0)` in the `LIVE_BROADCASTING` profiles.

| SDK                 | `AUDIO_PROFILE_DEFAULT(0)`                                    |
| :------------------ | :----------------------------------------------------------- |
| v3.0.1              | A sample rate of 48 KHz, music encoding, mono, and a bitrate of up to 52 Kbps. |
| Earlier than v3.0.1 | A sample rate of 32 KHz, music encoding, mono, and a bitrate of up to 44 Kbps. |

#### 2. Quality statistics

Adds the following members in the `RtcStats` class for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`

On iOS, to prevent the prompt to find local network devices from popping up when an end user launches the app on an iOS 14.0 device, the `gatewayRtt` parameter is disabled by default. See the [FAQ](https://docs.agora.io/en/faq/local_network_privacy) for details. If you do not mind the prompt, and want to enable the `gatewayRtt` parameter, please contact Agora technical support via [support@agora.io](mailto:support@agora.io).

#### 3. Others

This release enables interoperability between the RTC Unity SDK and the RTC Web SDK by default, and deprecates the `EnableWebSdkInteroperability` method.

**Issues fixed**

- Audio issues relating to audio mixing, audio encoding, and echoing.
- Issues relating to app crashes, log file, and unstable service during CDN live streaming.
- Issues relating to `SendStreamMessage` and the `TranscodingUser` of `SetLiveTranscoding`, which do not support an array format.
- Inaccurate report of the `OnClientRoleChangedHandler` callback, authentication issues with an App ID and token, and a garbled log directory.

**API changes**

#### Added

- [`GetEngine2`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ac1a02000088c915aa36065325f42d166)
- [`CreateChannel`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ac14119e2505e2f44931bed0181c5624f)
- [`AgoraChannel`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_agora_channel.html) class
- [`SetMultiChannelWant`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a50ddfe8050fd11f1537bd0dddc2eb8a3)
- [`SetAudioMixingPitch`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ac1f7b492430f2c3f38826804a418c2a7)
- [`AdjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Voice/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a6ae88c74d0dc4e80837cd0a351f81c00)
- Nine enumerations in [`VOICE_CHANGER_PRESET`](https://docs.agora.io/en/Voice/API%20Reference/unity/namespaceagora__gaming__rtc.html#a710b9754965ccb92ed968a562968df2c), such as `AUDIO_REVERB_FX_KTV`
- Twelve enumerations in [`AUDIO_REVERB_PRESET`](https://docs.agora.io/en/Voice/API%20Reference/unity/namespaceagora__gaming__rtc.html#a1e681589411dd2f5df62dab5c1fca7b9), such as `VOICE_BEAUTY_VIGOROUS`
- The [`gatewayRtt`](https://docs.agora.io/en/Voice/API%20Reference/unity/structagora__gaming__rtc_1_1_rtc_stats.html#aef762b5910ca3a7a06a4e37869c34fed), [`memoryAppUsageRatio`](https://docs.agora.io/en/Voice/API%20Reference/unity/structagora__gaming__rtc_1_1_rtc_stats.html#a5b7d328a6f8e6aca9e1b8b6c8ce16e02), [`memoryTotalUsageRatio`](https://docs.agora.io/en/Voice/API%20Reference/unity/structagora__gaming__rtc_1_1_rtc_stats.html#a232d695be9b723df8dae4ca219c6745f) and [`memoryAppUsageInKbytes`](https://docs.agora.io/en/Voice/API%20Reference/unity/structagora__gaming__rtc_1_1_rtc_stats.html#aeb37b39c64362e3954b279c6dfc5e774) members in the `RtcStats` class
- The [`totalActiveTime`](https://docs.agora.io/en/Voice/API%20Reference/unity/structagora__gaming__rtc_1_1_remote_audio_stats.html#a7453a27b08439186f35b3b7bb9eafd3b) member in the `RemoteAudioStats` struct
- The [`channelId`](https://docs.agora.io/en/Voice/API%20Reference/unity/structagora__gaming__rtc_1_1_audio_volume_info.html#a0b95567512ed7c6642671e805207a8e1) member in the `AudioVolumeInfo` struct

#### `Renamed`

Enum type `CLIENT_ROLE` changed to [`CLIENT_ROLE_TYPE`](https://docs.agora.io/en/Voice/API%20Reference/unity/namespaceagora__gaming__rtc.html#a901253f94f78da34371bf7eb656a19ff). Enum name `BROADCASTER` and `AUDIENCE` changed to `CLIENT_ROLE_BROADCASTER` and `CLIENT_ROLE_AUDIENCE`.

#### Deprecated

- `EnableWebSdkInteroperability`
- `OnUserMutedAudioHandler`, `OnFirstRemoteAudioDecodedHandler`, and `OnFirstRemoteAudioFrameHandler`, replaced by [`OnRemoteAudioStateChangedHandler`](https://docs.agora.io/en/Voice/API%20Reference/unity/namespaceagora__gaming__rtc.html#a92f016ea980eba06cf38192191d17e01)
- `OnStreamPublishedHandler` and `OnStreamUnpublishedHandler`, replaced by [`OnRtmpStreamingStateChangedHandler`](https://docs.agora.io/en/Voice/API%20Reference/unity/namespaceagora__gaming__rtc.html#a589c11ed19b0638824aa1b2e23971271)

## v2.9.2

v2.9.2 is released on Feb 17, 2020.

- This release fixed some abnormal behaviors on Android devices.
- This release fixed the stuck behavior of using the Editor debug mode on Windows platform.

## v2.9.1

Agora Unity SDK is widely used in games, education, AR, VR and other scenarios.

v2.9.1 was released on December 23, 2019.

**Functions and features**

#### 1. Multi-platform support
Supports iOS, Android, macOS and Windows (x86/x86_64) platforms.

#### 2. Interoperability with the Agora Web SDK
Provides the `EnableWebSdkInteroperability` method for enabling interoperability with the Agora Web SDK in `LIVE_BROADCASTING` profile. 

#### 3. Raw data
Supports raw audio data. You can capture and process the raw data according to your needs.

#### 4. External data source
Provides APIs for accessing to the external data source. You can configure the external audio source, and push the data to the Agora Unity SDK.

#### 5. Encryption
Supports encryption of audio streaming. The following table shows the information of the encryption libraries for the Android and iOS platforms. If you do not intend to use this function, you can remove the encryption libraries to decrease the SDK size.

   | Platform | Encryption libraries                          |
   | :------- | :-------------------------------------------- |
   | Android  | libagora-crypto.so                            |
   | iOS      | <ul><li>AgoraRtcCryptoLoader.framework <li>libcrypto.a</li></ul> |

#### 6. Cloud proxy

Supports the cloud proxy service. See [Use Cloud Proxy](../../en/Voice/cloudproxy_unity.md) for details.

**Related documentation**

See the following documentation to quickly integrate the SDK and implement real-time voice and video communication in your project.

- [Start a Voice Call](../../en/Voice/start_call_audio_unity.md) or [Start Live Interactive Audio Streaming](../../en/Voice/start_live_audio_unity.md)
- [API Reference](https://docs.agora.io/en/Voice/API%20Reference/unity/index.html) 

Agora also provides an open-source [Unity Sample](https://github.com/AgoraIO/Agora-Unity-Quickstart/tree/master/audio/Hello-Unity3D-Agora) on GitHub.
