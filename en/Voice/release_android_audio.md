
---
title: Release Notes
description: 
platform: Android
updatedAt: Wed Jan 23 2019 09:11:18 GMT+0000 (UTC)
---
# Release Notes
This page provides the release notes for the Agora Voice SDK for Android.

## Overview

The Voice SDK supports the following scenarios:

-   Voice Communication
-   Live Voice Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

## v2.3.3
The version 2.3.3 was released on Jan. 23rd, 2019. See below for issues fixed.

### Issues Fixed

- Occasional inaccurate statistics returned in the `onNetworkQuality` callback.
- Occasional crashes on Huawei P9.

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

#### Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. See [Adjust the Volume](../../en/Voice/volume_android.md) for the scenarios and corresponding APIs.

### Improvements

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `onAudioQuality` callback and replaces it with the `onRemoteAudioStats` callback to improve the accuracy of the call quality statistics. The `onRemoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `onNetworkQuality` callback for the uplink and downlink network qualities.

- [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`onNetworkQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a76be982389183c5fe3f6e4b03eaa3bd4): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`onLastmileQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Voice/in_call_statistics_android.md).

#### 2. New network connection policy 

v2.3.2 adds the following API method and callback to get the current network connection state and reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9): Gets the connection state of the SDK.
- [`onConnectionStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`onConnectionInterrupted`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88) and [`onConnectionBanned`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `onConnectionStateChanged` callback when the network connection state changes. The SDK also triggers the `onConnectionInterrupted` and `onConnectionBanned` callbacks under certain circumstances, but Agora does not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab7083355af531cc43d455024bd1f7662) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. Application developers can use this feedback for future product improvement. Agora strongly recommends integrating this method in your application.

#### 4. Other improvements

- Improves the stability in pushing streams.
- Improves the performance of the SDK on Android 6.0 or later.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.

### Issues Fixed

The following issues are fixed in v2.3.2:

#### SDK

- Crashes on emulators, such as Yeshen and mumu. 
- Crashes on Android 6.0+ due to x86 .so relocation.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- When the microphone is disabled, if the device connects to the headset, disabling the microphone is invalid.
- On Huawei Mate 20 X, the remote user cannot hear any voice when the application switches to the background and the user opens another application.
- Echo on Pixel 2.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 

### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

#### Added:

