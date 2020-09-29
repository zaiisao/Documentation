
---
title: Agora On-premise Recording Overview
description: 
platform: Linux
updatedAt: Sun Sep 27 2020 09:54:37 GMT+0800 (CST)
---
# Agora On-premise Recording Overview
<div class="alert note">Note: <br>If you do not want to prepare server resources, Agora recommends using <a href="https://docs.agora.io/en/cloud-recording/product_cloud_recording?platform=Linux">Agora Cloud Recording</a>, which enables you to record voice/video calls and interactive streaming on your cloud storage through RESTful APIs.  </div>

The Agora On-premise Recording SDK is a component provided by Agora to record and save voice calls, video calls, and live interactive streaming on your server. The Agora On-premise Recording SDK is compatible with the Agora Native SDK v1.7.0+ and the Agora Web SDK v1.12.0+.

For example, a user can either attend an online course at the time of the course or watch the recorded course later; made possible by the Agora On-premise Recording SDK being deployed at the server by the online course provider.

## Functions

The Agora On-premise Recording SDK enables you to record high-quality voice or video calls made via the [Agora RTC SDK](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk). See the following table for details.

| Function                                                     | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Record specified media type                                    | You can specify the media type to record:<li>Record only audio.<li>Record only video.<li>Record both audio and video. |
| Choose recording mode                                        | You can choose one of the following recording modes:<li>[Individual Recording](../../en/Recording/recording_individual_mode.md): The SDK generates one audio and/or video file for each UID.<li>[Composite Recording](../../en/Recording/recording_composite_mode.md): Generates a single mixed audio and video file for all UIDs in a channel, or mixes the audio of all UIDs in the channel into an audio file and the video of all UIDs into a video file. You can also set the audio and video profile of the recording file. |
| [Set Video Layout](../../en/Recording/recording_layout.md) | In composite recording mode, you can: <li>Set the size and position of the region for each user in the layout.<li>Set the background images associated with each user.<li>Set the background image of the canvas. |
| Record specified UIDs | You can specify the UIDs you want to record. |
| [Get the raw data](../../en/Recording/recording_raw_data.md) | You can get the raw data in the following formats: <li>Raw audio data in AAC or PCM formats.<li>Raw video data in H.264 or YUV formats. |
| [Capture Screenshots](../../en/Recording/recording_screen_capture.md) | <li>In individual recording mode, you can get one recording file and multiple screenshots in JPEG format for each UID, or only get screenshots in JPEG format.</li><li>In composite recording mode, you can get a video file in MP4 format and multiple screenshots in JPEG format.</li> |
| [Watermark](../../en/Recording/recording_watermark_cpp.md) | In composite recording mode, you can add watermarks to the video, including text, timestamp, and image watermarks. |
| Use the proxy                                                | You can configure the proxy server or [Use Cloud Proxy](../../en/Recording/cloudproxy_recording.md) to connect to Agora's services through a firewall. |
| Record dual streams                                          | If you enable the [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode) in the Agora RTC SDK, the Agora On-premise Recording SDK allows you to record the following streams:<li>Only record the high-video stream.</li><li>Only record the low-video stream.</li> |
|Record encrypted channels|You can record a channel that is encrypted using the following encryption modes:<li>AES128XTS</li><li>AES128ECB</li><li>AES256XTS</li> |

## Applications

The Agora On-premise Recording SDK can be used in the following scenarios:

| Industry                      | Applications                                                 |
| ----------------------------- | ------------------------------------------------------------ |
| Online Education              | One-to-one and one-to-many online courses. The Agora On-premise Recording SDK provides high-quality voice and video recordings. <li>Students can replay recordings for review.<li>Students can make up for missed courses at their convenience. |
| Live Streaming               | <li>The replay of live interactive streaming highlights.<li>Captures screenshots.<li>Detects sexually explicit content. |
| Financial Industry            | When conducting financial management, account registration, and face-to-face businesses, the financial industry can use audio and video recordings for record keeping and archival purposes. |
| Customer Service/Call Centers | The recordings can be used for service quality evaluations. |
| Remote Health Care            | <li>Recordings of remote diagnoses and online medical consultations enable patients to acquire medical resources in remote or inaccessible areas. <li> Can be used as references for subsequent treatments. |

## Features

The Agora On-premise Recording SDK consists of the following features:

| Feature          | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| High Reliability | The Agora On-premise Recording SDK supports cluster deployment, dynamic capacity expansion, and high availability services. |
| High Security    | Provides end-to-end security mechanisms for video calls, data transmission, data storage, and so on. For details, see [Information Security Policy](../../en/Agora%20Platform/security.md). |
| Compatibility    | Supports CentOS 6.5+ x64 and Ubuntu 14.04+ x64 operating systems. |
| Ease of Use      | Simple implementation and easy to learn. You can get started quickly, flexibly deploy recording services, and easily record on mobile and web pages. |
| Flexibility      | By flexibly combining various functions of the Agora On-premise Recording SDK, you can seamlessly apply the SDK to multiple scenarios to achieve better service. |

## Compatibility with the Agora SDKs

The recording SDK supports:

- Recording communication that uses the Native SDK.
- Recording communication that uses the Web SDK.
- Recording communication that uses both the Native SDK and the Web SDK.

The On-premise Recording SDK is compatible with the following Agora SDK versions:

<table>
<thead>
<tr><th>Agora SDK</th>
<th>Compatible versions</th>
</tr>
</thead>
<tbody>
<tr><td>Agora Native SDK</td>
<td>v1.7.0 or later</td>
</tr>
<tr><td>Agora Web SDK</td>
<td>v1.12.0 or later</td>
</tr>
</tbody>
</table>

> If any user in the channel uses an Agora SDK which is not compatible with the Agora On-premise Recording SDK, recording fails for the whole channel.

## References

- [Integrate the SDK](../../en/Quickstart%20Guide/recording_integrate_cpp.md) and [Record a Call](../../en/Quickstart%20Guide/recording_cmd_cpp.md) on how to deploy and use the Agora On-premise Recording SDK, manage, play, and protect recorded files.
- [API Reference](https://docs.agora.io/en/Recording/API%20Reference/recording_cpp/index.html) lists the API methods of the Agora On-premise Recording SDK.
- The [Agora Linux Server Recording](https://github.com/AgoraIO/Basic-Recording/) sample app enables recording on your Linux server.
- [Recording Concurrency](https://docs.agora.io/en/faq/recording_concurrence) introduces the concurrency of on-premise recording.
