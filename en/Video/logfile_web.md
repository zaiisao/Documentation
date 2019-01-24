
---
title: Set the Logger
description: 
platform: Web
updatedAt: Thu Jan 17 2019 09:47:06 GMT+0000 (UTC)
---
# Set the Logger
## Introduction
The Agora Web SDK provides methods for you to set the output log level or to enable/disable log upload.

## Implementation
Before proceeding, ensure that you have your development environment prepared. See [Integrate the SDK](../../en/Video/web_prepare.md).

### Set Output Log Level
The output log levels are in the order of NONE, ERROR, WARNING, INFO, and DEBUG. Select a level, and you can see the logs in the preceding levels. For example, enter `AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);`, and you can see the logs in the WARNING, ERROR and INFO levels.

#### API Reference

[`setLogLevel`](https://docs.agora.io/en/Video/API%20Reference/web/modules/agorartc.logger.html#setloglevel)

### Enable/Disable Log Upload
Call the `enableLogUpload` method to upload logs on to the Agora Server, or call the `disableLogUpload` method to stop the upload.

#### Sample Code
`AgoraRTC.Logger.enableLogUpload();`

`AgoraRTC.Logger.disableLogUpload();`

#### Considerations
- The log-upload function is disabled by default. To enable this function, call the `enableLogUpload` method.
- If the user fails to join the channel, the logs are unavailable on the Agora Server.

#### API Reference

- [`enableLogUpload`](https://docs.agora.io/en/Video/API%20Reference/web/modules/agorartc.logger.html#enablelogupload)
- [`disableLogUpload`](https://docs.agora.io/en/Video/API%20Reference/web/modules/agorartc.logger.html#disablelogupload)
