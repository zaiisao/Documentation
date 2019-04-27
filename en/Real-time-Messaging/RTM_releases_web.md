
---
title: Release Notes
description: 
platform: Web
updatedAt: Sat Apr 27 2019 12:59:23 GMT+0800 (CST)
---
# Release Notes
## Overview

Designed as a substitute for the legacy Agora Signaling SDK, the Agora Real-Time-Messaging SDK provides a more streamlined implementation and more stable messaging mechanism for you to quickly implement real-time messaging scenarios.

### Highlights

- Supports sending peer-to-peer or channel messages.
- Supports multiple instances so that you can call multiple RTM services in one process. 
- Categorizes the error codes so that you can quickly identify the problem.


## v0.9.0

v0.9.0 is released on February 14th, 2019.

Initial version. 

Key features:

- Sends or receives peer-to-peer messages.
- Joins or leaves a channel.
- Sends or receives channel messages.

> This release uploads the log to the server by default. To disable log upload, you have to set `enableLogUpload` as `false` when calling the `createInstance` method. 
> ```JavaScript
> AgoraRTM.createInstance('demoAppId', { enableLogUpload: false });
> ```
