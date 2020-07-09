
---
title: Glossary
description: 
platform: All Platforms
updatedAt: Mon Jul 06 2020 11:44:36 GMT+0800 (CST)
---
# Glossary
## A
#### <a name="agora-analytics"></a>[**<u>Agora Analytics</u>**](../../en/Agora%20Platform/agora_analytics.md)

Agora Analytics is a site for developers to track and analyze the usage and quality of calls.

#### <a name="agora-cloud-backup"></a>[**<u>Agora Cloud Backup</u>**](../../en/Agora%20Platform/agora_cloud_backup.md)

Agora Cloud Backup is a backup cloud storage service used in Cloud Recording. If the recording service cannot upload the recorded files to the specified third-party cloud storage, then the service automatically and temporarily stores them in the backup cloud.

#### <a name="console"></a>[**<u>Agora Console</u>**](../../en/Agora%20Platform/agora_console.md)

Agora Console is a site for developers to manage Agora projects and services.

#### <a name="agora-rtc-sdk"></a>[**<u>Agora RTC SDK</u>**](../../en/Agora%20Platform/term_agora_rtc_sdk.md)

Agora provides the Agora RTC (Real-time Communication) SDK for enabling real-time audio and video communications. 

#### <a name="agora-rtm-sdk"></a>[**<u>Agora RTM SDK</u>**](../../en/Agora%20Platform/term_agora_rtm_sdk.md)

You can use the Agora RTM (Real-time Messaging) SDK to implement real-time messaging scenarios that require low latency and high concurrency for a global audience.

#### <a name="appid"></a>[**<u>App ID</u>**](../../en/Agora%20Platform/term_appid.md)

An App ID is a randomly generated string provided by Agora and is the unique identifier of an app.

#### <a name="appcertificate"></a>[**<u>App Certificate</u>**](../../en/Agora%20Platform/term_app_certificate.md)

An App Certificate is a randomly generated string provided by Agora for enabling token authentication. It is one of the required arguments for generating a token.

#### <a name="audience"></a>**audience**

