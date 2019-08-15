
---
title: Agora Cloud Recording Overview
description: 
platform: Linux
updatedAt: Thu Aug 15 2019 10:25:01 GMT+0800 (CST)
---
# Agora Cloud Recording Overview
Agora Cloud Recording is an add-on service to record and save voice calls, video calls, and interactive broadcasts on your cloud storage. It is compatible with the Agora Native SDK v1.7.0+ and the Agora Web SDK v1.12.0+. 

You can quickly and flexibly record one-to-one or one-to-many audio and video calls or live broadcasts through simple integration. Compared with Agora On-premise Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying Linux servers.

With Agora Cloud Recording, you can record calls or live broadcasts for your users to watch at their convenience. For example, a user can either attend an online course at the time of the course or watch the recorded course later, made possible by the Agora Cloud Recording SDK deployed at the app server.

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
| Online Education              | One-to-one and one-to-many online courses. Agora Cloud Recording provides high-quality voice and video recordings. <li>Students can replay recordings for review.<li>Students can make up for missed courses at their convenience. |
| Live Broadcasts               | <li>The replay of live-broadcast highlights.<li>Captures screenshots.<li>Detects sexually explicit content. |
| Financial Industry            | When conducting financial management, account registration, and face-to-face businesses, the financial industry can use audio and video recordings for record keeping and archival purposes. |
| Customer Service/Call Centers | The recordings can be used for customer research and service quality evaluations. |
| Remote Health Care            | <li>Recordings of remote diagnoses and online medical consultations enable patients to acquire medical resources in remote or inaccessible areas. <li> Can be used as references for subsequent treatments. |

## Features

Agora Cloud Recording consists of the following features:

| Feature          | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| High Reliability | Supports globally distributed cluster deployment and highly available services. |
| High Security    | Provides end-to-end security mechanisms for video calls, data transmission, data storage, and so on. For details, see [Information Security Policy](../../en/Agora%20Platform/security.md). |
| Compatibility    | Supports third-party cloud storages, such as [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls), [Alibaba Cloud](https://www.alibabacloud.com/product/oss), and [Qiniu Cloud](https://www.qiniu.com/en/products/kodo). |
| Ease of Use      | Simple implementation and easy to learn. You can get started quickly, flexibly deploy recording services, and easily record on mobile and web pages. |

## Billing
	
Using the Cloud Recording SDK is equivalent to a dummy client joining a channel and subscribing to the audio and video streams that need to be recorded. Agora Cloud Recording is charged by the minute and [the aggregate resolution](https://docs.agora.io/en/faq/video_billing#calculating-the-recording-aggregate-resolution). The aggregate resolutions of the audio and video streams are divided into three types: Audio, HD, and HD+. Contact [sales-us@agora.io](mailto:sales-us@agora.io) for details.

The billing for each recording task is separate. For example, two recording tasks of the same channel are charged separately.

> The service is free of charge if the monthly voice and video (including recording) usage is less than 10,000 minutes. See [the free-of-charge policy](https://docs.agora.io/en/faq/billing_free) for details.

## References

- [Quickstart for RESTful API](../../en/cloud-recording/cloud_recording_rest.md) describes how to use Agora Cloud Recording with the RESTful APIs.
- [RESTful API Callback Service](../../en/cloud-recording/cloud_recording_callback_rest.md) describes the events of the RESTful API.
