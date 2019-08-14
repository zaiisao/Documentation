
---
title: Set the Video Profile
description: 
platform: Windows
updatedAt: Wed Aug 14 2019 07:08:47 GMT+0800 (CST)
---
# Set the Video Profile
## Introduction

You can set the video profile, either before or after a user joins a channel, for the user to enjoy better video quality during a video call or live broadcast.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md).

The Agora SDK uses the `setVideoEncoderConfiguration` method to set the video profile. Each video profile corresponds to a set of video parameters, including the resolution, frame rate, bitrate, and video orientation.

The parameters specified in the `setVideoEncoderConfiguration` method are ideal values under ideal network conditions. If the video engine cannot render the video with the specified parameters due to poor network conditions, the parameters further down the list are considered.

We recommend setting the parameters according to the [video profile table](#video_profile) below.

```cpp
// Set the video encoder configuration.
// Set the width and height of the video stream. Swapping the two values does not change the video orientation.
VideoEncoderConfiguration (lpVideoConfig(640, 360),
// Set the video frame rate of the video.
FRAME_RATE_FPS_15,
// Set the bitrate of the video in Kbps.
800,
// The orientation mode the video.
ORIENTATION_MODE_ADAPTIVE,
// The degradation preference under limited bandwidth. MIANTAIN_QUALITY means to degrade the frame rate to maintain the video quality.
MAINTAIN_QUALITY
);

lpAgoraEngine->setVideoEncoderConfiguration(lpVideoConfig);
```

<a id="video_profile"></a>
### Video Profile Table

| Resolution<br>(width x height) | Frame rate<br>(fps) | Base bitrate<br>(Kbps, for Communication) | Live Bitrate<br>(Kbps, for Live-Broadcast) |
| ------------------------------ | ------------------- | ----------------------------------------- | ------------------------------------------ |
| 160 x 120                      | 15                  | 65                                        | 130                                        |
| 120 x 120                      | 15                  | 50                                        | 100                                        |
| 320 x 180                      | 15                  | 140                                       | 280                                        |
| 180 x 180                      | 15                  | 100                                       | 200                                        |
| 240 x 180                      | 15                  | 120                                       | 240                                        |
| 320 x 240                      | 15                  | 200                                       | 400                                        |
| 240 x 240                      | 15                  | 140                                       | 280                                        |
| 424 x 240                      | 15                  | 220                                       | 440                                        |
| 640 x 360                      | 15                  | 400                                       | 800                                        |
| 360 x 360                      | 15                  | 260                                       | 520                                        |
| 640 x 360                      | 30                  | 600                                       | 1200                                       |
| 360 x 360                      | 30                  | 400                                       | 800                                        |
| 480 x 360                      | 15                  | 320                                       | 640                                        |
| 480 x 360                      | 30                  | 490                                       | 980                                        |
| 640 x 480                      | 15                  | 500                                       | 1000                                       |
| 480 x 480                      | 15                  | 400                                       | 800                                        |
| 640 x 480                      | 30                  | 750                                       | 1500                                       |
| 480 x 480                      | 30                  | 600                                       | 1200                                       |
| 848 x 480                      | 15                  | 610                                       | 1220                                       |
| 848 x 480                      | 30                  | 930                                       | 1860                                       |
| 640 x 480                      | 10                  | 400                                       | 800                                        |
| 1280 x 720                     | 15                  | 1130                                      | 2260                                       |
| 1280 x 720                     | 30                  | 1710                                      | 3420                                       |
| 960 x 720                      | 15                  | 910                                       | 1820                                       |
| 960 x 720                      | 30                  | 1380                                      | 2760                                       |
| 1920 x 1080                    | 15                  | 2080                                      | 4160                                       |
| 1920 x 1080                    | 30                  | 3150                                      | 6300                                       |
| 1920 x 1080                    | 60                  | 4780                                      | 6500                                       |
| 2560 x 1440                    | 30                  | 4850                                      | 6500                                       |
| 2560 x 1440                    | 60                  | 6500                                      | 6500                                       |
| 3840 x 2160                    | 30                  | 6500                                      | 6500                                       |
| 3840 x 2160                    | 60                  | 6500                                      | 6500                                       |


### API Reference
* [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
* For more information on the video orientation mode, see [Rotate the Video](../../en/Video/rotation_guide_android.md).

## Considerations

- Setting [`degradationPreference`](https://docs.agora.io/en/Video/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_video_encoder_configuration.html#a491316b0de64bf930938404b113f062f) as `MAINTAIN_QUALITY` means that the SDK degrades the frame rate under limited bandwidth so as to maintain the video quality. Developers can set the `minFrameRate` parameter to balance the frame rate and video quality under unreliable network connections:

	- When  `minFrameRate` is relatively low, the frame rate degrades significantly, so the poor network conditions have little impact on the video quality.
	- When `minFrameRate` is relatively high, the frame rate degrades within a limited range, so the poor network conditions can have huge impact on the video quality.

Do not set the `minFrameRate` parameter to a value greater than `frameRate`. The default value of `minFrameRate` is experiment verified and can satisfy most use scenarios. We do not recommend changing it.
- If you do not need to set the video profile after joining the channel, you can call the `setVideoEncoderConfiguration` method before the `enableVideo` method to reduce the render time of the first video frame.
- The Agora SDK may adjust the parameters under poor network conditions. 
-  A live broadcast channel generally requires a higher bitrate for better video quality. Therefore, Agora recommends setting the bitrate in the live broadcast profile to twice of that in the communication profile. See [Set the bitrate](https://docs.agora.io/en/Video/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#af10ca07d888e2f33b34feb431300da69).
- For better video quality during a live broadcast, a stable network connection is recommended.
- Setting parameters in the `setVideoEncoderConfiguration` method may affect your bill. For more information, see [Pricing and Billing](https://docs.agora.io/en/faq/video_billing).

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
