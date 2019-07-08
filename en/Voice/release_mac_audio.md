
---
title: Release Notes
description: 
platform: macOS
updatedAt: Mon Jul 08 2019 02:52:50 GMT+0800 (CST)
---
# Release Notes
## Overview

The Voice SDK supports the following scenarios:

- Voice communication
- Live voice broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

### New features

#### 1. Supporting string usernames

Many apps use string usernames. This release adds the following methods to enable apps to join an Agora channel directly with string usernames as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. For details, see [Use String User Accounts](../../en/Voice/string_mac.md).

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version.

#### 2. Adding remote statistics

To monitor the audio transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class.

### Improvements

This release adds a `AgoraConnectionChangedKeepAliveTimeout(14)` member to the `AgoraConnectionChangedReason` parameter of the [connectionChangedToState](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.


### Issues Fixed

- Occasional crashes.

### API Changes

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)
- [getUserInfoByUid](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUid:withError:)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUserAccount:withError:)
- [didRegisteredLocalUser](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRegisteredLocalUser:withUid:)
- [didUpdatedUserInfo](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class

#### Deprecated

- The `lowLatency` member in the [AgoraLiveTranscoding](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) class


## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

This is the first release of the Agora Voice SDK for macOS. Refer to the following guides to quickly integrate the SDK and enable real-time voice communication in your project.

- [Quick start](../../en/Voice/mac_video.md)
- [Use security keys](../../en/Voice/token.md)
- [Report in-call statistics](../../en/Voice/in_call_statistics_macos_audio.md)
- [Adjust the volume](../../en/Voice/volume_mac.md)
- [Play audio effects/audio mixing](../../en/Voice/effect_mixing_mac.md)
- [Set the voice changer and reverberation effects](../../en/Voice/voice_effect_mac.md)
- [Push Streams to the CDN](../../en/Voice/push_stream_ios2.0_audio.md)
- [Test or select a media device](../../en/Voice/switch_audio_device_mac.md)

If you migrate to this SDK from the macOS Video SDK, refer to the [Release notes for the macOS video SDK](../../en/Voice/release_mac_video.md) for audio improvements.
