
---
title: Release Notes
description: 
platform: macOS
updatedAt: Mon Oct 28 2019 02:58:06 GMT+0800 (CST)
---
# Release Notes
## Overview

The Voice SDK supports the following scenarios:

- Voice communication
- Live voice broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

## v2.9.1
v2.9.1 is released on Sep 19, 2019.

**New features**

#### Detecting local voice activity

This release adds the `report_vad(bool)` parameter to the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) method to enable local voice activity detection. Once it is enabled, you can check the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) struct of the [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:) callback for the voice activity status of the local user.

**Improvements**

#### Supporting more audio sample rates for recording

To enable more audio sample rate options for recording, this release adds a new [`startAudioRecording`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz. The original method supports only a fixed sample rate of 32 kHz and is deprecated.

**API changes**

To improve the user experience, we made the following changes in v2.9.1:

#### Added

- [`startAudioRecording`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:)
- The `report_vad` parameter in the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) method
- The `vad` member in the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) class

#### Deprecated

- `startAudioRecording`

## v2.9.0
v2.9.0 is released on Aug. 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`

If your app implements RTMP streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)
- [`rtmpStreamingChangedToState`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)

For how to implement the new methods, see [Push Streams to the CDN](../../en/Voice/cdn_streaming_apple.md).

#### 2. Disabling/enabling the local audio

To improve the audio quality in the Communication profile, this release sets the system volume to the media volume after you call the `enableLocalAudio`(true) method. Calling `enableLocalAudio`(false) switches the system volume back to the in-call volume.

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannelByToken`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method to enable the audience in a Live Broadcast channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the [`leaveChannel`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) and [`joinChannelByToken`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) methods. After the audience successfully switches to another channel by calling the [`switchChannelByToken`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method, the SDK triggers the [`didLeaveChannelWithStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLeaveChannelWithStats:) and [`didJoinChannel`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinChannel:withUid:elapsed:) callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different Live Broadcast channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)

During the media stream relay, the SDK reports the states and events of the relay with the  [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:) and [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host Across Channels](../../en/Voice/media_relay_mac.md).

#### 3. Reporting the local and remote audio state

This release adds the [`localAudioStateChange`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) and [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: Stopped(0), Recording(1), Encoding(2), or Failed(3). When the state is Failed(3), see the `error` parameter for troubleshooting.
- The remote audio: Stopped(0), Starting(1), Decoding(2), Frozen(3), or Failed(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`localAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

#### 5. Pulling the remote audio data

To improve the experience in audio playback, this release adds the following methods to pull the remote audio data. After getting the audio data, you can process it and play it with the audio effects that you want.

- [`enableExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

The difference between the `onPlaybackAudioFrame` callback and the `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte` method is as follows:

- `onPlaybackAudioFrame`: The SDK sends the audio data to the app once every 10 ms. Any delay in processing the audio frames may result in an audio delay.
- `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte`: The app pulls the remote audio data. After setting the audio data parameters, the SDK adjusts the frame buffer and avoids problems caused by jitter in external audio playback.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the [`AgoraChannelStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html) class:

- `AgoraChannelStats`: The total number of the sent audio bytes and received audio bytes during a session.

#### 2. Other Improvements

- Improves the audio quality when the audio scenario is set to `GameStreaming`.
- Improves the audio quality after the user disables the microphone in the Communication profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Crashes occur when testing the microphone.

#### Miscellaneous

- Occasionally mixed streams in RTMP streaming. 

**API changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added
- [`enableExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)
- [`localAudioStateChange`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:)
- [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:)
- [`localAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:)
- [`switchChannelByToken`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 
- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)
- [`AgoraChannelStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html): `txAudioBytes` and `rxAudioBytes`

#### Deprecated

- [`didMicrophoneEnabled`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didMicrophoneEnabled:). Use AgoraAudioLocalStateStopped(0) or AgoraAudioLocalStateRecording(1) in the [`localAudioStateChange`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) callback instead.
- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:). Use the [`remoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) callback instead.

#### Deleted

- `configPublisher`

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string usernames

Many apps use string usernames. This release adds the following methods to enable apps to join an Agora channel directly with string usernames as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. 

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote statistics

To monitor the audio transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class.

**Improvements**

This release adds a `AgoraConnectionChangedKeepAliveTimeout(14)` member to the `AgoraConnectionChangedReason` parameter of the [connectionChangedToState](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.


**Issues fixed**

- Occasional crashes.

**API changes**

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
