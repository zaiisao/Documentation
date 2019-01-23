
---
title: Release Notes
description: 
platform: iOS
updatedAt: Wed Jan 23 2019 09:12:05 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for iOS.

## Overview

The Video SDK supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms) and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

## v2.3.3
The version 2.3.3 was released on Jan. 23rd, 2019. See below for issues fixed.

### Issues Fixed

Occasional inaccurate statistics returned in the `networkQuality` callback.

## v2.3.2

The version 2.3.2 was released on Jan. 16th, 2019. See below for new features, improvements, and issues fixed.

### Before Getting Started

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the Live Broadcast profile.

Before upgrading your SDK, ensure that the version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

### New Features

#### 1. Automatic exposure at a point of interest
v2.3.2 adds the following methods and callback to support camera exposure and improve the captured video quality:

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported): Checks whether the device supports camera exposure.
- [`setCameraExposurePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:): Sets the camera exposure position.
- [`cameraExposureDidChangedToRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:): Occurs when the camera exposure area changes.
You can send the touch point coordinates in the view to the SDK for automatic exposure.

#### 2. Video quality in a live broadcast

v2.3.2 adds the [`minBitrate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minBitrate) parameter (minimum encoding bitrate) in the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

#### 3. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. See [Adjust the Volume](../../en/Interactive%20Broadcast/volume_ios.md) for the scenarios and corresponding APIs.

### Improvements

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `audioQualityOfUid` callback and replaces it with the `remoteAudioStats` callback to improve the accuracy of the call quality statistics. The `remoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `networkQuality` callback for the uplink and downlink network qualities.

- [`remoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`networkQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`lastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see Report [In-call Statistics](../../en/Interactive%20Broadcast/in_call_statistics_ios.md).

#### 2. New network connection policy

v2.3.2 adds the following API method and callback to get the current network connection state and reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState): Gets the connection state of the SDK.
- [`connectionChangedToState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:) and [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `connectionChangedToState` callback when the network connection state changes. The SDK also triggers the `rtcEngineConnectionDidInterrupted` and `rtcEngineConnectionDidBanned` callbacks under certain circumstances, but Agora does not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. Application developers can use this feedback for future product improvement. Agora strongly recommends integrating this method in your application.

#### 4. Improves the audio quality in music scenarios

v2.3.2 adds high-quality audio in scenarios such as music education. In the [`setAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) method, you can set [`AgoraAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Constants/AgoraAudioProfile.html) as MusicHighQuality(4) and [`AgoraAudioScenario`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Constants/AgoraAudioScenario.html) as GameStreaming(3) to minimize echo and noise while maintaining the quality of the music.

#### 5. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Improves the stability in pushing streams.
- Optimizes the API calling threads.
- Improves the performance of the SDK on mid-end and low-end iOS devices.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.

### Issues Fixed

The following issues are fixed in v2.3.2.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset. 
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 

#### Video

- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

#### Added

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported)
- [`setCameraExposurePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:)
- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`connectionChangedToState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)
- [`CameraExposureDidChangedToRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:)
- [`remoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)

#### Deprecated

- [`audioQualityOfUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioQualityOfUid:quality:delay:lost:)
- [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:)
- [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:)

## v2.3.1

The version 2.3.1 was released on September 28, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio streams after joining a channel, this release adds the `enableLocalAudio` method to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `didMicrophoneEnabled` callback function, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and `muteLocalAudioStream` is that `enableLocalAudio`does not capture or send any audio stream, while `muteLocalAudioStream` captures but does not send audio streams.


### Improvements

- Reduced the CPU consumption in the audio communication profile on some low-end iOS devices.


### Issues Fixed

- Occasional crashes on some iOS devices when using customized video source functions.
- Occasional crashes on some iOS devices when using customized remote renderer functions.
- Occasional crashes when switching between the front and rear camera.
- Occasionally in the live broadcast profile, the app freezes and the user cannot leave the channel after switching between the front and rear cameras on some iOS devices.
- Occasionally in the live broadcast profile, two taps are needed for a successful focus on some iOS devices.
- In the communication profile, if one user disables the video and re-enables it during a one-to-one call, it takes a long time for the other user to see the image.
- In the live broadcast profile, delay at the client due to incorrect statistics.
- The timestamp in the captured raw video frames does not refresh with the frame.

## v2.3.0

The version 2.3.0 was released on August 31, 2018. See below for new features, improvements, issues fixed and API Changes.

### Before Reading

-   To support video rotation and enable better quality for the custom video source, this release deprecates the `setVideoProfile` method and uses `setVideoEncoderConfiguration` instead to set the video encoding configurations. You can still use `setVideoProfile` but Agora recommends using `setVideoEncoderConfiguration` to set the video profile because:
    -   During a live broadcast, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.
    -   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.
-   An `Accelerate.framework` library was added to the SDK in v2.3.0, which is capable of large-scale mathematical computations and image calculations, optimized for high performance.
-   The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).


### New Features

#### 1. Fallback options for a live broadcast under unreliable network conditions.

The audio and video quality of a live broadcast will deteriorate under unreliable network conditions. To improve the efficiency of a live broadcast, the `setLocalPublishFallbackOption` and `setRemoteSubscribFallbackOption` methods are added. These methods allow the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and enable the video when the network conditions improve. `didLocalPublishFallbackToAudioOnly` or `didRemoteSubscribeFallbackToAudioOnly` is triggered when the stream falls back to audio-only or when the stream switches back to the video.

#### 2. Notifies the user that the Token will expire in 30 seconds.

The SDK returns the `tokenPrivilegeWillExpire` callback function 30 seconds before a Token expires to notify the app to renew it. When this callback function is received, you need to generate a new Token on your server and call `renewToken` to pass the newly-generated Token to the SDK.

#### 3. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate and time delay

The `audioTransportStatsOfUid` and `videoTransportStatsOfUid` callback functions are added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or a live broadcast, these callback functions are triggered once every two seconds after the user receives audio/video packets from a remote user. The callbacks include the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

#### 4. Sets the SDK’s control over an audio session

The SDK and app both have control over the audio session. However, the app may restrict the SDK’s control over an audio session and allow another app or a third-party component to control it by using the `setAudioSessionOperationRestriction` method. You can implement different levels of control by choosing the corresponding restriction. You can call this method before or after joining the channel.

#### 5. Sets the video encoder configurations

To support scenarios with video rotation and enable better quality for the custom video source, this release deprecates the `setVideoProfile` method and uses `setVideoEncoderConfiguration` instead to set the video encoding configurations. You can still use `setVideoProfile` but Agora recommends using `setVideoEncoderConfiguration` to set the video profile because:

-   During a live broadcast, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.
-   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.

The <code>AgoraVideoEncoderConfiguration</code> class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. For more information on the API, see the description of `Set the Video Encoder Configuration`.

#### 6. Adds support for background image settings in setLiveTranscoding

The backgroundImage parameter is added to the `setLiveTranscoding` method allowing you to set the background image in the combined video of a live broadcast.

### Improvements

-   Improved the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
-   Improved the audio encoder efficiency in a live broadcast to reduce user traffic while ensuring the call quality.
-   Improved the audio quality during a call or a live broadcast using the deep-learning algorithm.


### Issues Fixed

- Increased the memory usage when multiple delegated hosts broadcasted in the channel.
- Occasional video renderer crashes when the app switches to the background on some iOS devices.
- Occasional app crashes on some iOS devices.
- The remote view does not display on some devices.
- Occasional black screens on some iOS devices.
- Occasional ghost images.
- Occasional green lines at the bottom of the video when a user switches from a low stream to a high stream in the communication mode.
- Crashes after publishing streams from some iOS devices.
- Occasional crashes on some iOS devices.
- Crashes when a user frequently mutes and resumes all sound effects on some iOS devices.
- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasionally, the remote user cannot hear the host when the host switches between the AUDIENCE and  BROADCASTER.
- Occasionally, the settings applied to the background image of a live transcoding do not take effect.
- Occasionally on some devices, the video height and width are swapped in the communication mode.
- Occasionally, the destroy method does not respond after a user enables the video and joins a channel.
- Occasionally on some iOS devices, a user fails to hear any sound after returning to the channel from a system phone call.
- Occasionally, the audience cannot adjust the channel volume.
- Occasional crashes when a user frequently joins and leaves the channel.
- Occasional crashes when frequently setting the video encoder profile on some iOS devices.
- Occasionally on iOS devices, a remote user cannot send videos in a communication channel.
- Occasional failures to capture video of the delegated host when the hosts and the audience frequently change roles.
- Occasional issues on some iOS devices that a Web user can subscribe to the low video stream even when the iOS device does not have the dual-stream mode enabled.
- Occasional crashes when calling the setCameraFocusPositionInPreview method on some devices.
- Occasional crashes when one of the two broadcasters mutes or disables the local audio while playing the background music.
- Occasional crashes on the iOS device when the device interoperates with the Web and when a Web user frequently joins and leaves a channel.
- A user cannot join a communication channel after frequently changing the video encoder profiles.
- Occasional crashes on some devices when preloading the sound effects.
- Video resolution inconsistencies between the encoder and decoder in the live broadcast mode.
- Occasional video freezes in the communication or live broadcast mode.
- Occasional crashes when calling the muteRemoteVideoStream method after joining the channel.
- Occasionally, some iOS devices can still receive the video fallback-specific callbacks even when the video fallback option is not enabled.
- Incorrect video orientation on some iOS devices when setting the video profile of an external video source during a live broadcast.
- On iOS, the bitrates cannot reach the target values when manually setting the video profile.
- Occasional inter-operational failures between an iOS and a macOS device.
- Occasional crashes on some iOS devices when a user leaves the live broadcast channel while playing music using a third-party application.
- Occasional crashes on some iOS devices when leaving the channel.
- On iOS, when a host has injected a stream to a broadcast channel, other hosts can still inject a second stream to the channel.
- Occasional crashes on some iOS devices when the user frequently turns on and off the camera.
- Occasional video freezes when switching from multiple hosts to a single host.
- Occasional inter-operational failures between SIP devices and the SDK.
- Occasional echoes when using a specific audio card.
- Occasional video delays on some iOS devices.
- The video window size jumps from small to big on some iOS devices.
- Image blurs on some iOS devices when the camera vibrates.
- Failure to adjust the volume on some iOS devices.


### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

To avoid adding too many users with the same uid into the CDN publishing channel, the following API methods are added in v2.3.0:

-   `addUser`
-   `removeUser`


The following API methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. 

-   <code>startRecordingService</code>
-   <code>stopRecordingService</code>
-   <code>refreshRecordingServiceStatus</code>


The following deprecated API methods are deleted and no longer supported from v2.3.0:

-   <code>switchView</code>
-   <code>setSpeakerphoneVolume</code>


### Backwards Compatibility Issues

None.

### Known Issues

None.

## v2.2.3

The version 2.2.3 was released on July 5, 2018. See below for issues fixed.

### Read This First

The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

### Issues Fixed

- Occasional online statistics crashes.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Failing to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the broadcaster in a live broadcast.
- Occasional video freeze after a view size change.


## v2.2.1

The version 2.2.1 was released on May 30, 2018. See below for issues fixed.

### Issues Fixed

- Occasional crashes on some iOS devices.
- Occasional memory leak on some iOS devices.
- Occasional app crashes when the app starts audio mixing on some iOS devices.


## v2.2.0

The version 2.2.0 was released on May 4, 2018. See below for new features, improvements and issues fixed.


### New Feastures

#### 1. Play the audio effect in the channel

Added a <code>publish</code> parameter in the <code>playEffect</code> method, to enable the remote user in the channel to hear the audio effect played locally. 
>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

Agora has provided a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. 
#### 3. Get the remote video state

Added the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Added the watermark function which enables users to add a PNG file to the local or CDN broadcast as a watermark. Added the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast; the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface enables watermarks in CDN broadcasts. 

### Improvements

#### 1. Audio volume indication

Improved the function of <code>enableAudioVolumeIndication</code>. This method, once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support audio or video calls before joining the channel, <code>onLastmileQuality</code> has changed its detection base from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, this callback function is still triggered at a time interval of 2 seconds. 

#### 4. Audio quality enhancement

Improved the audio quality in scenarios that involve music playback. To achieve high-fidelity music playback, you can set <code>Scenario：AgoraAudioScenarioGameStreaming = 3</code> in the <code>setAudioProfile</code> method. 

#### 5. Bitcode support

Added support for Bitcode, which enables App optimization and thinning by the App Store. The package size of the Bitcode SDK is 2.5 times that of the normal one.

### Issues Fixed

- Occasional screen display abnormalities when the iOS device sets its video in the landscape mode.
- Occasional screen display abnormalities when a large number of the audience join as the host in the live-broadcast channel.
- Fixed occasional echo issues caused by some iOS device.


## v2.1.3

The version 2.1.3 was released on April 19, 2018. See below for improvements and issues fixed.

In v2.1.3, Agora has updated the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast mode. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

-   Block callbacks were occasionally not received if the delegate was not set.
-   NSAssertionHandler appeared in external links to the SDK.
-   Occasional recording failures on some phones when the user leaves the channel and turns on the built-in recording device.
-   In a live-broadcast mode, the iOS client could not see the video from the Web client in Windows 10 even after iOS called <code>enableWebSdkInteroperability</code>.
-   Occasional resolution changes after UIImagePickerController was used to enable the system camera and switch back to live broadcast mode.
-   Occasional crashes during a communication or live-broadcast session.


### Improvements

Improved the performance of screen sharing by shortening the time interval between which users switch from screen sharing to a normal communication or live-broadcast mode.

## v2.1.2

The version 2.1.2 was released on April 2, 2018. See below for new features and issues fixed.

>  If you upgraded the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth. 

### New Features

Extended the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

### Issues Fixed

-   Occasional crashes on iOS 11.
-   Video freeze in DTX + AAC mode.


## v2.1.1

The version 2.1.1 was released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### 1. Voice Optimization

Added a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Added Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   For voice or video calls, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   For interactive broadcasts, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


#### 4. 17-way video

Added the support of 17-way video in interactive broadcasts, see:

-   [Starting a Live Video Broadcast](../../en/Interactive%20Broadcast/broadcast_video_ios.md)
-   [Video Conference of 7+ Users](../../en/Interactive%20Broadcast/seventeen_people_iosmac.md)


#### 5. Video source customization

Supported the default video-capturing features provided by the camera and the customized video source. 
#### 6. Renderer customization

Supported the default functions provided by the renderers to display the local and remote videos to meet developers’ requirements. Agora provides a set of interfaces for customized renderers. 

#### 7. Injecting an external video stream

Added the function of injecting an external video stream to an ongoing live broadcast, see [Injecting an External Stream to a Live Broadcast](../../en/Quickstart%20Guide/inject_stream_ios.md).

#### 8. Camera focus change

Added a <code>cameraFocusDidChanged</code> callback function to indicate that the camera focus area has changed.

### Improvement

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
<tr><td>Video Freeze Rate</td>
<td>Reduced the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supported a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each Token in the new authentication mechanism includes all the privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions will be billed according to the voice-only mode, for example, 16 &times;  16.</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modified a set of names for the API attributes and enumeration values.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional crashes.
-   Occasional blank screens.
-   Occasionally no voice after turning off the microphone.


## v2.0.2

The version 2.0.2 was released on December 15, 2017. See below for issues fixed.

Fixed the FFmpeg symbol conflict.

## v2.0 and Earlier

### v2.0

The version 2.0 was released on December 6, 2017. See below for new features and issues fixed.

#### New Features

-   Added the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication scenario to support dual stream.
-   Updated the following callback functions for audio mixing and sound effects:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Name</strong></th>
<th>Description</strong></th>
</tr>
<tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
<td>Removed. Replaced by <code>rtcEngineLocalAudioMixingDidFinish<c/ode>.</td>
</tr>
<tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
<td>Added. Notifies that the local audio effect has stopped.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
<td>Added. Notifies that the remote user has started the audio mixing.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
<td>Added. Notifies that the remote user has stopped the audio mixing.</td>
</tr>
</tbody>
</table>



-   Added the camera management function in the communication and live broadcast scenarios by adding the following API methods:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th><strong>Name</strong></th>
<th><strong>Description</strong></th>
</tr>
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
<td>Checks whether the device supports auto-face focus.</td>
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



-   Supported the external audio source in the communication and live broadcast scenarios by adding the following API methods:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Name</strong></th>
<th>Description</strong></th>
</tr>
<tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
<td>Enables the external audio source.</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>Disables the external audio source.</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>Pushes the external audio frame.</td>
</tr>
</tbody>
</table>



-   Provided a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast scenarios. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function if required.


#### Issues Fixed

Audio routing and Bluetooth issues.

### v1.14 

The version 1.14 was released on October 20, 2017. See below for new features, improvements and issues fixed.

#### New Features

-   Added the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Added the <code>setLocalVoicePitch</code> method to set the local voice pitch.
-   Live Broadcast: Added the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimized the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (938 ms on average, and 734 ms under good network conditions).
-   Added the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was under poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed:

Occasional crashes on iOS devices.

### v1.13.1

The version 1.13.1 was released on September 28, 2017. 

-   Fixed the issue of not able to adjust the volume in the speaker mode on iOS 11 with iPhone 7+.
-   Optimized the echo issue under certain circumstances.


### v1.13

The version 1.13 was released on September 4, 2017. See below for new features, improvements, and issues fixed.

#### New Features

-   Added the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Added the function to disable the audio playback.
-   Added the module map for the SDK, which means that bridging header files are not necessary for Swift projects.
-   Supported the profile configuration for stream-pushing on the client side.
-   Added the <code>didClientRoleChanged</code> method to indicate a user role change between the host and audience in a live broadcast.
-   Supported the push-stream failure callback on the server side.


#### Improvements:

The video profile is controllable by the software codec.

#### Issues Fixed:

Occasional crashes.

### v1.12

The version 1.12  was released on July 25, 2017. See below for new features, improvements and issues fixed.

#### New Features:

-   Added the <code>injectStream</code> method to inject an RTMP stream into the current channel in live broadcasts.
-   Added the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Added the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Added a set of APIs to manage the audio effect.


#### Improvements:

In the communication scenario, improved the 320 &times; 180 resolution profile:

-   Keep the video smooth under poor network and equipment conditions.
-   Enhance the image quality better than 180p under good network and equipment conditions.


#### Issues Fixed:

Occasional crashes.




