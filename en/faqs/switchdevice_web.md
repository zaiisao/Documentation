
---
title: How can I switch the input device during a web call?
description: how to switch input devices for web sdk
platform: Web
updatedAt: Fri Apr 24 2020 17:05:47 GMT+0800 (CST)
---
# How can I switch the input device during a web call?
The audio/video input devices are identified by the device ID ([`deviceId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.mediadeviceinfo.html#deviceid)). Each device has a unique device ID which you can get by [`getDevices`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/globals.html#getdevices). The device ID is randomly generated, and might change for the same device, so we recommend you call the `getDevices` method every time before switching the device.

- For the Agora Web SDK v2.4.1 or earlier

  Set the `microphoneId` and `cameraId` parameters in the `createStream` method to switch the microphone and camera.

  1. Call the [`close`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#close) method to close the local stream.
  2. Call the [`getDevices`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/globals.html#getdevices) method to enumerate available devices and get the device IDs.
  3. Call the [`createStream`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/globals.html#createstream) method and fill the [`microphoneId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#microphoneid) or [`cameraId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.streamspec.html#cameraid) parameter with the device ID.

- For the Agora Web SDK v2.5.0 or later
  Call the [`switchDevice`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#switchdevice) method after getting the device ID. This method does not work on Safari 11 and Firefox.

If you do not need to select the input devices, set `cameraId` and `microphoneId` as `""` in `createStream` to use the first input device by default.
