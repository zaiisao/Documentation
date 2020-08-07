
---
title: Release Notes
description: 
platform: Windows
updatedAt: Thu Aug 06 2020 06:29:01 GMT+0800 (CST)
---
# Release Notes
## Overview

The Voice SDK supports the following scenarios:

- Voice and Video Call
- Live Interactive Audio and Video Streaming

For the key features included in each scenario, see [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Agora Live Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

The Windows Voice SDK supports the X86 and  X64 architecture.

## v3.0.1

v3.0.1 was released on May 27, 2020.

**New features**

#### 1. Audio mixing pitch

To set the pitch of the local music file during audio mixing, this release adds `setAudioMixingPitch`. You can set the `pitch` parameter to increase or decrease the pitch of the music file. This method sets the pitch of the local music file only. It does not affect the pitch of a human voice.

#### 2. Voice enhancement

To improve the audio quality, this release adds the following enumerate elements in `setLocalVoiceChanger` and `setLocalVoiceReverbPreset`:

- `VOICE_CHANGER_PRESET` adds several elements that have the prefixes `VOICE_BEAUTY` and `GENERAL_BEAUTY_VOICE`. The `VOICE_BEAUTY` elements enhance the local voice, and the `GENERAL_BEAUTY_VOICE` enumerations add gender-based enhancement effects.
- `AUDIO_REVERB_PRESET` adds the enumeration `AUDIO_VIRTUAL_STEREO` and several enumerations that have the prefix `AUDIO_REVERB_FX`. The `AUDIO_VIRTUAL_STEREO` enumeration implements reverberation in the virtual stereo, and the `AUDIO_REVERB_FX` enumerations implement additional enhanced reverberation effects.

See the advanced guide [Set the Voice Changer and Reverberation Effects](../../en/Voice/voice_changer_windows.md) for more information.

#### 3. Data post-processing in multiple channels

This release adds support for post-processing remote audio data in a multi-channel scenario by adding `isMultipleChannelFrameWanted` and `onPlaybackAudioFrameBeforeMixingEx` in the `IAudioFrameObserver` class.

After successfully registering the audio observer, if you set the return value of `isMultipleChannelFrameWanted` as `true`, you can get the corresponding audio data from `onPlaybackAudioFrameBeforeMixingEx`. In a multi-channel scenario, Agora recommends setting the return value as `true`.


**Fixed issues**

- Audio mixing issues and abnormal loopback test results.
- Inaccurate report of the [`onClientRoleChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36d3f45184cbb37ed2c4846654a14368) callback, authentication with an App ID and token, and a garbled log directory.

**API changes**

#### Added

- [`setAudioMixingPitch`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a26b117f7e097801b03522f7da9257425)
- Nine enumerations in [`VOICE_CHANGER_PRESET`](https://docs.agora.io/en/Voice/API%20Reference/cpp/namespaceagora_1_1rtc.html#ae29d1fb09d785334eabf0f3def8b4117), such as `AUDIO_REVERB_FX_KTV`
- Twelve enumerations in [`AUDIO_REVERB_PRESET`](https://docs.agora.io/en/Voice/API%20Reference/cpp/namespaceagora_1_1rtc.html#a2476d004b44df3950ef62022cd41e564), such as `VOICE_BEAUTY_VIGOROUS`
- [`isMultipleChannelFrameWanted`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a4b6bdf2a975588cd49c2da2b6eff5956) and [`onPlaybackAudioFrameBeforeMixingEx`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ab0cf02ba307e91086df04cda4355905b) in the [`IAudioFrameObserver`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html) class
- The `totalActiveTime` member in the [`RemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) struct
- `WARN_ADM_WINDOWS_NO_DATA_READY_EVENT(1040)` and `WARN_ADM_INCONSISTENT_AUDIO_DEVICE(1042)` in the [warning codes](https://docs.agora.io/en/Voice/API%20Reference/cpp/namespaceagora.html#a32d042123993336be6646469da251b21)

## v3.0.0.2

v3.0.0.2 was released on Apr 22, 2020.

**New features**

#### Specify the area of connection

This release adds `areaCode` member in the `RtcEngineContext` struct for specifying the area of connection when creating an `IRtcEngine` instance. This advanced feature applies to scenarios that have regional restrictions. You can choose from areas including Mainland China, North America, Europe, Asia (excluding Mainland China), and global (default).

After specifying the area of connection:

- When the app that integrates the Agora SDK is used within the specified area, it connects to the Agora servers within the specified area under normal circumstances.
- When the app that integrates the Agora SDK is used out of the specified area, it connects to the Agora servers either in the specified area or in the area where the SDK is located.

**API changes**

#### Added

`areaCode` member in the [`RtcEngineContext`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_engine_context.html) struct

## v3.0.0

v3.0.0 was released on Mar 5, 2020.

In this release, Agora improves the user experience under poor network conditions for both the `COMMUNICATION`and `LIVE_BROADCASTING` profiles through the following measures:
- Adopting a new architecture for the `COMMUNICATION` profile.
- Upgrading the last-mile network strategy for both the `COMMUNICATION`and `LIVE_BROADCASTING` profiles,  which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.0 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.0.

We also upgrade the On-premise Recording SDK to v3.0.0. Ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**New features**

#### 1. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the `IChannel` and `IChannelEventHandler` classes. By creating multiple `IChannel` objects, a user can join the corresponding channels at the same time.
After joining multiple channels, users can receive the audio and video streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams. See [Join Multiple Channels](../../en/Voice/multiple_channel_windows.md) for details.

#### 2. Adjusting the playback volume of the specified remote user

Adds `adjustUserPlaybackSignalVolume` for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or live interactive streaming to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile of `AUDIO_PROFILE_DEFAULT (0)` in the `LIVE_BROADCASTING` profile.

| SDK   | AUDIO_PROFILE_DEFAULT (0)                                   |
| :--------- | :---------------------------------------------------------- |
| v3.0.0      | A sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 52 Kbps. |
| Earlier than v3.0.0 | 3A sample rate of 32 kHz, music encoding, mono, and a bitrate of up to 64 Kbps. |

#### 2. Quality statistics

Adds the following members in the `RtcStats` class for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`

#### 3. Others

This release enables interoperability between the RTC Native SDK and the RTC Web SDK by default, and deprecates the `enableWebSdkInteroperability` method.

**Issues fixed**

* Audio issues relating to audio mixing, audio encoding, and echoing.
* Other issues relating to app crashes, log file, and unstable service during CDN live streaming.

**API changes**

#### Added

- The [`channelId`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html#ab67471def88118f010e6d5add7d83f64) member in the [`AudioVolumeInfo`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) struct
- [`createChannel`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine2.html#a9cabefe84d3a52400f941f1bd8c0f486)
- [`IChannel`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel.html) class
- [`IChannelEventHandler`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_channel_event_handler.html) class
- The [`gatewayRtt`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a30cc889ef34f63e23950b54591218cb5), [`memoryAppUsageRatio`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aab879ec9c52f2dd8d662c26c4939a7d3), [`memoryTotalUsageRatio`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#aae6d57e709b08258be372960f9e19fd6) and [`memoryAppUsageInKbytes`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a17258e12476bb9106ccc248af4cfe734) members in the [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) class

#### Deprecated

* `RtcEngineParameters` class
* `enableWebSdkInteroperability`
* `onUserMuteAudio`, `onFirstRemoteAudioDecoded`, and `onFirstRemoteAudioFrame`, replaced by [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
* `onStreamPublished` and `onStreamUnpublished`, replaced by [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)

## v2.9.1
v2.9.1 is released on Sep 19, 2019.

**New features**

#### Detecting local voice activity

This release adds the `report_vad(bool)` parameter to the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb) method to enable local voice activity detection. Once it is enabled, you can check the [`AudioVolumeInfo`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) struct of the [`onAudioVolumeIndication`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aab1184a2b276f509870c055a9ff8fac4) callback for the voice activity status of the local user.

**Improvements**

#### Supporting more audio sample rates for recording

To enable more audio sample rate options for recording, this release adds a new [`startAudioRecording`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz. The original method supports only a fixed sample rate of 32 kHz and is deprecated.

**Issues fixed**

#### Miscellaneous

A typo in the IAgoraRtcEngine.h file.

**API changes**

To improve the user experience, we made the following changes in v2.9.1:

#### Added

- [`startAudioRecording`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f)
- The `report_vad` parameter in the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4b30a8ff1ae50c4c114ae4f909c4ebcb) method
- The `vad` member in the [`AudioVolumeInfo`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_audio_volume_info.html) class

#### Deprecated

- `startAudioRecording`

## v2.9.0

v2.9.0 is released on Aug 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`

If your app implements RTMP streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d)


#### 2. Disabling/enabling the local audio

To improve the audio quality in the `COMMUNICATION` profile, this release sets the system volume to the media volume after you call the `enableLocalAudio`(true) method. Calling `enableLocalAudio`(false) switches the system volume back to the in-call volume.

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannel`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab) method to enable the audience in a `LIVE_BROADCASTING` channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the `leaveChannel` and `joinChannel` methods. After the audience successfully switches to another channel by calling the `switchChannel` method, the SDK triggers the `onLeaveChannel` and `onJoinChannelSuccess` callbacks to indicate that the audience has left the original channel and joined a new one.

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different `LIVE_BROADCASTING` channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)

During the media stream relay, the SDK reports the states and events of the relay with the [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22) and [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429) callbacks.

#### 3. Reporting the local and remote audio state

This release adds the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) and [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: STOPPED(0), RECORDING(1), ENCODING(2), or FAILED(3). When the state is FAILED(3), see the `error` parameter for troubleshooting.
- The remote audio: STOPPED(0), STARTING(1), DECODING(2), FROZEN(3), or FAILED(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`onLocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the `RtcStats` class:

- [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html): The total number of the sent audio bytes and  received audio bytes during a session.

#### 2. Other Improvements

- Improves the audio quality when the audio scenario is set to Game Streaming.
- Improves the audio quality after the user disables the microphone in the `COMMUNICATION` profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Invalid call of the `muteRemoteAudioStream` method.
- Occasionally no audio.

#### Miscellaneous

- Occasionally mixed streams in RTMP streaming.
- Occasional crashes occur.
- Failure to join the channel.

**API changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added

- [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a)
- [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa168380f86f1dc2df1c269a785c56612)
- [`onLocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0cb47df6a8ef7acee229eb307d6f32c3)
- [`switchChannel`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3eb5ee494ce124b34609c593719c89ab)
- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acb72f911830a6fdb77e0816d7b41dd5c)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afad0d3f3861c770200a884b855276663)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab4a1c52a83a08f7dacab6de36f4681b8)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8f22b85194d4b771bbab0e1c3b505b22)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89a4085f36c25eeed75c129c82ca9429)
- [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html): `txAudioBytes` and `rxAudioBytes`

#### Deprecated

- `onMicrophoneEnabled`. Use LOCAL_AUDIO_STREAM_STATE_CHANGED(0) or LOCAL_AUDIO_STREAM_STATE_RECORDING(1) in the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9296c329331eb83b3af1315c52e7f91a) callback instead.
- `onRemoteAudioTransportStats`. Use the [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a) callback instead.


#### Deleted

- `configPublisher`


## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string user IDs

Many apps use string user IDs. This release adds the following methods to enable apps to join an Agora channel directly with string user IDs as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth call, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel.

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your user IDs into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote audio statistics

To monitor the audio transmission quality during a call live interactive streaming, this release adds the `totalFrozenTime` and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_audio_stats.html) class.

**Improvements**

This release adds a `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` member to the `reason` parameter of the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.

**API changes**

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

This is the first release of the Agora Voice SDK for Windows. Refer to the following guides to quickly integrate the SDK and enable real-time communication in your project.

- [Quick start](../../en/Voice/start_call_windows.md)
- [Use security keys](../../en/Voice/token.md)
- [Report in-call statistics](../../en/Voice/in-call_quality_windows.md)
- [Adjust the volume](../../en/Voice/volume_windows.md)
- [Play audio effects/audio mixing](../../en/Voice/audio_effect_mixing_windows.md)
- [Set the voice changer and reverberation effects](../../en/Voice/voice_changer_windows.md)
- [Test or select a media device](../../en/Voice/test_switch_device_windows.md)
- [Use Cloud Proxy](../../en/Voice/cloudproxy_native.md)

If you migrate to this SDK from the Windows Video SDK, refer to the [Release notes for the Windows video SDK](../../en/Voice/release_windows_video.md) for audio improvements.
