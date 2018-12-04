
---
title: Technical Specification
description: 
platform: Technical Specification
updatedAt: Wed Nov 28 2018 07:52:04 GMT+0000 (UTC)
---
# Technical Specification
## Platform and Scale

#### What platforms does Agora support?

Android, iOS, macOS, Windows, and the web.

### How many users can the Agora products support simultaneously?

<table>
  <tr>
    <th>Function</th>
    <th>Scale</th>
    <th>Aggregate Resolution</th>
  </tr>
  <tr>
    <td>Communication(Voice/Video)</td>
    <td>Android, iOS, macOS, Windows, the web</td>
    <td>Voice Communication: Up to 10,000 users in a channel.<br>Video Communication: Up to seven users in a channel (a 17-user scenario is under development).<br>Number of Channels: No limit.</td>
  </tr>
  <tr>
    <td>Live Broadcast</td>
    <td>Android, iOS, macOS, Windows, the web</td>
    <td>Hosts: Up to seven video hosts in a channel; 10,000 voice hosts in a channel (a scenario with more users is under development).<br>Audience: Up to 30,000 people in a channel.<br>Number of Channels: No limit.<br></td>
  </tr>
  <tr>
    <td>Signaling</td>
    <td>Android, iOS, macOS, Windows, the web</td>
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

### What is the supported scope of the audio-capture sampling rate on each platform?

You can select one of the following sampling rates in Windows (including Windows systems built on a Mac): 48000, 44100, 16000, 96000, 32000, 8000, 192000, 88200, and 176400.

Mac systems do not have any limitation on the supported sampling rate of the audio devices, and do not require selecting any sampling rate.

### What transport protocol is the user-related logic based on?

We use a private application layer protocol based on UDP.

### Does the current Android demo have an Eclipse edition?

No. Due to the Eclipse development environment is outmoded, the current demo does not support Eclipse, but only Android Studio.

## Codec

#### What makes codecs used by Agora unique？

Agora uses its proprietary audio codec, NOVA, which is simple, low bitrate, and highly adaptive. It ranks in the industry’s highest quality level (on average, the score of broadband PESQ-WB is 3.92, and the score of the average narrowband PESQ in 4-kpbs low-bitrate mode is 3.21). NOVA supports narrowband, broadband, and ultra-high frequency, all in one. The bitrate can vary in the range of 2 kbps to 24 kbps (2 kbps for ultra-low bitrate communications).

At the same time, it can flexibly switch between different modes, including adaptable, high-quality, low energy consumption, super high frequency, and ultra-low bitrate.

We use our proprietary video codec, and the performance in the RTC environment is excellent with low latency and no jitter.

#### Which voice codecs does Agora support？

Agora supports the following voice codecs:

* Third-party ultra-wideband (SWB) and full-band codecs：AAC-ELD and OPUS.
* Third-party broadband (WB) and narrowband (NB) codecs.
* Other codecs (ITU G.7xx series, SILK, iLBC, and iSAC).

## Performance

### Does Agora provide echo cancellation?

Echo cancellation is one of our strengths. Agora echo cancellation has the following characteristics:

* Simple and efficient.
* Support super high quality.
* Quick convergence and high echo return loss enhancement (ERLE).
* Optimization for high- and low-end phones.
* Device-specific parameter configuration.

### Will a call be interrupted when switching networks, such as from Wi-Fi to 3G/4G?

There might be a short interruption after switching networks during a call. For example, from Wi-Fi to 3G/4G due to the clients’ need to reconnect with the servers. The call continues after the network is reconnected.

### How much bandwidth does the Agora SDK require?

It depends on the network conditions:

* If the Wi-Fi connection is strong, Agora will increase the bandwidth allocation to ensure an optimal user experience.
* In a non Wi-Fi environment (for example in 3G or 2G), the Agora algorithm automatically lowers the bandwidth allocation according to the network conditions in order to allow quality calls.

Generally speaking, the bandwidth usage is 4 kB to 13 kB per second (in a non Wi-Fi environment, it may be 4 kB to 6 kB per second). In some cases, it can be adjusted to as low as 1 kB per second.

For more information, contact support@agora.io.

### How much bandwidth is required for a call？

You use bandwidth whenever you need to access the Internet in a voice or video call. Compared with similar products, the traffic consumption of Agora products is very low:

* Voice Call: 5 kbps to 10 kbps.
* Video Call: The bitrate changes according to the resolution and frame rate. In general terms (for example without taking color depth into account), if the resolution is 180 by 320 pixels, and the frame rate is 15 fps, then the bit rate is 160 kbps.

### What percentage of CPU resources does the Agora SDK typically use?

It depends on phone modes, operating systems, and CPU chips. The minimum usage is 3% of the CPU resources. For the latest high-end mobile phones, it may use 20% or more. However, we can optimize the CPU usage according to your needs.

For more information, contact support@agora.io.

### How can Agora ensure that the data transmission is smooth without data loss and with little latency?

* Agora patented NOVA codec is simple, low bitrate ,and highly adaptive.
* Users automatically access the Agora Global Virtual Network in poor network conditions.

###  Does Agora support reconnection automatically?

Yes.

### Why does my phone get hot after being in a call for a while?

Your phone performs software processing in a voice or video call, which uses lots of CPU resources and causes your phone to heat up. It is normal for the phone to heat up after a call or playing videos for a long time with any app.

### How can it only take 30 minutes to implement voice and video communications?

Agora SDKs have all the required voice/video functions ready to go, and developers just need four lines of code for integration.

### What is the benchmark of the voice/video quality?

On a subjective basis, refer to the MOS (Mean Opinion Score).

Benchmark for the quality of one host:

The host uses 360p @ 15 fps @ 500 kbps by default:

* Excellent: If the percentage of host bandwidth > 450 kbps exceeds 80%.
* Good: If the percentage of host bandwidth > 400 kbps exceeds 80%.
* Poor: If the percentage of host bandwidth > 300 kbps exceeds 80%.
* Bad: If the percentage of host bandwidth > 200 kbps exceeds 80%.
* Very Bad: If the percentage of host bandwidth > 100 kbps exceeds 80%.

Benchmark for the FPS quality of one audience:

* Excellent: If the percentage of audience receiving 13 fps exceeds 80%.
* Good: If the percentage of audience receiving 11 fps exceeds 80%.
* Poor: If the percentage of audience receiving 9 fps exceeds 80%.
* Bad: If the percentage of audience receiving 6 fps exceeds 80%.
* Very Bad: If the percentage of audience receiving 4 fps exceeds 80%.

Benchmark for voice communication:

* Excellent: The average packet loss rate is less than 1%.
* Good: The average packet loss rate is less than 2%.
* Poor: The average packet loss rate is less than 3%.
* Bad: The average packet loss rate is less than 5%.
* Very Bad: The average packet loss rate equals to or more than 5%.

Benchmark for video communication:

* Excellent: The average receiving frame rate equals to or is higher than 12 fps.
* Good: The average receiving frame rate is lower than 12 fps.
* Poor: The average receiving frame rate is lower than 10 fps.
* Bad: The average receiving frame rate is lower than 7 fps.
* Very Bad: The average receiving frame rate is lower than 4 fps.


