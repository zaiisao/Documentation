
---
title: Agora Cloud Recording Overview
description: 
platform: Linux
updatedAt: Thu Jun 11 2020 02:20:12 GMT+0800 (CST)
---
# Agora Cloud Recording Overview
Agora Cloud Recording is a component provided by Agora to record and save voice calls, video calls, and interactive broadcasts on your cloud storage. It is compatible with the Agora Native SDK v1.7.0+ and the Agora Web SDK v1.12.0+. 

You can quickly and flexibly record one-to-one or one-to-many audio and video calls or live broadcasts through simple integration. Compared with Agora On-premise Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying Linux servers.

With Agora Cloud Recording, you can record calls or live broadcasts for your users to watch at their convenience. For example, a user can either attend an online course at the time of the course or watch the recorded course later, made possible by the Agora Cloud Recording service.

## Functions

The following table lists the main functions that Agora Cloud Recording provides. To learn more about these functions, click the links below.

| <span style="white-space:nowrap;">&emsp;&emsp;&emsp;Feature&emsp;&emsp;&emsp;</span>    | Description                                                  |
| :------------------------------------------------ | :----------------------------------------------------------- |
| Recording mode                                    | Supports two recording modes:<ul><li>[Composite recording mode](../../en/cloud-recording/cloud_recording_composite_mode.md): Generates a single mixed audio and video file for all UIDs in a channel.</li><li>[Individual recording mode](../../en/cloud-recording/cloud_recording_individual_mode.md): Records the audio and video as separate files for each UID in a channel.</li></ul> |
| Capture screenshots                           | You can [take screenshots of each video stream](../../en/cloud-recording/cloud_recording_screen_capture.md) in individual recording mode.                |
| Record specified UIDs                             | You can specify the UIDs to record.             |
| Record specified media type                       | You can specify the type of media to record:<ul><li>Record audio only</li><li>Record video only</li><li>Record both audio and video</li></ul>|
| Set audio and video profiles                      | In composite recording mode, you can set audio and video profiles, such as the bit rate and resolution. |
| Set video layout                                  | In composite recording mode, you can [customize the video layout](https://docs.agora.io/en/cloud-recording/cloud_recording_layout?platform=Linux#a-namecustomacustomize-the-video-layout) or [use predefined layouts](https://docs.agora.io/en/cloud-recording/cloud_recording_layout?platform=Linux#a-namepredefinedaselect-from-the-predefined-layout-types), and set the background color of the canvas. You can update video layout or background color during recording. |
| Store recorded files in third-party cloud storage | You can store recorded files in the following third-party cloud storage services. You can customize the directory of the recorded files in the cloud storage.<ul><li>Amazon S3</li><li>Alibaba Cloud</li><li>Tencent Cloud</li><li>Qiniu Cloud</li><li>Kingsoft Cloud</li>|
| Record dual streams                               | If the [Agora RTC SDK ](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk)enables the [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#dual-stream), you can choose to record the high-video stream or the low-video stream. |
| Record encrypted channels                         | You can record a channel that is encrypted using the following encryption modes:<ul><li>AES128XTS</li><li>AES128ECB</li><li>AES256XTS</li></ul> |
| Transcoding                               | Agora Cloud Recording provides transcoding scripts for you to [merge audio and video files](https://docs.agora.io/en/cloud-recording/cloud_recording_merge_files?platform=All%20Platforms) and to [convert file formats](https://docs.agora.io/en/cloud-recording/cloud_recording_convert_format?platform=All%20Platforms). |
| Callback service                                  | The [callback service](https://docs.agora.io/en/cloud-recording/cloud_recording_callback_rest?platform=All%20Platforms) provides information including:<ul><li>The filenames of the recorded files</li><li>The start time of the first slice file</li><li>The timestamp when the stream status changes</li></ul> |

## Applications

Agora Cloud Recording can be used in the following scenarios:

| Industry                      | Applications                                                 |
| ----------------------------- | ------------------------------------------------------------ |
| Online Education              | One-to-one and one-to-many online courses. Agora Cloud Recording provides high-quality voice and video recordings. <li>Students can replay recordings for review.</li><li>Students can make up for missed courses at their convenience.</li> |
| Live Broadcasts               | <li>The replay of live-broadcast highlights.</li><li>Captures screenshots.</li><li>Detects sexually explicit content.</li> |
| Financial Industry            | When conducting financial management, account registration, and face-to-face businesses, the financial industry can use audio and video recordings for record keeping and archival purposes. |
| Customer Service/Call Centers | The recordings can be used for customer research and service quality evaluations. |
| Remote Health Care            | <li>Recordings of remote diagnoses and online medical consultations enable patients to acquire medical resources in remote or inaccessible areas. </li><li> Can be used as references for subsequent treatments.</li> |

## Features

Agora Cloud Recording consists of the following features:

| Feature          | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| High Reliability | <li>Supports globally distributed cluster deployment and highly available services.</li><li>Automatically backs up files on Agora cloud server when the third-party cloud storage fails and automatically uploads the backup to the third-party cloud storage when it recovers.</li> |
| High Security    | Provides end-to-end security mechanisms for video calls, data transmission, data storage, and so on. For details, see [Information Security Policy](../../en/Agora%20Platform/security.md). |
| Compatibility    | Supports third-party cloud storages, such as [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls), [Alibaba Cloud](https://www.alibabacloud.com/product/oss), [Tencent Cloud](https://intl.cloud.tencent.com/product/cos) and [Qiniu Cloud](https://www.qiniu.com/en/products/kodo). |
| Ease of Use      | Simple implementation and easy to learn. With four RESTful API calls, you can start, stop, and query the recording. You can get started quickly, flexibly deploy recording services, and easily record on mobile and web pages. |

## Billing

Use Agora Cloud Recording to join a channel, subscribe to the audio and video streams that need to be recorded, and upload the recorded files to the specified cloud storage. Agora Cloud Recording is charged by the minute and [the aggregate resolution](https://docs.agora.io/en/faq/video_billing#calculating-the-recording-aggregate-resolution). The aggregate resolutions of the audio and video streams are divided into three types: Audio, HD, and HD+. Contact [support@agora.io](mailto:support@agora.io) for details.

The billing for each recording task is separate. For example, two recording tasks of the same channel are charged separately.

> The service is free of charge if the monthly voice and video (including recording) usage is less than 10,000 minutes. See [the free-of-charge policy](https://docs.agora.io/en/faq/billing_free) for details.

## References

- [Quickstart for RESTful API](../../en/cloud-recording/cloud_recording_rest.md) describes how to use Agora Cloud Recording with the RESTful API.
- [Agora Cloud Recording RESTful API](https://docs.agora.io/en/cloud-recording/restfulapi) describes the functions of the RESTful API.
- [RESTful API Callback Service](../../en/cloud-recording/cloud_recording_callback_rest.md) describes the events of the RESTful API.
