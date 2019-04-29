
---
title: Release Notes for Agora Cloud Recording
description: 
platform: Linux
updatedAt: Mon Apr 29 2019 02:58:41 GMT+0800 (CST)
---
# Release Notes for Agora Cloud Recording
## Overview

Agora Cloud Recording is an add-on service to record and save voice calls, video calls, and interactive broadcasts on your cloud storage. Compared with Agora On-premise Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying Linux servers.

### Compatibility

Agora Cloud Recording is compatible with the following SDKs:

| SDK                  | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| The Agora Native SDK | Agora Cloud Recording is compatible with the Agora Native SDK v1.7.0 or later. |
| The Agora Web SDK    | Agora Cloud Recording is compatible with the Agora Web SDK v1.12.0 or later. |

## v1.0.0

v1.0.0 is released on April 30, 2019.

This is the first release of Agora Cloud Recording with the following functions:

- High-quality voice and video recordings.
- Mixed-stream voice and video recordings of all users in a channel.
- Three composite video layouts: float (default), best fit, and vertical, see the [TranscodingConfig](../../en/cloud-recording/cloud_recording_api.md) table for details.
- Third-party cloud storage. Currently, Agora Cloud Recording supports Qiniu Cloud, Aliyun, and Amazon S3.
- Provides C++ and Java SDK packages.
