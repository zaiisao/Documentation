
---
title: Set the Video Profile
description: 
platform: Android
updatedAt: Tue Mar 26 2019 06:30:17 GMT+0000 (UTC)
---
# Set the Video Profile
## Introduction

You can set the video profile, either before or after a user joins a channel, for the user to enjoy better video quality during a video call or live broadcast.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/android_video.md).

The Agora SDK uses the `setVideoEncoderConfiguration` method to set the video profile. Each video profile corresponds to a set of video parameters, including the resolution, frame rate, bitrate, and video orientation.

The parameters specified in the `setVideoEncoderConfiguration` method are ideal values under ideal network conditions. If the video engine cannot render the video with the specified parameters due to poor network conditions, the parameters further down the list are considered.

```java
// java
// Create a VideoEncoderConfiguration instance.
// See the descriptions of the parameters in API Reference.
VideoEncoderConfiguration config = new VideoEncoderConfiguration(
	VideoDimensions.VD_640x480,  // Choose a video resolution or customize one.
	FRAME_RATE_FPS_15,           // Frame rate. 15 is the default setting. Agora recommends not setting to over 30.
	STANDARD_BITRATE,            // The standard bitrate. See the description in API Reference. Agora recommends setting the bitrate to the standard mode.
	ORIENTATION_MODE_ADAPTIVE    // The adaptive orientation mode. See the description in API Reference.
);

rtcEngine.setVideoEncoderConfiguration(config);
```

### API Reference
* [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af5f4de754e2c1f493096641c5c5c1d8f)
* For more information on the video orientation mode, see [Rotate the Video](../../en/Video/rotation_guide_android.md).

## Considerations
- If you do not need to set the video profile after joining the channel, you can call the `setVideoEncoderConfiguration` method before the `enableVideo` method to reduce the render time of the first video frame.
- The Agora SDK may adjust the parameters under poor network conditions. 
-  A live broadcast channel generally requires a higher bitrate for better video quality. Therefore, Agora recommends setting the bitrate in the live broadcast profile to twice of that in the communication profile. See [Set the bitrate](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1video_1_1_video_encoder_configuration.html#a4b090cd0e9f6d98bcf89cb1c4c2066e8).
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
