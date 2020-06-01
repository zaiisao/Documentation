
---
title: Release Notes
description: 
platform: Web
updatedAt: Mon Jun 01 2020 02:04:16 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Web SDK.

## Overview

The Agora Web SDK is a JavaScript library loaded by an HTML web page. The Agora Web SDK library uses APIs in the web browser to establish connections and control the communication and live broadcast services. For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), and [Interactive Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

### Compatibility

See the table below for the web browser support of the Agora Web SDK:

<table>
  <tr>
    <th>Platform</th>
    <th>Chrome 58 or later</th>
    <th>Firefox 56 or later</th>
    <th>Safari 11 or later</th>
    <th>Opera 45 or later</th>
    <th>QQ Browser 10.5 or later</th>
    <th>360 Secure Browser</th>
    <th>WeChat Built-in Browser</th>
  </tr>
   <tr>
    <td>Android 4.1 or later</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11 or later</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

<div class="alert info">Other browser support:
	<li>The Agora Web SDK v2.5 or later supports Chrome 49 on Windows XP (supports the VP8 codec only, and cannot interop with the Native SDK).</li>
	<li>The Agora Web SDK v2.7 or later supports Edge on Windows 10, see <a href="https://docs.agora.io/en/faq/browser_support#edge">Edge support</a> for details.</li>
	<li>The Agora Web SDK theoretically supports 360 Extreme Browser, but we do not guarantee full support.</li>
</div>
<div class="alert note"> Upgrade to Agora Web SDK v2.6 or later in the following scenarios:
	<li>Safari on iOS 12.1.4 or later.</li>
	<li>Safari 12.1 or later on macOS.</li>
</div>

> To enable interoperability between the Agora Native SDK and Agora Web SDK, use the Agora Native SDK v1.12 or later.

### Known issues and limitations

- Due to web browser autoplay policy changes, the `Stream.play`, `Stream.startAudioMixing`, and `Stream.getAudioLevel` methods need to be triggered by the user's gesture on Chrome 70 or later and on Safari. See [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes).
- The Agora Web SDK supports video profiles of up to 1080p resolutions if the client has a true HD camera installed. However, the maximum resolution is limited by camera device capabilities.
- On some iOS devices, when you switch the Safari browser to the background and back during a call, black bars might appear around your video.
- The Agora Web SDK does not support code obfuscation.

For more issues, see [Web FAQs](https://docs.agora.io/en/search?type=faq&platform=Web).

## v3.1.0

v3.1.0 was released on April 30, 2020.

**Behavior changes**

v3.1.0 optimizes [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms#dual-stream) and makes the following changes in SDK behavior:

- The low-quality stream has a fixed default video profile: A resolution (width × height) of 160 × 120, a bitrate of 50 Kbps, and a frame rate of 15 fps.
- The low-quality video stream keeps the aspect ratio of the high-quality video stream. If you set the resolution of the low-quality stream to a different aspect ratio, the SDK automatically adjusts the height of the low-quality stream so that the aspect ratio remains the same.

**New features**

<!--#### 1. Media metadata

Adds `Client.sendMetadata` to support attaching media metadata to a published stream. You can use this feature to broaden the types of interactions in a live broadcast, such as sharing digital coupons and online quizzes.

Adds the `Client.on("receive-metadata")` callback to notify the app when the SDK receives metadata from a remote user.

<div class="alert note">Note:
	<li>This feature supports the H.264 codec only.</li>
	<li>When the network is unstable, the SDK cannot guarantee strict synchronization between the metadata and the media stream.</li>
</div>-->

#### Event of unpublishing a stream

Adds the `Client.on("stream-unpublished")` callback. When the local user successfully unpublishes a stream, the SDK triggers this callback.

#### Multiple TURN servers

This release enables you to pass configurations of multiple TURN servers to `ClientConfig.turnServer.`

**Fixed issues**

- Occasional reconnection failure.
- Video streams published from iOS Safari rotated by 90 degrees on the receiving clients.
- On Firefox 75, calling `Stream.getStats` got an empty result.
- In audio-only scenarios, calling `Stream.switchDevice` caused errors.
- If reconnection occurred when calling `Client.join`, the SDK did not return the user ID after joining the channel successfully.
- The value of the `status` property in `StreamPlayError` was inaccurate.
- Occasionally, the SDK triggered the `Client.on("mute-audio")` and `Client.on("mute-video")` callbacks repeatedly.
- The SDK failed to get the local audio when creating streams in Electron.

**API changes**

Added the event `"stream-unpublished"` in [`Client.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on).

## v3.0.2

v3.0.2 was released on March 16, 2020. This version made some internal improvements.

## v3.0.1

v3.0.1 was released on February 12, 2020.

**New features**

#### Dual-stream mode support for custom video source streams

This version supports the dual-stream mode (`enableDualStream`) for video streams created by defining the `videoSource` property.

**Improvements**

Optimizes the reconnection strategy to improve the user experience under poor network conditions.

**Fixed issues**

- A repeated call of `setClientRole` causes errors.
- The console prints error messages after calling `Client.leave` when the network is disconnected.
- An error occurs when creating a stream with the `videoSource` property.
- The `muteAudio` and `muteVideo` methods do not take effect after unsubscribing from and then subscribing to a remote stream.

##  v3.0.0

v3.0.0 was released on December 2, 2019.

This release optimizes the SDK's performance in terms of transmission quality and interoperability, significantly reducing the time to render the first remote video frame, and improving the video experience under poor downlink network conditions.

**New features**


#### Channel media stream relay

Adds the following methods for relaying the media stream of a host from a source channel to a destination channel. This feature applies to scenarios where hosts from different live-broadcast channels interact with each other.

- `startChannelMediaRelay`
- `updateChannelMediaRelay`
- `stopChannelMediaRelay`

During a media stream relay, the SDK reports the states and events of the relay with the `Client.on("channel-media-relay-state")` and `Client.on("channel-media-relay-event")` callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host across Channels](https://docs.agora.io/en/Interactive%20Broadcast/media_relay_android?platform=Web).

#### Audio sharing

Adds the `screenAudio` property to `StreamSpec` for sharing the local audio playback when sharing a screen. See [Share audio](https://docs.agora.io/en/Video/screensharing_web?platform=Web#a-name--screenaudioashare-audio) for details.

#### Image enhancement

Adds the `setBeautyEffectOptions `method for setting image contrast, brightness, sharpness, and red saturation. See [Image enhancement](https://docs.agora.io/en/Interactive%20Broadcast/image_enhancement_web?platform=Web) for details.

#### Adding watermark images to a live stream

Adds the `images` property to `LiveTranscoding` for inserting online PNG images as watermarks to a live streaming.

#### Encryption/decryption failure notification

Adds the `Client.on("crypt-error")` callback for notifying the app that an encryption/decryption failure occurs when the local users is publishing or subscribing to a stream.

**Improvements**

#### Reporting the state of a remote video stream

Adds the `Client.on("enable-local-video")` and `Client.on("disable-local-video") `callbacks for notifying the app that a remote user on the Native SDK calls `enableLocalVideo` to enable or disable video capture.

#### Reporting the reason why a remote user goes offline

Adds `reason` to the `Client.on("peer-leave")` callback for reporting the reason why the remote user goes offline.

**Fixed issues**

- `Client.join` does not report the error of an incorrect App ID in its onFailure callback function.

- Occasionally, the SDK does not automatically reconnect after being disconnected from the servers for pushing and pulling streams.
  For answers to questions arising from common disconnection issues, click the following FAQ links:
  - [When pushing streams to the CDN, what should I do when a disconnection happens?](https://docs.agora.io/en/faq/live_streaming_disconnection_web)
  - [When injecting online streams to the CDN, what should I do when a disconnection happens?](https://docs.agora.io/en/faq/injecting_stream_disconnection_web)
- Occasionally, the error message `"Cannot read property 'getLastMsgTime' of null"` appears.
- The console reports an error when the local user enables or disables the audio or video track of a remote stream.

**API changes**

#### Added

- [`Client.startChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#startchannelmediarelay)

- [`Client.updateChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#updatechannelmediarelay)

- [`Client.stopChannelMediaRelay`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#stopchannelmediarelay)

- [`Stream.setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setbeautyeffectoptions)

- Adds the following events in [`Client.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on)
  - `"enable-local-video"`
  - `"disable-local-video"`
  - `"channel-media-relay-event"`
  - `"channel-media-relay-state"`
  - `"crypt-error"`

#### Updated

Adds the [`screenAudio`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#screenaudio) property in[`AgoraRTC.createStream`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/globals.html#createstream).


## v2.9.0
v2.9.0 is released on September 5, 2019.

**Compatibility changes**

To improve the usability of the RTMP streaming service, v2.9.0 defines the following parameter limits in [`LiveTranscoding`:](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.livetranscoding.html)

- `videoFramerate`: Frame rate (fps) of the CDN live output video stream.  Agora adjusts all values over 30 to 30.
- `videoBitrate`: Bitrate (Kbps) of the CDN live output video stream. Set this parameter according to the [Video Bitrate Table](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.videoencoderconfiguration.html#bitrate). If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.
- `videoCodecProfile`: The video codec profile. Set it as 66, 77, or 100. If you set this parameter to other values, Agora adjusts it to the default value of 100.
- `width` and `height`: Pixel dimensions of the video. The minimum value of width x height is 16 x 16.

**New features**

#### Support for NAT64

Supports using the Agora Web SDK in NAT64 networks.

#### Using the front or rear camera

Adds the `facingMode` parameter in the `createStream` method to set using the front or rear camera on mobile devices.

#### Unbinding events

Adds the `Client.off` method to support removing the events attached by the `Client.on()` method.

**Improvements**

- Reduces the number of required firewall ports. Users with firewalls do not need to open UDP ports 10000 to 65535. See [Web SDK firewall ports](https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms#web-sdk).
- Further improves the NAT type compatibility and the media stream connection.
- Shortens the time to join the channel and the time to render the first remote video frame.
- Optimizes the experience in poor downlink network conditions.
- Specifies error messages in the failure callback function of some API methods.
- Syncs the mute status every time a stream is successfully published.
- Only triggers the volume-indicator after joining a channel.
- Optimizes the verification of parameters.

**Issues fixed**

- The subscribing options are reset after reconnection.
- Calling `Stream.close` when initializing the stream causes abnormal behaviors.
- Setting `cacheResource` in `Stream.startAudioMixing` does not take effect.
- Abnormal behaviors after network disconnection.

**API changes**

- Adds the `Client.off` method.
- Adds the `facingMode` parameter in`AgoraRTC.createStream`.

## v2.8.0
v2.8.0 is released on July 8, 2019.

This version optimizes the support for string UIDs.

All users in the same channel should have the same type (number or string) of user ID. If you use string UIDs to interoperate with the Agora Native SDK, ensure that the Native SDK uses the string user account to join the channel. See [Use String User Accounts](https://docs.agora.io/en/faq/string) for details.

## v2.7.1

v2.7.1 is released on July 3, 2019. 

**Issues fixed**

Setting the video profile of the shared screen by the `Stream.setScreenProfile` method does not take effect.

## v2.7.0

v2.7.0 is released on June 21, 2019.

**New features**

#### 1. Customizing the video encoder configuration

Adds the `Stream.setVideoEncoderConfiguration` method to customize the video encoder configuration. Compared with the`Stream.setVideoProfile` method, this method is more flexible and supports customizing the video resolution, frame rate, and bitrate. See [Set the Video Profile](../../en/Interactive%20Broadcast/video_profile_web.md) for details.

#### 2. Reporting the status of the stream playback

For easier management of the stream playback status, v2.7.0 adds the following functions:

- Adds the `callback` parameter in the `Stream.play` method to report the result of playing a stream. If the playback fails, this parameter provides the error details.
- Adds the `"player-status-change"` callback in `Stream.on`. When the stream playback status changes, this callback is triggered to report the playback status and the reason why the status changes.
- Adds the `Stream.resume` method to resume the playback when the `Stream.play` method fails.

#### 3. Reporting when the first remote audio/video frame is decoded

Adds the `"first-audio-frame-decode"` and `"first-video-frame-decode"` callbacks in `Client.on`  to notify the app when the first remote audio/video is decoded after subscribing to the remote stream.

#### 4. Reporting changes to the media device

Adds the `"audioTrackEnded"` and `"videoTrackEnded"` callbacks in `Stream.on` to notify the app when the audio/video track no longer provides data to the stream, for example when the device is removed or deauthorized.

#### 5. Supporting Microsoft Edge

Supports audio/video calls and live broadcasts on the Microsoft Edge browser. For details, see [Agora Web SDK FAQ](https://docs.agora.io/en/faq/browser_support#edge).

**Improvement**

This version allows updating the video encoder configuration dynamically. You can call `setVideoProfile` or `setVideoEncoderConfiguration` before or after `Stream.init`.

> Do not set the video encoder configuration when publishing a stream.

**Issues fixed**

- Some of the statistics returned by calling `Stream.getStats` are incorrect.
- Calling `Client.leave` does not take effect when the network connection is lost.
- Some of the statistics on the call quality are incorrect when String UIDs are used.
- The return value of `Stream.getAudioMixingPosition` is inaccurate when the audio mixing file is playing.
- Calling Stream.unmuteVideo immediately after subscribing to an audio-only stream causes an audio playback failure.

**API changes**

#### New APIs

- [`Stream.setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setvideoencoderconfiguration)
- [`Stream.resume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resume)
- New callbacks added in the [`Client.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on)  method:
  - `"first-audio-frame-decode"`
  - `"first-video-frame-decode"`
- New callbacks added in the [`Stream.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#on) method:
  - `"audioTrackEnded"`
  - `"videoTrackEnded"`
  - `"player-status-change"`

#### API update

[`Stream.play`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#play): Adds the `callback` parameter.

## v2.6.1

v2.6.1 is released on April 11, 2019.

**Improvement**

This version supports using empty strings for the `cameraId` and `microphoneId` properties in the `createStream` method.

**Issues fixed**

- Only two video streams at most can be played on Safari on iOS at the same time.
- Errors occur when calling the `enableDualStream` method before publishing a stream.
- Some statistics returned by the `getStats` method are missing in calls using string UIDs.

## v2.6.0

v2.6.0 is released on April 3, 2019.

**New features**

#### 1. Audio effect file playback and management

Supports playing multiple audio effect files at the same time, and managing the files. You can set the volume, pause/stop the playback, and preload the audio effect file. See [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/audio_effect_mixing_web.md).

#### 2. Screen sharing on Chrome without extension

Supports screen sharing on Chrome 72 and later without using the Chrome extension. See [Share the Screen](../../en/Interactive%20Broadcast/screensharing_web.md).

#### 3. Other new features

- Supports Firefox ESR 60.1.0 and later on PC.
- Supports releasing the audio mixing cache: adds the `cacheResource` property in the `Stream.startAudioMixing` method to set whether or not to store the audio mixing file in the cache.
- Adds the `AgoraRTC.getSupportedCodec` method to get the supported codecs of the web browser.
- Adds the `stream-updated` callback in the `Client.on` method to notify users when the remote stream adds or removes a track.
- Supports using URLs with Chinese characters in the `startAudioMixing` method.

**Improvements**

- Improves the user experience in unreliable network conditions.
- Adds the `stream-fallback` callback in the `Client.on` method to notify the user when the remote video stream falls back to audio-only when the network conditions worsen or switches back to video when the network conditions improve.

**Issues fixed**

- The `switchDevice` method fails to switch between microphones on Chrome 72.
- Muting audio/video does not take effect after calling the `addTrack` method.
- After the audio mixing stops, if you call the `switchDevice` method and start audio mixing again, the remote user cannot hear the audio mixing.
- Users with the string UID cannot receive the `active-speaker` callback.
- Calling the `muteAudio` and  `muteVideo` methods immediately after subscribing to the remote stream might not take effect.
- After calling the `replaceTrack` method to switch the video track on Windows, if you call the `startAudioMixing` method, the remote user cannot hear the audio mixing.

**API changes**

#### New APIs

- [`AgoraRTC.getSupportedCodec`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/globals.html#getsupportedcodec)
- [`Stream.playEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#playeffect)
- [`Stream.stopEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#stopeffect)
- [`Stream.pauseEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#pauseeffect)
- [`Stream.resumeEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resumeeffect)
- [`Stream.setVolumeOfEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setvolumeofeffect)
- [`Stream.preloadEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#preloadeffect)
- [`Stream.unloadEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#unloadeffect)
- [`Stream.getEffectsVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#geteffectsvolume)
- [`Stream.setEffectsVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#seteffectsvolume)
- [`Stream.stopAllEffects`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#stopalleffects)
- [`Stream.pauseAllEffects`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#pausealleffects)
- [`Stream.resumeAllEffects`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resumealleffects)
- New callbacks added in the [`Client.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on) method: 
  - `stream-fallback`
  - `stream-updated`

#### API updates

- Adds the `cacheResource` parameter in the [`Stream.startAudioMixing`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing) method.


## v2.5.2

v2.5.2 is released on February 28, 2019. 

**Issues fixed**

- The `Stream.switchDevice` method fails to switch audio devices on Chrome 72 or later.
- Errors occur when none of the optional parameters are set for the `Client.subscribe` method.

## v2.5.1

v2.5.1 is released on February 19, 2019. 

**New features**

#### 1. More call quality statistics

Adds more statistics on the call quality. 

- Adds the `Client.getSessionStats` method to get the statistics of the call sessions, such as the duration in the channel, the total received and sent bitrate of the stream, and the number of users in the channel.
- Adds the `network-quality` callback to report the uplink and downlink network conditions of the local user once every two seconds. 
- Adds the following audio statistics of the remote stream to the `Client.getRemoteAudioStats` method:
  - The total freeze time of the received audio.
  - The total playback duration of the received audio.
- Adds the following video statistics of the remote stream to the `Client.getRemoteVideoStats` method:
  - The resolution width of the received video.
  - The resolution height of the received video.
  - The total playback duration of the received video.
  - The total freeze time of the received video.
- Adds the following video statistics of the local stream to the `Client.getLocalVideoStats` method:
  - The resolution width of the sent video.
  - The resolution height of the sent video.
  - The frame rate of the sent video.
  - The total duration of the encoded video.
  - The total freeze time of the encoded video.
- Adds the following statistics of the transmission quality to the Agora service to the `Client.getTransportStats` method:
  - The estimated uplink bandwidth.
  - The network type.

#### 2. Support for receiving the audio/video data independently

Adds the `options` parameter to the `Client.subscribe` method to set whether or not to receive the audio and/or video data. 

This method can be called multiple times and enables users to switch between receiving and not receiving the audio and/or video data flexibly.

#### 3. Support for injecting online media streams to live broadcasts

Adds the `Client.addInjectStreamUrl` method to pull a voice or video stream and inject it into a live channel. This is applicable to scenarios where all of the audience members in the channel can watch a live show and interact with each other.

Adds the `streamInjectedStatus` callback to inform the app of changes to the injection status.

#### 4. Support for setting the user role

Adds the `Client.setClientRole` method to set the user role as a host or an audience in a live broadcast. A host can both send and receive streams while an audience can only receive streams.

Adds the `client-role-changed` callback to inform the app of changes to the user role.

#### 5. Connection status

- Adds the `Client.getConnectionState` method to retrieve the connection status between the SDK and Agora's edge server.
- Adds the `connection-state-change` callback to inform the app of changes to the connection status. 

#### 6. Cloud proxy

Supports the cloud proxy service. See [Use Cloud Proxy](../../en/Interactive%20Broadcast/cloud_proxy_web.md) for details.

#### 6. Other new features

- Adds the `Stream.isPlaying` method to detect whether or not a stream is playing.
- Adds the following callbacks in the `Client.on` method:
  - `peer-online`: Informs the app that a remote user or host joins the channel.
  - `stream-reconnect-start`: Informs the app that the SDK starts republishing or re-subscribing to streams.
  - `stream-reconnect-end`: Informs the app that the SDK finishes republishing or re-subscribing to streams.
  - `exception`: Informs the app of exceptions.
- Adds the following callbacks in the `Stream.on` method:
  - `audioMixingPlayed`: Informs the app that the audio mixing stream starts or resumes playing.
  - `audioMixingFinished`: Informs the app that the audio mixing stream finishes playing. 

**Improvements**

- Adds the `muted` parameter to the `Stream.play` method to work around the web browser's autoplay policy.
- Adds the `callback` parameter to the `Stream.setAudioMixingPosition` method to return error messages when the method call fails.
- Modifies the names of some callbacks in the `Client.on` method to be consistent in style.

**Issues fixed**

- The video resolution of the remote video returned by the `getStats` method is 0 on Safari for macOS.
- The user cannot hear the microphone when the `enableAudio` method is called after disabling audio and then starting audio mixing.
- Logs cannot be uploaded when offline.
- The volume retrieved by calling the `getAudioLevel` method is 0 after calling the `switchDevice` method to switch audio devices.
- Users hear their own voice after calling the `replaceTrack` method.
- Users cannot switch devices twice on iOS when calling the `switchDevice` method.

**API changes**

#### New APIs

- [`AgoraRTC.getScreenSources`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/globals.html#getscreensources)
- [`Client.addInjectStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#addinjectstreamurl)
- [`Client.removeInjectStreamUrl`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#removeinjectstreamurl)
- [`Client.getSessionStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getsessionstats)
- [`Client.setClientRole`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setclientrole)
- [`Client.getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#getconnectionstate)
- [`Client.startProxyServer`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#startproxyserver)
- [`Client.stopProxyServer`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#stopproxyserver)
- [`Stream.isPlaying`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#isplaying)
- [`Stream.muteAudio`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#muteaudio)
- [`Stream.unmuteAudio`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#unmuteaudio)
- [`Stream.muteVideo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#mutevideo)
- [`Stream.unmuteVideo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#unmutevideo)
- New callbacks added in the [`Client.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on) method:
  - `streamInjectedStatus`
  - `client-role-changed`
  - `peer-online`
  - `network-quality`
  - `connection-state-change`
  - `stream-reconnect-start`
  - `stream-reconnect-end`
  - `exception`
- New callbacks added in the [`Stream.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#on) method:
  - `audioMixingPlayed`
  - `audioMixingFinished`

#### API updates

- [`Client.subscribe`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#subscribe): Adds the `options` parameter.

- [`Stream.setAudioMixingPosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudiomixingposition): Adds the `callback` parameter.

- [`Stream.play`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#play): Adds the `muted` parameter.

- Renames the following callbacks in the [`Client.on`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#on) method:

  | Old Callback Name      | Current Callback Name    |
  | ---------------------- | ------------------------ |
  | networkTypeChanged     | network-type-changed     |
  | recordingDeviceChanged | recording-device-changed |
  | playoutDeviceChanged   | playout-device-changed   |
  | cameraChanged          | camera-changed           |
  | streamTypeChange       | stream-type-changed      |

#### Deprecated APIs

- `Client.getNetworkStats`
- `Stream.enableAudio`
- `Stream.disableAudio`
- `Stream.enableVideo`
- `Stream.disableVideo`

## v2.5.0

v2.5.0 is released on October 30, 2018. 

> <font color="red">If you set the domain firewall, ensure that `*.agoraio.cn` and `*.agora.io` are added to your whitelist before using this version.</font>

**New features**

To enable better interoperability between the Agora Web SDK and other Agora SDKs, this version adds the following features. For detailed descriptions of the APIs, see [Agora Web SDK API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/index.html).

#### 1. Quality transparency

Adds the following API methods on call statistics to give users a better idea of the call quality:

- `Client.getNetworkStats`: Retrieves the network statistics (network type).
- `Client.getSystemStats`: Retrieves the system statistics (system battery level).
- `Client.getRemoteAudioStats`: Retrieves the audio statistics of the remote stream.
- `Client.getLocalAudioStats`: Retrieves the audio statistics of the local stream.
- `Client.getRemoteVideoStats`:  Retrieves the video statistics of the remote stream.
- `Client.getLocalVideoStats`: Retrieves the video statistics of the local stream.
- `Client.getTransportStats`: Retrieves the statistics of network transport.

#### 2. Device management

Provides flexible device management and device status notification.

- **Device enumeration**

Adds the following API methods:

  - `Client.getRecordingDevices`: Enumerates the audio input device, such as the microphone.
  - `Client.getPlayoutDevices`: Enumerates the audio output device, such as the speaker.
  - `Client.getCameras`: Enumerates the video input device, such as the camera.

Adds the following callbacks to inform the app on relevant device changes:

  - `recordingDeviceChanged`: The audio input device is changed.
  - `playoutDeviceChanged`: The audio output device is changed.
  - `cameraChanged`: The video input device is changed.

- **Device switching**

Adds the following API methods:

  - `Stream.switchDevice`: Switches the media input devices in the channel. For example, the microphone and camera.
  - `Stream.setAudioOutput`: Sets the audio output device. You can use this method to switch between the microphone and the speaker.

#### 3. Audio mixing

Supports audio mixing, namely mixing the original sound (the audio captured by the microphone) with the audio playback (an audio file).

Adds the following API methods:

- `Stream.startAudioMixing`: Starts audio mixing.
- `Stream.stopAudioMixing`: Stops audio mixing.
- `Stream.pauseAudioMixing`: Pauses audio mixing.
- `Stream.resumeAudioMixing`: Resumes audio mixing.
- `Stream.adjustAudioMixingVolume`: Adjusts the audio mixing volume.
- `Stream.getAudioMixingDuration`: Retrieves the audio mixing duration.
- `Stream.getAudioMixingCurrentPosition`: Retrieves the current position of the audio mixing.
- `Stream.setAudioMixingPosition`: Sets the playback position of the audio mixing.

#### 4. Audio/Video track management

Provides flexible management of the audio and video tracks.

Adds the following API methods:

- `Stream.getAudioTrack`: Retrieves the audio track.
- `Stream.getVideoTrack`: Retrieves the video track.
- `Stream.replaceTrack`:  Replaces the audio/video track.
- `Stream.addTrack`: Adds an audio/video track.
- `Stream.removeTrack`: Removes an audio/video track.

#### 5. Other new features

- Support for two video display modes. You can set the display mode in the `Stream.play` method.
- Adds the `Client.enableAudioVolumeIndicator` method to enable the SDK to regularly report on the active speakers and their volumes.
- Adds the `Stream.setAudioVolume` method, which sets the audio volume of the subscribed stream.
- Adds the `networkTypeChanged` callback to inform the app on network type changes.
- Adde the `streamTypeChanged` callback to inform the app on stream type changes (from a low-stream video to a high-stream video or vice versa).
- Supports both string and number types for the `uid` parameter in the `Client.join` method.
- Supports 360 Secure Browser 9.1.0.432 and later.
- Supports Chrome 49 on Windows XP.

**Issues fixed**

- The dependency on the video codec in audio-only calls when a user joins a channel from Safari or Chrome on mobile devices.
- A failure to receive the `stream-removed` callback 10 seconds after another user calls the `Stream.close` method to stop streaming from the Safari browser.
- A warning after a user uses the same UID to reset the `Stream.userID` method.

## v2.4.1

v2.4.1 is released on September 19, 2018. 

**Issues fixed**

- The local publisher may not automatically publish after an IP address change, so the remote subscriber cannot receive the video stream.
- Unsubscribing from the remote stream triggers the `stream-removed` callback.

## v2.4.0

v2.4.0 is released on August 24, 2018. 

**New features**

#### 1. Support for Token Authentication

See [Security Keys](../../en/Agora%20Platform/token.md) for details.

#### 2. Support for Audio Processing

Adds the support for AEC (Acoustic Echo Cancellation) and ANS (Automatic Noise Suppression) in the `audioProcessing` property. 

#### 3. Support for Audio/Video Pre-processing

Adds the `audioSource` and `videoSource` properties in the `client.createStream` method, which specifies the audio and video tracks. Therefore, you can process the audio/video before creating a stream. 

#### 4. Support for Setting the Audio Profile

Adds the `stream.setAudioProfile` method, which provides various audio profile options (the sample rate, mono or stereo, and the encoding rate).  

#### 5. Support for Setting the Stream Fallback

Adds the <code>client.setStreamFallbackOption</code> method, which allows the receiver to automatically subscribe to the low-stream video or the audio-only stream under poor network conditions.

#### 6. Adds Delay-related Statistics

Adds delay-related statistics in the `stream.getStats` callback, including the delays from the publisher to the SD-RTN, from the SD-RTN to the subscriber, from end to end, and from sending to playing audio/video. 

#### 7. Support for Log Upload

Adds the `AgoraRTC.Logger.enableLogUplaod` method, which supports uploading the SDK’s log to Agora’s server. 

**Issues fixed**

- The `cameraId` and `microphoneId` settings do not take effect unless the `stream.setVideoProfile` method is called.
- Calling the `setScreenProfile` method does not take effect on the Firefox browser.
- An error occurs if both audio and the screen are enabled in the `client.createStream` method.
- The `microphoneId` setting does not take effect in screen sharing.
- The `audioProcessing` settings do not take effect.
- The `stream.play` method can be called repeatedly without calling the `stream.stop` method.

## v2.3.1

v2.3.1 is released on June 7, 2018. 

**Issues fixed**

- Occasional publishing failures on web browsers that installed the Adblock Plus extension.
- Invalid IP jumps.

## v2.3.0

v2.3.0 is released on June 4, 2018.

**New features**

#### 1. New Session Mode

To increase the application scenarios and improve interoperability with the Agora Native SDK, the `mode` and `codec` properties are added in the `createClient` method, in which `mode` includes `rtc` and `live`, while `codec` includes `vp8` and `h264`. 

#### 2. Support for Audio Gain Control

To meet customers’ needs for audio control during a communication or live broadcast session, the `audioProcessing` property is added in the `createStream` method.

#### 3. Support for Proxy on the Web Side

To enable enterprises with a company firewall to access Agora’s services, the `setProxyServer` and `setTurnServer` methods are added. By calling these methods, you can bypass the firewall and send the signaling messages and media data directly to the Agora SD-RTN through the servers. You need to deploy the Nginx and TURN servers before calling these methods before joining the channel.

#### 4. Support for Encryption

Encryption is supported to enhance security for communications or live broadcasts. Users need to set the encryption mode and password before joining a channel to use this function. 

**Issues fixed**

In the case of p2plost, the SDK stops reconnecting to the server after one or several reconnections.

## v2.2

v2.2 is released on April 16, 2018. 

**New features**

#### 1. Gets the version information.

Supports getting the version of the current Web SDK. 

#### 2. Sets the low-stream parameter.

Adds the `setLowStreamParameter` method to set the low-stream parameter. 

#### 3. Supports screen-sharing for the Firefox browser.

Adds the support for the Firefox browser to share the screen by adding the <code>mediaSource</code> property in the <code>createStream</code> method. See [Screen Sharing on Firefox](../../en/Interactive%20Broadcast/screensharing_web.md) for details.

#### 4. Supports the QQ browser.

Adds the support for the QQ browser.

**Issues fixed**

No voice in the voice-only mode when an iOS device joins a session using the Safari browser.

## v2.1.1

v2.1.1 is released on March 19, 2018. 

Fixes the issue of unable to view the remote video when using the Web SDK on the Firefox browser v59.01 on macOS.

## v2.1.0

v2.1.0 is released on March 7, 2018. 

**New features**

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Function</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Quality Detection</td>
<td>Adds callbacks to return the quality of the compatibility, microphone, camera, and network status.</td>
</tr>
<tr><td>Simulcast</td>
<td>Adds the function of controlling which stream type (high or low) to send or receive.</td>
</tr>
<tr><td>User-ban Notification</td>
<td>Adds a callback to notify a user of being banned from a channel.</td>
</tr>
<tr><td>RTMP Stream-Pushing</td>
<td>Adds a method to support RTMP stream-pushing.</td>
</tr>
<tr><td>Log Output Level Settings</td>
<td>Adds a method to set the log output level.</td>
</tr>
<tr><td>Mute/Unmute</td>
<td>Adds a method to mute or unmute users in a call or an interactive broadcast.</td>
</tr>
</tbody>
</table>



**Improvements**

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
<tr><td>P2P Connection Establishment</td>
<td>Shortens the time from 1.8 s to 500 ms.</td>
</tr>
<tr><td>Packet Loss</td>
<td>Optimizes the FEC packet loss and ULP FEC packet loss.</td>
</tr>
</tbody>
</table>



**Issues fixed**

- 90-degree video rotation on Safari
- Not seeing the remote video when using the Web SDK on Firefox v59.01 on macOS

## v2.0 and Earlier

### v2.0

v2.0 is released on November 21, 2017. 

#**New features**

- Adds the check web-browser compatibility function before calling the `CreateClient` method, to check the compatibility between the system and the web browser.
- Adds the callback to notify the app that a user has granted or denied access to the camera or microphone.
- Adds the video self-adjustment function, by automatically lowering the resolution or frame rate until the video profile matches the input hardware’s capability.

#**Issues fixed**

- Volume issues with iOS SDK interop.
- Disconnections on Chrome caused by prolonged communication.
- Abnormal screen display on Android devices when the device switches between front and rear cameras.
- Abnormal screen display on Android devices when one of the Android devices in the session leaves the channel.
- No camera-access-grant notification on Chrome when it joins the channel without access to a camera.
- No image on Safari on iOS after the user returns from another app.

### v1.14

v1.14 is released on October 20, 2017. 

Adds the screen-sharing function by modifying the `screen` property and adding the `extensionId` property in the <code>createStream</code> method.

### v1.13

v1.13 is released on September 4, 2017.

#**New features**

- Supports CDN Live with the `configPublisher` method.
- Supports Chrome for Android.

### v1.12 

v1.12 is released on July 25, 2017.

- Adds and updates the following APIs:

  <table>
  <colgroup>
  <col/>
  <col/>
  <col/>
  </colgroup>
  <thead>
  <tr><th>API</strong></th>
  <th>Type</strong></th>
  <th>Description</strong></th>
  </tr>
  </thead>
  <tbody>
  <tr><td><code>createClient</code></td>
  <td>New</td>
  <td>Creates a client object for web-only or web interop depending on which mode you set.</td>
  </tr>
  <tr><td><code>renewChannelKey</code></td>
  <td>New</td>
  <td>Updates a Channel Key when the previous Channel Key expires.</td>
  </tr>
  <tr><td><code>active-speaker</code></td>
  <td>New</td>
  <td>Indicates who is the active speaker in the current channel.</td>
  </tr>
  <tr><td><code>setRemoteVideoStreamType</code></td>
  <td>New</td>
  <td>Specifies the video stream type of the remote user to be received by the local user when the remote user sends dual streams.</td>
  </tr>
  <tr><td><code>setVideoProfile</code></td>
  <td>Update</td>
  <td>Sets the video profile. The default value is 480p_1.</td>
  </tr>
  <tr><td><code>init</code></td>
  <td>Update</td>
  <td>Initializes the client object.</td>
  </tr>
  <tr><td><code>join</code></td>
  <td>Update</td>
  <td>Allows a user to join an Agora channel.</td>
  </tr>
  </tbody>
  </table>

- Updates the error codes.

### v1.8.1

v1.8.1 is released on March 16, 2017.

Fixes the incompatibility issue on Chrome v57.

### v1.8

v1.8 is released on December 26, 2016. 

The Agora Web SDK supports both communication and live broadcast scenarios starting from v1.8. This release is a beta version.
