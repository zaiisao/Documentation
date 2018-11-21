
---
title: Set the Video Profile
description: 
platform: iOS
updatedAt: Wed Nov 21 2018 03:21:37 GMT+0000 (UTC)
---
# Set the Video Profile
## Introduction

Setting the video profile, either before or after a user joins a channel, enables the user to enjoy better video quality during a video call or video live broadcast.

## Implementations

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/ios_video.md) for more information.

Agora SDK uses the `setVideoEncoderConfiguration` API to set the video profile and each video profile corresponds to a set of video parameters, including the resolution, frame rate, bitrate and video orientation.

The parameters specified in this API are the ideal values under ideal network conditions. If the video engine cannot render the video with the specified parameters dut to poor network conditions, the parameters further down the list are considered.

```swift
//swift
// Set a VideoEncoderConfiguration instance
// See detailed description of each parameter in the API link
let config = AgoraVideoEncoderConfiguration(size: size, frameRate: frameRate, bitrate: bitrate, orientationMode: orientationMode)

agoraKit.setVideoEncoderConfiguration(config)
```

```objective-c
//objective-c
// Set a VideoEncoderConfiguration instance
// See detailed description of each parameter in the API link
AgoraVideoEncoderConfiguration *config = [AgoraVideoEncoderConfiguration alloc] initWithSize: size frameRate: frameRate bitrate: bitrate orientationMode: AgoraVideoOutputOrientationModeAdaptative];

[agoraKit setVideoEncoderConfiguration: config];
```

**Relevant APIs and descriptions**
* [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:)
* For more information on video orientation mode, see [Rotate the Video](../../en/Video/rotation_guide_ios.md).

## Considerations
- If you do not need to set the video profile after joining the channel, call `setVideoEncoderConfiguration` before `enableVideo` to reduce the render time of the first video frame.
- Adjustments to the set parameters can be made by the Agora SDK under poor network conditions. 
-  A live broadcast channel generally requires a higher bitrate for better video quality. Therefore, Agora recommends setting the bitrate in the live broadcast mode twice that in the communication mode. See [Set the bitrate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate) for details.
- For better video quality during a live broadcast, stable network connection is recommended.
- Setting parameters in the `setVideoEncoderConfiguration` API may cause billing changes. For more information, see [Pricing and Billing](../../en/Agora%20Platform/billing_faq.md).

## Frequently Asked Questions
### How to choose the video resolution, frame rate and bitrate?

Video profiles vary from case to case. For example, in a one-to-one online class, the video windows of the teacher and student are both large, which requires higher resolutions, frame rates, and bitrates. While in a one-to-four online class, the video windows of the teacher and students are smaller, so lower resolutions, frame rates, and bitrates are used to accommodate the downward bandwidth.

 The following profiles for different scenario are recommended:

- One-to-one video call: 
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps
  - Resolution: 640 x 360; frame rate: 15 fps; bitrate: 400 Kbps
- One-to-many video call: 
  - Resolution: 160 x 120; frame rate: 15 fps; bitrate: 65 Kbps
  - Resolution: 320 x 180; frame rate: 15 fps; bitrate: 140 Kbps
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps 

You can also customize the video parameters, such as by increasing the bitrate to ensure the video quality with the `setVideoEncoderConfiguration` API.
