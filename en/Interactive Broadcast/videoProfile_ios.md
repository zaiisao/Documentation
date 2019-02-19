
---
title: Set the Video Profile
description: 
platform: iOS
updatedAt: Thu Dec 27 2018 02:41:18 GMT+0000 (UTC)
---
# Set the Video Profile
## Introduction

You can set the video profile, either before or after a user joins a channel, for the user to enjoy better video quality during a video call or live broadcast.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

The Agora SDK uses the `setVideoEncoderConfiguration` method to set the video profile. Each video profile corresponds to a set of video parameters, including the resolution, frame rate, bitrate, and video orientation.

The parameters specified in the `setVideoEncoderConfiguration` method are ideal values under ideal network conditions. If the video engine cannot render the video with the specified parameters due to poor network conditions, the parameters further down the list are considered.

```swift
// swift
// Set a VideoEncoderConfiguration instance.
// See the descriptions of the parameters in API Reference.
let config = AgoraVideoEncoderConfiguration(size: size, frameRate: frameRate, bitrate: bitrate, orientationMode: orientationMode)

agoraKit.setVideoEncoderConfiguration(config)
```

```objective-c
// objective-c
// Set a VideoEncoderConfiguration instance.
// See the descriptions of the parameters in API Reference.
AgoraVideoEncoderConfiguration *config = [AgoraVideoEncoderConfiguration alloc] initWithSize: size frameRate: frameRate bitrate: bitrate orientationMode: AgoraVideoOutputOrientationModeAdaptative];

[agoraKit setVideoEncoderConfiguration: config];
```

### API Reference
* [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:)
* For more information on the video orientation mode, see [Rotate the Video](../../en/Interactive%20Broadcast/rotation_guide_ios.md).

## Considerations
- If you do not need to set the video profile after joining the channel, you can call the `setVideoEncoderConfiguration` method before the `enableVideo` method to reduce the render time of the first video frame.
- The Agora SDK may adjust the parameters under poor network conditions. 
-  A live broadcast channel generally requires a higher bitrate for better video quality. Therefore, Agora recommends setting the bitrate in the live broadcast profile to twice of that in the communication profile. See [Set the bitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate).
- For better video quality during a live broadcast, a stable network connection is recommended.
- Setting parameters in the `setVideoEncoderConfiguration` method may affect your bill. For more information, see [Pricing and Billing](../../en/Agora%20Platform/billing_faq.md).

## Frequently Asked Questions
### How do I choose the video resolution, frame rate, and bitrate?

Video profiles vary from case to case. For example, in a one-to-one online class, the video windows of the teacher and student are both large, which requires higher resolutions, frame rates, and bitrates. While in a one-to-four online class, the video windows of the teacher and students are smaller, so lower resolutions, frame rates, and bitrates are used to accommodate the downward bandwidth.

 The following profiles for different scenarios are recommended:

- One-to-one video call: 
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps
  - Resolution: 640 x 360; frame rate: 15 fps; bitrate: 400 Kbps
- One-to-many video call: 
  - Resolution: 160 x 120; frame rate: 15 fps; bitrate: 65 Kbps
  - Resolution: 320 x 180; frame rate: 15 fps; bitrate: 140 Kbps
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps 

You can also customize the video parameters with the `setVideoEncoderConfiguration` method, such as increasing the bitrate to ensure the video quality.
