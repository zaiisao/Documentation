
---
title: Agora Platform Overview
description: 
platform: All Platforms
updatedAt: Wed Oct 30 2019 09:30:41 GMT+0800 (CST)
---
# Agora Platform Overview
Agora.io provides building blocks for you to add real-time voice and video communications through a simple and powerful SDK. You can integrate the Agora SDK to enable real-time communications in your own application quickly.

## Agora SDK

After integrating the Agora SDK, you can call different sets of APIs to implement voice/video communications in different scenarios. 

| Agora SDK  | Functions                   | Description                                                  |
| ---------- | ------------------------------------ | ------------------------------------------------------------ |
| Voice SDK  | [Voice Call](../../en/Voice/product_voice.md) <br>[Interactive Broadcast](../../en/Interactive%20Broadcast/product_live.md) | The Voice SDK package size is smaller than the Video SDK package size and applies to voice-only calls and voice-only live broadcasts.  |
| Video SDK  | [Video Call](../../en/Video/product_video.md) <br>[Interactive Broadcast](../../en/Interactive%20Broadcast/product_live.md) | Provides both voice and video functions. |
| Gaming SDK | [Interactive Gaming](../../cn/Interactive%20Gaming/product_gaming.md)                   | Optimized for gaming applications. The package size is about 1 MB. |
| Recording Add-on  | [Recording](../../en/Recording/product_recording.md)                     | Records and saves voice/video calls and live broadcasts on your server. |
| Signalling Add-on | [Signalling](../../en/Signaling/product_signaling.md)                    | Based on the TCP and provides a stable messaging channel for real-time communication scenarios. |

## Self-built Infrastructure

Agora's Software Defined Real-time Network (SD-RTN™) is a real-time transmission network built by Agora and is the only network infrastructure specifically designed for real-time communications in the world. All voice and video services provided by the Agora SDK are deployed and transmitted through the Agora SD-RTN™. 

Agora deploys about 200 data centers worldwide that use intelligent dynamic routing algorithms to achieve millisecond latency and ensure high availability of Agora's service.

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

## Self-developed Audio and Video Codecs

Agora is the only RTC service provider in the world using self-developed audio and video codecs. This allows Agora to have unique advantages in audio and video qualities.

### Audio

- High-fidelity, 3D surround sound experience
- 48 KHz full-band acquisition: Highly restored acoustic sound
- 3A algorithm based on machine learning: Echo cancellation, automatic gain, and noise suppression
- Audio enhancement: Stereo sound, 3D surround sound, sound localization, audio mixing, reverberation effects, in-ear monitoring, and voice changes

### Video

- Immersive visual experience

- Continuous network detection: Network detection before and after encoding, and network friendliness
- Dynamic network flow control: Maintains a dynamic balance of network bandwidth resources
- Highly efficient anti-loss coding products: Optimized coding algorithms and smooth video transmission that minimizes network impact
- Packet loss compensation: Automatically repairs content to ensure the best experience
- Visual enhancement: Image enhancement based on machine learning

## Developer Tools and Support

- The [Developer Center](https://docs.agora.io/en) provides documentation for developers to integrate and use Agora SDKs, and for SDK and sample code downloads.
- [Agora Console](https://dashboard.agora.io/) is a self-service system that enables developers to monitor usage statistics, track the QoE, manage projects, manage account privileges, and submit tickets.
- [Agora Github](https://github.com/AgoraIO) and [GitHub Community](https://github.com/AgoraIO-Community) provide demos and use cases, which can also be found at the [Developer Center](https://docs.agora.io/en/Agora%20Platform/sampleapps).
- 5 &times; 8 technical support. Developers can ask questions about integration on [Stack Overflow](https://stackoverflow.com/questions/tagged/agora.io), and [submit tickets](https://dashboard.agora.io/show-ticket-submission) for quality issues.
- [Agora Console](https://dashboard.agora.io/) provides [Agora Analytics](https://dashboard.agora.io/analytics/call/search) to track the QoE. Agora Analytics displays data of the call process in diagrams. For example:

  - Device status, including the system CPU usage and the app's CPU usage
  - User events. For example, stop sending audio or start receiving video
  - Bitrates of the sent/received audio and video
  - The freeze time in rendering the audio and video
  - The packet loss rates of the audio and video

  You can quickly see the QoE and identify the issues from the diagrams. [Check out](https://dashboard.agora.io/analytics/call/tutorial) how you can analyze your calls with Agora Analytics.
