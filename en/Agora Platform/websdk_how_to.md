
---
title: Web SDK-related Issues
description: 
platform: All Platforms
updatedAt: Fri Jun 21 2019 04:17:34 GMT+0800 (CST)
---
# Web SDK-related Issues
### How do you change the audio and video input devices during a video call?
You can get all available audio and video input and output devices of the user by calling the `getDevices` method. If the user wants to use the first identified device, set `cameraId` and `microphoneId` as "" in the createStream method. The user can also create arrays of objects to manage local devices (according to the "kind" classification). `cameraId` and `microphoneId` are randomly generated and subject to change in some cases. Therefore, if you want to change the audio and video input devices during a video call, you need to first close the stream, call the `getDevice` method, and create a stream again. 

### How do you enable the dual-stream mode?
The sender calls the `enableDualStream` method to enable the dual-stream mode. The receiver switches between the high stream and low stream by calling the `setRemoteVideoStreamType` method and sets the parameters of the low stream by calling the `setLowStreamParameter` method.

### <a name="edge"></a>Does the Agora Web SDK support the Microsoft Edge browser?

The Agora Web SDK v2.7 or later supports the Microsoft Edge browser. Due to the browser limitations, the Agora Web SDK supports the following functions:

- Communicates with the Agora Native/Web SDK in audio/video calls and live broadcasts.

- Gets the connection statistics by calling the [getStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getstats) method. 

- Gets the audio level by calling the [getAudioLevel](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiolevel) method.

- Disables/Enables the audio track by calling the [muteAudio](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#muteaudio)/[unmuteAudio](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#unmuteaudio) method.

- Disables/Enables the video track by calling the [muteVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#mutevideo)/[unmuteVideo](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#unmutevideo) method.
