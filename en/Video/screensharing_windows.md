
---
title: Share the Screen
description: 
platform: Windows
updatedAt: Tue Nov 27 2018 06:25:39 GMT+0000 (UTC)
---
# Share the Screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances communication by bringing whatever is on the speaker's screen to the other speakers or audience in the channel.

Screen share has extensive application in the following scenarios:

- For a video conference, the speaker can share the image of the local file, web page, and PPT with other users in the channel.
- For an online class, the teacher can share the image of the slides or notes with the students.

## Implementations

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/windows_video.md) for details.

```cpp
//cpp
// 1. start screensharing

if (lpRect != NULL) { // share some area on window
	rect.left = lpRect->left;
	rect.right = lpRect->right;
	rect.top = lpRect->top;
	rect.bottom = lpRect->bottom;

	lpAgoraEngine->startScreenCapture(hWnd, nFPS, &rect, nBitrate);
} else { // share the window by window id(hWnd), 0 means the full screen
	lpAgoraEngine->startScreenCapture(hWnd, nCapFPS, NULL, nBitrate);
}

// 2. stop screensharing

lpAgoraEngine->stopScreenCapture();
```

**Relevant APIs and descriptions**
* [`startScreenCapture`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#af71935ad435402f776bcfc2be3cf687f)
* [`stopScreenCapture`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a77412ab7c8653289a28212e60bd00673)
* [`updateScreenCaptureRegion`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a99ce13ce3b9b2c65e5ec35b9861b56e3)
