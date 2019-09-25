
---
title: Agora Recording Overview
description: 
platform: Linux
updatedAt: Wed Sep 25 2019 03:42:19 GMT+0800 (CST)
---
# Agora Recording Overview
The Agora On-premise Recording SDK is an add-on to record and save voice calls, video calls, and interactive broadcasts on your server. The Agora On-premise Recording SDK is compatible with the Agora Native SDK v1.7.0+ and the Agora Web SDK v1.12.0+.

For example, a user can either attend an online course at the time of the course or watch the recorded course later; made possible by the Agora On-premise Recording SDK being deployed at the server by the online course provider.

## Functions

The Agora On-premise Recording SDK provides the following functions:

- High-quality voice and video recordings.
- Single- or mixed-stream voice and video recordings of all users in the channel.
- Raw voice and video data.
- Screen captures of all users in the channel.
- Customizable video mixing layouts.

## Applications

The Agora On-premise Recording SDK can be used in the following scenarios:

| Industry                      | Applications                                                 |
| ----------------------------- | ------------------------------------------------------------ |
| Online Education              | One-to-one and one-to-many online courses. The Agora On-premise Recording SDK provides high-quality voice and video recordings. <li>Students can replay recordings for review.<li>Students can make up for missed courses at their convenience. |
| Live Broadcasts               | <li>The replay of live-broadcast highlights.<li>Captures screenshots.<li>Detects sexually explicit content. |
| Financial Industry            | When conducting financial management, account registration, and face-to-face businesses, the financial industry can use audio and video recordings for record keeping and archival purposes. |
| Customer Service/Call Centers | The recordings can be used for customer research and service quality evaluations. |
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
- The [Agora Linux Server Recording](https://github.com/AgoraIO/Basic-Recording/tree/master/Agora-LinuxServer-Recording) sample app enables recording on your Linux server.
