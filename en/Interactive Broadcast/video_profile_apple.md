
---
title: Set the Video Profile
description: 
platform: iOS,macOS
updatedAt: Mon Jul 06 2020 06:38:18 GMT+0800 (CST)
---
# Set the Video Profile
## Introduction

You can set the video profile, either before or after a user joins a channel, for the user to enjoy better video quality during a video call or live interactive streaming.

The Agora SDK uses the `setVideoEncoderConfiguration` method to set the video profile. Each video profile corresponds to a set of video parameters, including the resolution, frame rate, bitrate, and video orientation.

## Implementation

Before setting the video profile, ensure that you have implemented the basic real-time communication functions in your project. For details, see the following documents:
- iOS: [Start a Video Call](../../en/Interactive%20Broadcast/start_call_ios.md) or [Start a Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_ios.md)
- macOS: [Start a Video Call](../../en/Interactive%20Broadcast/start_call_mac.md) or [Start a Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_mac.md)

After initializing `AgoraRtcEngine`, you can call the `setVideoEncoderConfiguration` method to set the video profile and set the video resolution, frame rate, birtate and orientation mode.

### API call sequence

Refer to the following diagram to set the video profile in your project:

![](https://web-cdn.agora.io/docs-files/1568871728079)

You can also choose when to call the `setVideoEncoderConfiguration` method according to your scenarios:

- Call this method after `enableVideo` and before `joinChannelByToken` to set the local video encoding parameters before joining the channel.
- If you do not set the video profile after joining the channel, we recommend calling this method before `enableVideo` to reduce the render time of the first video frame.
- You can also call this method after joining the channel to update the video profile in real time.


### Sample Code

```swift
// swift
// Set a VideoEncoderConfiguration instance. See the descriptions of the parameters in API Reference.
let config = AgoraVideoEncoderConfiguration(size: size, frameRate: frameRate, bitrate: bitrate, orientationMode: orientationMode, degradationPreference: degradationPreference)

agoraKit.setVideoEncoderConfiguration(config)
```

```objective-c
// objective-c
// Set a VideoEncoderConfiguration instance. See the descriptions of the parameters in API Reference.
AgoraVideoEncoderConfiguration *config = [[AgoraVideoEncoderConfiguration alloc] initWithSize: size frameRate: frameRate bitrate: bitrate orientationMode: AgoraVideoOutputOrientationModeAdaptative degradationPreference: AgoraDegradationMaintainQuality];

[agoraKit setVideoEncoderConfiguration: config];
```

We provide an open-source One-to-One-Video demo project on GitHub. You can try the demo and view the source code of the `setupVideo` method in the following files:

- iOS
	- Swift: The [VideoChatViewController.swift](https://github.com/AgoraIO/Basic-Video-Call/blob/master/One-to-One-Video/Agora-iOS-Tutorial-Swift-1to1/Agora-iOS-Tutorial/VideoChatViewController.swift) file
	- Objective-C: The [VideoChatViewController.m](https://github.com/AgoraIO/Basic-Video-Call/blob/master/One-to-One-Video/Agora-iOS-Tutorial-Objective-C-1to1/Agora-iOS-Tutorial-Objective-C/VideoChatViewController.m) file

- macOS
	- Swift: The [VideoChatViewController.swift](https://github.com/AgoraIO/Basic-Video-Call/blob/master/One-to-One-Video/Agora-macOS-Tutorial-Swift-1to1/Agora-Mac-Tutorial-Swift/VideoChatViewController.swift) file
	- Objective-C: The [VideoChatViewController.m](https://github.com/AgoraIO/Basic-Video-Call/blob/master/One-to-One-Video/Agora-macOS-Tutorial-Objective-C-1to1/Agora-Mac-Tutorial-Objective-C/VideoChatViewController.m) file

### API Reference
* [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:)

## Considerations

- Setting [`degradationPreference`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/degradationPreference) as `AgoraDegradationMaintainQuality` means that the SDK degrades the frame rate under limited bandwidth so as to maintain the video quality. Developers can set the `minFrameRate` parameter to balance the frame rate and video quality under unreliable network connections:

	- When  `minFrameRate` is relatively low, the frame rate degrades significantly, so the poor network conditions have little impact on the video quality.
	- When `minFrameRate` is relatively high, the frame rate degrades within a limited range, so the poor network conditions can have huge impact on the video quality.

 Do not set the `minFrameRate` parameter to a value greater than `frameRate`. The default value of `minFrameRate` is experiment verified and can satisfy most use scenarios. We do not recommend changing it.
- If you do not need to set the video profile after joining the channel, you can call the `setVideoEncoderConfiguration` method before the `enableVideo` method to reduce the render time of the first video frame.
- The Agora SDK may adjust the parameters under poor network conditions. 
- A live interactive streaming channel generally requires a higher bitrate for better video quality. Therefore, Agora recommends setting the bitrate in the `LiveBroadcasting` profile to twice of that in the `Communication` profile. See [Set the bitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate).
- For better video quality during the live interactive streaming, a stable network connection is recommended.
- Setting parameters in the `setVideoEncoderConfiguration` method may affect your bill. For more information, see [Billing](../../en/Interactive%20Broadcast/billing_rtc.md).

## Recommended video profiles

Video profiles vary from case to case. For example, in a one-to-one online class, the video windows of the teacher and student are both large, which requires higher resolutions, frame rates, and bitrates. While in a one-to-four online class, the video windows of the teacher and students are smaller, so lower resolutions, frame rates, and bitrates are used to accommodate the downward bandwidth.

 The following profiles for different scenarios are recommended:

- One-to-one video call: 
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps
  - Resolution: 640 x 360; frame rate: 15 fps; bitrate: 400 Kbps
- One-to-many video call: 
  - Resolution: 160 x 120; frame rate: 15 fps; bitrate: 65 Kbps
  - Resolution: 320 x 180; frame rate: 15 fps; bitrate: 140 Kbps
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps 

You can also customize the video parameters with the `setVideoEncoderConfiguration` method, such as increasing the bitrate to ensure the video quality according to the table below.

<div class="alert note">Video profiles with a resolution above 1920 x 1080 (included) apply to macOS only.</div>

| Resolution<br>(width x height) | Frame rate<br>(fps) | Base bitrate<br>(Kbps, for `Communication`) | Live Bitrate<br>(Kbps, for `LiveBroadcasting`) |
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

