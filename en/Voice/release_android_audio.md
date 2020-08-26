
---
title: Release Notes
description: 
platform: Android
updatedAt: Wed Aug 26 2020 07:45:27 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Voice SDK for Android.

## Overview

The Voice SDK supports the following scenarios:

-   Voice call
-   Live interactive audio streaming

For the key features included in each scenario, see [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Agora Live Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

#### Known Issues and Limitations

##### Privacy changes

If your app targets Android 9, you should keep the following behavior changes in mind. These updates to device serial and DNS information enhance user privacy.

**Build serial number deprecation**

In Android 9, `Build.SERIAL` is always set to "UNKNOWN" to protect users' privacy.
If your app needs to access a device's hardware serial number, you should instead request the READ_PHONE_STATE permission, then call `getSerial()`.

**DNS privacy**

Apps targeting Android 9 should honor the private DNS APIs. In particular, apps should ensure that, if the system resolver is doing DNS-over-TLS, any built-in DNS client either uses encrypted DNS to the same hostname as the system, or is disabled in favor of the system resolver.

For more information about privacy changes, see [Android Privacy Changes](https://developer.android.com/about/versions/pie/android-9.0-changes-28#privacy-changes-p).

## v3.1.0
v3.1.0 was released on August 11, 2020.

**New features**

#### 1. Publishing and subscription states

This release adds the following callbacks to report the current publishing and subscribing states:

- `onAudioPublishStateChanged`: Reports the change of the audio publishing state.
- `onAudioSubscribeStateChanged`: Reports the change of the audio subscribing state.

#### 2. First local frame published callback

This release adds the `onFirstLocalAudioFramePublished` callback to report that the first audio frame is published. The `onFirstLocalAudioFrame` callback is deprecated from v3.1.0.

#### 3. Custom data report

This release adds the `sendCustomReportMessage` method for reporting customized messages. To try out this function, contact [support@agora.io](mailto:support@agora.io) and discuss the format of customized messages with us.

**Improvement**

#### 1. Regional connection

This release adds the following regions for regional connection. After you specify the region for connection, your app that integrates the Agora SDK connects to the Agora servers within that region.

- `AREA_CODE_JAPAN`: Japan.
- `AREA_CODE_INDIA`: India.

#### 2. Encryption

This release adds the `enableEncryption` method for enabling built-in encryption, and deprecates the following methods:

- `setEncryptionSecret`
- `setEncryptionMode`

#### 3. More in-call statistics

This release adds the following attributes to provide more in-call statistics:

- Adds `txPacketLossRate` in `LocalAudioStats`, which represents the audio packet loss rate (%) from the local client to the Agora edge server before applying anti-packet loss strategies.
- Adds `publishDuration` in `RemoteAudioStats`, which represents the total publish duration (ms) of the remote media stream.

#### 4. Audio profile

To improve audio performance, this release adjusts the maximum audio bitrate of each audio profile as follows:

| Profile                                   | v3.1.0                                                       | Earlier than v3.1.0                                          |
| :---------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `AUDIO_PROFILE_DEFAULT`                   | <li>For the interactive streaming profile: 64 Kbps</li><li>For the communication profile: 18 Kbps</li> | <li>For the interactive streaming profile: 52 Kbps</li><li>For the communication profile: 18 Kbps</li> |
| `AUDIO_PROFILE_SPEECH_STANDARD`           | 18 Kbps                                                      | 18 Kbps                                                      |
| `AUDIO_PROFILE_MUSIC_STANDARD`            | 64 Kbps                                                      | 48 Kbps                                                      |
| `AUDIO_PROFILE_MUSIC_STANDARD_STEREO`     | 80 Kbps                                                      | 56 Kbps                                                      |
| `AUDIO_PROFILE_MUSIC_HIGH_QUALITY`        | 96 Kbps                                                      | 128 Kbps                                                     |
| `AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO` | 128 Kbps                                                     | 192 Kbps                                                     |

#### 5. Log files

This release increases the default number of log files that the Agora SDK outputs from 2 to 5, and increases the default size of each log file from 512 KB to 1024 KB. By default, the SDK outputs five log files, `agorasdk.log`, `agorasdk_1.log`, `agorasdk_2.log`, `agorasdk_3.log`, `agorasdk_4.log`. The SDK writes the latest logs in `agorasdk.log`. When `agorasdk.log` is full, the SDK deletes the log file with the earliest modification time among the other four, renames `agorasdk.log ` to the name of the deleted log file, and creates a new `agorasdk.log` to record the latest logs.

#### 6. In-ear monitoring improvement on OPPO models (Android)

This release reduces the delay of in-ear monitoring on the following OPPO models:

- Reno4 Pro 5G
- Reno4 5G 

#### 7. Others

- Reduces the audio delay.
- Reduces the playback time of the first remote audio frame.

**Issues fixed**

This release fixed the following issues:

- `setAudioMixingPitch` did not work when setting the `pitch` parameter to certain values.

**API changes**

#### Added

- [`onAudioPublishStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a19d2c72ed37bc3c1e8fbb9744060cec8)
- [`onAudioSubscribeStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3fdd1d93b146c58e7bf69f36766b2f3a)
- [`onFirstLocalAudioFramePublished`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a94c87921fc48dbd80048efc785270808)
- [`enableEncryption`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8d283886c17dbd2555e1f967c7faff2d)
- [`txPacketLossRate`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_audio_stats.html#a3f39c69e3a02c05044603b28da879e9c) in `LocalAudioStats`
- [`publishDuration`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_audio_stats.html#ad56757c408074784356bbfac47f58af2) in `RemoteAudioStats`
- [`onRtmpStreamingEvent`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2c26ecc40133c2bb18b30f4752edc61c)
- Error code: [`ERR_NO_SERVER_RESOURCES(103)`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#ab0e9fe12b5357df5f03019d084183799)
- Warning code:
  - [`WARN_ADM_RECORD_IS_OCCUPIED(1033)`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#adead939e929d2a89b458ae7ece72f797)
  - [`WARN_ADM_RECORD_ABNORMAL_FREQUENCY(1021)`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#a1c0d1e891192c8a37a59cb9f32b7ba64)
  - [`WARN_ADM_PLAYOUT_ABNORMAL_FREQUENCY(1020)`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#a67dfe3691ed974e46f6f37cb696b01b3)
  - [`WARN_APM_RESIDUAL_ECHO(1053)`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#a449523bb31a7ce15f38006531244d537)

#### Deprecated

- `setEncryptionSecret`
- `setEncryptionMode`
- `onFirstLocalAudioFrame`

## v3.0.1

v3.0.1 was released on May 27, 2020.

**New features**

#### 1. Audio mixing pitch

To set the pitch of the local music file during audio mixing, this release adds `setAudioMixingPitch`. You can set the `pitch` parameter to increase or decrease the pitch of the music file. This method sets the pitch of the local music file only. It does not affect the pitch of a human voice.

#### 2. Voice enhancement

To improve the audio quality, this release adds the following enumerate elements in `setLocalVoiceChanger` and `setLocalVoiceReverbPreset`:

- Adds several elements that have the prefixes `VOICE_BEAUTY` and `GENERAL_BEAUTY_VOICE`. The `VOICE_BEAUTY` elements enhance the local voice, and the `GENERAL_BEAUTY_VOICE` enumerations add gender-based enhancement effects.
- Adds the enumeration `AUDIO_VIRTUAL_STEREO` and several enumerations that have the prefix `AUDIO_REVERB_FX`. The `AUDIO_VIRTUAL_STEREO` enumeration implements reverberation in the virtual stereo, and the `AUDIO_REVERB_FX` enumerations implement additional enhanced reverberation effects.

See [Set the Voice Changer and Reverberation Effects](../../en/Voice/voice_changer_android.md) for more information.

#### 3. Data post-processing in multiple channels (C++)

This release adds support for post-processing remote audio and video data in a multi-channel scenario by adding the following C++ methods:

- The `IAudioFrameObserver` class: [`isMultipleChannelFrameWanted`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a4b6bdf2a975588cd49c2da2b6eff5956) and [`onPlaybackAudioFrameBeforeMixingEx`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ab0cf02ba307e91086df04cda4355905b).

After successfully registering the audio observer, if you set the return value of `isMultipleChannelFrameWanted` as `true`, you can get the corresponding audio data from `onPlaybackAudioFrameBeforeMixingEx`. In a multi-channel scenario, Agora recommends setting the return value as `true`.

**Improvements**

- Implements low in-ear device latency on Huawei phones with EMUI v10 and above.
- Improves in-call audio quality. When multiple users speak at the same time, the SDK does not decrease volume of any speaker.
- Reduces overall CPU usage of the device.

**Fixed issues**

- Inaccurate report of the `onRemoteAudioStateChanged` callback, no audio, audio mixing and audio freezing.
- Failure to end a call, inaccurate report of the `onClientRoleChanged` callback, occasional crashes, and interoperability when using encryption.

**API changes**

This release adds the following APIs:

- [`setAudioMixingPitch`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1ffa38f7445ff0ba71515c931f2f4f6a)
- The enumeration [`AUDIO_VIRTUAL_STEREO`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#a4488f5ef2274a3e2e0dff5721f3bb708) and several elements that have the prefixes [`VOICE_BEAUTY`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#a6e16001b5e0f252460d584131fc11750), [`GENERAL_BEAUTY_VOICE`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#ab77b264331a44b104e5d1b333fc39ed8), and [`AUDIO_REVERB_FX`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_constants.html#a238965ba87ce04aaaa50c45f57c8727d)
- [`totalActiveTime`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_audio_stats.html#a18b02fb2d2af40bc0730c2c916a5729d) in `RemoteAudioStats`

## v3.0.0.2

v3.0.0.2 was released on Apr 22, 2020.

**New features**

#### Specify the area of connection

This release adds `create` for specifying the area of connection when creating an `RtcEngine` instance. This advanced feature applies to scenarios that have regional restrictions. You can choose from areas including Mainland China, North America, Europe, Asia (excluding Mainland China), and global (default).

After specifying the area of connection:

- When the app that integrates the Agora SDK is used within the specified area, it connects to the Agora servers within the specified area under normal circumstances.
- When the app that integrates the Agora SDK is used out of the specified area, it connects to the Agora servers either in the specified area or in the area where the SDK is located.

**Issues fixed**

This release fixed the occasional no-audio issue.

**API changes**

#### Added

[`create`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a45832a91b1051bc7641ccd8958288dba)

## v3.0.0

v3.0.0 was released on Mar 4, 2020.

On Mar 24, 2020, we fixed occasional issues relating to no audio, audio mixing, multiple `onClientRoleChanged` callbacks, and SDK crashes.

In this release, Agora improves the user experience under poor network conditions for both the `COMMUNICATION` and `LIVE_BROADCASTING` profiles through the following measures:

- Adopting a new architecture for the `COMMUNICATION` profile.
- Upgrading the last-mile network strategy for both the `COMMUNICATION` and `LIVE_BROADCASTING` profiles,  which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.0 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.0.

We also upgrade the On-premise Recording SDK to v3.0.0. Ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**Compatibility changes**

#### Default log file path change

To avoid privilege issues, this release changes the default log file path from `/storage/emulated/0/<package name>/` to `/storage/emulated/0/Android/data/<package name>/files/`.

**New features**

#### 1. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the [`RtcChannel`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_channel.html) and [`IRtcChannelEventHandler`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_channel_event_handler.html) classes. By creating multiple `RtcChannel` objects, a user can join the corresponding channels at the same time.

After joining multiple channels, users can receive the audio and video streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams. See [Join multiple channels](../../en/Voice/multiple_channel_android.md) for details.

#### 2. Adjusting the playback volume of the specified remote user

Adds [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aac9c5135996428d9a238fe8e66858268) for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or live interactive streaming to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

#### 3. Agora Mediaplayer Kit

To enrich the playability of the live interactive streaming, Agora releases the Mediaplayer Kit plug-in, which supports the host playing local or online media resources and sharing them with all users in the channel during the streaming. See [Mediaplayer Kit release notes](https://docs.agora.io/en/Interactive%20Broadcast/mediaplayer_release_android?platform=Android) for details.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile of `AUDIO_PROFILE_DEFAULT (0)` in the `LIVE_BROADCASTING` profile.

| SDK | AUDIO_PROFILE_DEFAULT (0) | 
| ---------------- | ---------------- | 
| v3.0.0      | A sample rate of 48 KHz, music encoding, mono, and a bitrate of up to 52 Kbps.     | 
| Earlier than v3.0.0   | A sample rate of 32 KHz, music encoding, mono, and a bitrate of up to 44 Kbps.   |

#### 2. Quality statistics

Adds the following members in the [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html) class for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`

#### Others

This release enables interoperability between the RTC Native SDK and the RTC Web SDK by default, and deprecates the `enableWebSdkInteroperability` method. 

**Issues fixed**

- Audio issues relating to audio mixing, audio encoding, and echoing.
- Other issues relating to app crashes, log file, and unstable service during CDN live streaming.

**API changes**

#### Behavior change

- Calling `enableLocalAudio`(false) does not change the in-call volume to media volume. 
- When the device is connected to the earpiece or Bluetooth, calling `setEnableSpeakerphone`(true) does not route the audio to the speakerphone.

#### Added

- `channelId` in [`AudioVolumeInfo`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_audio_volume_info.html)
- `gatewayRtt`, `memoryAppUsageRatio`, `memoryTotalUsageRatio`, and `memoryAppUsageInKbytes` in [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html)
- [`createRtcChannel`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9eb0770851a8ba489564f72f9b280bca)
- [`RtcChannel`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_channel.html)
- [`IRtcChannelEventHandler`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_channel_event_handler.html)


#### Deprecated

- `enableWebSdkInteroperability`.
- `onUserMuteAudio`, `onFirstRemoteAudioDecoded`, and `onFirstRemoteAudioFrame`, replaced by [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a24fd6b0d12214f6bc6fa7a9b6235aeff).
- `onStreamPublished` and `onStreamUnpublished`, replaced by [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7b9f1a5d87480cfd6187c3da0ade3f94).

## v2.9.4

v2.9.4 was released on Feb 17, 2020.

This release fixed some abnormal behaviors on Android devices.

## v2.9.2

v2.9.2 is released on Oct 18, 2019.

This release fixed crashes on some Android device.

## v2.9.1

v2.9.1 is released on Sep 19, 2019.

**New features**

#### 1. Detecting local voice activity
This release adds the `report_vad`(bool) parameter to the [enableAudioVolumeIndication](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aaec0b8db9458b45d14cdcb3003f76fbe) method to enable local voice activity detection. Once it is enabled, you can check the [AudioVolumeInfo](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_audio_volume_info.html) struct of the `onAudioVolumeIndication` callback for the voice activity status of the local user.

#### 2. Removing the event handler
This release adds the [removeHandler](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a5e807ee4302756e6912a4fd1ed7a0db3) method to remove specified IRtcEngineEventHandler objects when you want to stop listening for specific events.

**Improvements**

#### 1. Supporting more audio sample rates for recording
To enable more audio sample rate options for recording, this release adds a new [startAudioRecording](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac2ad403a7a75617316673f251615ef92) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz. The original method supports only a fixed sample rate of 32 kHz and is deprecated.

#### 2. Adding error codes

This release adds the following error codes in the ErrorCode class:

- `ERR_ALREADY_IN_USE(19)`
- `ERR_WATERMARK_PATH(125)`
- `ERR_INVALID_USER_ACCOUNT(134)`
- `ERR_AUDIO_BT_SCO_FAILED(1030)`
- `ERR_ADM_NO_RECORDING_DEVICE(1359)`
- `ERR_VCM_UNKNOWN_ERROR(1600)`
- `ERR_VCM_ENCODER_INIT_ERROR(1601)`
- `ERR_VCM_ENCODER_ENCODE_ERROR(1602)`
- `ERR_VCM_ENCODER_SET_ERROR(1603)`

For detailed descriptions for each error, see [Error Codes](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_error_code.html).

**Issues fixed**

#### Audio
- A user makes a call after connecting to a Bluetooth device. After the call ends, the user watches YouTube and cannot hear any sound.
- The audio route is different from the settings in the `setEnableSpeakerphone` method when Bluetooth is connected in the `COMMUNICATION` profile.
- Exceptions occur in the audio route when a user is in the channel.
- The app crashes when using external audio sources in the push mode. 
- Audio freezes.
- After turning off the Bluetooth headset, the audio route becomes the earpiece instead of the loudspeaker.
- Echos occur when a user is in the channel.
- Occasional noise occurs in the `LIVE_BROADCASTING` profile.
- The app fails to play online music files using the `startAudioMixing` method on devices running Android 10.

#### Miscellaneous

- The OpenSSL version is outdated.

**API Changes**

#### Added

- [startAudioRecording](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ac2ad403a7a75617316673f251615ef92)
- [removeHandler](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a5e807ee4302756e6912a4fd1ed7a0db3)
- The `report_vad` parameter in [enableAudioVolumeIndication](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aaec0b8db9458b45d14cdcb3003f76fbe)
- The `vad` member in [AudioVolumeInfo](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_audio_volume_info.html) 

#### Deprecated

- `startAudioRecording`

## v2.9.0

v2.9.0 is released on Aug 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`

If your app implements CDN streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a3cb9804ae71819038022d7575834b88c)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4445b4ca9509cc4e2966b6d308a8f08f)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a87b3f2f17bce8f4cc42b3ee6312d30d4)
- [`onRtmpStreamingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7b9f1a5d87480cfd6187c3da0ade3f94)

For how to implement the new methods, see [Push Streams to the CDN](../../en/Voice/cdn_streaming_android.md).

#### 2. Disabling/enabling the local audio

To improve the audio quality in the `COMMUNICATION` profile, this release sets the system volume to the media volume after you call the `enableLocalAudio`(true) method. Calling `enableLocalAudio`(false) switches the system volume back to the in-call volume.

**New features**

#### 1. Faster switching to another channel

This release adds the  [`switchChannel`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a72f13225defc1b14dfb29820a0495da2) method to enable the audience in a live-streaming channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the `leaveChannel` and `joinChannel` methods. After the audience successfully switches to another channel by calling the `switchChannel` method, the SDK triggers the `onLeaveChannel` and `onJoinChannelSuccess` callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different live-streaming channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6f09ba685f8ab01d7dc06173286950f6)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#abd40d706379d27cf617376a504f394bd)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0f9f19e48c21190dd4e697dec632c328)

During the media stream relay, the SDK reports the states and events of the relay with the [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89fd95b3536e8e6afd5f001926162f66) and [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a6fe2367e9ea61e48a4cc3b373d198b54) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host across Channels](../../en/Voice/media_relay_android.md).

#### 3. Reporting the local and remote audio state

This release adds the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a59946a989f87c737899e2284539adf09) and [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a24fd6b0d12214f6bc6fa7a9b6235aeff) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: STOPPED(0), RECORDING(1), ENCODING(2), or FAILED(3). When the state is FAILED(3), see the `error` parameter for troubleshooting.
- The remote audio: STOPPED(0), STARTING(1), DECODING(2), FROZEN(3), or FAILED(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`onLocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aeba2aa3fc29404fc6f25bff5c00bfdf9) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

#### 5. Pulling the remote audio data

To improve the experience in audio playback, this release adds the following methods to pull the remote audio data. After getting the audio data, you can process it and play it with the audio effects that you want.

- [`setExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a270c0607d443790e92cdbd0d45ba1732)
- [`pullPlaybackAudioFrame`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae15064944870692e9a0a59fdc87654c4)

The difference between the [`onPlaybackFrame`](https://docs.agora.io/en/Voice/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_frame_observer.html#a3781dd30d34a0634140872a9dd131488) callback and the `pullPlaybackAudioFrame` method is as follows:

- `onPlaybackFrame`: The SDK sends the audio data to the app once every 10 ms. Any delay in processing the audio frames may result in an audio delay.
- `pullPlaybackAudioFrame`: The app pulls the remote audio data. After setting the audio data parameters, the SDK adjusts the frame buffer and avoids problems caused by jitter in external audio playback.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the `RtcStats` class:

- [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html): The total number of the sent audio bytes and received audio bytes during a session.


#### 2. Other Improvements

- Reduces the earpiece delay.
- Improves the audio quality when the audio scenario is set to Game Streaming.
- Improves the audio quality after the user disables the microphone in the `COMMUNICATION` profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- The audience cannot hear the host after the host sets the in-ear monitoring volume to 0.
- Failure to play the audio file by calling the `startAudioMixing` method. 
- The audio route cannot be set to Bluetooth on some devices.
- Crashes occur when using the raw audio data.
- The audio route does not conform to the default settings after the device disconnects from Bluetooth.

#### Miscellaneous

- Occasionally mixed streams in CDN streaming. 
- Occasional crashes occur after joining the channel on some devices.

**API Changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added

- [`setExternalAudioSink`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a270c0607d443790e92cdbd0d45ba1732)
- [`pullPlaybackAudioFrame`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae15064944870692e9a0a59fdc87654c4)
- [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a59946a989f87c737899e2284539adf09)
- [`onRemoteAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a24fd6b0d12214f6bc6fa7a9b6235aeff)
- [`onLocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aeba2aa3fc29404fc6f25bff5c00bfdf9)
- [`switchChannel`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a72f13225defc1b14dfb29820a0495da2)
- [`startChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6f09ba685f8ab01d7dc06173286950f6)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#abd40d706379d27cf617376a504f394bd)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0f9f19e48c21190dd4e697dec632c328)
- [`onChannelMediaRelayStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a89fd95b3536e8e6afd5f001926162f66)
- [`onChannelMediaRelayEvent`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a6fe2367e9ea61e48a4cc3b373d198b54)
- [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html): `txAudioBytes` and `rxAudioBytes`

#### Deprecated

- `onMicrophoneEnabled`. Use LOCAL_AUDIO_STREAM_STATE_CHANGED(0) or LOCAL_AUDIO_STREAM_STATE_RECORDING(1) in the [`onLocalAudioStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aeba2aa3fc29404fc6f25bff5c00bfdf9) callback instead. 
- `onRemoteAudioTransportStats`. Use the [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54) callback instead.

#### Deleted

- `configPublisher`


## v2.8.2

v2.8.2 is released on Aug 1, 2019. 

This release fixed the interoperating problem with the Agora Web SDK.

## v2.8.1

v2.8.1 is released on Jul. 20, 2019.

**New features**

- Support for the x86-64 architecture.
- Support for Android Q.

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string user IDs

Many apps use string user IDs. This release adds the following methods to enable apps to join an Agora channel directly with string user IDs as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa37ea6307e4d1513c0031084c16c9acb)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a310dbe072dcaec3892c4817cafd0dd88)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. 

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your user IDs into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote audio statistics

To monitor the audio transmission quality during a call or live interactive streaming, this release adds the `totalFrozenTime` and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_audio_stats.html) class, to report the audio freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_audio_stats.html) class.

**Improvements**

This release adds a `CONNECTION_CHANGED_KEEP_ALIVE_TIMEOUT(14)` member to the `reason` parameter of the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.

**Issues Fixed**

#### Audio

- Occasional audio freezes. 

#### Miscellaneous

- When the log file path specified in the `setLogFile` method does not exist, no log file is generated and the default log file is incomplete.

**API Changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa37ea6307e4d1513c0031084c16c9acb)
- [joinChannelWithUserAccount](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a310dbe072dcaec3892c4817cafd0dd88)
- [getUserInfoByUid](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9a787b8d0784e196b08f6d0ae26ea19c)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#afd4119e2d9cc360a2b99eef56f74ae22)
- [onLocalUserRegistered](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aca1987909703d84c912e2f1e7f64fb0b)
- [onUserInfoUpdated](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa3e9ead25f7999272d5700c427b2cb3d)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [RemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_audio_stats.html) class

#### Deprecated

- The lowLatency member in the [LiveTranscoding](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html) class

## v2.4.1
v2.4.1 is released on Jun 12, 2019.

**Compatibility changes**

Ensure that you read the following SDK behavior changes if you migrate from an earlier SDK version.

#### Publishing streams to the CDN

To improve the usability of the CDN streaming service, v2.4.1 defines the following parameter limits:

| Class **/** Interface  | Parameter Limit                                              |
| ---------------------- | ------------------------------------------------------------ |
| [LiveTranscoding](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html)        | <li>[videoFramerRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html#a514340a98a537fdc4f91003aed2068a6): Frame rate (fps) of the CDN live output video stream. The value range is [0, 30], and the default value is 15. Agora adjusts all values over 30 to 30.<li>[videoBitrate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html#a514340a98a537fdc4f91003aed2068a6): Bitrate (Kbps) of the CDN live output video stream. The default value is 400. Set this parameter according to the [Video Bitrate Table](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1video_1_1_video_encoder_configuration.html#a4b090cd0e9f6d98bcf89cb1c4c2066e8). If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.<li>[videoCodecProfile](https://docs.agora.io/en/Voice/API%20Reference/java/enumio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding_1_1_video_codec_profile_type.html): The video codec profile. Set it as **BASELINE**, **MAIN**, or **HIGH** (default). If you set this parameter to other values, Agora adjusts it to the default value of **HIGH**.<li>[width](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html#a514340a98a537fdc4f91003aed2068a6) and [height](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html#a80960c1a972e9b3851fd16d921f8a75c): Pixel of the video. The minimum value of width x height is 16 x 16.</li> |
| [AgoraImage](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1video_1_1_agora_image.html)             | `url`: The maximum length of this parameter is **1024** bytes. |
| [addPublishStreamUrl](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4445b4ca9509cc4e2966b6d308a8f08f)    | `url`: The maximum length of this parameter is **1024** bytes. |
| [removePublishStreamUrl](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4445b4ca9509cc4e2966b6d308a8f08f) | `url`: The maximum length of this parameter is **1024** bytes. |

This release also adds the [audioCodecProfile](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html#ac7d4a839af2994e68d8f14544d323ae9) parameter in the `LiveTranscoding` class to set the audio codec profile type. The default type is LC-AAC, which means the low-complexity audio codec profile.

v2.4.1 also adds five error codes to the `error` parameter in the [onStreamPublished](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7b9f1a5d87480cfd6187c3da0ade3f94) method for quick troubleshooting.

**New features**

#### 1. State of the RTMP streaming

v2.4.1 adds the [onRtmpStreamingStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7b9f1a5d87480cfd6187c3da0ade3f94) callback to indicate the state of the RTMP streaming and help you troubleshoot issues when exceptions occur. In this callback, the SDK returns the IDLE, `CONNECTING`, `RUNNING`, `RECOVERING`, or `FAILURE` state. When the state is `FAILURE`, you can use the error code for troubleshooting. You can still use the `onStreamPublished` and `onStreamUnpublished` callbacks, but we do not recommend using them.

#### 2. More reasons for a network connection state change

In the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) callback, v2.4.1 adds error codes to the `reason` parameter to help you troubleshoot issues when exceptions occur. The SDK returns the `onConnectionStateChanged` callback whenever the connection state changes. This release also deprecates `WARN_LOOK_UP_CHANNEL_REJECTED(105)`, `ERR_TOKEN_EXPIRED(109)`, and `ERR_INVALID_TOKEN(110)`.

#### 3. State of the local network type 

v2.4.1 adds the  [onNetworkTypeChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a75b014a87d0ead6cd4fa519a630f6f90) callback to indicate the local network type. In this callback, the SDK returns the `UNKNOWN`, `DISCONNECTED`, `LAN`, `WIFI`, `2G`, `3G`, or `4G` type. When the network connection is interrupted, this callback indicates whether or not the interruption is caused by a network type change or poor network conditions.

#### 4. Getting the audio mixing volume

v2.4.1 adds the [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6eef6e4d3d148c25993376f93ebbb8e9) and [getAudioMixingPublishVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a962819abd0e5458b89cefb756bb9e631) methods, which respectively gets the audio mixing volume for local playback and remote playback, to help you troubleshoot audio volume related issues.

#### 5. Reporting when the first remote audio frame is received and decoded

To get the more accurate time of the first audio frame from a specified remote user, v2.4.1 adds the  [onFirstRemoteAudioDecoded](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0fcac6cafae63e4ef453ddd041e80f8a) callback to report to the app that the SDK decodes first remote audio. This callback is triggered in either of the following scenarios:

- The remote user joins the channel and sends the audio stream.
- The remote user stops sending the audio stream and re-sends it after 15 seconds.

The difference between the onFirstRemoteAudioDecoded and `onFirstRemoteAudioFrame` callbacks is that the `onFirstRemoteAudioFram`e callback occurs when the SDK receives the first audio packet. It occurs before the `onFirstRemoteAudioDecoded` callback.

**Improvements**

#### 1. Playing multiple online audio effect files simultaneously

v2.4.1 adds the support for playing multiple online audio effect files simultaneously by allowing you to call the [playEffect](https://docs.agora.io/en/Voice/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#a6fd330db3e3e5735f7f8d5c36bc3fea1) method multiple times with the URLs of the online audio effect files.

#### 2. Reporting more statistics

- v2.4.1 adds the [txPacketLossRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html#a6b0c3798427c6bf07b829896e29ace5e) and [rxPacketLossRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html#a72df02822bfcc37dfcdb543fd2ba46af) parameters in the [RtcStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html) class. These parameters return the packet loss rate from the local client to the server and vice versa.

- To provide more accurate statistics of the local and remote video, v2.4.1 makes the following changes to the following classes:
  - [LocalVideoStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html): Adds the  [encoderOutputFrameRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html#af6350acef5578bf0501a234fc36d86a3) and [rendererOutputFrameRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html#aa754035a384b502a45c6fed6f17038da) parameters.
  - [RemoteVideoStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_video_stats.html): Adds the[decoderOutputFrameRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_video_stats.html#aafc03c6a890c36dc9810537c47ce0cd9) parameter, and renames the `receivedFrameRate` parameter to the [rendererOutputFrameRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_remote_video_stats.html#aa09441cb1b9a0f4318cd59b0ca5b3ffb) parameter.

#### 3. Miscellaneous

- Improved the sound quality of the [GAME_STREAMING](https://docs.agora.io/en/Voice/API%20Reference/java/enumio_1_1agora_1_1rtc_1_1_constants_1_1_audio_scenario.html#aedcb78447298f4794ba8df7a72d71909) audio scenario.
- Reduced the audio latency.
- Reduced the SDK package size by 0.5 M.
- Improved the accuracy of the network quality after users change the video bitrate.
- Enabled the audio quality notification callback by default, that is, enabled the [onRemoteAudioStats](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54) callback without calling the `enableAudioVolumeIndication` method.
- Improved the stability of CDN streaming services.

**Issues fixed**

#### Audio

- The audio stream is played through the loudspeaker even after the user plugs in the earphone. 
- The user cannot hear the audio mixing file through Bluetooth in the single-host scenario.
- Exceptions occur when playing the audio mixing file in the `LIVE_BROADCASTING` profile.

#### Miscellaneous

- Users still receive the `onNetworkQuality` callback after leaving the channel.
- Occasional crashes.
- The app quits after calling `joinChannel`.

**API changes**

To improve your experience, we made the following changes to the APIs:

#### Unified the C++ interface for all platforms

v2.4.1 unifies the behavior of the C++ interfaces across different platforms so that you can apply the same code logic on different platforms. v2.4.1 implements the methods of the `RtcEngineParameters` class in the `IRtcEngine` class. Refer to [Agora C++ API Reference for All Platforms home page](https://docs.agora.io/en/Voice/API%20Reference/cpp/index.html) for the applicable platforms and considerations of each interface.

#### Added

- [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6eef6e4d3d148c25993376f93ebbb8e9)
- [getAudioMixingPublishVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a962819abd0e5458b89cefb756bb9e631)
-  [onFirstRemoteAudioDecoded](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0fcac6cafae63e4ef453ddd041e80f8a)
- [onNetworkTypeChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a75b014a87d0ead6cd4fa519a630f6f90)
- [onRtmpStreamingStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7b9f1a5d87480cfd6187c3da0ade3f94)
- The  [audioCodecProfile](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_transcoding.html#ac7d4a839af2994e68d8f14544d323ae9) parameter in the `LiveTranscoding` class
- The [txPacketLossRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html#a6b0c3798427c6bf07b829896e29ace5e) and [rxPacketLossRate](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_rtc_stats.html#a72df02822bfcc37dfcdb543fd2ba46af) parameters in the `RtcStats` class

#### Deprecated

- `enableAudioQualityIndication`
- The `WARN_LOOKUP_CHANNEL_REJECTED(105)` warning code. Use CONNECTION_CHANGED_REJECTED_BY_SERVER(10) in the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) callback instead.
- The `ERR_TOKEN_EXPIRED(109)` error code. Use CONNECTION_CHANGED_TOKEN_EXPIRED(9) in the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) callback instead.
- The `ERR_INVALID_TOKEN(110)` error code. Use CONNECTION_CHANGED_INVALID_TOKEN(8) in the [onConnectionStateChanged](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) callback instead.

## v2.4.0 and Earlier

**v2.4.0**

v2.4.0 is released on April 1, 2019.

#### New features

##### 1. Voice changer and voice reverberation

Adding voice changer and reverberation effects in an audio chat room brings much more fun. v2.4.0 adds the [`setLocalVoiceChanger`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ade6883c7878b7a596d5b2563462597dd) and [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a10dd25bc8e129512cd6727133b7fc42f) methods, allowing you to change your voice or reverberation by choosing from the preset options. See [Adjust the pitch and tone](../../en/Voice/voice_effect_android.md).

##### 2. Tracking the sound position of a remote user 

v2.4.0 adds the [`enableSoundPositionIndication`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aaeb3e1df5d2cb091bd2e9c41f156d3c0) and [`setRemoteVoicePosition`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a7d851c2cabde18c2438c1ebe8bf763de) methods. Call the [`enableSoundPositionIndication`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aaeb3e1df5d2cb091bd2e9c41f156d3c0) method before joining a channel to enable stereo panning for the remote users, and then you can call the [`setRemoteVoicePosition`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a7d851c2cabde18c2438c1ebe8bf763de) method to track the position of a remote user.

##### 3. Pre-call last-mile network probe test

Conducting a last-mile probe test before joining the channel helps the local user to evaluate or predict the uplink network conditions. v2.4.0 adds the [`startLastmileProbeTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a81c6541685b1c4437d9779a095a0f871), [`stopLastmileProbeTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae21243b8da8bda9ee5f3a00621cbf959), and [`onLastmileProbeResult`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad74a9120325bfeccdec4af4611110281) APIs, allowing you to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

##### 4. State of an audio mixing file 

v2.4.0 adds the [`onAudioMixingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aee0aa9286a39654312b162750713e986) callback to report any change of the audio-mixing file playback state (playback succeeds or fails) and the corresponding reason. This release also adds the warning code 701, which is triggered if the local audio-mixing file does not exist, or if the SDK does not support the file format or cannot access the music file URL when playing the audio-mixing file.

##### 5. Setting the log file size

The SDK has two log files, each with a default size of 512 KB. In case some customers require more than the default size, v2.4.0 adds the [`setLogFileSize`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a50fd37c6f5b8fc144b18ed4620aee6fc) method for setting the log file size (KB).

##### 6. Cloud proxy

Supports the cloud proxy service. See [Use Cloud Proxy](../../en/Voice/cloudproxy_native.md) for details.

#### Improvements

##### 1. Accuracy of call quality statistics

- v2.4.0 adds the `intervalInSeconds` parameter to the [`startEchoTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a712bb50be350186d097f4deed8e1aa37) method, allowing you to set the interval between when you speak and when the recording plays back.
- v2.4.0 adds three parameters to the [`LocalVideoStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html) class: [`targetBitrate`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html#a4e5c046867a773a74096663bd894e843) for setting the target bitrate of the current encoder, [`targetFrameRate`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html#a01b15bb718064ed086edbafcd1678c9a) for setting the target frame rate, and [`qualityAdaptIndication`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler_1_1_local_video_stats.html#a7a93886b530e5f9ed2fec22ca9d3f5c0) for reporting the quality of the local video since last count.

##### 2. Core quality improvements

- Reduces the audio delay.
- Improves the video quality and stability. 
- Shortens the time to render the first remote video frame. 
- Reduces the time delay when playing through the earpiece and minimizes the echo.

#### Issues Fixed

##### Audio

- Calling the `enableLocalAudio` method disconnects all connected Bluetooth devices.
- The SDK does not support audio mixing URLs with Chinese characters.
- Volume levels of the high-pitch sound are lowered.
- Sounds are occasionally played fast.
- The app cannot adjust the volume on some devices.

##### Miscellaneous

- The user drop-offline time between Android and iOS is not unified.
- The SEI information does not synchronize with the media stream when publishing transcoded streams to the CDN.

#### API Changes

To improve your experience, we made the following changes to the APIs:

##### Added

- [`setLocalVoiceChanger`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ade6883c7878b7a596d5b2563462597dd)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a10dd25bc8e129512cd6727133b7fc42f)
- [`enableSoundPositionIndication`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aaeb3e1df5d2cb091bd2e9c41f156d3c0)
- [`setRemoteVoicePosition`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a7d851c2cabde18c2438c1ebe8bf763de)
- [`startLastmileProbeTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a81c6541685b1c4437d9779a095a0f871)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ae21243b8da8bda9ee5f3a00621cbf959)
- [`setRemoteUserPriority`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ad4705f9e56f0cf7e0a3e9cbd09ec8f61)
- [`startEchoTest`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a712bb50be350186d097f4deed8e1aa37)
- [`setLogFileSize`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a50fd37c6f5b8fc144b18ed4620aee6fc)
- [`onAudioMixingStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aee0aa9286a39654312b162750713e986)
- [`onLastmileProbeResult`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad74a9120325bfeccdec4af4611110281)

##### Deprecated

- `startEchoTest`

**v2.3.3**

v2.3.3 is released on January 24, 2019. 

#### Issues Fixed

- Occasional inaccurate statistics returned in the `onNetworkQuality` callback.
- Occasional crashes on Huawei P9.

**v2.3.2**

v2.3.2 is released on January 16, 2019.

#### Compatibility changes

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the `LIVE_BROADCASTING` profile. 

Before upgrading your SDK, ensure that the version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

#### New features

##### Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. 

This release also changes the behavior of the [adjustPlaybackSignalVolume](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b) method to control only the voice volume. Therefore, to mute the local audio playback, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods.

See [Adjust the Volume](../../en/Voice/volume_android.md) for the scenarios and corresponding APIs.

#### Improvements

##### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `onAudioQuality` callback and replaces it with the `onRemoteAudioStats` callback to improve the accuracy of the call quality statistics. The `onRemoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `onNetworkQuality` callback for the uplink and downlink network qualities.

- [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`onNetworkQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a76be982389183c5fe3f6e4b03eaa3bd4): Reports the last mile network quality of each user in the channel.

We plan to improve the following callback in subsequent versions:

- [`onLastmileQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Voice/in-call_quality_android.md).

##### 2. New network connection policy 

v2.3.2 adds the following API method and callback to get the current network connection state and the reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9): Gets the connection state of the SDK.
- [`onConnectionStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`onConnectionInterrupted`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88) and [`onConnectionBanned`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `onConnectionStateChanged` callback when the network connection state changes. The SDK also triggers the `onConnectionInterrupted` and `onConnectionBanned` callbacks under certain circumstances, but we do not recommend using them.

##### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live interactive streaming. You can use this feedback for future product improvement. We strongly recommend integrating this method in your app.

##### 4. Other improvements

- Minimizes packet loss under unreliable network conditions in the `LIVE_BROADCASTING` profile.
- Improves the stability in pushing streams.
- Improves the performance of the SDK on Android 6.0 or later.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.

#### Issues Fixed

The following issues are fixed in v2.3.2:

##### SDK

- Crashes on emulators, such as Yeshen and mumu. 
- Crashes on Android 6.0+ due to x86 .so relocation.

##### Audio

- A user joins a live-streaming channel with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another app.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- On Huawei Mate 20 X, a remote user cannot hear any voice when the app switches to the background and the user opens another app.
- Echo on Pixel 2.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an app switches back from the background. 

#### API Changes

To improve your experience, we made the following changes to the APIs:

##### Added:

- [`getConnectionState`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`onConnectionStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)
- [`onCameraExposureAreaChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ab6bc82a55191e596d5bf5a7c56bdf95e)
- [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54)

##### Deprecated

- [`onAudioQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abeac442a777e103536dcf9c8ce2d7135)
- [`onConnectionInterrupted`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88)
- [`onConnectionBanned`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db)

**v2.3.1**

v2.3.1 is released on October 12, 2018. 

#### New features

##### Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this version adds the `enableLocalAudio` method is to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `onMicrophoneEnabled` callback, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and the `muteLocalAudioStream` method is that the `enableLocalAudio` method does not capture or send any audio stream, while the `muteLocalAudioStream` method captures but does not send audio streams.

#### Issues Fixed

- `LIVE_BROADCASTING` profile: Delay at the client due to incorrect statistics.
- `LIVE_BROADCASTING` profile: Occasional crashes on some Android devices after a user repeats the process of switching roles between `BROADCASTER` and `AUDIENCE`.
- Occasionally on some Android devices, a user hears a popping sound if the user leaves the channel at the same time another user is speaking.

**v2.3.0**

v2.3.0 is released on August 31, 2018.

#### Compatibility changes

-   From v2.3.0, the `LiveTranscoding` class is moved from the *io.agora.live* package to the `io.agora.rtc.live` package.
-   Fixed a typo in the constants.java API in v2.3.0.
    -   Before:

    ```
    public static final int SOFEWARE_ENCODER = 1;
    ```

    -   After:

    ```
    public static final int SOFTWARE_ENCODER = 1;
    ```

-   The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

#### New features

##### 1. Notifies the user that the token expires in 30 seconds

The SDK returns the `onTokenPrivilegeWillExpire` callback 30 seconds before a token expires to notify the app to renew it. When this callback is received, you need to generate a new token on your server and call the `renewToken` method to pass the newly-generated token to the SDK.

##### 2. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate and time delay

The `onRemoteAudioTransportStats` callback is added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or the live interactive streaming, the SDK triggers these callbacks once every two seconds after the user receives audio/video packets from a remote user. The callbacks include the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

#### Improvements

-   Improves the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
-   Improves the audio encoder efficiency in the live interactive streaming to reduce user traffic while ensuring the call quality.
-   Improves the audio quality during a call or the live interactive streaming using the deep-learning algorithm.

#### Issues Fixed

- Excessive increase in memory usage when multiple delegated hosts start streaming in the channel.
- Occasional crashes on some Android devices.
- Occasional crashes after interoperating with devices of other platforms for some Android devices.
- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasionally, the remote user cannot hear the host when the host switches between `AUDIENCE` and `BROADCASTER`.
- Occasionally, the `destroy` method does not respond after a user enables the video and joins a channel.
- Occasional crashes on Android devices when remote users frequently join and leave the channel.
- Occasionally, the audience cannot adjust the channel volume.
- Occasional crashes when one of the two hosts mutes or disables the local audio while playing the background music.
- Occasional crashes on some devices when preloading the sound effects.
- Failure to enable the hardware encoder on some Android devices.
- The host cannot receive the audio/video stream of the delegated host on some Android devices.
- Occasional crashes on some Android devices when a user frequently changes the token.
- Occasional inter-operational failures between SIP devices and the SDK.
- Occasional echo issues when using a specific audio card.

#### API Changes

To improve your experience, we made the following changes to the APIs:

To avoid adding too many users with the same uid into the CDN publishing channel, the following API method is deleted in v2.3.0, and the return value type of `addUser` is changed from void to int.

-   <code>setUser</code>

The following API methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. For more information on the Recording SDK, see [Release Notes for Agora Recording SDK](../../en/Voice/release_recording.md).

-   <code>startRecordingService</code>
-   <code>stopRecordingService</code>
-   <code>refreshRecordingServiceStatus</code>

The following deprecated API methods are deleted and no longer supported from v2.3.0:

-   <code>monitorConnectionEvent</code>
-   <code>monitorBluetoothHeadsetEvent</code>
-   <code>monitorHeadsetEvent</code>
-   <code>setPreferHeadset</code>
-   <code>switchView</code>
-   <code>setSpeakerphoneVolume</code>

**v2.2.3**

v2.2.3 is released on July 5, 2018. 

#### Compatibility changes

The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see `Token Migration Guide`.

#### Issues Fixed

- Occasional online statistics crashes.
- The host's voice distorts occasionally on some Android devices.
- Occasional crashes during the live interactive streaming.
- Excessive increase in the memory usage when multiple delegated hosts start streaming in the channel.
- Receiving the <code>onLeaveChannel</code> callback long after a user has left the channel on some Android devices.
- Failing to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the host's in the live interactive streaming.

**v2.2.2**

v2.2.2 is released on June 21, 2018.

#### Issues Fixed

- Fixed occasional online statistics crashes.
- Fixed occasional audio crashes on some Android devices.
- Fixed the issue that the host's voice distorts occasionally on some Android devices.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of receiving the `onLeaveChannel` callback long after a user has left the channel on some Android devices.

**v2.2.1**

v2.2.1 is released on May 30, 2018.

#### Issues Fixed

- Occasional crashes during gaming on some Android devices.
- The soundtrack pointer cannot be retrieved on some Android devices.
- Occasional crashes on some Android devices.
- The audio volume on some Android devices cannot be adjusted after a headset is plugged in.


**v2.2.0**

v2.2.0 is released on May 4, 2018. 

#### New features

##### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method for the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

##### 2. Deploy the proxy at the server

We provide a proxy package for enterprise users with corporate firewalls to deploy before accessing our services. 


#### Improvements

##### 1. Audio volume indication

Improves the <code>enableAudioVolumeIndication</code> method. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

##### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method improves its data accuracy. 

##### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, the <code>onLastmileQuality</code> callback changes the detection from a fixed bitrate to the bitrate set by the customer in the <code>setVideoProfile</code> method to improve data accuracy. When the network condition is unknown, the SDK triggers this callback once every two seconds. 

##### 4. Audio quality enhancement

Improves the audio quality in scenarios that involve music playback.


**v2.1.3**

v2.1.3 is released on April 19, 2018. 

In v2.1.3, Agora updates the bitrate values of the <code>setVideoProfile</code> method in the `LIVE_BROADCASTING` profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

#### Issues Fixed

Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.


**v2.1.2**

v2.1.2 is released on April 2, 2018. 


#### Issues Fixed

Video freeze in DTX + AAC mode.

**v2.1.1**

v2.1.1 is released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

**v2.1.0**

v2.1.0 is released on March 7, 2018.

#### New features

##### 1. Voice Optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

##### 2. Enhance the audio effect input from the built-in microphone

In an interactive-streaming scenario, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

##### 3. Online statistics query

Adds RESTful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host. For details, see [Online Statistics Query API](../../en/Voice/dashboard_restful_communication.md).

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
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
</tbody>
</table>


#### Issues Fixed

-   Occasional playback noise on specific devices.
-   Occasional crackling voice playback on specific devices.
-   Occasional crashes.


**v2.0.2**

v2.0.2 is released on December 15, 2017, and fixes occasional audio routing issues.


**v2.0**

v2.0 is released on December 6, 2017. 

#### New Features

-   Supports external audio sources in the `COMMUNICATION` and `LIVE_BROADCASTING` profiles by adding the following API methods:

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
<td>Enables the external audio source function.</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>Pushes the external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>

-   Provides a set of RESTful APIs to ban a peer user from the server in the `COMMUNICATION` and `LIVE_BROADCASTING` profiles profiles. Contact [support@agora.io](mailto:support@agora.io) to enable this function, if required.
-   Supports the following Android emulators: NOX, Lightning, and Xiaoyao.

#### Issues Fixed

-   Audio routing and Bluetooth issues.
-   Optimizes the volume balance control.


**v1.14**

v1.14 is released on October 20, 2017. 

#### New Features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch
-   `LIVE_BROADCASTING`: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor

#### Improvements

-   Optimizes the audio at high bitrates.
-   `LIVE_BROADCASTING`: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions).
-   Adds the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.


#### Issues Fixed

Camera related issues on Android devices.

**v1.13.1**

v1.13.1 is released on September 28, 2017, and optimizes the echo issue under certain circumstances.

**v1.13**

v1.13 is released on September 4, 2017. 

#### New Features

-   Adds the function to dynamically enable and disable acquiring the sound card in the live interactive streaming.
-   Adds the function to disable the audio playback.
-   Adds the <code>onClientRoleChanged</code> callback to report to the app on a user role switch between the host and the audience in the live interactive streaming.
-   Supports the push-stream failure callback on the server side.

#### Issues Fixed:

Occasional crashes on some devices.

**v1.12**

v1.12 is released on July 25, 2017.

#### New Features:

-   Adds the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Adds a set of APIs for audio effect management.

#### Issues Fixed:

-   Android: Bluetooth issues related to audio routing.
-   Android/iOS/Mac/Windows: Occasional crashes.
