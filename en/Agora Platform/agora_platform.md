
---
title: Agora Platform Overview
description: 
platform: All Platforms
updatedAt: Mon Jul 06 2020 11:16:19 GMT+0800 (CST)
---
# Agora Platform Overview
Agora.io provides building blocks for you to add real-time voice and video communications through a simple and powerful SDK. You can integrate the Agora SDK to enable real-time communications in your own application quickly.

## Agora SDK

After integrating the Agora SDK, you can call different sets of APIs to implement voice/video communications in different scenarios. 

| Agora SDK        | Functions                                                    | Description                                                  |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Voice SDK        | [Voice Call](../../en/Voice/product_voice.md) <br>[Live Interactive Audio Streaming](../../en/Audio%20Broadcast/product_live_audio.md) | The Voice SDK package size is smaller than the Video SDK package size and applies to voice-only calls and voice-only live interactive streaming. |
| Video SDK        | [Video Call](../../en/Video/product_video.md) <br>[Live Interactive Video Streaming](../../en/Interactive%20Broadcast/product_live.md) | Provides both voice and video functions.                     |
| Gaming SDK       | [Interactive Gaming](../../en/Interactive%20Gaming/product_gaming.md) | Optimized for gaming applications. The package size is about 1 MB. |
| RTM SDK          | [Real-time Messaging](../../en/Real-time-Messaging/product_rtm.md)   | Provides a stable messaging mechanism for real-time messaging scenarios that require low latency and high concurrency for a global audience. |
| Recording Add-on | [On-premise Recording](../../en/Recording/product_recording.md)<br/>[Cloud Recording](../../en/cloud-recording/product_cloud_recording.md) | Records and saves voice/video calls and live interactive streaming on your server. |

## Self-built infrastructure

SD-RTN™, or Software Defined Real-time Network, is a real-time transmission network built by Agora and is the only network infrastructure specifically designed for real-time communications in the world. 

All audio and video services provided by the Agora SDK are deployed and transmitted through the Agora SD-RTN™. Agora deploys over 250 data centers worldwide that use intelligent dynamic routing algorithms to achieve millisecond latency and ensure high availability of Agora's service.

| Feature                                         | Description                                                  |
| ----------------------------------------------- | ------------------------------------------------------------ |
| Global network coverage                         | <li>Covers 200+ countries and regions<li>Covers dozens of small and medium telecommunication providers in China |
| Mass access capability                          | <li>Supports multiple intelligent terminal access<li>A single channel can support a million people online at the same time |
| QoS (Quality of Service) capability enhancement | <li>Prevents network congestion in advance<li>Weak network anti-loss guarantee |
| QoS-based dynamic routing                       | <li>Comprehensive assessment of network resources<li>QoS optimal path guarantee |
| SLA (Service Level Agreement) guarantee         | <li>7 &times; 24 support, including ticketing system/IM/community<li>One-to-one VIP service |
| Global network reliability                      | <li>Global network availability at 99.999%<li>Invisible core business, such as anti-DDoS |
| Compatibility and Interoperability              | <li>Support for 6000+ devices <li> Support for mainstream web browsers, including Google Chrome, Safari, and Firefox<li>Support for iOS, Android, the Web, Windows, macOS, Electron, Linux, CoCos, Unity, and so on |
| UDP (User Datagram Protocol) optimization       | Optimizes multiple private protocols based on the UDP        |
| Self-developed audio and video codecs           | <li>Efficient use of network resources<li>Self-developed SOLO and NOVA codecs |
| Anti-packet-loss optimization                   | <li>Algorithm for optimizing anti-packet-loss mechanism under weak network conditions<li>Audio anti-packet-loss rate of 70% |

## Self-developed audio and video codecs

Agora is the only RTC service provider in the world using self-developed audio and video codecs. This allows Agora to have unique advantages in audio and video qualities.

### Audio

- High-fidelity, 3D surround sound experience
- A maximum audio sampling rate of 48 kHz for full band frequency: Highly restored acoustic sound
- 3A algorithm based on machine learning: Echo cancellation, automatic gain, and noise suppression
- Audio enhancement: Stereo sound, 3D surround sound, sound localization, audio mixing, reverberation effects, in-ear monitoring, and voice changes

### Video

- Immersive visual experience

- Continuous network detection: Network detection before and after encoding, and network friendliness
- Dynamic network flow control: Maintains a dynamic balance of network bandwidth resources
- Highly efficient anti-loss coding products: Optimized coding algorithms and smooth video transmission that minimizes network impact
- Packet loss compensation: Automatically repairs content to ensure the best experience
- Visual enhancement: Image enhancement based on machine learning

## Key concepts

Things you need to know before using the Agora SDK.

#### **Agora Console**

