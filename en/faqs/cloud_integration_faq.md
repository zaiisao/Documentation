
---
title: Cloud recording FAQ
description: Cloud recording faq
platform: Linux Java,Linux,Linux C++
updatedAt: Thu Aug 08 2019 11:31:12 GMT+0800 (CST)
---
# Cloud recording FAQ
 ## Fails to upload to the cloud storage

Check if your cloud storage settings are correct:

- bucket: name of your cloud storage bucket, created by yourself in your cloud storage account.
- accessKey: access key of your cloud storage account.
- secretKey: secret key of your cloud storage account.

## How to get the URL of the M3U8 file

The URL of the M3U8 file consists of the domain of your cloud storage and the filename. You can copy the URL in your cloud storage.

![](https://web-cdn.agora.io/docs-files/1556174270602)

You can get the filename of the M3U8 file from the following fileds:
- The `fileList` field in the responses of [`query`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest#query) and [`stop`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest#stop) 
- The `fileList` field in the [`cloud_recording_file_infos`](https://docs.agora.io/cn/cloud-recording/cloud_recording_callback_rest#a-name4acloud_recording_file_infos) callback event

## How to stop cloud recording?
You can call the [`stop`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#stop) method to leave the channel and stop recording.

Agora Cloud Recording automatically stops recording and leaves the channel when there is no user in the recording channel after a time period (30 seconds by default). You can set this timeout interval by the `maxIdleTime` parameter when you start the recording.

## 101 error

The `"ErrorUint32":101` error in the log file is usually caused by a token error, such as in the following situations:

- A wrong token.
- An expired token.
- The Native/Web SDK uses a token while the cloud recording does not use a token.
- The cloud recording uses a token while the Native/Web SDK does not use a token.

## Abnormal exit

The recording exits abnormally when the app crashes. As long as the call or live broadcast is ongoing, the Agora Cloud Recording service keeps recording and uploading. You can use the original App ID, channel name, and UID to join the channel again and take control of the original recording instance.
