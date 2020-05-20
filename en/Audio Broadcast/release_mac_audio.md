
---
title: Release Notes
description: 
platform: macOS
updatedAt: Wed May 20 2020 03:10:57 GMT+0800 (CST)
---
# Release Notes
## Overview

The Voice SDK supports the following scenarios:

- Voice communication
- Live voice broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

## v3.0.0.2

v3.0.0.2 was released on Apr 22, 2020.

**New features**

#### Specify the area of connection

This release adds `sharedEngineWithConfig` for specifying the area of connection when creating an `AgoraRtcEngineKit` instance. This advanced feature applies to scenarios that have regional restrictions. You can choose from areas including Mainland China, North America, Europe, Asia (excluding Mainland China), and global (default).

After specifying the area of connection:

- When the app that integrates the Agora SDK is used within the specified area, it connects to the Agora servers within the specified area under normal circumstances.
- When the app that integrates the Agora SDK is used out of the specified area, it connects to the Agora servers either in the specified area or in the area where the SDK is located.

**Issues fixed**

This release fixed issues relating to no audio, disconnecting from a Bluetooth device after joining a channel, and the occasional failure to join a channel.

**API changes**

#### Added

[`sharedEngineWithConfig`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithConfig:delegate:)

## v3.0.0

v3.0.0 was released on Mar 4, 2020.

In this release, Agora improves the user experience under poor network conditions for both the Communication and Live-broadcast profiles through the following measures:

- Adopting a new architecture for the Communication profile.
- Upgrading the last-mile network strategy for both the Communication and Live-broadcast profiles,  which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.0 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.0.

We also upgrade the On-premise Recording SDK to v3.0.0. Ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**Compatibility changes**

#### Renaming the static library and adding support for dynamic library