- [`getConnectionState`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8635e3c9e26ffe95e7ab9a518af533b9)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`onConnectionStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)
- [`onRemoteAudioStats`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54)

#### Deprecated

- [`onAudioQuality`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abeac442a777e103536dcf9c8ce2d7135)
- [`onConnectionInterrupted`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a0841fb3453a1a271249587fa3d3b3c88)
- [`onConnectionBanned`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80cfde2c8b1b9ae499f6d7a91481c5db)

## v2.3.1

The version 2.3.1 was released on Oct. 12th, 2018.  See below for new features, improvements, and issues fixed.

### New features

#### Disables/Re-enables the local audio function.

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this release adds the `enableLocalAudio` method to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `onMicrophoneEnabled` callback function, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and `muteLocalAudioStream` is that `enableLocalAudio` does not capture or send any audio stream, while `muteLocalAudioStream` captures but does not send the audio stream.


### Issues Fixed

- In the live broadcast profile, occasional crashes on some Android devices after the user repeats the process of switching roles between the BROADCASTER and AUDIENCE.
- In the live broadcast profile, delay at the client due to incorrect statistics.
- Occasionally on some Android devices, a user hears a popping sound if the user leaves the channel at the same time another user is speaking..


## v2.3.0 

The version 2.3.0 was released on August 31, 2018. See below for new features, improvements, issues fixed and API Changes.

### Before Reading

-   From v2.3.0, the `LiveTranscoding Class` was moved from the `io.agora.live` package to the `io.agora.rtc.live` package.

-   Fixed a typo in the `constants.java` API in v2.3.0.

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

#### 1. Notifies the user that the Token will expire in 30 seconds

The SDK returns the `onTokenPrivilegeWillExpire` callback function 30 seconds before a Token expires to notify the app to renew it. When this callback function is received, you need to generate a new Token on your server and call `renewToken` to pass the newly-generated Token to the SDK.

#### 2. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate, and time delay

The `onRemoteAudioTransportStats` callback function is added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or a live broadcast, this callback function is triggered once every two seconds after the user receives audio packets from a remote user. This callback includes the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

### Improvements

- Improved the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
- Improved the audio encoder efficiency in a live broadcast to reduce the user traffic while ensuring the call quality.
- Improved the audio quality during a call or a live broadcast using the deep-learning algorithm.


### Issues Fixed

- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasionally the remote user cannot hear the host when the host switches between the AUDIENCE and BROADCASTER.
- Occasional crashes on Android devices when remote users frequently join and leave the channel.
- Occasionally, the audience cannot adjust the channel volume.
- Occasionally, applications do not respond on some Android devices.
- Occasional crashes when one of the two broadcasters mutes or disables the local audio while playing the background music.
- Occasional crashes on some devices when preloading the sound effects.
- Occasionally, an Android client can still interoperate in a communication channel when removed from the dashboard.
- Failure to enable the hardware encoder on some Android devices.
- Occasional crashes on some Android devices when frequently turning on and off the flashlight during a live broadcast.
- Occasional crashes on some Android devices when a user frequently changes the Token.
- Occasionally, users cannot see each other on Android devices.
- Occasional echoes when using a specific audio card.


### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

To avoid adding too many users with the same uid into the CDN publishing channel, the following method is deleted in v2.3.0, and the return value type of `addUser` is changed from void to int.

-   <code>setUser</code>


The following methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. For more information on the Recording SDK, see [Release Notes for Agora Recording SDK](../../en/Product%20Overview/release_recording.md).

-   <code>startRecordingService</code>
-   <code>stopRecordingService</code>
-   <code>refreshRecordingServiceStatus</code>


The following deprecated methods are deleted and no longer supported from v2.3.0:

-   <code>monitorConnectionEvent</code>
-   <code>monitorBluetoothHeadsetEvent</code>
-   <code>monitorHeadsetEvent</code>
-   <code>setPreferHeadset</code>
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
- The broadcaster’s voice distorts occasionally on some Android devices.
- Occasional crashes during a live broadcast.
- Excessive increase in the memory usage when multiple delegated hosts broadcast in the channel.
- Receiving the <code>onLeaveChannel</code> callback function long after a user has left the channel on some Android devices.
- Failure to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the broadcaster in a live broadcast.


## v2.2.1

The version 2.2.1 was released on May 30, 2018. See below for issues fixed.

### Issues Fixed

- Occasional crashes during gaming on some Android devices.
- The soundtrack pointer cannot be retrieved on some Android devices.
- Occasional crashes on some Android devices.
- The audio volume on some Android devices cannot be adjusted after a headset is plugged in.


## v2.2.0

The version 2.2.0 was released on May 4, 2018. See below for new features and improvements.

### New Features

#### 1. Play the audio effect in the channel

Added a `publish` parameter in the <code>playEffect</code> method, to enable the remote user in the channel to hear the audio effect played locally. 

> If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the proxy at the server

Agora has provided a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. For details, see [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

### Improvements

#### 1. Audio volume indication

Improved the function of <code>enableAudioVolumeIndication</code>. This method, once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method has improved its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support voice or video calls before joining the channel, <code>onLastmileQuality</code> changed the detection from a fixed bitrate to the bitrate set by the customer in <code>setVideoProfile</code> to improve data accuracy. When the network condition is unknown, this callback function is still triggered at two-second intervals. 

#### 4. Audio quality enhancement

Improved the audio quality in scenarios that involve music playback.

## v2.1.3

The version 2.1.3 was released on April 19, 2018. See below for issues fixed.

### Issues Fixed

Occasional recording failures on some phones when the user leaves the channel and turns on the built-in recording device.

## v2.1.2

The version 2.1.2 was released on April 2, 2018. See below for issues fixed.

### Issues fixed

Intermittent crashes under certain circumstances.

## v2.1.1

The version 2.1.1 was released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

The version 2.1.0 was released on March 7, 2018. See below for new features, improvements, and issues fixed.

### New Features

#### 1. Voice optimization

Added a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhanced audio effect input from a built-in microphone

In an interactive broadcast, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

#### 3. Online statistics query

Added Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host:

-   For voice or video calls, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md).
-   For interactive broadcasts, see [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md).


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
<tr><td>Authentication</td>
<td>Supported a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each Token in the new authentication mechanism includes all the privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional playback noise on specific devices.
-   Occasional crackling voice playback on specific devices.
-   Occasional crashes.


## v2.0.2

The version 2.0.2 was released on December 15, 2017. See below for issues fixed.

Fixed occasional audio routing issues.

## v2.0 and Earlier
### v2.0

The version 2.0 was released on December 6, 2017. See below for new features and issues fixed.

#### New Features

-   Supported the external audio source in the communication and live broadcast scenarios by adding the following methods:

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



-   Provided a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast scenarios. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function if required.
-   Supported the following Android emulators: NOX, Lightning, and Xiaoyao.

#### Issues Fixed

-   Audio routing and Bluetooth issues.
-   Optimized the volume balance control.


### v1.14

The version 1.14 was released on October 20, 2017. See below for new features and improvements.

#### New Fesatures

-   Added the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Added the <code>setLocalVoicePitch</code> method to set the local voice pitch.
-   Live Broadcast: Added the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimized the audio at high bitrates.
-   Added the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.


### v1.13.1

The version 1.13.1 was released on September 28, 2017. 

Optimized the echo issue under certain circumstances.

### v1.13

The version 1.13 was released on September 4, 2017. See below for new features, improvements, and issues fixed.

-   Fixed occasional crashes.
-   Added the <code>onClientRoleChanged</code> method to indicate a user role change between the host and audience in a live broadcast scenario.
-   Supported Android emulators.


### v1.12

The version 1.12  was released on July 25, 2017. See below for new features and issues fixed.

#### New Featuress:

-   Added the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code>  method.
-   Added the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Added the following API methods to manage the audio effects:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>API Reference</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>getEffectsVolume</code></td>
<td>Gets the volume of the audio effects from 0.0 to 1.0.</td>
</tr>
<tr><td><code>setEffectsVolume</code></td>
<td>Sets the volume of the audio effects.</td>
</tr>
<tr><td><code>setVolumeOfEffect</code></td>
<td>Adjusts the volume of the specified sound effect in real time.</td>
</tr>
<tr><td><code>playEffect</code></td>
<td>Plays the audio effect.</td>
</tr>
<tr><td><code>stopEffect</code></td>
<td>Stops playing a specific audio effect.</td>
</tr>
<tr><td><code>stopAllEffects</code></td>
<td>Stops playing all audio effects.</td>
</tr>
<tr><td><code>preloadEffect</code></td>
<td>Preloads a specific audio-effect file (compressed audio file) to the memory.</td>
</tr>
<tr><td><code>unloadEffect</code></td>
<td>Releases the specific preloaded audio effect from the memory.</td>
</tr>
<tr><td><code>pauseEffect</code></td>
<td>Pauses the specific audio effect.</td>
</tr>
<tr><td><code>pauseAllEffects</code></td>
<td>Pauses all audio effects.</td>
</tr>
<tr><td><code>resumeEffect</code></td>
<td>Resumes playing a specific audio effect.</td>
</tr>
<tr><td><code>resumeAllEffects</code></td>
<td>Resumes all audio effects.</td>
</tr>
</tbody>
</table>




#### Issues Fixed:

-   Bluetooth issues related to audio routing.
-   Occasional crashes.

