
---
title: Agora Cloud Recording Overview
description: 
platform: Linux
updatedAt: Mon Apr 29 2019 02:58:32 GMT+0800 (CST)
---
# Agora Cloud Recording Overview
Agora Cloud Recording is an add-on SDK to record and save voice calls, video calls, and interactive broadcasts on your cloud storage. It is compatible with the Agora Native SDK v1.7.0 or later and the Agora Web SDK v1.12.0 or later. 

You can quickly and flexibly record one-to-one or one-to-many audio and video calls or live broadcasts through simple integration. Compared with Agora Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying Linux servers.

With Agora Cloud Recording, you can record the calls or live broadcasts for your users to watch at their convenience. For example, a user can either attend an online course at the time of the course or watch the recorded course later, made possible by the Agora Cloud Recording SDK deployed at the app server.

## Functions

Agora Cloud Recording provides the following functions:

- High-quality voice and video recordings.
- Mixed-stream voice and video recordings of all users in the channel.
- Various composite video layouts.
- Third-party cloud storage.

## Applications

Agora Cloud Recording can be used in the following scenarios:

| Industry                      | Applications                                                 |
| ----------------------------- | ------------------------------------------------------------ |
| Online Education              | One-to-one and one-to-many online courses. Agora Cloud Recording provides high-quality voice and video recordings. <li>Students can replay recordings for review.<li>Students can make up missed courses at their convenience. |
| Live Broadcasts               | <li>The replay of live broadcast highlights.<li>Captures screenshots.<li>Detects sexually explicit content. |
| Financial Industry            | When conducting financial management, account registration, and face-to-face business, the financial industry can use audio and video recordings for record keeping and archival purposes. |
| Customer Service/Call Centers | The recordings can be used for customer research and service quality evaluation. |
| Remote Health Care            | <li>Recordings of remote diagnosis and online medical consultations enable patients to acquire medical resources in remote or inaccessible areas. <li> Can be used as references for subsequent treatments. |

## Features

Agora Cloud Recording consists of the following features:

| Feature          | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| High Reliability | Supports globally distributed cluster deployment and highly available services. |
| High Security    | Provides end-to-end security mechanisms for video calls, data transmission, data storage, and so on. For details, see [Information Security Policy](https://docs-preview.agoralab.co/en/Agora%20Platform/security). |
| Compatibility    | Supports third-party cloud storages, such as Qiniu Cloud, Aliyun and Amazon S3. |
| Ease of Use      | Simple implementation and easy to learn. You can get started quickly, flexibly deploy recording services, and easily record on mobile and web pages. |

## Billing
	
When using the Cloud Recording SDK, it is equivalent to a dummy client joining a channel and subscribing to the audio and video streams that need to be recorded. Agora Cloud Recording is charged by minutes and [the aggregate resolution](../../en/Agora%20Platform/billing_faq.md). The aggregate resolution of the audio and video streams is divided into three types: Audio, HD, and HD+. Contact [sales@agora.io](mailto:sales@agora.io) for details.

The billing for each recording task is separate. For example, two recording tasks of the same channel are charged separately.

## References

- [Quickstart for Cloud Recording](https://docs.agora.io/en/Quickstart%20Guide/cloud_recording) describes how to deploy and use Agora Cloud Recording.
- [API Reference](https://docs.agora.io/en/API%20Reference/cloud_recording_api) lists the API methods of the Agora Cloud Recording SDK.