The audience are users who do not have streaming permissions in the channel. The audience can only subscribe to the remote audio and video streams, but cannot publish the audio and video streams. For more information, see [user role](#user_role).

#### <a name="becoming-audience"></a>[**<u>audience (becoming)</u>**](../../en/Agora%20Platform/becoming_an_audience.md)

Becoming an audience describes a scenario within a live interactive streaming channel (the channel profile is Live-Broadcast) when a host switches the user role and becomes an audience.

#### <a name="audio-mixing"></a>[**<u>audio mixing</u>**](../../en/Agora%20Platform/audio_mixing.md)

Audio mixing means combining multiple audio streams into one.

#### <a name="audio-profile"></a>[**<u>audio profile</u>**](../../en/Agora%20Platform/audio_profile.md)

An audio profile includes the sample rate, encoding scheme, number of channels, and bitrate for encoded audio data.

#### <a name="audio-route"></a>[**<u>audio route</u>**](../../en/Agora%20Platform/term_audio_route.md)

The audio route is the pathway audio data takes through audio hardware components during playback.

## C

#### <a name="callee"></a>**callee**

A callee is an RTM user who receives a [call invitation](#call-invitation).

#### <a name="caller"></a>**caller**

A caller is an RTM user who sends a [call invitation](#call-invitation).

#### <a name="call-invitation"></a>**call invitation**

Call invitation is a communication protocol based on the peer-to-peer messaging functionality of the Agora RTM SDK. Call invitation supports starting, ending, accepting, and refusing calls. For more information, see [Call Invitation](../../en/Real-time-Messaging/rtm_invite_android.md).

#### <a name="channel"></a>[**<u>channel</u>**](../../en/Agora%20Platform/channel.md)

A channel is created by a developer calling the methods provided by Agora for transmitting real-time data.

#### <a name="channel-attribute"></a>[**<u>channel attribute</u>**](../../en/Agora%20Platform/channel_attribute.md)

Channel attributes are tags added to RTM channels, including the property name, property value, the ID of the last RTM user who updated the attribute, and the time of the last update.

#### <a name="channel-message"></a>[**<u>channel message</u>**](../../en/Agora%20Platform/channel_message.md)

A channel message is a message that an RTM user sends to all RTM users in a channel.

#### <a name="channel-profile"></a>[**<u>channel profile</u>**](../../en/Agora%20Platform/channel_profile.md)

The channel profile is a configuration that Agora uses to apply optimized algorithms for different real-time scenarios.

#### <a name="cloud-proxy"></a>[**<u>Cloud Proxy</u>**](../../en/Agora%20Platform/term_cloud_proxy.md)

Cloud Proxy is a proxy service that enables users to connect to Agora services through a firewall by using fixed IP addresses.

#### <a name="cloud-recording"></a>[**<u>Cloud Recording</u>**](../../en/Agora%20Platform/cloud_recording.md)

Cloud Recording is a component provided by Agora for recording and saving voice and video calls and interactive streaming on a third-party cloud storage through RESTful APIs.

#### <a name="co-hosting"></a>[**<u>co-hosting</u>**](../../en/Agora%20Platform/co_hosting.md)

Co-hosting describes a scenario with more than one host. 

#### <a name="composite-recording"></a>[**<u>composite recording mode</u>**](../../en/Agora%20Platform/composite_recording_mode.md)

Composite recording mode generates a single mixed audio and video file for all UIDs in a channel. 

#### <a name="custom-rendering"></a>[**<u>custom rendering</u>**](../../en/Agora%20Platform/custom_rendering.md)

Custom rendering is the process where developers collect raw data from the SDK and process it according to specific needs.

#### <a name="custom-source"></a>[**<u>custom source</u>**](../../en/Agora%20Platform/custom_source.md)

Custom source is the process where an app captures raw data by itself.

## D

#### <a name="delay"></a>[**<u>delay</u>**](../../en/Agora%20Platform/delay.md)

In real-time audio and video communication, delay refers to the time elapsed from when the data is sent to when it is received.

#### <a name="dual-stream"></a>[**<u>dual-stream mode</u>**](../../en/Agora%20Platform/dual_stream_mode.md)

In the dual-stream mode, the SDK transmits a high-quality and a low-quality video stream from the sender. 

## F

#### <a name="freeze"></a>[**<u>freeze</u>**](../../en/Agora%20Platform/freeze.md)

Freeze refers to choppy audio or video playback caused by a poor network connection or limited device performance during real-time audio and video communication.

## H

#### <a name="high-stream"></a>**high-quality video stream**

In dual-stream mode, the SDK transmits two video streams of differing quality at the same time. See [dual-stream mode](#dual-stream) for details.

#### <a name="host"></a>[**<u>host</u>**](../../en/Agora%20Platform/term_host.md)

The host refers to a user who has streaming permissions in the channel.

#### <a name="becoming-host"></a>[**<u>host (becoming)</u>**](../../en/Agora%20Platform/becoming_a_host.md)

Becoming a host describes a scenario within a live interactive streaming channel (the channel profile is Live-Broadcast) when an audience switches the user role and becomes a host.

## I

#### <a name="individual-recording"></a>[**<u>individual recording mode</u>**](../../en/Agora%20Platform/individual_recording_mode.md)

Individual recording mode records audio and video of each UID as separate files.

#### <a name="inject-stream"></a>**Inject Online Media Stream**

"Inject Online Media Stream" refers to injecting an online media stream in a live interactive streaming channel to share the stream with all users in the channel. The Agora RTC SDK provides a method for developers to inject an online mixed audio and video stream or an audio only stream to a channel. For details, see [*Inject Online Media Stream*](https://docs.agora.io/en/Interactive%20Broadcast/inject_stream_android).

## J
#### <a name="jitter"></a>[**<u>jitter</u>**](../../en/Agora%20Platform/jitter.md)

In real-time audio and video communication, jitter is the variation in the delay of data packets transmitted continuously on the network.

## L

#### <a name="last-mile"></a>[**<u>last mile</u>**](../../en/Agora%20Platform/last_mile.md)

The last mile refers to the network between the Agora edge server and the end user's device.

#### <a name="loopback-test"></a>**loopback test**

A loopback test sends a signal from a communication device and is then returned (looped back) to it. It is often used to determine whether a device is working properly. For more information, see [Test a media device](https://docs.agora.io/en/Interactive%20Broadcast/test_switch_device_android).

#### <a name="low-stream"></a>**low-quality video stream**

In dual-stream mode, the SDK transmits two video streams of differing quality at the same time. The low-quality video stream has a lower resolution and bitrate than the high-quality video stream. See [dual-stream mode](#dual-stream) for details.

## M
#### <a name="media-player"></a>[**<u>MediaPlayer Kit</u>**](../../en/Agora%20Platform/mediaplayer_kit.md)

The MediaPlayer Kit is a plug-in of the Agora RTC SDK to play local and online media resources and publish the media streams to other users in a live interactive streaming channel. 

#### <a name="media-stream"></a>[**<u>media stream</u>**](../../en/Agora%20Platform/media_stream.md)

A media stream is an object that contains media data.

#### <a name="mirror"></a>[**<u>mirror</u>**](../../en/Agora%20Platform/mirror.md)

Mirroring is an effect that a video image renders.

## O

#### <a name="offline"></a>[**<u>offline</u>**](../../en/Agora%20Platform/offline.md)

Offline describes the status of an RTM user who has successfully logged out of the Agora RTM system.

#### <a name="offline-message"></a>[**<u>offline message</u>**](../../en/Agora%20Platform/offline_message.md)

An offline message is a peer-to-peer message that an online RTM user sends to an offline RTM user.

#### <a name="online"></a>[**<u>online</u>**](../../en/Agora%20Platform/online.md)

Online describes the status of a user who has successfully logged in to the Agora RTM system or stays disconnected from the Agora RTM system for more than 30 seconds.

#### <a name="on-premise-recording"></a>[**<u>On-premise Recording</u>**](../../en/Agora%20Platform/on_premise_recording.md)

On-premise Recording is a component provided by Agora for recording and saving voice and video calls and interactive streaming on a Linux server.

## P
#### <a name="packet-loss"></a>[**<u>packet loss</u>**](../../en/Agora%20Platform/packet_loss.md)

Packet loss refers to the data packets transmitted on the network failing to arrive at their intended destination.

#### <a name="peer-to-peer-message"></a>[**<u>peer-to-peer message</u>**](../../en/Agora%20Platform/peer_to_peer_message.md)

A peer-to-peer message is a message that an online RTM user sends to an online or offline user.

#### <a name="publish"></a>[**<u>publish</u>**](../../en/Agora%20Platform/publish.md)

Publishing is the action of sending the user's audio and/or video data to the channel. 

#### <a name="push-stream-cdn"></a>[**<u>Push streams to CDN</u>**](../../en/Agora%20Platform/push-stream-cdn.md)

Pushing streams to CDN (Content Delivery Network) means that the Agora server converts the stream in the RTC channel from the Agora protocol to the standard protocol, and pushes the stream to a third-party CDN.

## R
#### <a name="raw-data"></a>[**<u>raw data</u>**](../../en/Agora%20Platform/raw_data.md)

Raw data, including raw audio data and raw video data, is the unprocessed data which developers can collect during real-time communication.

#### <a name="render-first-video"></a>[**<u>render the first video frame</u>**](../../en/Agora%20Platform/render_the_first_video_frame.md)

Rendering the first video frame is the action of rendering the first video frame on the local device. 

## S
#### <a name="sd-rtn"></a>[**<u>SD-RTN™</u>**](../../en/Agora%20Platform/sd_rtn.md)

SD-RTN™, or Software Defined Real-time Network, is a real-time transmission network built by Agora and is the only network infrastructure specifically designed for real-time communications in the world. 

#### <a name="slice"></a>**slice**

Slicing means cutting recorded audio or video into separate files according to specific rules. During an Agora Cloud Recording, the recording service cuts the streams and generates multiple slice files (TS or WebM files) and M3U8 files that serve as a playlist of the slice files. See [Manage Recorded Files](https://docs.agora.io/en/cloud-recording/cloud_recording_manage_files?platform=All%20Platforms).

#### <a name="sound-localization"></a>[**<u>sound localization</u>**](../../en/Agora%20Platform/sound_localization.md)

Sound localization means determining the distance to and direction of a sound through hearing the difference of volume, time, and timbre between users' ears.

#### <a name="fallback"></a>[**<u>stream fallback</u>**](../../en/Agora%20Platform/stream_fallback.md)

In scenarios where multiple users engage in real-time audio and video communication, user experience can be impaired if the network condition is too poor to guarantee both audio and video at the same time.

#### <a name="stream-mixing"></a>[**<u>stream mixing</u>**](../../en/Agora%20Platform/stream_mixing.md)

Stream mixing means combining multiple media streams into one. It may include the mixing of video streams (video mixing) and audio streams (audio mixing).

#### <a name="sub"></a>[**<u>subscribe</u>**](../../en/Agora%20Platform/subscribe.md)

In the Agora RTC SDK, subscribing is the action of receiving media streams published to the channel. In the Agora RTM SDK, subscribing is the action of monitoring the online status of one or multiple RTM users.

## T
#### <a name="token"></a>[**<u>token</u>**](../../en/Agora%20Platform/term_token.md)

A token, also known as a dynamic key, is used for authentication when an app user joins an RTC channel or logs onto the Agora RTM system.


#### <a name="transcoding"></a>[**<u>transcoding</u>**](../../en/Agora%20Platform/transcoding.md)

Transcoding is the process of decoding audio and video data and then re-encoding them into the target conversion output or format.

## U

#### <a name="user-attribute"></a>[**<u>user attribute</u>**](../../en/Agora%20Platform/user_attribute.md)

User attributes are tags added to RTM users, including property names and property values.

#### <a name="username"></a>**user ID**

In the Agora RTC SDK, a user ID identifies a user in the RTC channel. In the Agora RTM SDK, a user ID identifiers a user in the RTM system. The user ID in the Agora RTC SDK and the Agora RTM SDK are independent from each other.

#### <a name="user-role"></a>[**<u>user role</u>**](../../en/Agora%20Platform/user_role.md)

The type of user role determines whether the user in the channel has streaming permissions.

## V
#### <a name="layout"></a>[**<u>video layout</u>**](../../en/Agora%20Platform/video_layout.md)

Video layout arranges the display of users when multiple users are mixed into one stream, such as in CDN live streaming or a composite recording. 

#### <a name="video-mixing"></a>[**<u>video mixing</u>**](../../en/Agora%20Platform/video_mixing.md)

Video mixing means combining multiple video streams into one.

#### <a name="video-profile"></a>[**<u>video profile</u>**](../../en/Agora%20Platform/video_profile.md)

The video profile refers to a set of video attributes, such as resolution, bitrate, and frame rate. 
