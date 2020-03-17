
---
title: How to stop cloud recording?
description: 
platform: All Platforms
updatedAt: Tue Nov 19 2019 10:56:23 GMT+0800 (CST)
---
# How to stop cloud recording?
You can call the [`stop`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#stop) method to leave the channel and stop recording.

Agora Cloud Recording automatically stops recording and leaves the channel when there is no user in the recording channel after a time period (30 seconds by default). You can set this timeout interval by the `maxIdleTime` parameter when you start the recording.
