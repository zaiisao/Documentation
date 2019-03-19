
---
title: Release Notes
description: 
platform: Android
updatedAt: Tue Mar 19 2019 02:55:55 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for Android.

## Overview

The Video SDK supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

#### Known Issues and Limitations

##### Privacy changes
If your app targets Android 9, you should keep the following behavior changes in mind. These updates to device serial and DNS information enhance user privacy.

Build serial number deprecation
In Android 9, `Build.SERIAL` is always set to "UNKNOWN" to protect users' privacy.
If your app needs to access a device's hardware serial number, you should instead request the READ_PHONE_STATE permission, then call `getSerial()`.

DNS privacy
Apps targeting Android 9 should honor the private DNS APIs. In particular, apps should ensure that, if the system resolver is doing DNS-over-TLS, any built-in DNS client either uses encrypted DNS to the same hostname as the system, or is disabled in favor of the system resolver.

For more information about privacy changes, see [Android Privacy Changes](https://developer.android.com/about/versions/pie/android-9.0-changes-28#privacy-changes-p).


## v2.3.3
v2.3.3 is released on January 24, 2019. 

### Issues Fixed

- Occasional inaccurate statistics returned in the `onNetworkQuality` callback.
- Occasional crashes on Huawei P9.

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

#### 1. Automatic exposure at a point of interest

v2.3.2 adds the following methods and callback to support camera exposure and improve the captured video quality: 

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6818c2a98bebeb72e4802b1c585da99b): Checks whether the device supports camera exposure.
- [`setCameraExposurePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0ac20919f60df42635850c53c9cbdefd): Sets the camera exposure position.
- [`onCameraExposureAreaChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ab6bc82a55191e596d5bf5a7c56bdf95e): Occurs when the camera exposure area changes.

You can send the touch point coordinates in the view to the SDK for automatic exposure.

#### 2. Video quality in a live broadcast

v2.3.2 adds the [`minBitrate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1video_1_1_video_encoder_configuration.html#a9cd44566bc19eca4006fda264ea96dc7) parameter (minimum encoding bitrate) in the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af5f4de754e2c1f493096641c5c5c1d8f) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

#### 3. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. See [Adjust the Volume](../../en/Interactive%20Broadcast/volume_android.md) for the scenarios and corresponding APIs.

### Improvements

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `onAudioQuality` callback and replaces it with the `onRemoteAudioStats` callback to improve the accuracy of the call quality statistics. The `onRemoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `onNetworkQuality` callback for the uplink and downlink network qualities.

- [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`onNetworkQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a76be982389183c5fe3f6e4b03eaa3bd4): Reports the last mile network quality of each user in the channel.

We plan to improve the following callback in subsequent versions:

- [`onLastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Interactive%20Broadcast/in_call_statistics_android.md).

#### 2. New network connection policy 

v2.3.2 adds the following API method and callback to get the current network connection state and the reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9): Gets the connection state of the SDK.
- [`onConnectionStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`onConnectionInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88) and [`onConnectionBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `onConnectionStateChanged` callback when the network connection state changes. The SDK also triggers the `onConnectionInterrupted` and `onConnectionBanned` callbacks under certain circumstances, but we do not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. You can use this feedback for future product improvement. We strongly recommend integrating this method in your app.

#### 4. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Improves the stability in pushing streams.
- Improves the performance of the SDK on Android 6.0 or later.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.
- Supports Smartisan camera.

### Issues Fixed

The following issues are fixed in v2.3.2:

#### SDK

- Crashes on emulators, such as Yeshen and mumu. 
- Crashes on Android 6.0+ due to x86 .so relocation.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another app.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- On Huawei Mate 20 X, a remote user cannot hear any voice when the app switches to the background and the user opens another app.
- Echo on Pixel 2.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an app switches back from the background. 

#### Video

- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

### API Changes

To improve your experience, we made the following changes to the APIs:

#### Added:

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6818c2a98bebeb72e4802b1c585da99b)
- [`setCameraExposurePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0ac20919f60df42635850c53c9cbdefd)
- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`onConnectionStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)
- [`onCameraExposureAreaChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ab6bc82a55191e596d5bf5a7c56bdf95e)
- [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54)

#### Deprecated

