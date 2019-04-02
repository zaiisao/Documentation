
---
title: Release Notes
description: 
platform: macOS
updatedAt: Tue Apr 02 2019 09:49:53 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for macOS.

## Overview

The Video SDK supports the following scenarios:

- Voice/Video Communication
- Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) and [Video Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

#### Known Issues and Limitations

A USB device driver issue occurs when you do not hear any audio or the audio is corrupted with a USB headset. USB is not user-friendly on macOS, and we recommend using higher quality headsets.

## v2.4.0

v2.4.0 is released on April 1, 2019.

### Before Getting started

If you integrate the SDK by using CocoaPods，ensure that you run `pod update` in your Terminal before `pod install`. If you prefer to specify the SDK version, ensure that you specify it in the format of `AgoraRtcEngine_macOS 2.4.0.1` in the Podfile.

### New Features

#### 1. Advanced screen sharing

v2.4.0 upgrades screen sharing and provides the following advanced functions:

- Shares the whole or part of a specified screen in a multi-display environment ([`startScreenCaptureByDisplayId`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)).
- Shares the whole or part of a specified window ([`startScreenCaptureByWindowId`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)).
- Sets the content hint of the screen sharing to prioritize motion or detail ([`setScreenCaptureContentHint`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)).
- Sets the dimensions, frame rate and bitrate for screen sharing ([`updateScreenCaptureParameters`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)).

v2.4.0 deprecates the `startScreenCapture` method. We recommend using the new methods for screen sharing. With the new methods, developers need to design the code logic to obtain the `displayId` and `windowId`. For more information, see [Share the Screen](../../en/Video/screensharing_mac.md).

#### 2. Voice changer and voice reverberation

Adding voice changer and reverberation effects in an audio chat room brings much more fun. v2.4.0 adds the [`setLocalVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:) and [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:) methods, allowing you to change your voice or reverberation by choosing from the preset options. See Adjust the pitch and tone.

#### 3. Tracking the sound position of a remote user 

v2.4.0 adds the [`enableSoundPositionIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) and [`setRemoteVoicePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) methods. Call the [`enableSoundPositionIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) method before joining a channel to enable stereo panning for the remote users, and then you can call the [`setRemoteVoicePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) method to track the position of a remote user.

#### 4. Pre-call last-mile network probe test

Conducting a last-mile probe test before joining the channel helps the local user to evaluate or predict the uplink network conditions. v2.4.0 adds the [`startLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:), [`stopLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest), and [`lastmileProbeResult`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:) APIs, allowing you to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

#### 5. Audio device loopback test

v2.4.0 adds the [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:) and [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest) methods for testing whether the local audio devices are working properly. The test involves only the local audio devices and does not report the network condition.

#### 6. Setting the priority of a remote user's stream

v2.4.0 adds the [`setRemoteUserPriority`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:) method for setting the priority of a remote user's media stream. You can use this method with the [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) method. If the fallback function is enabled for a remote stream, the SDK ensures the high-priority user gets the best possible stream quality.

#### 7. State of an audio mixing file 

v2.4.0 adds the [`localAudioMixingStateDidChanged`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:) callback to report any change of the audio-mixing file playback state (playback succeeds or fails) and the corresponding reason. This release also adds the warning code 701, which is triggered if the local audio-mixing file does not exist, or if the SDK does not support the file format or cannot access the music file URL when playing the audio-mixing file.

#### 8. Setting the log file size

The SDK has two log files, each with a default size of 512 KB. In case some customers require more than the default size, v2.4.0 adds the [`setLogFileSize`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:) method for setting the log file size (KB).

### Improvements

#### 1. Accuracy of call quality statistics

- v2.4.0 replaces the [`startEchoTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTest:) method with the [`startEchoTestWithInterval`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:) method. The `intervalInSeconds` parameter of `startEchoTestWithIntervals` allows you to set the interval between when you speak and when the recording plays back.
- v2.4.0 adds three parameters to the [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html) class: [`sentTargetBitrate`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetBitrate) for setting the target bitrate of the current encoder, [`sentTargetFrameRate`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetFrameRate) for setting the target frame rate, and [`qualityAdaptIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/qualityAdaptIndication) for reporting the quality of the local video since last count.

#### 2. Video encoder preferences

v2.4.0 provides the following options for setting video encoder preferences:

- Setting preferences under limited bandwidth. v2.4.0 adds two parameters to the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html) class: [`minFrameRate`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minFrameRate) and [`degradationPreference`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/degradationPreference). You can use these parameters together to set the minimum video encoder frame rate and the video encoding degradation preference under limited bandwidth. For more information, see [Set the Video Profile](../../en/Video/videoProfile_mac.md).
- Setting the camera capture preference. v2.4.0 adds the [`setCameraCaptureConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:) method, allowing you to set the camera capture preference. You can choose system performance over video quality or vice versa as needed. For more information, see the [API Reference](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:).

#### 3. Core quality improvements

- Reduces the audio delay.
- Improves the video quality and stability.
- Shortens the time to render the first remote video frame. 
- Improves the video smoothness and reduces the time delay when sharing a screen under poor network conditions. 
- Optimizes the usage of CPU and RAM resources.

### Issues Fixed

#### Audio

- Calling the `enableLocalAudio` method disconnects all connected Bluetooth devices.
- The SDK does not support audio mixing URLs with Chinese characters.
- The SDK does not return YES after the `pushExternalAudioFrameSampleBuffer` method call succeeds.
- Volume levels of the high-pitch sound are lowered.
- Sounds are occasionally played fast.
- The app cannot get the virtual sound card information with the `getAudioPlaybackDevices` method.

#### Video

- If you skip the `renderMode` setting, the video stretches due to a mismatch with the display.
- Video freezes on some lower-end devices.
- It takes too long to render the first received video frame.
- The Electron SDK crashes if the virtual camera does not support 640 x 480.
- The cursor on the local screen is not accurately projected onto the remote screen.

#### Miscellaneous:

- The SEI information does not synchronize with the media stream when publishing transcoded streams to the CDN.

### API Changes

To improve your experience, we made the following changes to the APIs:

#### Added

- [`setBeautyEffectOptions`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:)
- [`startScreenCaptureByDisplayId`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)
- [`startScreenCaptureByWindowId`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)
- [`updateScreenCaptureParameters`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)
- [`setScreenCaptureContentHint`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`enableSoundPositionIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:)
- [`setRemoteVoicePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:)
- [`startLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`setRemoteUserPriority`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:)
- [`startEchoTestWithInterval`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
- [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:)
- [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest)
- [`setCameraCaptureConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:)
- [`setLogFileSize`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)
- [`localAudioMixingStateDidChanged`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)
- [`lastmileProbeResult`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

#### Deprecated

- `startEchoTest`
- `startScreenCapture`
- `setVideoQualityParameters`

#### Miscellaneous

v2.4.0 changes the type of the [`frameRate`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/frameRate) parameter in the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html) class from `enum` to `int`.

## v2.3.3

v2.3.3 is released on January 24, 2019. 

### Improvements

v2.3.3 optimizes the screen-sharing algorithm for different scenarios. The video smoothness and quality are enhanced when a user presents slides or browses websites. v2.3.3 also improves the initial image quality in the Communication profile.

### Issues Fixed

Occasional inaccurate statistics returned in the `networkQuality` callback.

## v2.3.2

v2.3.2 is released on January 16, 2019. 

### Before Getting Started

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the Live Broadcast profile.

Before upgrading your SDK, ensure that the version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

### New Features

#### 1. Video quality in a live broadcast

v2.3.2 adds the [minBitrate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minBitrate) parameter (minimum encoding bitrate) in the [setVideoEncoderConfiguration](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

#### 2. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. See [Adjust the Volume](../../en/Video/volume_mac.md) for the scenarios and corresponding APIs.

#### 3. Fallback options for a live broadcast under unreliable network conditions

Unreliable network conditions affect the overall quality of a live broadcast. v2.3.2 adds the [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:) and [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) methods to allow the SDK to:

- Automatically disable the video stream when the network conditions cannot support both audio and video, or
- Enable the video when the network conditions improve. 

The SDK triggers the [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:) or [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:) callback when the stream falls back to audio-only or switches back to the video.

#### 4. Upstream and downstream statistics of each remote user/host

v2.3.2 adds the [`audioTransportStatsOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:) and [`videoTransportStatsOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:) callbacks to provide the upstream and downstream statistics of each remote user/host. During a call or live broadcast, the SDK triggers these callbacks once every two seconds after the local user receives audio/video packets from a remote user. The callbacks return the user ID, received audio/video bitrate, packet loss rate, and network time delay (ms).

#### 5. New video encoder configuration

To support video rotation scenarios and improve the quality of the custom video source, v2.3.2 deprecates the [`setVideoProfile`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoProfile:swapWidthAndHeight:) method and replaces it with the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) method to set the video encoder configurations. The [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. You can still use the `setVideoProfile` method, but we recommend using the `setVideoEncoderConfiguration` method to set the video profile.

#### 6. Virtual sound card

v2.3.2 adds the `deviceName` parameter in the [enableLoopbackRecording](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLoopbackRecording:deviceName:) method, allowing you to use a virtual sound card for audio recording:

- To use the current sound card, set `deviceName` as NULL.
- To use a virtual card, set `deviceName` as the name of the virtual card.

### Improvements

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `audioQualityOfUid` callback and replaces it with the `remoteAudioStats` callback to improve the accuracy of the call quality statistics. The `remoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real-user experience. In addition, v2.3.2 optimizes the algorithm of the `networkQuality` callback for the uplink and downlink network qualities.

- [`remoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`networkQuality`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`lastmileQuality`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Video/in_call_statistics_macos.md).

#### 2. New network connection policy

v2.3.2 adds the following API method and callback to get the current network connection state and the reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState): Gets the connection state of the SDK.
- [`connectionChangedToState`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:) and [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `connectionChangedToState` callback when the network connection state changes. The SDK also triggers the `rtcEngineConnectionDidInterrupted` and `rtcEngineConnectionDidBanned` callbacks under certain circumstances, but we do not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. You can use this feedback for future product improvement. We strongly recommend integrating this method in your application.

#### 4. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.
- Optimizes video capture methods on macOS and reduces performance loss.

### Issues Fixed

The following issues are fixed in v2.3.2:

#### SDK

- Crashes on macOS.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 

#### Video

- The users on the Web client cannot see the video sent from the Native client due to codec bugs.
- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

### API Changes

To improve your experience, we made the following changes to the APIs:

#### Added

- [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)
- [`getConnectionState`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`connectionChangedToState`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)
- [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:)
- [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:)
- [`remoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)
- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:)
- [`videoTransportStatsOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:)

#### Deprecated

- [`setVideoProfile`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoProfile:swapWidthAndHeight:)
- [`audioQualityOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioQualityOfUid:quality:delay:lost:)
- [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:)
- [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:)

## v2.2.3

v2.2.3 is released on July 5, 2018. 

> The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Issues Fixed

- Occasional online statistics crashes.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Occasional video freeze after a view size change.
- Failing to report the uid and volume of the speaker in a channel.

## v2.2.2

v2.2.2 is released on June 21, 2018.

### Issues Fixed

- Fixed occasional online statistics crashes.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of occasional video freeze after a view size change.

## v2.2.1

v2.2.1 is released on May 30th, 2018 and improves the internal code implementation.

## v2.2.0

v2.2.0 is released on May 4, 2018. 

### New Features

#### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method to enable the remote user in the channel to hear the audio effect played locally. 

> If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

We provide a proxy package for enterprise users with corporate firewalls to deploy before accessing our services. 

#### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Adds the watermark function for users to add a PNG file to the local or CDN broadcast as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast. Adds the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface to add watermarks in CDN broadcasts. 

### Improvements

#### 1. Audio volume indication

Improves the <code>enableAudioVolumeIndication</code> method. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method improves its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, the <code>onLastmileQuality</code> callback changes its detection from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK still triggers this callback once every 2 seconds. 

#### 4. Audio Quality Enhancement

Improves the audio quality in scenarios that involve music playback.

### Issues Fixed

- Occasional crashes on the macOS device.
- Occasional screen display abnormalities when a large number of audience members join as the host in a live-broadcast channel.

## v2.1.3

v2.1.3 is released on April 19, 2018. 

In v2.1.3, we updated the bitrate values of the <code>setVideoProfile</code> method in the Live-broadcast profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

- Block callbacks are occasionally not received if the delegate is not set.
- NSAssertionHandler appears in external links to the SDK.
- Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.

### Improvements

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to the normal communication or live-broadcast mode.

## v2.1.2

v2.1.2 is released on April 2, 2018. 

> If you upgraded the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth. 

### New Features

Extends the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

### Issues Fixed

The video resolution of the shared screen is worse in the Communication profile than in Live-broadcast profile.

## v2.1.1

v2.1.1 is released on March 16, 2018. 

We identified a critical bug in SDK v2.1. Upgrade to v2.1.1 if you are using the Agora SDK v2.1.

## v2.1.0

V2.1.0 is released on March 7, 2018. 

### New Features

#### 1. Voice optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhances the audio effect input from the built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Adds Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

- Voice or video calls: See [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
- Interactive broadcasts: See [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).

#### 4. 17-way Video

Adds the support of 17-way video in interactive broadcasts, see:

- [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_mac.md)
- [Video Conference of 7+ Users](../../en/Video/seventeen_people_iosmac.md)

#### 5. Video source customization

Supports the default video-capturing features provided by the camera and the customized video source. 

#### 6. Renderer customization

Supports the default functions provided by the renderers to display the local and remote videos to meet your requirements. We provide a set of interfaces for customized renderers. 

#### 7. Screen sharing for interactive broadcast

- Before v2.1.0: The Agora SDK only supported the screen-sharing function in video calls
- From v2.1.0: The Agora SDK added the screen-sharing function in interactive broadcasts.

### Improvements

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
<tr><td>Billing Optimization</td>
<td>Small video resolutions are charged according to voice-only mode. For example, 16 x 16.</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modifies a set of names for the API attributes and enumeration values.</td>
</tr>
</tbody>
</table>



## v2.0.2

v2.0.2 is released on December 15, 2017 and fixes the FFmpeg symbol conflict.

## v2.0 and Earlier

### v2.0

v2.0 is released on December 6, 2017. 

#### New Features

- Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the Communication profile to support dual streams.

- Updates the following callbacks for audio mixing and sound effects:

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
  <tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
  <td>Removed. Replaced by rtcEngineLocalAudioMixingDidFinish.</td>
  </tr>
  <tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
  <td>Added. Notifies the app when the local audio effect playback stops.</td>
  </tr>
  <tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
  <td>Added. Notifies the app when the remote user starts audio mixing.</td>
  </tr>
  <tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
  <td>Added. Notifies the app when the remote user stops audio mixing.</td>
  </tr>
  </tbody>
  </table>

- Adds the camera management function in the Communication and Live-broadcast profiles by adding the following API methods:

  <table>
  <colgroup>
  <col/>
  <col/>
  </colgroup>
  <tbhead>
  <tr><th>Name</th>
  <th>Description</th>
  </tr>
  </thead>
  <tbody>
  <tr><td><code>isCameraZoomSupported</code></td>
  <td>Checks whether the device supports camera zoom.</td>
  </tr>
  <tr><td><code>isCameraTorchSupported</code></td>
  <td>Checks whether the device supports camera flash.</td>
  </tr>
  <tr><td><code>isCameraFocusPositionInReviewSupported</code></td>
  <td>Checks whether the device supports camera manual focus.</td>
  </tr>
  <tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
  <td>Checks whether the device supports camera auto-face focus.</td>
  </tr>
  <tr><td><code>setCameraZoomFactor</code></td>
  <td>Sets the camera zoom ratio.</td>
  </tr>
  <tr><td><code>setCameraFocusPositionInPreview</code></td>
  <td>Sets the position for manual focus and activates focusing.</td>
  </tr>
  <tr><td><code>setCameraTorchOn</code></td>
  <td>Sets the device to turn on the camera flash.</td>
  </tr>
  <tr><td><code>setCameraAutoFocusFaceModeEnabled</code></td>
  <td>Sets the device to start auto-face focusing.</td>
  </tr>
  </tbody>
  </table>



- Supports the external audio source in the Communication and Live-broadcast profiles by adding the following API methods:

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
  <tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
  <td>Enables the external audio source.</td>
  </tr>
  <tr><td><code>disableExternalAudioSource</code></td>
  <td>Disables the external audio source.</td>
  </tr>
  <tr><td><code>pushExternalAudioFrameRawData</code></td>
  <td>Pushes the external audio frame to the Agora SDK.</td>
  </tr>
  </tbody>
  </table>



- Provides a set of RESTful APIs to ban a peer user from the server in the Communication and Live-broadcast profiles. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function, if required.

#### Issues Fixed

Audio routing and Bluetooth issues.

### v1.14

v1.14 is released on October 20, 2017. 

#### New Features

- Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
- Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.
- Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.

#### Improvements

- Optimizes the audio at high bitrates.
- Live Broadcast: The audience can view the host within one second in a single-stream mode (858 ms on average, and 625 ms in good network conditions).
- Adds the ability to reduce the bandwidth.
  - Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
  - Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
- Accurate control over the bitrate:
  - Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
  - Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.

### v1.13.1

v1.13.1 is released on September 28, 2017 and optimizes the echo issue under certain circumstances.

### v1.13

v1.13 is released on September 4, 2017. 

#### New Features

- Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
- Adds the function to disable the audio playback.
- Adds the module map for the SDK, which means bridging header files are not necessary for Swift projects.
- Supports the profile configuration for stream-pushing on the client side.
- Adds the <code>didClientRoleChanged</code> callback to indicate a user role change between the host and audience in a live broadcast.
- Supports the push-stream failure callback on the server side.

#### Improvements:

- Screen Sharing: Enhances the video definition and fluency.
- Screen Sharing: Supports updating the captured region dynamically.
- The video profile is controllable by the software codec.

#### Issues Fixed:

Occasional crashes on some devices.

### v1.12

v1.12  is released on July 25, 2017. 

#### New Functions:

- Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in the Live-broadcast profile.
- Adds the <code>aes-128-ecb</code> encryption mode in the `setEncryptionMode` method.
- Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
- Adds a set of API methods to manage the audio effect.
- Adds the <code>ActiveSpeaker</code> method to report on the active speaker in the current channel.
- Removes the <code>setScreenCaptureWindow</code> method, and updates the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the Communication profile.
- Adds displaying the mouse function when the screen-sharing function is enabled in the Communication profile.

#### Improvements:

In the Communication profile, the 320 &times; 180 resolution profile is improved.

- Keeps the video smooth under poor network and equipment conditions.
- Enhances the image quality to be better than 180p under good network and equipment conditions.

#### Issues Fixed:

Occasional crashes.
