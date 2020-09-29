
---
title: Release Notes
description: Release notes
platform: React Native
updatedAt: Mon Sep 28 2020 13:16:01 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora React Native SDK.

## Overview

The Agora React Native SDK wraps the Agora RTC SDKs for Android and iOS, so you can quickly implement real-time communication functionality in your Android and iOS apps developed using the React Native framework.

The Agora React Native SDK supports the following scenarios:

- Voice and Video Call
- Live Interactive Audio and Video Streaming

For key features included in each scenario, see the following documentation:

- [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms)
- [Agora Video Call Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms)
- [Agora Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) 
- [Agora Interactive Video Streaming Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)


## v3.1.2 

v3.1.2 was released on September 28, 2020. 

**Compatibility changes**

#### 1. Disabling the local network connection quality report (iOS only)

To improve user experience, this version disables the local network connection quality report by default, to prevent the prompt to find local network devices from popping up when an end user launches the app on an iOS 14.0 device. The `gatewayRtt` parameter in the [`RtcStats`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcstats.html) callback is disabled by default. Do not use [`gatewayRtt`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcstats.html#gatewayrtt) to obtain the round-trip delay between the client and the local router. See the [FAQ](https://docs.agora.io/en/faq/local_network_privacy) for details.

If you don't mind the prompt, and want to enable the `gatewayRtt` parameter, please contact Agora technical support via [support@agora.io](mailto:support@agora.io).

#### 2. Regional connection

This release changes the `AreaCode` for regional connection. The latest area codes are as follows:

- `CN`: Mainland China.
- `NA`: North America.
- `EU`: Europe.
- `AS`: Asia, excluding Mainland China.
- `JP`: Japan.
- `IN`: India.
- `GLOB`: (Default) Global.

If you have specified a region for connection when calling `createWithAreaCode`, ensure that you use the latest area code when migrating from an earlier SDK version.

**New features**

#### 1. Publishing and subscription states

This release adds the following callbacks to report the current publishing and subscribing states:

- `AudioPublishStateChanged`: Reports the change of the audio publishing state.
- `VideoPublishStateChanged`: Reports the change of the video publishing state.
- `AudioSubscribeStateChanged`: Reports the change of the audio subscribing state.
- `VideoSubscribeStateChanged`: Reports the change of the video subscribing state.

#### 2. First local frame published callback

This release adds the `FirstLocalAudioFramePublished` and `FirstLocalVideoFramePublished` callbacks to report that the first audio or video frame is published. The `FirstLocalAudioFrame` callback is deprecated from v3.1.2.

**Improvement**

#### 1. CDN live streaming

This release adds the `RtmpStreamingEvent` callback to report events during CDN live streaming, such as failure to add a background image or watermark image.

#### 2. Encryption

This release adds the `enableEncryption` method for enabling built-in encryption, and deprecates the following methods:

- `setEncryptionSecret`
- `setEncryptionMode`

#### 3. More in-call statistics

This release adds the following attributes to provide more in-call statistics:

- Adds `txPacketLossRate` in `LocalAudioStats`, which represents the audio packet loss rate (%) from the local client to the Agora edge server before applying anti-packet loss strategies.

- Adds the following attributes in `LocalVideoStats`:
  - `txPacketLossRate`: The video packet loss rate (%) from the local client to the Agora edge server before applying anti-packet loss strategies.
  - `captureFrameRate`: The capture frame rate (fps) of the local video.
- Adds `publishDuration` in `RemoteAudioStats` and `RemoteVideoStats`, which represents the total publish duration (ms) of the remote media stream.

#### 4. Audio profile

To improve audio performance, this release adjusts the maximum audio bitrate of each audio profile as follows:

| Profile                  | v3.1.2                                                       | Earlier than v3.1.2                                          |
| :----------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `Default`                | For the interactive streaming profile: 64 Kbps</br>For the communication profile: 18 Kbps | For the interactive streaming profile: 52 Kbps</br>For the communication profile: 18 Kbps |
| `SpeechStandard`         | 18 Kbps                                                      | 18 Kbps                                                      |
| `MusicStandard`          | 64 Kbps                                                      | 48 Kbps                                                      |
| `MusicStandardStereo`    | 80 Kbps                                                      | 56 Kbps                                                      |
| `MusicHighQuality`       | 96 Kbps                                                      | 128 Kbps                                                     |
| `MusicHighQualityStereo` | 128 Kbps                                                     | 192 Kbps                                                     |

#### 5. Log files

This release increases the default number of log files that the Agora SDK outputs from 2 to 5, and increases the default size of each log file from 512 KB to 1024 KB. By default, the SDK outputs five log files, `agorasdk.log`, `agorasdk_1.log`, `agorasdk_2.log`, `agorasdk_3.log`, `agorasdk_4.log`. The SDK writes the latest logs in `agorasdk.log`. When `agorasdk.log` is full, the SDK deletes the log file with the earliest modification time among the other four, renames `agorasdk.log` to the name of the deleted log file, and creates a new `agorasdk.log` to record the latest logs.

#### 6. Underlying implementation of TextureView

This release changes the underlying implementation of the `TextureView` object, from instantiating a `TextureView` object to creating a `TextureView` object with the `CreateTextureView` method.

#### 7. Others

- Reduces the audio delay.
- Reduces the playback time of the first remote audio frame.
- This release improves the performance of some iOS models (using Apple A10 and below chips), reducing CPU utilization and memory footprint.
- In-ear monitoring improvement on OPPO models (Android):
  - Reno4 Pro 5G
  - Reno4 5G

**Issues fixed**

This release fixed the following issues:

- The `FirstLocalVideoFrame` and `FirstRemoteVideoFrame` callbacks are not triggered at the right time.
- `setAudioMixingPitch` did not work when setting the `pitch` parameter to certain values.
- The image enhancement feature does not work.
- For Android:
  - Occasional video freeze.
  - Crashes on x86 phones running on Android 6 and later.
- For iOS:
  - When a remote user leaves the channel, the view of the remote user becomes black.
  - Occasional audio freeze when disconnecting from a Bluetooth device.
  - Occasional crashes when the user left the channel after talking in multiple channels.

**API changes**

#### Added


- [`AudioPublishStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#audiopublishstatechanged)
- [`VideoPublishStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#videopublishstatechanged)
- [`AudioSubscribeStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#audiosubscribestatechanged)
- [`VideoSubscribeStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#videosubscribestatechanged)
- [`FirstLocalAudioFramePublished`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#firstlocalaudioframepublished)
- [`FirstLocalVideoFramePublished`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#firstlocalvideoframepublished)
- [`enableEncryption`](https://docs.agora.io/en/Voice/API%20Reference/react_native/classes/rtcengine.html#enableencryption)
- [`txPacketLossRate`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/localaudiostats.html#txpacketlossrate) in [`LocalAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/localaudiostats.html)  class
- [`txPacketLossRate`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/localvideostats.html#txpacketlossrate)  and  [`captureFrameRate`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/localvideostats.html#captureframerate) in [`LocalVideoStats`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/localvideostats.html) class
- `publishDuration` in [`RemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/remoteaudiostats.html) and  [`RemoteVideoStats` ](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/remotevideostats.html) class
- [`RtmpStreamingEvent`](https://docs.agora.io/en/Voice/API%20Reference/react_native/interfaces/rtcengineevents.html#rtmpstreamingevent)
- Error code: [`NoServerResources(103)`](https://docs.agora.io/en/Voice/API%20Reference/react_native/enums/errorcode.html#noserverresources)
- Warning code:
  - [`AdmCategoryNotPlayAndRecord(1029)`](https://docs.agora.io/en/Voice/API%20Reference/react_native/enums/warningcode.html#admcategorynotplayandrecord)
  - [`AdmNoDataReadyCallback(1040)`](https://docs.agora.io/en/Voice/API%20Reference/react_native/enums/warningcode.html#admnodatareadycallback)
  - [`AdmInconsistentDevices(1042)`](https://docs.agora.io/en/Voice/API%20Reference/react_native/enums/warningcode.html#adminconsistentdevices)
  - [`ApmResidualEcho(1053)`](https://docs.agora.io/en/Voice/API%20Reference/react_native/enums/warningcode.html#apmresidualecho)

#### Deprecated

- `setEncryptionSecret`
- `setEncryptionMode`
- `FirstLocalAudioFrame`

#### Deleted

- Warning code: `AdmImproperSettings(1053)`

## v3.0.1

v3.0.1 was released on September 4th, 2020.

**Functions and features**

- Adapts to Agora RTC SDK v3.0.1.
- Supports asynchronous programming with Promises.
- Supports multiple channels.
- Supports TextureView on Android platform.

**Related documentation**

See the following documentation to quickly integrate the SDK and implement real-time voice and video communication in your project.

- [Start a Voice Call](../../en/Voice/start_call_audio_react_native.md)
- [Start a Video Call](../../en/Voice/start_call_react_native.md)
- [Start Live Interactive Video Streaming](../../en/Voice/start_live_react_native.md)
- [Start Live Interactive Audio Streaming](../../en/Voice/start_live_audio_react_native.md)
- [API Reference](https://docs.agora.io/en/Voice/API%20Reference/react_native/index.html)

Agora also provides an open-source [Agora React Native Quickstart](https://github.com/AgoraIO-Community/Agora-RN-Quickstart) on GitHub.
