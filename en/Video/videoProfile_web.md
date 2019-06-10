
---
title: Set the Video Profile
description: 
platform: Web
updatedAt: Mon Jun 10 2019 03:42:52 GMT+0800 (CST)
---
# Set the Video Profile
## Introduction

You can set the video profile, either before or after a user joins a channel, for the user to enjoy better video quality during a video call or live broadcast.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/web_prepare.md).

The Agora SDK uses the `setVideoProfile` method to set the video profile. Each video profile corresponds to a set of video parameters, including the resolution, frame rate, and bitrate.

The parameters specified in the `setVideoProfile` method are ideal values under ideal network conditions. If the video engine cannot render the video with the specified parameters due to poor network conditions, the parameters further down the list are considered.

```javascript
// javascript
// Set the video profile before initializing the local stream.
localStream.setVideoProfile("480p_3");
localStream.init(function(){
	// init successful
});
```

### API Reference

* [`setVideoProfile`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#setvideoprofile)

## Considerations
* Resolution support varies from browser to browser. See [Video Profile Definition](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.stream.html#setvideoprofile) for details.
* Due to limitations of some devices and web browsers, the resolution you set may fail to take effect and be adjusted by the web browser. In this case, your charges will be calculated based on the actual resolution.
* Whether 1080 resolution or above can be supported depends on the device. If the device cannot support 1080p, the frame rate will be lower than the set value.
* The Safari browser has a fixed video frame rate of 30 fps and does not support customization.

## Frequently Asked Questions
### How do I choose the video resolution, frame rate, and bitrate?

Video profiles vary from case to case. For example, in a one-to-one online class, the video windows of the teacher and student are both large, which requires higher resolutions, frame rates, and bitrates. While in a one-to-four online class, the video windows of the teacher and students are smaller, so lower resolutions, frame rates, and bitrates are used to accommodate the downward bandwidth.

 The following profiles for different scenarios are recommended:

- One-to-one video call: 
  - 240p (resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps)
  - 360p (resolution: 640 x 360; frame rate: 15 fps; bitrate: 400 Kbps)
- One-to-many video call: 
  - 120p (resolution: 160 x 120; frame rate: 15 fps; bitrate: 65 Kbps)
  - 180p (resolution: 320 x 180; frame rate: 15 fps; bitrate: 140 Kbps)
  - 240p (resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps) 

You can also customize the video parameters with the `setVideoEncoderConfiguration` method, such as increasing the bitrate to ensure the video quality.
