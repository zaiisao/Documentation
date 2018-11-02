
---
title: Publish and Subscribe to Streams
description: 
platform: Windows
updatedAt: Fri Nov 02 2018 04:08:48 GMT+0000 (UTC)
---
# Publish and Subscribe to Streams
## Enable the video mode
Call <code>enableVideo</code> to enable the video mode. The voice function is enabled by default in the Agora SDK, so you can call this method before or after joining in the channel.

-   If you enable the video mode before joining the channel, you enter directly into a video broadcast.
-   If you enable the video mode after joining the channel, the voice broadcast switches to a video broadcast.


```
nRet = m_lpAgoraEngine->enableVideo();
```

## Set the video profile
After the video mode is enabled, call <code>setVideoProfile</code> to set the video encoding profile.

In this method, secify the video encoding resolution, frame rate bitrate and orientation mode. See <code>setChannelProfile</code> for more information.

> -   The parameters specified in this method are the maximum values under ideal network conditions. If the video engine cannot render the video using the specified parameters due to poor network conditions, the parameters further down the list are considered until success.

> -   If the device camera does not support the resolution specified by the video profile, the SDK automatically chooses a suitable camera resolution. This does not change the encoder resolution, and encoding will still use the resolution specified by <code>setVideoProfile</code> .


```
int nVideoSolution = m_dlgSetup.GetVideoSolution();
lpAgoraEngine->setVideoProfile((VIDEO_PROFILE_TYPE)nVideoSolution, m_dlgSetup.IsWHSwap());
```

## Set the local video view 
The local video view is the display of the local video streams on the user’s local device.

Call the <code>setupLocalVideo</code> method before entering into a channel to bind the application with the video window of the local stream and configure the local video display.

In this method:

-   Choose the display window of the local video streams.

-   Specify the video display mode.

    -   Hidden mode: The SDK scales the video until it fills the visible view boundaries. One dimension of the video may be clipped.
    -   Fit mode: The SDK scales the video until one of its dimensions fits the boundaries. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.

-   Pass the UID of the local user. The default value is 0.


```
VideoCanvas vc;

vc.uid = 0;
vc.view = m_dlgVideo.GetLocalVideoWnd();
vc.renderMode = RENDER_MODE_TYPE::RENDER_MODE_FIT;

lpRtcEngine->setupLocalVideo(vc);
```


## Set the remote local view
The remote video view is the display of the remote video streams on the user’s local device.

Call the <code>setupRemoteVideo</code> method to configure the remote video display.

In this method:

-   Choose the display window of the remote video streams.

-   Specify the video display mode.

    -   Hidden mode: The SDK scales the video until it fills the visible view boundaries. One dimension of the video may be clipped.
    -   Fit mode: The SDK scales the video until one of its dimensions fits the boundaries. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.

-   Pass the UID of the remote user. If the remote UID is unknown to the application, set it later when the application receives the <code>onUserJoined</code> event.


```
canvas.uid = agvWndInfo.nUID;
canvas.view = m_wndVideo[nIndex].GetSafeHwnd();

CAgoraObject::GetEngine()->setupRemoteVideo(canvas);
```