[Agora Console](https://dashboard.agora.io/) is a site for developers to manage Agora projects and services. 

Agora Console provides an intuitive interface for developers to make payments, query, manage and accomplish other operations when using Agora services. After registering an Agora account, developers can use the following main features:

- Manage the account
- Check and manage Agora projects and services
- Get an [App ID](#appid)
- Check call quality and usage
- Check billing and make payments
- Manage members and roles

Agora also provides [RESTful APIs](../../en/Agora%20Platform/dashboard_restful_communication.md), so developers can implement some of the above features directly, such as create a project and fetch usage numbers.

#### <a name="appid"></a>**App ID**

App ID is a random string created within [Agora Console](https://console.agora.io/) and is the unique identifier of an app. 

Agora uses App ID to identify each app and provides billing and other statistical data services based on it. After signing up within Agora Console, you can create multiple apps, each with a unique App ID. When initializing a client,you need to pass an App ID as an argument. Clients created by different App IDs cannot communicate with each other. See [Get an App ID](../../en/Agora%20Platform/token.md).

<div class="alert warning">For situations requiring high security, such as in a production environment, you must use a <a href="#key">token</a> for user authentication; otherwise, your environment is open to anyone who has your App ID.</div>

#### **App Certificate**

An App Certificate is a random string created within [Agora Console](https://console.agora.io/). It enables [token](#key) authentication and is one of the required arguments for generating a token.  See [Enable the App Certificate](../../en/Agora%20Platform/token.md).

#### <a name="key"></a>**Token**

A token, also known as a dynamic key, is used by the Agora server for dynamic authentication.

A token is used in situations requiring high security, such as in a production environment. You need a token for authentication when joining an RTC channel or when logging into the Agora RTM system. 

- For users of the Agora RTC SDK, Agora On-premise Recording SDK, or Agora Cloud Recording service, the Agora server uses a token to verify each user's App ID, privileges (for joining a channel and for publishing different types of streams), privilege expiration period, and token validity. 
- For users of the Agora RTM SDK, the Agora RTM server uses a token to verify each user's App ID, user ID, and token validity. 

See [Set up Authentication](../../en/Agora%20Platform/token.md) for details.


#### **Channel**

A channel is created by a developer calling the methods provided by Agora for transmitting real-time data.

Agora uses different channels to transmit different types of data. The RTC channel transmits audio or video data, while the RTM channel transmits messaging or signaling data. RTC channels and RTM channels are independent of each other.

Additional components provided by Agora, such as On-premise Recording and Cloud Recording, can join the RTC channel and provide real-time recording, transmission acceleration, media playback, and content moderation.  

Agora identifies channels by channel name. Users with the same channel name join the same channel and interact with each other. A channel no longer exists when the last user leaves the channel.

#### **Channel profile**

The SDK applies different optimization methods according to the channel profile. Agora supports the following channel  profiles:


| Channel Profile | Description |
| ---------------- | ---------------- |
| Communication     | One-on-one or group calls, where all users in the channel can talk freely.    |
| Live Interactive Streaming     | In a live interactive streaming channel, users have two client roles: [Host](#host) and [audience](#audience). The host sends and receives audio/video, and the audience receives audio/video with the sending function disabled.   |
| Gaming    | Any user in the channel can talk freely. This profile uses the codec with low-power consumption and low bitrate by default.   |

<div class="alert note">The gaming profile applies to the Agora Gaming SDK only.</div>


#### <a name="username"></a>**User ID**

A user ID (UID) identifies a user in the channel. Each user in the same channel should have a unique user ID.

#### **Stream**

A stream is an object that contains audio/video data. Users in a channel can [publish](#pub) the local stream and [subscribe](#sub) to the remote streams from other users.

#### <a name ="pub"></a>**Publish**

Publishing is the action of sending the user's audio and/or video data to the channel. Usually, the published object is a media stream created by the audio data sampled from a microphone and/or the video data captured by a camera. Developers can also publish media streams from other sources, including an online music file and the user's screen.

After publishing succeeds, the SDK continues sending media data to other users in the channel. By publishing one's stream and subscribing to others' streams, users have real-time voice or video calls with each other.


#### <a name ="sub"></a>**Subscribe**

Subscribing is the action of receiving media streams [published](#pub) to the channel. A user can receive other users' audio and/or video data by subscribing to their streams. A user can subscribe to one or more streams published by other users.

Developers can either directly play the subscribed streams or process them, for example by taking screenshots, or recording the streams.
	
## Developer tools and support

- The [Developer Center](https://docs.agora.io/en) provides documentation for developers to integrate and use Agora SDKs, and for SDK and sample code downloads.
- [Agora Console](https://dashboard.agora.io/) is a self-service system that enables developers to monitor usage statistics, track the QoE, manage projects, manage account privileges, and submit tickets.
- [Agora GitHub](https://github.com/AgoraIO) and [GitHub Community](https://github.com/AgoraIO-Community) provide demos and use cases, which can also be found at the [Developer Center](https://docs.agora.io/en/Agora%20Platform/sampleapps).
- 5 &times; 8 technical support. Developers can ask questions about integration on [Stack Overflow](https://stackoverflow.com/questions/tagged/agora.io), and [submit tickets](https://dashboard.agora.io/show-ticket-submission) for quality issues.
-  [Agora Analytics](https://console.agora.io/analytics/call/search), a tool that tracks and analyzes the usage and quality of calls. You can use this tool to locate quality issues, find root causes, and fix the issues to improve the final user experience. See [Agora Analytics Overview](../../en/Agora%20Platform/aa_guide.md) for more information.
