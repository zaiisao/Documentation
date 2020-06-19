
---
title: Glossary
description: 
platform: All Platforms
updatedAt: Fri Jun 19 2020 14:28:25 GMT+0800 (CST)
---
# Glossary
## A
#### <a name="agora-analytics"></a>**Agora Analytics**

Agora Analytics is a site for developers to track and analyze the usage and quality of calls.

Agora Analytics provides an intuitive interface where developers can locate quality issues across several dimensions, including call quality, call duration, and other factors. Developers can also discover root causes and fix them to improve user experience.

Agora also provides RESTful APIs for developers to retrieve the statistics of their calls and use the data in their own applications.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Agora%20Platform/aa_guide?platform=All%20Platforms">Agora Analytics Overview</a></li><li><a href="https://docs.agora.io/en/Agora%20Platform/aa_api?platform=All%20Platforms">Agora Analytics RESTful API</a></li></div>

#### <a name="console"></a>**Agora Console**

Agora Console is a site for developers to manage Agora projects and services.

Agora Console provides an intuitive interface for developers to make payments, query, manage and accomplish other operations when using Agora services. After registering an Agora account, developers can use the following main features:

- Manage the account
- Check and manage Agora projects and services
- Get an App ID
- Check call quality and usage
- Check billing and make payments
- Manage members and roles

Agora also provides RESTful APIs, so developers can implement some of the above features directly, such as create a project and fetch usage numbers.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Agora%20Platform/console_overview?platform=All%20Platforms">Agora Console Overview</a></li><li><a href="https://docs.agora.io/en/Agora%20Platform/dashboard_restful_communication?platform=All%20Platforms">Agora Console RESTful API</a></li></div>

#### <a name="agora-rtc-sdk"></a>**Agora RTC SDK**

Agora provides the Agora RTC (Real-time Communication) SDK for enabling real-time audio and video communications. By integrating the Agora RTC SDK, developers can add voice call, video call, audio broadcast, and video broadcast functions in their projects.

Based on the different functions and platforms, the RTC SDK is also categorized as follows:

| SDK | Platform | Feature |
| ---------------- | ---------------- | ---------------- |
| Voice SDK      | <ul><li>Native: Android, iOS, macOS, and Windows</li><li>Third-party framework: Unity</li></ul>      | <ul><li>Voice call</li><li>Audio broadcast</li></ul>      |
| Video SDK     | <ul><li>Native: Android, iOS, macOS, Web, and Windows</li><li>Third-party framework: Unity, Electron, React Native, and Flutter</li></ul>    | <ul><li>Voice call</li><li>Audio broadcast</li><li>Audio and video call</li><li>Audio and video broadcast</li></ul> |

