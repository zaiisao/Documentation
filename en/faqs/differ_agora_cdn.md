
---
title: What is the difference between the Agora Interactive Broadcast technologies and common CDN + RTMP technologies?
description: 
platform: All Platforms
updatedAt: Tue Jan 14 2020 11:35:34 GMT+0800 (CST)
---
# What is the difference between the Agora Interactive Broadcast technologies and common CDN + RTMP technologies?
Most CDN + RTMP technologies for live broadcast allow users to watch the live broadcast in a web browser, which lowers the audience's threshold. 
Agora provides a solution for the Agora Cloud, host, and audience to have the same real-time communication quality as an individual line with:

- Private voice and video coding
- Private transport protocol
- Private node deployment
- Private transmission algorithms

See the following table for details:

| Feature                | CDN + RTMP        | Agora Interactive Broadcast                                  |
| --------------------------- | ----------------- | ------------------------------------------------------------ |
| Video Encoding and Decoding | H.264             | Private                                                      |
| Audio Encoding and Decoding | AAC               | Private                                                      |
| Transport Protocol          | TCP based on RTMP | Private protocol based on UDP                                |
| Transmission Algorithm      | TCP               | Private algorithm for fixing packet loss and adjusting the bitrate automatically according to the current bandwidth |
| Picture-in-picture layout   | Fixed             | Can be adjusted dynamically                                  |

Agora also enables the function of [publishing streams into the CDN](https://docs.agora.io/en/Interactive%20Broadcast/push_stream_android2.0?platform=Android) for social media sharing.
