
---
title: Custom Video Source and Renderer
description: 
platform: Unity
updatedAt: Fri Dec 13 2019 09:31:48 GMT+0800 (CST)
---
# Custom Video Source and Renderer
## Introduction

By default, the Agora RTC SDK enables default audio and video modules for capturing and rendering in real-time communications.

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This article tells you how to use the Agora RTC SDK for Unity to customize the video source and renderer.

## Implementation

Before customizing the video source or renderer, ensure that you have implemented the basic real-time communication functions in your project. See [Start a Video Call](https://docs.agora.io/en/Video/start_call_unity?platform=Unity) or [Start a Video Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/start_live_unity?platform=Unity) for details.

Refer to the following steps to customize the video source and renderer in your project:

1. Call `SetExternalVideoSource` before `JoinChannelByKey` to enable the external video source.
2. Manage the capturing and processing of the specified video data on your own.
3. Send the video data back to the SDK using the `PushVideoFrame` method.

The following steps show how to implement the screen sharing function by customizing the video source or renderer.

1. Specify the external video source by calling `SetExternalVideoSource` before `JoinChannelByKey`.

   ```C#
	 mRtcEngine.SetExternalVideoSource(true, false);
	 ```

2. Define the Texture2D, and use Texture2D to read screen pixels as the external video source.

   ```C#
 	 mRect = new Rect(0, 0, Screen.width, Screen.height);
   mTexture = new Texture2D((int)mRect.width, (int)mRect.height, TextureFormat.RGBA32, false);
   mTexture.ReadPixels(mRect, 0, 0);
   mTexture.Apply();
	 ```
   
3. Call `PushVideoFrame` to send the video source to the SDK for implementing the screen sharing function.

   ```C#
	 int a = rtc.PushVideoFrame(externalVideoFrame);
	 ```
   

### API call sequence

The following diagram shows the detailed API call sequence for customizing the video source and renderer.

![](https://web-cdn.agora.io/docs-files/1576229371972)

### Sample code

Refer to the following code to customize the video source in your project, for such as screen sharing function.

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using agora_gaming_rtc;
using UnityEngine.UI;
using System.Globalization;
using System.Runtime.InteropServices;
using System;

public class ShareScreen : MonoBehaviour
{
Texture2D mTexture;
Rect mRect;
[SerializeField]
private string appId = "Your_AppID";
[SerializeField]
private string channelName = "agora";
public IRtcEngine mRtcEngine;
int i = 100;
void Start()
{
Debug.Log("ScreenShare Activated");
mRtcEngine = IRtcEngine.GetEngine(appId);
// Set the output log level of the SDK.
mRtcEngine.SetLogFilter(LOG_FILTER.DEBUG | LOG_FILTER.INFO | LOG_FILTER.WARNING | LOG_FILTER.ERROR | LOG_FILTER.CRITICAL);
// Enable the video module.
mRtcEngine.EnableVideo();
// Enable the video observer.
mRtcEngine.EnableVideoObserver();
// Configure the external video source.
mRtcEngine.SetExternalVideoSource(true, false);
// Join a channel
mRtcEngine.JoinChannel(channelName, null, 0);
// Create a rectangular region of the screen.
mRect = new Rect(0, 0, Screen.width, Screen.height);
// Create a texture of the rectangle you create.
mTexture = new Texture2D((int)mRect.width, (int)mRect.height, TextureFormat.RGBA32, false);
}

void Update()
{
 shareScreen();
}

// Start to share the screen.
void shareScreen()
{
// Read the Pixels of the rectangle you create.
mTexture.ReadPixels(mRect, 0, 0);
// Apply the Pixels read from the rectangle to the texture.
mTexture.Apply();
// Get the Raw Texture data from the texture and apply it to an array of bytes.
byte[] bytes = mTexture.GetRawTextureData();
// Give enough space for the bytes array.
int size = Marshal.SizeOf(bytes[0]) * bytes.Length;
// Check whether the IRtcEngine instance is existed.
IRtcEngine rtc = IRtcEngine.QueryEngine();

if (rtc != null)
{
// Create a new external video frame.
ExternalVideoFrame externalVideoFrame = new ExternalVideoFrame();
// Set the buffer type of the video frame.
externalVideoFrame.type = ExternalVideoFrame.VIDEO_BUFFER_TYPE.VIDEO_BUFFER_RAW_DATA;
// Set the format of the video pixel.
externalVideoFrame.format = ExternalVideoFrame.VIDEO_PIXEL_FORMAT.VIDEO_PIXEL_UNKNOWN;
// Apply raw data.
externalVideoFrame.buffer = bytes;
// Set the width (pixel) of the video frame.
externalVideoFrame.stride = (int)mRect.width;
// Set the height (pixel) of the video frame.
externalVideoFrame.height = (int)mRect.height;
// Remove pixels from the sides of the frame
externalVideoFrame.cropLeft = 10;
externalVideoFrame.cropTop = 10;
externalVideoFrame.cropRight = 10;
externalVideoFrame.cropBottom = 10;
// Rotate the video frame (0, 90, 180, or 270)
externalVideoFrame.rotation = 180;
// Increment i with the video timestamp.
externalVideoFrame.timestamp = i++;
// Push the external video frame with the frame you create.
int a = rtc.PushVideoFrame(externalVideoFrame);
}
}
}
```

### API reference

- SetExternalVideoSource
- PushVideoFrame
