
---
title: Video-related Questions
description: 
platform: All Platforms
updatedAt: Thu Nov 22 2018 07:39:44 GMT+0000 (UTC)
---
# Video-related Questions
### How to set the video orientation mode?

From v2.3.0, Agora provides a `setVideoEncoderConfiguration` API for users to set the video profile. This API includes an `orientationMode` parameter with which users can set the video orientation mode.

The following figure shows how the Agora SDK captures, processes, and outputs videos.

<img alt="../_images/rotation_encoding_decoding.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_encoding_decoding.jpg" />

Video rotation involves the video capturer and the video player.

* Video Capturer: The video capturer captures the video and outputs the video information, including the video frame and the relative position of the video and the status bar.
* Video Player: The video player renders the received video information.

The `orientationMode` parameter provides three modes, ADAPTIVE, FIXED_LANDSCAPE and FIXED_PORTRAIT for different user needs. **The relative position of the video and the status bar on the video capturer and the player remain the same for all modes.**

Agora recommends using the flowchart to select your video orientation mode (applicable to both communication and live broadcast scenarios).

<img alt="../_images/rotation_mode.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_mode.jpg" />

#### ADAPTIVE

In the Adaptive mode, the video capturer captures the video frame and sends out the video together with its relative position to the status bar. The player notes the relative position and renders the video frame. No video cropping occurs in the Adaptive mode.

The following figures show the video orientations at the video capturer and player when a rear camera is used as the video capturer. Note that the video orientation differs according to the UI lock of your app.

-   UI Lock \(or UI Unlock with the app disabling the screen auto-rotation\):

    The relative position of the status bar remains the same as the screen and not according to the phone tilt \(for example in Wechat\). Therefore, the relative position of the video and the screen remains the same for the video capturer and the player.
		
    For a Landscape capturer:

    <img alt="../_images/rotation_adaptive_uilock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uilock_landscape.jpg" />

    For a Portrait capturer:
   
    <img alt="../_images/rotation_adaptive_uilock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uilock_portrait.jpg" />


-   UI Unlock with the app enabling the screen auto-rotation:

    The status bar of the app remains horizontal, regardless of the orientation of the screen \(for example in Facetime\). Therefore, the relative position of the video and the phone tilt remains the same for the video capturer and the player.

    For a Landscape capturer:

    <img alt="../_images/rotation_adaptive_uiunlock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uiunlock_landscape.jpg" />

    For a Portrait capturer:

    <img alt="../_images/rotation_adaptive_uiunlock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uiunlock_portrait.jpg" />


#### FIXED_LANDSCAPE

In the Fixed\_Landscape mode, the video capturer sends out the video in the landscape orientation relative to the status bar and video cropping may be necessary. The player renders the received video frame directly without rotating the video.

> In this mode, both sides of the captured video will be cropped in the direction perpendicular to the status bar.

The following figures show the video orientations at the video capturer and the player when a rear camera is used as the video capturer.

-   If the captured video is in the landscape mode \(where video cropping is not needed\):

    <img alt="../_images/rotation_fixed_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_landscape.jpg" />


-   If the captured video is in the portrait mode \(where video cropping **is** needed\):

    <img alt="../_images/rotation_fixed_landscape_cut.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_landscape_cut.jpg" />

#### FIXED_PORTRAIT

In the Fixed\_Portrait mode, the video capturer sends out the video in the portrait orientation relative to the status bar and video cropping may be necessary. The player renders the received video frame directly without rotating the video.

> In this mode, both sides of the captured video will be cropped in the direction parallel to the status bar.

The following figures show the video orientations at the video capturer and the player when a rear camera is used as the video capturer.

-   If the captured video is in the portrait mode \(where video cropping is not needed\):

    <img alt="../_images/rotation_fixed_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_portrait.jpg" />


-   If the captured video is in the landscape mode \(where video cropping **is** needed\):

    <img alt="../_images/rotation_fixed_portrait_cut.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_portrait_cut.jpg" style="width: 912.0px; height: 265.8px;"/>

