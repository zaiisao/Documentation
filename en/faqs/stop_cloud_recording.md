
---
title: How can I stop cloud recording?
description: 
platform: All Platforms
updatedAt: Wed Mar 25 2020 22:46:04 GMT+0800 (CST)
---
# How can I stop cloud recording?
You can call the [`stop`](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#stop) method to leave the channel and stop recording.

Agora Cloud Recording automatically stops recording and leaves the channel when there is no user in the recording channel after a time period (30 seconds by default). You can set this timeout interval by the `maxIdleTime` parameter when you start the recording.
