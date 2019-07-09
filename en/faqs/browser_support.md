
---
title: Which browsers does the Agora Web SDK support?
description: 
platform: Web
updatedAt: Tue Jul 02 2019 14:35:23 GMT+0800 (CST)
---
# Which browsers does the Agora Web SDK support?
The Agora Web SDK supports all mainstream browsers, see [Compatibility](https://docs.agora.io/en/Interactive%20Broadcast/release_web_video?platform=Web#compatibility) for details. Due to the various browser engine implementations, support for some features may vary by browser and platform. The following are known issues and limitations.

## Chrome

The Agora Web SDK is based on WebRTC and works best on Chrome.
- The Agora Web SDK supports Chrome 58 or later.
- Some APIs require later versions of Chrome, see the API Reference for details.

## Safari

- Safari only supports the H.264 codec.
- Safari does not support changing the frame rate (30 fps by default).
- Device permissions
  - Safari does not support getting the output device information, so it does not support the `getPlayoutDevices` and `setAudioOutput` methods.
  - If Auto-Play is not enabled on Safari, the stream playback has no audio. You have to call the `navigator.mediaDevices.getUserMedia` method to get the device permissions before playing a stream.
- Safari does not support the `addTrack` and `removeTrack` methods.
- Safari does not support the `getAudioLevel` method on iOS.

## Firefox

- When the Firefox browser interoperates with the Native SDK for iOS, the video in the Firefox browser is rotated.
- Setting the video profile on Firefox does not take effect on the following devices:
  - MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
  - Windows 10 (MI)

## Edge

The Agora Web SDK v2.7 or later supports the Microsoft Edge browser. Due to browser limitations, the Agora Web SDK supports the following functions:

- Communicates with the Agora Native/Web SDK in audio/video calls and live broadcasts.
- Gets the connection statistics by calling the `getStats` method.
- Gets the audio level by calling the `getAudioLevel` method.
- Disables/Enables the audio track by calling the `muteAudio`/`unmuteAudio` method.
- Disables/Enables the video track by calling the `muteVideo`/`unmuteVideo` method.
