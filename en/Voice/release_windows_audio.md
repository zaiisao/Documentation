
---
title: Release Notes
description: 
platform: Windows
updatedAt: Wed Jul 10 2019 06:54:21 GMT+0800 (CST)
---
# Release Notes
## Overview

The Voice SDK supports the following scenarios:

- Voice communication
- Live voice broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

The Windows Voice SDK supports the X86 and  X64 architecture.

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

### New features

#### 1. Supporting string usernames

Many apps use string usernames. This release adds the following methods to enable apps to join an Agora channel directly with string usernames as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. For details, see [Use String User Accounts](../../en/Voice/string_windows.md).

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. You can use either a string user account or an integer user ID to generate a token. If you join the channel with a user account, ensure that you use the same user account to generate a token. You can also use the integer user ID that corresponds to the string user account to generate a token. Call the `getUserInfoByUserAccount` method to get the corresponding user ID.

#### 2. Adding remote audio statistics

To monitor the audio transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class.

### Improvements

This release adds a `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` member to the `reason` parameter of the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.

### API Changes

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [getUserInfoByUid](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [onLocalUserRegistered](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [onUserInfoUpdated](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class

#### Deprecated

- The `lowLatency` member in the [LiveTranscoding](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html) class

## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

This is the first release of the Agora Voice SDK for Windows. Refer to the following guides to quickly integrate the SDK and enable real-time voice communication in your project.

- [Quick start](../../en/Voice/windows_video.md)
- [Use security keys](../../en/Voice/token.md)
- [Report in-call statistics](../../en/Voice/in_call_statistics_windows_audio.md)
- [Adjust the volume](../../en/Voice/volume_windows.md)
- [Play audio effects/audio mixing](../../en/Voice/effect_mixing_windows.md)
- [Set the voice changer and reverberation effects](../../en/Voice/voice_effect_windows.md)
- [Push Streams to the CDN](../../en/Voice/push_stream_windows2.0_audio.md)
- [Test or select a media device](../../en/Voice/switch_audio_device_windows.md)

If you migrate to this SDK from the Windows Video SDK, refer to the [Release notes for the Windows video SDK](../../en/Voice/release_windows_video.md) for audio improvements.
