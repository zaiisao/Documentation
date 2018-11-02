
---
title: Video Rotation Guide
description: 
platform: iOS
updatedAt: Thu Nov 01 2018 09:19:18 GMT+0000 (UTC)
---
# Video Rotation Guide
# Video Rotation Guide

From v2.3.0, Agora provides a `setVideoEncoderConfiguration` API for users to set the video profile. This API includes an `orientationMode` parameter with which users can set the video orientation mode.

This guide shows you how to set the video orientation mode in various scenarios.

## Video Rotation

The following figure shows how the Agora SDK captures, processes, and outputs videos.

<img alt="../_images/rotation_encoding_decoding.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_encoding_decoding.jpg" style="width: 648.5px; height: 268.0px;"/>

Video rotation involves the video capturer and the video player.

### Video Capturer

The video capturer captures the video and outputs the video information, including the

- Video frame
- Relative position of the video and the status bar

### Video Player

The video player renders the received video information.

## setVideoEncoderConfiguration API

After you have joined the Agora channel, use the `setVideoEncoderConfiguration` API to set your preferred video profile.

```objective-c
- (int)setVideoEncoderConfiguration:(AgoraVideoEncoderConfiguration * _Nonnull)config;
```

The `AgoraVideoEncoderConfiguration` class includes an `orientationMode` parameter, with which you can set the rotation mode of your video. Agora recommends using the flowchart to select your video orientation mode \(applicable to both communication and live broadcast scenarios\).

<img alt="../_images/rotation_mode.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_mode.jpg" style="width: 744.66px; height: 407.7px;"/>

## Orientation Mode

The `orientationMode` parameter provides three modes, [Adaptive](#adaptive), [Fixed\_Landscape](#fixedl), and [Fixed\_Portrait](#fixedp) for different user needs. **The relative position of the video and the status bar on the video capturer and the player remain the same for all modes.**

### <a name = "adaptive"></a>Adaptive

In the Adaptive mode, the video capturer captures the video frame and sends out the video together with its relative position to the status bar. The player notes the relative position and renders the video frame. No video cropping occurs in the Adaptive mode.

The following figures show the video orientations at the video capturer and player when a rear camera is used as the video capturer. Note that the video orientation differs according to the UI lock of your app.

- UI Lock \(or UI Unlock with the app disabling the screen auto-rotation\):

  The relative position of the status bar remains the same as the screen and not according to the phone tilt \(for example in WeChat\). Therefore, the relative position of the video and the screen remains the same for the video capturer and the player.

  <img alt="../_images/rotation_adaptive_uilock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uilock_landscape.jpg" style="width: 786.0px; height: 280.2px;"/>

  <img alt="../_images/rotation_adaptive_uilock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uilock_portrait.jpg" style="width: 738.6px; height: 273.6px;"/>

- UI Unlock with the app enabling the screen auto-rotation:

  The status bar of the app remains horizontal, regardless of the orientation of the screen \(for example in Facetime\). Therefore, the relative position of the video and the phone tilt remains the same for the video capturer and the player.

  <img alt="../_images/rotation_adaptive_uiunlock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uiunlock_landscape.jpg" style="width: 797.4px; height: 283.8px;"/>

  <img alt="../_images/rotation_adaptive_uiunlock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uiunlock_portrait.jpg" style="width: 741.0px; height: 281.4px;"/>



### <a name = "fixedl"></a>Fixed\_Landscape

In the Fixed\_Landscape mode, the video capturer sends out the video in the landscape orientation relative to the status bar and video cropping may be necessary. The player renders the received video frame directly without rotating the video.

> In this mode, both sides of the captured video will be cropped in the direction perpendicular to the status bar.

The following figures show the video orientations at the video capturer and the player when a rear camera is used as the video capturer.

- If the captured video is in the landscape mode \(where video cropping is not needed\):

  <img alt="../_images/rotation_fixed_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_landscape.jpg" style="width: 918.0px; height: 262.8px;"/>

- If the captured video is in the portrait mode \(where video cropping **is** needed\):

  <img alt="../_images/rotation_fixed_landscape_cut.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_landscape_cut.jpg" style="width: 864.6px; height: 281.4px;"/>



### <a name = "fixedp"></a>Fixed\_Portrait

In the Fixed\_Portrait mode, the video capturer sends out the video in the portrait orientation relative to the status bar and video cropping may be necessary. The player renders the received video frame directly without rotating the video.

> In this mode, both sides of the captured video will be cropped in the direction parallel to the status bar.

The following figures show the video orientations at the video capturer and the player when a rear camera is used as the video capturer.

- If the captured video is in the portrait mode \(where video cropping is not needed\):

  <img alt="../_images/rotation_fixed_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_portrait.jpg" style="width: 855.0px; height: 274.8px;"/>

- If the captured video is in the landscape mode \(where video cropping **is** needed\):

  <img alt="../_images/rotation_fixed_portrait_cut.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_portrait_cut.jpg" style="width: 912.0px; height: 265.8px;"/>



See the description of the `setVideoEncoderConfiguration` API in [Video Call API](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/index.html).
