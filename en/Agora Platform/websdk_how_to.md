
---
title: Web SDK-related Issues
description: 
platform: Web SDK-related Issues
updatedAt: Tue Jun 11 2019 01:54:26 GMT+0800 (CST)
---
# Web SDK-related Issues
### How do you change the audio and video input devices during a video call?
You can get all available audio and video input and output devices of the user by calling the `getDevices` method. If the user wants to use the first identified device, set `cameraId` and `microphoneId` as "" in the createStream method. The user can also create arrays of objects to manage local devices (according to the "kind" classification). `cameraId` and `microphoneId` are randomly generated and subject to change in some cases. Therefore, if you want to change the audio and video input devices during a video call, you need to first close the stream, call the `getDevice` method, and create a stream again. 

### How do you enable the dual-stream mode?
The sender calls the `enableDualStream` method to enable the dual-stream mode. The receiver switches between the high stream and low stream by calling the `setRemoteVideoStreamType` method and sets the parameters of the low stream by calling the `setLowStreamParameter` method.