To unify the library names across platforms, this release renames the library from `AgoraRtcEngineKit.framework` to `AgoraRtcKit.framework`. If you upgrade your SDK to v3.0.0, you must re-import the `AgoraRtcKit` class. For details, see [Import the class](https://docs.agora.io/en/Voice/start_call_mac?platform=macOS#a-nameimportclassa-2-import-the-class) in the Quickstart.

To improve your development experience, this release also adds support for the dynamic library. You can integrate either the static or the dynamic library in your project, and the name of the dynamic library package is Agora_Native_SDK_for_macOS_v3_0_0_VOICE_Dynamic. 

Integrating the dynamic library has the following advantages:

- The overall security level is improved.
- Incompatibility issues with other third-party libraries are avoided.
- Uploading the app onto App Store is easier.

If you prefer the dynamic library, you need to re-integrate the SDK and re-import the `AgoraRtcKit` class. This process should take no more than five minutes. See [Integrate the SDK](https://docs.agora.io/en/Voice/start_call_mac?platform=macOS#a-nameintegratesdkaintegrate-the-sdk) and [Import the class](https://docs.agora.io/en/Voice/start_call_mac?platform=macOS#a-nameimportclassa-2-import-the-class) in the Quickstart.

<div class="alert info">The following table shows the difference in the file size when generating ipa files with a dynamic and static library:

<table>
    <tr>
        <td width="8%"><b>Library type</b></td>
        <td width="15%"><b>ipa size (M)</b></td>
        <td width="10%"><b>Decompressed ipa size (M)</b></td>
        <td width="17%"><b>Frameworks folder size (M)</b></td>
        <td width="15%"><b>Binary file size (M)</b></td>
        <td width="25%"><b>Total size of frameworks folder + binary file (M)</b></td>
    </tr>
    <tr>
        <td>Dynamic library</td>
        <td>27</td>
        <td>56.6</td>
        <td>44</td>
        <td>2.4</td>
        <td>46.4</td>
    </tr>
    <tr>
        <td>Static library</td>
        <td>26.5</td>
        <td>55.3</td>
        <td>30.1</td>
        <td>15.1</td>
        <td>45.2</td>
    </tr>
</table>

The dynamic library is located in the framework folder as an independent library. Note that the corresponding binary file size does not include the SDK size. Overall, this decreases the binary file size by 12.7 M and increases the framework folder size by 13.9 M.</div>

**New features**

#### 1. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the [`AgoraRtcChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcChannel.html) and [`AgoraRtcChannelDelegate`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) classes. By creating multiple `AgoraRtcChannel` objects, a user can join the corresponding channels at the same time.

After joining multiple channels, users can receive the audio and video streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams. See [Join multiple channels](../../en/Audio%20Broadcast/multiple_channel_apple.md) for details.

#### 2. Adjusting the playback volume of the specified remote user

Adds [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustUserPlaybackSignalVolume:volume:) for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or a live broadcast to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile of `AgoraAudioProfileDefault(0)` in the Live-Broadcast profile.

| SDK   | `AgoraAudioProfileDefault(0)`                                  |
| :--------- | :---------------------------------------------------------- |
| v3.0.0      | A sample rate of 48 KHz, music encoding, mono, and a bitrate of up to 52 Kbps. |
| Earlier than v3.0.0 | A sample rate of 32 KHz, music encoding, mono, and a bitrate of up to 52 Kbps. |


#### 2. Quality statistics

Adds the following members in the [`AgoraChannelStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html) class for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`

#### 3. Others

This release enables interoperability between the Native SDK and the Web SDK by default, and deprecates the `enableWebSdkInteroperability` method.

**Issues fixed**

- Audio issues concerning audio mixing, audio encoding, and echo.
- Other issues related to app crashes, log file, and unstable service when pushing streams to the CDN.

**API changes**

#### Behavior change

- Calling [`enableLocalAudio (NO)`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLocalAudio:) does not change the in-call volume to media volume.
- When connected to a headset or Bluetooth,  the macOS device changes its audio route to be uniform as the audio route shown in the device manager. 

#### Added

- The `channelId` parameter in the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) struct
- [`createRtcChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/createRtcChannel:)
- [`AgoraRtcChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcChannel.html) class
- [`AgoraRtcChannelDelegate`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) class
- The `gatewayRtt`, `memoryAppUsageRatio`, `memoryTotalUsageRatio` and `memoryAppUsageInKbytes` members in the [`AgoraChannelStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html) class

#### Deprecated

- `enableWebSdkInteroperability`
- `didAudioMuted`, `firstRemoteAudioFrameDecodedOfUid` and `firstRemoteAudioFrameOfUid`. Use the [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) callback instead.
- `streamPublishedWithUrl` and `streamUnpublishedWithUrl`. Use the [`rtmpStreamingChangedToState`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) callback instead.

## v2.9.1
v2.9.1 is released on Sep 19, 2019.

**New features**

#### Detecting local voice activity

This release adds the `report_vad(bool)` parameter to the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) method to enable local voice activity detection. Once it is enabled, you can check the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) struct of the [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:) callback for the voice activity status of the local user.

**Improvements**

#### Supporting more audio sample rates for recording

To enable more audio sample rate options for recording, this release adds a new [`startAudioRecording`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz. The original method supports only a fixed sample rate of 32 kHz and is deprecated.

**API changes**

To improve the user experience, we made the following changes in v2.9.1:

#### Added

- [`startAudioRecording`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:)
- The `report_vad` parameter in the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) method
- The `vad` member in the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) class

#### Deprecated

- `startAudioRecording`

## v2.9.0
v2.9.0 is released on Aug. 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`

If your app implements RTMP streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)
- [`rtmpStreamingChangedToState`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)

For how to implement the new methods, see [Push Streams to the CDN](../../en/Audio%20Broadcast/cdn_streaming_apple.md).

#### 2. Disabling/enabling the local audio

To improve the audio quality in the Communication profile, this release sets the system volume to the media volume after you call the `enableLocalAudio`(true) method. Calling `enableLocalAudio`(false) switches the system volume back to the in-call volume.

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannelByToken`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method to enable the audience in a Live Broadcast channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the [`leaveChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) and [`joinChannelByToken`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) methods. After the audience successfully switches to another channel by calling the [`switchChannelByToken`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method, the SDK triggers the [`didLeaveChannelWithStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLeaveChannelWithStats:) and [`didJoinChannel`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinChannel:withUid:elapsed:) callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different Live Broadcast channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)

During the media stream relay, the SDK reports the states and events of the relay with the  [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:) and [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host Across Channels](../../en/Audio%20Broadcast/media_relay_apple.md).

#### 3. Reporting the local and remote audio state

This release adds the [`localAudioStateChange`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) and [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: Stopped(0), Recording(1), Encoding(2), or Failed(3). When the state is Failed(3), see the `error` parameter for troubleshooting.
- The remote audio: Stopped(0), Starting(1), Decoding(2), Frozen(3), or Failed(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`localAudioStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

#### 5. Pulling the remote audio data

To improve the experience in audio playback, this release adds the following methods to pull the remote audio data. After getting the audio data, you can process it and play it with the audio effects that you want.

- [`enableExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

The difference between the `onPlaybackAudioFrame` callback and the `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte` method is as follows:

- `onPlaybackAudioFrame`: The SDK sends the audio data to the app once every 10 ms. Any delay in processing the audio frames may result in an audio delay.
- `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte`: The app pulls the remote audio data. After setting the audio data parameters, the SDK adjusts the frame buffer and avoids problems caused by jitter in external audio playback.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the [`AgoraChannelStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html) class:

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
- [`enableExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)
- [`localAudioStateChange`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:)
- [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:)
- [`localAudioStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:)
- [`switchChannelByToken`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 
- [`startChannelMediaRelay`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)
- [`AgoraChannelStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html): `txAudioBytes` and `rxAudioBytes`

#### Deprecated

- [`didMicrophoneEnabled`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didMicrophoneEnabled:). Use AgoraAudioLocalStateStopped(0) or AgoraAudioLocalStateRecording(1) in the [`localAudioStateChange`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) callback instead.
- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:). Use the [`remoteAudioStats`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) callback instead.

#### Deleted

- `configPublisher`

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string user IDs

Many apps use string user IDs. This release adds the following methods to enable apps to join an Agora channel directly with string user IDs as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. 

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your user IDs into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote statistics

To monitor the audio transmission quality during a call or live broadcast, this release adds the `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class.

**Improvements**

This release adds a `AgoraConnectionChangedKeepAliveTimeout(14)` member to the `AgoraConnectionChangedReason` parameter of the [connectionChangedToState](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.


**Issues fixed**

- Occasional crashes.

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)
- [getUserInfoByUid](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUid:withError:)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUserAccount:withError:)
- [didRegisteredLocalUser](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRegisteredLocalUser:withUid:)
- [didUpdatedUserInfo](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class

#### Deprecated

- The `lowLatency` member in the [AgoraLiveTranscoding](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) class


## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

This is the first release of the Agora Voice SDK for macOS. Refer to the following guides to quickly integrate the SDK and enable real-time voice communication in your project.

- Quick start
- Use security keys
- Report in-call statistics
- Adjust the volume
- Play audio effects/audio mixing
- Set the voice changer and reverberation effects
- Push Streams to the CDN
- Test or select a media device

If you migrate to this SDK from the macOS Video SDK, refer to the [Release notes for the macOS video SDK](../../en/Audio%20Broadcast/release_mac_video.md) for audio improvements.