RTC SDKs for Android, iOS, macOS, and Windows are known as RTC Native SDK. Agora maintains and releases repositories on GitHub for both [React Native](https://github.com/AgoraIO/React-Native-SDK) and [Flutter](https://github.com/AgoraIO/Flutter-SDK).

In addition to basic real-time audio and video communication, RTC SDK also supports advanced features such as audio mixing, screen sharing, modifying raw data, using external audio and video data, and pushing streams to the CDN.

Developers can use other Agora SDKs or services to implement the following:

- Enable real-time audio and video recording with the Agora On-premise Recording SDK or Cloud Recording service.
- Enable playback of online media resource playback with the Agora MediaPlayer Kit plug-in.
- Enable real-time messaging or signaling with the Agora RTM SDK.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms">Agora Voice Overview</a></li><li><a href="https://docs.agora.io/en/Video/product_video?platform=All%20Platforms">Agora Video Overview</a></li><li><a href="https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All%20Platforms">Agora Audio Broadcasting Overview</a></li><li><a href="https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms">Agora Video Broadcasting Overview</a></li>
</div>

#### <a name="agora-rtm-sdk"></a>**Agora RTM SDK**

You can use the Agora RTM (Real-time Messaging) SDK to implement real-time messaging scenarios that require low latency and high concurrency for a global audience.

Agora RTM SDK supports the following platforms:

- Android
- iOS
- macOS
- Linux Java
- Linux C++
- Windows C++
- Web

Agora RTM SDK is also known as RTM Native SDK for all of these platforms except for Web. 

You can use the Agora RTM SDK along with the Agora RTC SDK or a third-party audio-and-video SDK. The Agora RTM SDK can find uses in live streaming, social, education, and IoT scenarios. Integrating the Agora RTM SDK enables the following functions:

- Real-time commentaries
- Whiteboard
- Call invitation
- Privilege management
- Subscription
- Remote control.

<div class="alert info">References: <li><a href="#agora-rtc-sdk">Agora RTC SDK</a></li><li><a href="#rtm-native-sdk">RTM Native SDK</a></li>
</div>

#### <a name="appid"></a>**App ID**

App ID is a random string created within [Agora Console](https://console.agora.io/) and is the unique identifier of an app. 

Agora uses App ID to identify each app and provides billing and other statistical data services based on it. After signing up within Agora Console, you can create multiple apps, each with a unique App ID. When initializing a client, you need to pass an App ID as an argument. Clients created by different App IDs cannot communicate with each other. 

<div class="alert warning">For situations requiring high security, such as in a production environment, you must use a token for user authentication; otherwise, your environment is open to anyone who has your App ID.</div>

<div class="alert info">References: <li><a href="#console">Agora Console</a></li><li><a href="https://docs.agora.io/en/Agora%20Platform/token#getappid">Get an App ID</a></li><li><a href="https://docs.agora.io/en/Agora%20Platform/token#appcertificate">Enable the App Certificate</a></li><li><a href="https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token">Get a temporary token</a></li><li><a href="https://docs.agora.io/en/Interactive%20Broadcast/token_server_cpp">Get a token</a></li>
</div>

#### <a name="appcertificate"></a>**App Certificate**

App Certificate is a random string generated by Agora for enabling token authentication. It is one of the required arguments for generating a token.

Anyone who obtains a developer's App ID and App Certificate, legally or otherwise, can implement real-time audio and video functions in their apps. All the billings and statistics thus generated go to the developer's account. To avoid any possible loss, keep the App Certificate in a safe place. Agora recommends storing the App Certificate on the business sever, not on the client.

<div class="alert info">References: <a href="https://docs.agora.io/en/Agora%20Platform/token#appcertificate">Enable the App Certificate</a></div>

#### <a name="audience"></a>**audience**

In a live-broadcast channel, an audience is a group of users who can only subscribe to streams. The audience cannot publish streams. 

#### <a name="becoming-audience"></a>**audience (becoming)**

Becoming an audience describes a scenario within a live-broadcast channel (the channel profile is live broadcast) when a host switches the user role and becomes an audience.

This former host can no longer publish audio and video streams, and all users in the channel can no longer hear and see this former host.

<div class="alert info">Reference:
<li><a href="#channel_prpofile">channel profile</a></li>
<li><a href="#audience">audience</a></li>
<li><a href="#host">host</a></li>
	<li><a href="#becoming-host">host (becoming)</a></li>
<li><a href="#co-hosting">co-hosting</a></li>
</div>

#### <a name="audio-mixing"></a>**audio mixing**

Audio mixing is the process that developers mix multiple audio streams into only one audio stream. Common audio mixing scenarios are as follows:

- During real-time communication, when a user simultaneously speaks and plays a music file in a channel, after audio mixing, all users in the channel will hear the user's mixed voice.
- In a group call, any voice heard by the local user is the mixed audio stream of all remote users.
- When use composite recording mode to record the audio or video, the recorded file contains all users' audio streams after mixing.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/audio_effect_mixing_android?platform=Android">Audio Effects/Mixing</a></li><li><a href="https://docs.agora.io/en/cloud-recording/cloud_recording_composite_mode?platform=All%20Platforms">Composite Recording</a></li></div>

## C
#### <a name="cdn-streaming"></a>**CDN live streaming**

The process of publishing streams into the CDN (Content Delivery Network) is called CDN live streaming, where users can view the live broadcast through a web browser.

#### <a name="channel"></a>**channel**

A channel is created by a developer calling the methods provided by Agora for transmitting real-time data.

Agora uses different channels to transmit different types of data. The RTC channel transmits audio or video data, while the RTM channel transmits messaging or signaling data. RTC channels and RTM channels are independent of each other.

Additional components provided by Agora, such as On-premise Recording and Cloud Recording, can join the RTC channel and provide real-time recording, transmission acceleration, media playback, and content moderation.  

Agora identifies channels by channel name. Users with the same channel name join the same channel and interact with each other. A channel no longer exists when the last user leaves the channel.

To ensure communication security, users should provide a token for authentication when joining a channel. When the token expires, the user can no longer access Agora services. 

<div class="alert info">Reference:
<li><a href="https://docs.agora.io/en/Agora%20Platform/token">Set up authentication</a></li>
<li><a href="#agora-rtc-sdk">Agora RTC SDK</a></li>
<li><a href="#agora-rtm-sdk">Agora RTM SDK</a></li>	
</div>

#### <a name="channel-profile"></a>**channel profile**

The SDK applies different optimization methods according to the channel profile. Agora supports the following channel  profiles:

| Channel profile | Description | 
| ---------------- | ---------------- | 
| Communication     | One-on-one or group calls, where all users in the channel can talk freely.    | 
| Live Broadcast     | In a live broadcast channel, users have two client roles: [Host](#host) and [audience](#audience). The host sends and receives audio/video, and the audience receives audio/video with the sending function disabled.   | 
| Gaming    | Any user in the channel can talk freely. This profile uses the codec with low-power consumption and low bitrate by default.   | 

<div class="alert note">The gaming profile applies to the Agora Gaming SDK only.</div>

#### <a name="cloud-recording"></a>**Cloud Recording**
Cloud Recording is a component provided by Agora for recording and saving voice and video calls and interactive broadcasts on a third-party cloud storage through RESTful APIs.

Developers can quickly and flexibly record one-to-one or one-to-many voice and video calls or interactive broadcasts through integration. Unlike Agora On-premise Recording, Agora Cloud Recording does not require a Linux server. Developers can use Cloud Recording for scenarios such as online classrooms, interactive broadcasts, customer service, and more.

Cloud Recording RESTful APIs are compatible with the Agora RTC SDK. Cloud Recording supports two recording modes: [individual](https://docs.agora.io/en/cloud-recording/cloud_recording_individual_mode) and [composite](https://docs.agora.io/en/cloud-recording/cloud_recording_composite_mode).

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/cloud-recording/product_cloud_recording">Cloud Recording Overview</a></li></div>

#### <a name="co-hosting"></a>**co-hosting**

Co-hosting describes a scenario with more than one host. All hosts can interact with each other. The audience can hear and see these hosts.

There are two possible scenarios:

Co-hosting in a channel. It can either be multiple hosts joining a channel, or an audience switching user role and becoming a co-host when another host is already present.
Co-hosting across channels. The Agora RTC SDK supports relaying the media stream of a host in one channel to other channels. For details, see [Co-host across Channels](https://docs.agora.io/en/Interactive%20Broadcast/media_relay_android).
For the maximum number of co-hosts possible in a channel, see [How many users can Agora RTC SDK support at the same time?](https://docs.agora.io/en/faqs/capacity) A channel with too many co-hosts may cause network latency and packet loss. Refer to [Video for 7+ Users](https://docs.agora.io/en/Interactive%20Broadcast/multi_user_video_android) to ensure communication quality.

<div class="alert info">Reference:
<li><a href="#become-host">host (becoming)</a></li>
<li><a href="#become-audience">audience (becoming)</a></li>
<li><a href="#channel_prpofile">channel profile</a></li>
<li><a href="#host">host</a></li>
<li><a href="#audience">audience</a></li>
</div>

#### <a name="composite-recording"></a>**composite recording mode**

Composite recording mode generates a single mixed audio and video file for all UIDs in a channel. It is one of the recording modes for both On-premise Recording and Cloud Recording.

For example, if there are three UIDs sending audio and video in a channel, composite recording mode generates a single mixed audio and video file.

> On-premise recording also enables developers to generate a separate audio and video file for all UIDs in a channel, instead of combining audio and video into a single file.
> 
<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/cloud-recording/cloud_recording_composite_mode">Composite Recording</a> (Cloud Recording)</li><li><a href="https://docs.agora.io/en/Recording/recording_composite_mode">Composite Recording</a> (On-premise Recording)</li></div>

#### <a name="custom-rendering"></a>**custom rendering**

Custom rendering is the process where developers collect raw data from the SDK and process it according to specific needs.

When the default audio or video renderer cannot meet requirements, developers can use an external audio or video renderer to render raw data. Common custom rendering scenarios are as follows:

- When storing the raw data in other engine for rendering, developers need to use the custom rendering function.
- When rendering animation, developers can use the custom rendering function.
- To avoid any conflict between real-time communication and other processes that may be working in parallel, developers can use an external audio or video renderer to render raw data.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/custom_audio_android?platform=Android">Custom Audio Source and Renderer</a></li><li><a href="https://docs.agora.io/en/Interactive%20Broadcast/custom_video_android?platform=Android">Custom Video Source and Renderer</a></li></div>

## D
#### <a name="dual-stream"></a>**dual-stream mode**

In the dual-stream mode, the SDK transmits a high-quality and a low-quality video stream from the sender. The high-quality video stream has a higher resolution and bitrate, and the low-quality video stream has a lower resolution and bitrate.

Subscribing to low-quality streams improves communication continuity because this reduces bandwidth consumption. Developers can choose to receive low-quality video streams when network condition are unreliable, or when multiple users publish streams.

The SDK sets the default video profile of the low-quality video stream based on that of the high-quality video stream.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/multi_user_video_android?platform=Android">Video for 7+ Users</a></li>
<li><a href="#fallback">Stream fallback</a></li>
<li><a href="#high-stream">High-quality video stream</a></li>
<li><a href="#low-stream">Low-quality video stream</a></li>
</div>

## H

#### <a name="high-stream"></a>**high-quality video stream**

In the dual-stream mode, the SDK transmits two video streams of differing quality at the same time. The high-quality video stream has a higher resolution and bitrate than the low-quality video stream. See [dual-stream mode](#dual-stream) for details.

#### <a name="host"></a>**host**

In a live broadcast channel, the broadcasters or hosts are users who can publish and subscribe to streams.

#### <a name="becoming-host"></a>**host (becoming)**

Becoming a host describes a scenario within a live-broadcast channel (the channel profile is live broadcast) when an audience switches the user role and becomes a host (broadcaster).

If there is already a host in the channel, the new host becomes a co-host. This new host or co-host can publish audio and video streams. All users in the channel hear and see this co-host.

<div class="alert info">Reference:
<li><a href="#channel_prpofile">channel profile</a></li>
<li><a href="#audience">audience</a></li>
<li><a href="#becoming-audience">audience (becoming)</a></li>
<li><a href="#host">host</a></li>
<li><a href="#co-hosting">co-hosting</a></li>
</div>

## I
#### <a name="individual-recording"></a>**individual recording mode**
Individual recording mode records audio and video of each UID as separate files. It is one of the recording modes for both On-premise Recording and Cloud Recording.

For example, if there are three UIDs sending audio and video in a channel, individual recording mode generates three audio files and three video files.

Developers can use Agora's Audio & Video File Merging script to merge the audio and video files of each UID. See [Merge Audio and Video Files](https://docs.agora.io/en/cloud-recording/cloud_recording_merge_files) (Cloud Recording) or [Use the Transcoding Script](https://docs.agora.io/en/Recording/recording_merge_files) (On-premise Recording) for details.

 <div class="alert info">Reference:<li><a href="https://docs.agora.io/en/cloud-recording/cloud_recording_individual_mode">Individual Recording</a> (Cloud Recording)</li><li><a href="https://docs.agora.io/en/Recording/recording_individual_mode">Individual Recording</a> (On-premise Recording)</li></div>


#### <a name="inject-stream"></a>**Inject Online Media Stream**

"Inject Online Media Stream" refers to injecting an online media stream in a live-broadcast channel to share the stream with all users in the channel. The Agora RTC SDK provides a method for developers to inject an online mixed audio and video stream or an audio only stream to a channel.


<div class="alert info">Reference:<a href="https://docs.agora.io/en/Interactive%20Broadcast/inject_stream_android?platform=Android"> Inject Online Media Stream</a>
</div>

## L

#### <a name="low-stream"></a>**low-quality video stream**

In the dual-stream mode, the SDK transmits two video streams of differing quality at the same time. The low-quality video stream has a lower resolution and bitrate than the high-quality video stream. See [dual-stream mode](#dual-stream) for details.

## M
#### <a name="media-player"></a>**MediaPlayer Kit**

The MediaPlayer Kit is a plug-in of the Agora RTC SDK to play local and online media resources and publish the media streams to other users in a live-broadcast channel. Developers can use the media metadata, raw audio data, and raw video data of the media resources that the MediaPlayer Kit returns to implement custom functions.


<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/mediaplayer_release_android?platform=Android">MediaPlayer Kit Release Notes</a></li>
<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/mediaplayer_android?platform=Android">MediaPlayer Kit Guide</a></li>
</div>


## O
#### <a name="on-premise-recording"></a>**On-premise Recording**

On-premise Recording is a component provided by Agora for recording and saving voice and video calls and interactive broadcasts on a Linux server.

On-premise Recording enables developers to record, transfer and store audio and video data on a local server. For this reason, On-premise Recording is useful in high-security scenarios, such as surveillance and internal government or enterprise communications.

The On-premise Recording SDK is compatible with the Agora RTC SDK. It supports two recording modes: [Individual](https://docs.agora.io/en/Recording/recording_individual_mode) and [composite](https://docs.agora.io/en/Recording/recording_composite_mode). Developers can record communications either directly from the [command line](https://docs.agora.io/en/Recording/recording_cmd_cpp?platform=Linux%20CPP)or by [calling APIs](https://docs.agora.io/en/Recording/recording_api_cpp?platform=Linux). On-premise Recording offers additional advanced features such as adding [watermarks](https://docs.agora.io/en/Recording/recording_watermark_cpp) and [capturing screenshots](https://docs.agora.io/en/Recording/recording_screen_capture).

<div class="alert info">Reference:<ul><li><a href="https://docs.agora.io/en/Recording/product_recording">On-premise Recording Overview</a></li></ul></div>

## P
#### <a name="publish"></a>**publish**

Publishing is the action of sending the user's audio and/or video data to the channel. Usually, the published object is a media stream created by the audio data sampled from a microphone and/or the video data captured by a camera. Developers can also publish media streams from other sources, including an online music file and the user's screen.

After publishing succeeds, the SDK continues sending media data to other users in the channel. By publishing one's stream and subscribing to others' streams, users have real-time voice or video calls with each other.

<div class="alert info">Reference:
	<li><a href="#channel">Channel</a></li>
	<li><a href="#sub">Subscribe</a></li>
</div>

## R
#### <a name="raw-data"></a>**raw data**
Raw data, including raw audio data and raw video data, is the unprocessed data which developers can collect during real-time communication.

- Raw data collected from a sender is the unprocessed data coming directly from the data source such as microphone or camera.
- Raw data retrieved from a receiver is the data sent by remote users after decoding.

Generally, the data format of raw audio is PCM, and the data formats of raw video are RGB and YUV420.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/raw_data_audio_android?platform=Android">Raw Audio Data</a></li><li><a href="https://docs.agora.io/en/Interactive%20Broadcast/raw_data_video_android?platform=Android">Raw Video Data</a></li><li><a href="https://docs.agora.io/en/Recording/recording_raw_data?platform=Linux">Get the Raw Data</a></li></div>

#### <a name="render-first-video"></a>**render the first video frame**

Rendering the first video frame is the action of rendering the first video frame on the local device. There are two scenarios:

- Rendering the first local video frame: The SDK renders the first video frame captured by the local camera on the local video view of the local device.
- Rendering the first remote video frame: The SDK renders the first video frame received from a remote user on the remote video view of the local device.

The time elapsed from the time when the local or remote user joins a channel to the time when the SDK renders the first local or the first remote video frame is correlated with the user's hardware and software capacity. Agora strives to reduce the time elapsed so that an application can render the video to less than a second.

#### <a name="rtm-native-sdk"></a>**RTM Native SDK**

Agora RTM SDK for the following platforms is also known as RTM Native SDK:

- Android
- iOS
- macOS
- Linux Java
- Linux C++
- Windows C++.

<div class="alert info">Reference: <a href="#agora-rtm-sdk">Agora RTM SDK</a>
</div>

## S
#### <a name="sd-rtn"></a>**SD-RTN™**

SD-RTN™, or Software Defined Real-time Network, is a real-time transmission network built by Agora and is the only network infrastructure specifically designed for real-time communications in the world. 

All audio and video services provided by the Agora SDK are deployed and transmitted through the Agora SD-RTN™. Agora deploys over 250 data centers worldwide that use intelligent dynamic routing algorithms to achieve millisecond latency and ensure high availability of Agora's service.

#### <a name="stream"></a>**stream**

A stream is an object that contains audio and/or video data. Users in a channel can [publish](#pub) the local stream and [subscribe](#sub) to the remote streams from other users.

#### <a name="fallback"></a>**stream fallback**

In scenarios where multiple users engage in real-time audio and video communication, user experience can be impaired if the network condition is too poor to guarantee both audio and video at the same time.

Stream fallback minimizes the impact of poor network connections by ensuring that some form of communication continues between the sender and receiver. Audio and video streams change to audio-only, or from high-quality to low-quality video.

If the network connection improves, the low-quality video stream or audio-only stream return to the original quality.

<div class="alert info">Reference:<li><a href="#dual-stream">Dual-stream mode</a></li><li><a href="#high-stream">High-quality stream</a></li><li><a href="#low-stream">Low-quality stream</a></li><li><a href="https://docs.agora.io/en/Interactive%20Broadcast/fallback_android?platform=Android">Video stream fallback</a></li></div>

#### <a name="sub"></a>**subscribe**

Subscribing is the action of receiving media streams [published](#pub) to the channel. A user can receive other users' audio and/or video data by subscribing to their streams. A user can subscribe to one or more streams published by other users.

Developers can either directly play the subscribed streams or process them, for example by taking screenshots, or recording the streams.

<div class="alert info">Reference:
	<li><a href="#channel">Channel</a></li>
	<li><a href="#pub">Publish</a></li>
</div>

## T
#### <a name="token"></a>**token**

A token, also known as a dynamic key, is used in situations requiring high security, such as in a production environment. You need a token for authentication when joining an RTC channel or when logging into the Agora RTM system. 

- For users of the Agora RTC SDK, Agora On-premise Recording SDK, or Agora Cloud Recording service, the Agora server uses a token to verify each user's App ID, privileges (for joining a channel and for publishing different types of streams), privilege expiration period, and token validity. 
- For users of the Agora RTM SDK, the Agora RTM server uses a token to verify each user's App ID, user ID, and token validity. 

In a production environment, an app user must generate a token on a business server and pass the token when joining an RTC channel or when logging in the Agora RTM system. If you are still testing your app or do not wish to take the trouble of setting up your own business server for generating tokens, have [Agora Console](https://console.agora.io/) generate a temporary token for you when creating your project. In this case, a temporary token suffices because it has the same function. 

Each token has a privilege expiration period and a validity period. The privilege expiration period is when the user privilege expires; the validity period is when the token itself expires (24 hours by default). 

Users of the Agora RTC SDK, Agora On-premise SDK, or Agora Cloud Recording service are kicked out of the channel immediately when their token expires. Users of the Agora RTM SDK are not immediately kicked out of the Agora RTM system but will not be able to log in the RTM system the next time their client connects to the system. Therefore, when notified that your privilege will soon expire or that the token validity has expired, generate a new token at your earliest convenience and save it for the next channel-join or login. 

<div class="alert info">References: <li><a href="https://docs.agora.io/en/Agora%20Platform/token#getappid">Get an App ID</a></li><li><a href="https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token">Get a temporary token</a></li><li><a href="https://docs.agora.io/en/Interactive%20Broadcast/token_server_cpp">Generate a token for Agora RTC SDK, On-premise Recording, or Cloud Recording</a></li><li><a href="https://docs.agora.io/en/Real-time-Messaging/rtm_token">Generate a token for Agora RTM SDK</a></li>
</div>

#### <a name="transcoding"></a>**transcoding**

Transcoding is used in CDN live streaming when multiple hosts are in the channel.

In CDN live streaming, the audio and video streams sent to the SD-RTN™ are transferred into RTMP (Real-Time Messaging Protocol) protocol and pushed to the CDN. If there are multiple hosts, their streams need to be combined into a single stream by transcoding. 

Transcoding sets the audio/video profiles and the picture-in-picture layout for the stream to be pushed to the CDN.

<div class="alert note">Agora does not recommend transcoding in the case of a single host.</div>

## U
#### <a name="username"></a>**user ID**

A user ID identifies a user in the channel. Each user in the same channel should have a unique user ID.

## V
#### <a name="layout"></a>**video layout**
Video layout arranges the display of users when multiple users are mixed into one stream, such as in CDN live streaming or a composite recording. Video layout sets the relative size and position of each user on the canvas. It is sometimes referred to as **picture-in-picture** layout.

In the following image, the background of the video is the canvas, and each user occupies a region on the canvas.

![img](https://web-cdn.agora.io/docs-files/1577697787996)

Developers may have to set video layout when using Cloud Recording, On-premise Recording, or pushing streams to CDN.

- Recording: When using On-premise Recording or Cloud Recording to record in the composite recording mode, developers can choose from three predefined video layouts: floating, best fit, and vertical. Developers can also customize the video layout by setting the size and position of each user's region on the canvas.

- Push Streams to CDN: When multiple hosts are in a CDN live streaming channel, transcoding combines the streams of all hosts into a single stream. Use `TranscodingUser` to set the video layout for each user.

<div class="alert info">Reference:<li><a href="https://docs.agora.io/en/cloud-recording/cloud_recording_layout">Set Video Layout</a>（Cloud Recording）</li><li><a href="https://docs.agora.io/en/Recording/recording_layout">Set Video Layout</a>（On-premise Recording）</li><li><a href="https://docs.agora.io/en/Audio%20Broadcast/cdn_streaming_apple">Push Streams to CDN</a></li></div>

#### <a name="video-profile"></a>**video profile**

The video profile refers to a set of video attributes, such as resolution, bitrate, and frame rate. It is also known as "video encoder configurations". Agora provides methods for developers to set the resolution, bitrate, frame rate, orientation mode, and other attributes under ideal network conditions when encoding the local video. Once encoded, the local video streams to the remote users in the channel. The settings of the video profile affect what the remote users see.

<div class="alert info">Reference:<a href="https://docs.agora.io/en/Interactive%20Broadcast/video_profile_android?platform=Android"> Set the Video Profile</a>
</div>
