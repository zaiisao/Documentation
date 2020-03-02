
---
title: Why haven’t the recorded files been uploaded to the cloud storage
description: 
platform: All Platforms
updatedAt: Mon Mar 02 2020 11:54:39 GMT+0800 (CST)
---
# Why haven’t the recorded files been uploaded to the cloud storage
Uploading the recorded files to the cloud storage fails if:

- No user is sending a stream in the channel, and the recording times out.
- Token has expired, or the token authentication has failed.
- When calling the [`acquire`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#a-nameacquireagets-the-recording-resource) method to get the recording resource, you set the `uid` parameter that matches that of a user ID already in the channel. For example, suppose three users are in the channel with user IDs as `123`, `234`, and `345`. If you set `uid` as `123` when calling the `acquire` method, the recording fails. 
- The settings of `transcodingConfig` in the [`start`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#a-namestartastarts-cloud-recording) method do not follow the recommended settings, which causes the recording to fail. See [How do I set the video profile of the recorded video?](https://docs.agora.io/en/faq/recording_video_profile) before setting `transcodingConfig`.
- Your cloud storage settings are incorrect. Ensure that the following settings are correct:
  - bucket: the name you have given to your cloud storage bucket, previously created in your cloud storage account.
  - accessKey: the access key of your cloud storage account.
  - secretKey: the secret key of your cloud storage account.