- [`onAudioQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abeac442a777e103536dcf9c8ce2d7135)
- [`onConnectionInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88)
- [`onConnectionBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db)


## v2.3.1

v2.3.1 is released on October 12, 2018. 

### New features

#### Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this version adds the `enableLocalAudio` method is to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `onMicrophoneEnabled` callback, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and the `muteLocalAudioStream` method is that the `enableLocalAudio` method does not capture or send any audio stream, while the `muteLocalAudioStream` method captures but does not send audio streams.


### Improvements

- Improves the SDK's adaptiveness to the Android video encoder.

### Issues Fixed

- Occasional crashes when switching between front and rear cameras.
- Occasionally, failing to render the video on some Android devices.
- Occasional image freezes on some Android devices.
- Occasionally on some Android devices, a user hears a popping sound if the user leaves the channel at the same time another user is speaking.
- Communication profile: If a user disables the video and re-enables it during a one-to-one call, it takes a long time for the other user to see the image.
- Live-broadcast profile: Delay at the client due to incorrect statistics.
- Live-broadcast profile: Occasional crashes on some Android devices after a user repeats the process of switching roles between BROADCASTER and AUDIENCE.
- The timestamp in the captured raw video frames does not refresh with the frame.

## v2.3.0

v2.3.0 is released on August 31, 2018.

### Before Reading

-   To support video rotation and enable better quality for the custom video source, this version deprecates the `setVideoProfile` method and uses the `setVideoEncoderConfiguration` method instead to set the video encoding configurations. You can still use the `setVideoProfile` method, but Agora recommends using the `setVideoEncoderConfiguration` method to set the video profile because:
    -   During a live broadcast, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.
    -   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.
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


### New Features

#### 1. Fallback options for a live broadcast under unreliable network conditions

The audio and video quality of a live broadcast deteriorate under unreliable network conditions. To improve the efficiency of a live broadcast, the `setLocalPublishFallbackOption` and `setRemoteSubscribeFallbackOption` methods are added. These interfaces allow the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and enable the video when the network conditions improve. The SDK triggers the `onLocalPublishFallbackToAudioOnly` or `onRemoteSubscribeFallbackToAudioOnly` callback when the stream falls back to audio-only or when the stream switches back to the video.

#### 2. Notifies the user that the token expires in 30 seconds

The SDK returns the `onTokenPrivilegeWillExpire` callback 30 seconds before a token expires to notify the app to renew it. When this callback is received, you need to generate a new token on your server and call the `renewToken` method to pass the newly-generated token to the SDK.

#### 3. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate, and time delay

The `onRemoteAudioTransportStats` and `onRemoteVideoTransportStats` callbacks are added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or a live broadcast, the SDK triggers these callbacks once every two seconds after the user receives audio/video packets from a remote user. The callbacks include the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

#### 4. Sets the video encoder configurations

To support scenarios with video rotation and enable better quality for the custom video source, this version deprecates the `setVideoProfile` method and uses the `setVideoEncoderConfiguration` method instead to set the video encoding configurations. You can still use the `setVideoProfile` method, but Agora recommends using the `setVideoEncoderConfiguration` method to set the video profile because:

-   During a live broadcast, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.

-   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.


The <code>VideoEncoderConfiguration</code> class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. For more information on the API, see `Set the Video Encoder Configuration`.

#### 6. Adds support for background image settings in setLiveTranscoding

The `backgroundImage` parameter is added to the `setLiveTranscoding` method allowing you to set the background image in the combined video of a live broadcast.

### Improvements

-   Improves the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
-   Improves the audio encoder efficiency in a live broadcast to reduce user traffic while ensuring the call quality.
-   Improves the audio quality during a call or a live broadcast using the deep-learning algorithm.


### Issues Fixed

- The users on the Web client cannot see the video sent from the Native client due to codec bugs.
- Excessive increase in memory usage when multiple delegated hosts broadcast in the channel.
- Occasional crashes on some Android devices.
- The remote view does not display on some devices.
- The local video cannot be enabled on some Android devices.
- Occasional ghost images.
- Occasional green lines at the bottom of the video when a user switches from a low stream to a high stream video in the Communication profile.
- Occasional crashes after interoperating with devices of other platforms for some Android devices.
- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasional black screens on some Android devices.
- Occasionally, the remote user cannot hear the host when the host switches between AUDIENCE and BROADCASTER.
- Occasionally, the settings applied to the background image in live transcoding do not take effect.
- Occasionally on some devices, the video height and width are swapped in the Communication profile.
- Occasionally, the `destroy` method does not respond after a user enables the video and joins a channel.
- Occasional crashes on Android devices when remote users frequently join and leave the channel.
- Black screen due to failure to render the remote video on some Android devices.
- Occasionally, the audience cannot adjust the channel volume.
- Occasionally, apps do not respond on some Android devices.
- Occasional crashes on some Android devices when switching video resolutions in a live broadcast.
- A delegated host cannot see the video of the other hosts in the channel on some Android devices.
- The bitrates cannot reach the target values on some Android devices when a user frequently joins and leaves the communication channel with different video profiles.
- Occasional failures to capture the video of the delegated host when the hosts and the audience members frequently change roles.
- Occasional failures to capture video or publish streams on some Android devices when a user frequently joins and leaves a communication channel with different video profiles.
- Occasional crashes when calling the <code>setCameraFocusPositionInPreview</code> method on some devices.
- Occasional failures to enable the camera during communication on some Android devices.
- Occasional video freezes and stream publishing hangs on some Android devices after a user joins a communication or live broadcast channel.
- Occasional crashes when one of the two broadcasters mutes or disables the local audio while playing the background music.
- A user cannot join a communication channel after frequently changing the video encoder profiles.
- Occasional crashes on some devices when preloading the sound effects.
- Failure to render videos of lower resolutions on some Android devices.
- Occasionally, an Android client still interoperates in a communication channel when removed from Dashboard.
- Video resolution inconsistencies between the encoder and the decoder in the Live-broadcast profile.
- Failure to enable the hardware encoder on some Android devices.
- Occasional video freezes in the Communication or Live-broadcast profile.
- Occasional crashes when calling the <code>muteRemoteVideoStream</code> method after joining the channel.
- Occasional video freezes on some Android devices when switching from the Communication profile to the Live-broadcast profile.
- Occasional crashes on some Android devices when frequently turning on and off the camera flash during a live broadcast.
- The host cannot receive the audio/video stream of the delegated host on some Android devices.
- Occasional crashes on some Android devices when setting the video encoder profile of an external video source during a live broadcast.
- Incorrect video orientation on some Android devices when setting the video profile of an external video source during a live broadcast.
- Occasionally on some Android devices, the video fallback option does not take effect under poor network conditions.
- Occasional crashes on some Android devices when a user frequently changes the token.
- Occasional failures to split the screen on some Android devices.
- The CDN audience on some Android devices cannot see the video of the host.
- Occasional video freezes when switching from multiple hosts to a single host.
- Occasional inter-operational failures between SIP devices and the SDK.
- Occasionally on Mi 8, the local video cannot be seen locally or remotely.
- Occasionally on some Android devices, users cannot see each other.
- Occasional echo issues when using a specific audio card.
- Occasional video delays on some Android devices.
- Occasional crashes on some Android devices when transmitting the video streams.


### API Changes

To improve your experience, we made the following changes to the APIs:

To avoid adding too many users with the same uid into the CDN publishing channel, the following API method is deleted in v2.3.0, and the return value type of `addUser` is changed from void to int.

-   <code>setUser</code>


The following API methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. For more information on the Recording SDK, see [Release Notes for Agora Recording SDK](../../en/Product%20Overview/release_recording.md).

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


### Backward Compatibility Issues

None.

### Known Issues

None.

## v2.2.3 

v2.2.3 is released on July 5, 2018. 

### Read This First

The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see `Token Migration Guide`.

### Issues Fixed

- Occasional online statistics crashes.
- The broadcaster’s voice distorts occasionally on some Android devices.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Receiving the <code>onLeaveChannel</code> callback long after a user has left the channel on some Android devices.
- Failing to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the broadcaster in a live broadcast.
- Occasional video freezes during a live broadcast.
- Occasional ANR (application no response) problem on some Android devices after a user turns off the camera to end a video session.
- Occasional video freeze after a view size change.

## v2.2.2

v2.2.2 is released on June 21, 2018.

### Issues Fixed

- Fixed occasional online statistics crashes.
- Fixed occasional audio crashes on some Android devices.
- Fixed the issue that the broadcaster’s voice distorts occasionally on some Android devices.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of receiving the onLeaveChannel callback long after a user has left the channel on some Android devices.
- Fixed the issue of occasional video freeze after a view size change.
- Fixed the occasional ANR (application no response) problem on some Android devices after the user turns off the camera to end a video session.

## v2.2.1

v2.2.1 is released on May 30, 2018.

### Issues Fixed

- Occasional crashes during gaming on some Android devices.
- The soundtrack pointer cannot be retrieved on some Android devices.
- Occasional crashes on some Android devices.
- The audio volume on some Android devices cannot be adjusted after a headset is plugged in.


## v2.2.0

v2.2.0 is released on May 4, 2018. 

### New Features

#### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method for the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

We provide a proxy package for enterprise users with corporate firewalls to deploy before accessing our services. See [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

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

To test if the customers’ network condition can support voice or video calls before joining the channel, the <code>onLastmileQuality</code> callback changes the detection from a fixed bitrate to the bitrate set by the customer in the <code>setVideoProfile</code> method to improve data accuracy. When the network condition is unknown, the SDK triggers this callback once every two seconds. 

#### 4. Audio quality enhancement

Improves the audio quality in scenarios that involve music playback.

### Issues Fixed

- Occasional screen display abnormalities when a large number of audience members join as the host in a live-broadcast channel.
- Fixes occasional CDN streaming abnormalities when some app switches to run in the background during a live broadcast.


## v2.1.3

v2.1.3 is released on April 19, 2018. 

In v2.1.3, Agora updates the bitrate values of the <code>setVideoProfile</code> method in the Live-broadcast profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.

### Improvements

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to the normal Communication or Live-broadcast profile.

## v2.1.2

v2.1.2 is released on April 2, 2018. 

>  If you upgrade the SDK to v2.1.2 from a previous version, the live-broadcast video quality is better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth.

### New Features

Extends the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

### Issues Fixed

Video freeze in DTX + AAC mode.

## v2.1.1 

v2.1.1 is released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0 

v2.1.0 is released on March 7, 2018.

### New Features

#### 1. Voice Optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhance the audio effect input from the built-in microphone

In an interactive broadcast scenario, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Adds Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   Voice or video calls: See [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   Interactive broadcasts: See [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


#### 4. 17-way video

Adds the support of 17-way video in interactive broadcasts, see:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_android.md)
-   [Video Conference of 7+ Users](../../en/Interactive%20Broadcast/seventeen_people_android.md)


#### 5. Video source customization

Supports the default video-capturing features provided by the camera and the customized video source. 

#### 6. Renderer customization

Supports the default functions provided by the renderers to display the local and remote videos to meet your requirements. We provide a set of interfaces for customized renderers. 

#### 7. Injecting an external video stream

Adds the function of injecting an external video stream to an ongoing live broadcast.

#### 8. Camera focus change

Adds an <code>onCameraFocusAreaChanged</code> callback to report to the app when the camera focus area changes.

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
<tr><td>Hardware Encoder</td>
<td>Enables the H.264 hardware encoder on the Qualcomm, MTK, HiSilicon, and Orion chips.</td>
</tr>
<tr><td>Hardware Encoder</td>
<td>Improves the bitrate control for the hardware encoder.</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are charged according to the voice-only mode. For example, 16 &times; 16.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional playback noise on specific devices.
-   Occasional crackling voice playback on specific devices.
-   Occasional crashes.


## v2.0.2

v2.0.2 is released on December 15, 2017, and fixes occasional audio routing issues.

## v2.0 and Earlier

### v2.0

v2.0 is released on December 6, 2017. 

#### New Features

-   Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the Communication profile to support dual streams.
-   Adds the camera management function in the Communication and Live-broadcast profiles by adding the following API methods:

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
<tr><td><code>isCameraZoomSupported</code></td>
<td>Checks whether the device supports camera zoom.</td>
</tr>
<tr><td><code>isCameraTorchSupported</code></td>
<td>Checks whether the device supports camera flash.</td>
</tr>
<tr><td><code>isCameraFocusSupported</code></td>
<td>Checks whether the device supports camera manual focus.</td>
</tr>
<tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
<td>Checks whether the device supports camera auto-face focus.</td>
</tr>
<tr><td><code>setCameraZoomFactor</code></td>
<td>Sets the camera zoom ratio.</td>
</tr>
<tr><td><code>getCameraMaxZoomFactor</code></td>
<td>Gets the maximum zoom ratio of the camera.</td>
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



-   Supports external audio sources in the Communication and Live-broadcast profiles by adding the following API methods:

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



-   Provides a set of RESTful APIs to ban a peer user from the server in the Communication and Live-broadcast profiles. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function, if required.
-   Supports the following Android emulators: NOX, Lightning, and Xiaoyao.


#### Improvements

Optimizes the hardware encoder by supporting encoding resolutions as low as 64 x 64.

#### Issues Fixed

-   Audio routing and Bluetooth issues.
-   Optimizes the volume balance control.


### v1.14

v1.14 is released on October 20, 2017. 

#### New Features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch
-   Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor


#### Improvements

-   Optimizes the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions).
-   Adds the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially under poor network conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed

Camera related issues on Android devices.

### v1.13.1

v1.13.1 is released on September 28, 2017, and optimizes the echo issue under certain circumstances.

### v1.13

v1.13 is released on September 4, 2017. 

#### New Features

-   Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Adds the function to disable the audio playback.
-   Supports the profile configuration for stream-pushing on the client side.
-   Adds the <code>onClientRoleChanged</code> callback to report to the app on a user role switch between the host and the audience in a live broadcast.
-   Supports the push-stream failure callback on the server side.


#### Improvements:

The video profile is controllable by the software codec.

#### Issues Fixed:

Occasional crashes on some devices

### v1.12

v1.12 is released on July 25, 2017.

#### New Features:

-   Adds the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.


#### Issues Fixed:

-   Android: Bluetooth issues related to audio routing.
-   Android/iOS/Mac/Windows: Occasional crashes.





