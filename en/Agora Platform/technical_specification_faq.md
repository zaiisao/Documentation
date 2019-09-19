
---
title: Technical Specification
description: 
platform: All Platforms
updatedAt: Thu Sep 19 2019 01:41:18 GMT+0800 (CST)
---
# Technical Specification
## Platform and Scale

#### What platforms does Agora support?

Agora supports Android, iOS, macOS, Windows, and the Web.

### How many users can be supported at the same time on each of Agora's products? 

<table>
  <tr>
    <th>Function</th>
    <th>Scale</th>
    <th>Aggregate Resolution</th>
  </tr>
  <tr>
    <td>Communication (Voice/Video)</td>
    <td>Android, iOS, macOS, Windows, the Web</td>
    <td>Voice Communication: Up to 10,000 users in a channel.<br>Video Communication: Up to seven users in a channel (a 17-user scenario is under development).<br>Number of Channels: No limit.</td>
  </tr>
  <tr>
    <td>Live Broadcast</td>
    <td>Android, iOS, macOS, Windows, the Web</td>
    <td>Hosts: Up to seven video hosts in a channel; 10,000 voice hosts in a channel (a scenario with more users is under development).<br>Audience: Up to 1,000,000 people in a channel.<br>Number of Channels: No limit.<br></td>
  </tr>
  <tr>
    <td>Signaling</td>
    <td>Android, iOS, macOS, Windows, the Web</td>
    <td>Number of Users: Up to 100,000 users in a channel.<br>Number of Channels: No limit.</td>
  </tr>
</table>

### What programming languages does Agora support?

* Android: Java, C
* iOS/macOS: Objective-C, Swift
* Windows: C++
* Web: JavaScript
* Cocos: C++
* Unity: C#

For other requirements, contact sales-us@agora.io.

### What is the supported audio-capture sampling rate on each platform?

You can select one of the following sampling rates in Windows (including Windows systems built on a Mac): 48000, 44100, 16000, 96000, 32000, 8000, 192000, 88200, and 176400.

Mac systems do not have any limitation on the supported sampling rate of the audio devices and do not require selecting any sampling rate.

### What transport protocol does Agora use for the user-related logic?

Agora uses a private application layer protocol based on UDP.

### Is there an Eclipse edition of the Android demo?

No, the Eclipse development environment is no longer supported. The Android demo only supports Android Studio.

## Codec

#### How are Agora's codecs unique？

Agora uses a proprietary audio codec, NOVA, which is simple, low bitrate, and highly adaptive. NOVA ranks in the industry’s highest quality level (on average, the score of broadband PESQ-WB is 3.92, and the score of the average narrowband PESQ in 4-Kbps low-bitrate mode is 3.21). NOVA supports narrowband, broadband, and ultra-high frequency, all in one. The bitrate varies in the range between 2 Kbps and 24 Kbps (2 Kbps for ultra-low bitrate communications).

NOVA flexibly switches between different modes, including adaptable, high-quality, low energy consumption, super high frequency, and ultra-low bitrate.

Agora uses a proprietary video codec, and the performance in the RTC environment is excellent with low latency and no jitter.

#### Which voice codecs does Agora support？

Agora supports the following voice codecs:

* Third-party ultra-wideband (SWB) and full-band codecs：AAC-ELD and OPUS.
* Third-party broadband (WB) and narrowband (NB) codecs.
* Other codecs (ITU G.7xx series, SILK, iLBC, and iSAC).

## Performance

### Does Agora provide echo cancellation?

Yes, Agora's superior echo cancellation has the following characteristics:

* Simple and efficient.
* Super high-quality support.
* Quick convergence and high echo return loss enhancement (ERLE).
* Optimization for high- and low-end phones.
* Device-specific parameter configuration.

### Will a call be interrupted when switching networks, such as from a Wi-Fi to a cellular network?

A short interruption may occur after switching networks during a call. The call continues after the network reconnects.

### How much bandwidth does the Agora SDK require?

Depending on the network conditions:

* If the Wi-Fi connection is strong, Agora increases the bandwidth allocation to ensure an optimal user experience.
* In a cellular network, the Agora algorithm automatically lowers the bandwidth allocation according to the network conditions to allow quality calls.

Generally speaking, the bandwidth allocation ranges between 4 Kbps and 13 Kbps (between 4 Kbps and 6 Kbps in a cellular network). In some cases, the bandwidth allocation can be as low as 1 Kbps.

### How much bandwidth does a call use？

You use bandwidth whenever you access the Internet in a voice or video call. Compared with similar products, the bandwidth used by Agora products is very low:

* Voice Call: 5 Kbps to 10 Kbps.
* Video Call: The bitrate changes according to the resolution and frame rate. In general terms (for example without taking color depth into account), if the resolution is 180 by 320 pixels, and the frame rate is 15 fps, then the bitrate is 160 Kbps.

### What percentage of CPU resources does the Agora SDK typically use?

Depending on the phone modes, operating systems, and CPU chips, the minimum CPU usage is 3%. For the latest high-end mobile phones, the CPU usage may be 20% or more. However, Agora can optimize the CPU usage according to your needs.


### How can Agora ensure that the data transmission is smooth without any data loss and with minimal latency?

* Agora's patented NOVA codec is simple, low bitrate, and highly adaptive.
* Users automatically access the Agora Global Virtual Network in poor network conditions.

###  Does Agora support automatic reconnection?

Yes.

### Why is my phone hot after being in a call for a while?

Your phone performs software processing in a voice or video call, which uses lots of CPU resources and causes your phone to heat up. It is normal for the phone to heat up after a call or playing videos for a long time with an app.

### How can it take only 30 minutes to implement voice and video communications?

Agora SDKs have all the required voice/video functions ready to go. Developers need only four lines of code for integration.

