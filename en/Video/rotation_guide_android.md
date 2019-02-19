
---
title: Rotate the Video
description: 
platform: Android,Windows
updatedAt: Tue Dec 11 2018 17:41:17 GMT+0000 (UTC)
---
# Rotate the Video
The Agora SDK v2.3.0+ provides a `setVideoEncoderConfiguration` method for users to set the video profile. This method includes an `orientationMode` parameter for users to set the video orientation mode.

This page shows you how to set the video orientation mode in various scenarios.

## Video Rotation

The following figure shows how the Agora SDK captures, processes, and outputs videos.

<img alt="../_images/rotation_encoding_decoding.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_encoding_decoding.jpg" style="width: 840px; "/>


Video rotation involves the video capturer and the video player.

### Video Capturer

The video capturer captures the video and outputs the video information, including the:

-   Video frame.

-   Relative position of the video and the status bar.


### Video Player

The video player renders the received video information.

## setVideoEncoderConfiguration

After you have joined the Agora channel, use the `setVideoEncoderConfiguration` method to set your preferred video profile.

```
public abstract int setVideoEncoderConfiguration(VideoEncoderConfiguration config);
```

The `VideoEncoderConfiguration` class includes an `orientationMode` parameter to set the rotational mode of your video. Agora recommends using the following flowchart to select your video orientation mode \(applicable to both Communication and Live Broadcast scenarios\).

<img alt="../_images/rotation_mode.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_mode.jpg" style="width: 840px; "/>


## Orientation Mode

The `orientationMode` parameter provides three modes, [ADAPTIVE](#orientation_adaptive), [FIXED_LANDSCAPE](#orientation_fixed_landscape), and [FIXED_PORTRAIT](#orientation_fixed_portrait) for different user needs. **The relative position of the video and the status bar on the video capturer and the player remain the same for all modes.**

<a name = "orientation_adaptive"></a>
### ADAPTIVE

In the adaptive mode, the video capturer captures the video frame and sends the video together with its relative position to the status bar. The player notes the relative position and renders the video frame. 

The following figures show the video orientations at the video capturer and player when a rear camera is used as the video capturer. Note that the video orientation differs according to the UI lock of your app.

-   UI lock \(or UI unlock with the app disabling the screen auto-rotation\):

    The relative position of the status bar remains the same as the screen and not according to the phone tilt \(for example in WeChat\). Therefore, the relative position of the video and the screen remains the same for the video capturer and the player.
		
    For a landscape capturer:

  <img alt="../_images/rotation_adaptive_uilock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uilock_landscape.jpg" />

    For a portrait capturer:
   
    <img alt="../_images/rotation_adaptive_uilock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uilock_portrait.jpg" />


-   UI unlock with the app enabling the screen auto-rotation:

    The status bar of the app remains horizontal, regardless of the orientation of the screen \(for example in Facetime\). Therefore, the relative position of the video and the phone tilt remains the same for the video capturer and the player.

    For a landscape capturer:

    <img alt="../_images/rotation_adaptive_uiunlock_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uiunlock_landscape.jpg" />

    For a portrait capturer:

    <img alt="../_images/rotation_adaptive_uiunlock_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_adaptive_uiunlock_portrait.jpg" />


<a name = "orientation_fixed_landscape"></a>
### FIXED_LANDSCAPE

In the Fixed\_Landscape mode, the video capturer sends the video in the landscape orientation relative to the status bar and video cropping may be necessary. The player renders the received video frame directly without rotating the video.

> In this mode, both sides of the captured video will be cropped in the direction perpendicular to the status bar.

The following figures show the video orientations at the video capturer and the player when a rear camera is used as the video capturer.

-   The captured video in the landscape mode \(video cropping is not needed\):

    <img alt="../_images/rotation_fixed_landscape.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_landscape.jpg" />


-   The captured video in the portrait mode \(video cropping **is** needed\):

    <img alt="../_images/rotation_fixed_landscape_cut.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_landscape_cut.jpg" />

<a name = "orientation_fixed_portrait"></a>
### FIXED_PORTRAIT

In the Fixed\_Portrait mode, the video capturer sends out the video in the portrait orientation relative to the status bar and video cropping may be necessary. The player renders the received video frame directly without rotating the video.

> In this mode, both sides of the captured video will be cropped in the direction parallel to the status bar.

The following figures show the video orientations at the video capturer and the player when a rear camera is used as the video capturer.

-   The captured video in the portrait mode \(video cropping is not needed\):

    <img alt="../_images/rotation_fixed_portrait.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_portrait.jpg" />


-   The captured video in the landscape mode \(video cropping **is** needed\):

    <img alt="../_images/rotation_fixed_portrait_cut.jpg" src="https://web-cdn.agora.io/docs-files/en/rotation_fixed_portrait_cut.jpg" />


See the description of the `setVideoEncoderConfiguration` method in [Video Call API](https://docs.agora.io/en/Video/API%20Reference/java/index.html) for more information.
