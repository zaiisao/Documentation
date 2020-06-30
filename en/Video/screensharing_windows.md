
---
title: Share the Screen
description: 
platform: Windows
updatedAt: Mon Jun 29 2020 02:56:42 GMT+0800 (CST)
---
# Share the Screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances communication by displaying the speaker's screen on the display of other speakers or audience members in the channel.

Screen sharing is applied in the following scenarios:

- In a video conference, the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- In an online class, the teacher can share the slides or notes with students.

## Implementation

Ensure that you implement a video call or an interactive broadcast in your project. For details, see [Start a Call](../../en/Video/start_call_windows.md) or [Start an Interactive Broadcast](../../en/Video/start_live_windows.md).

From v2.4.0, Agora supports the following screen sharing functions on Windows:

- Shares the whole or part of a screen by specifying `screenRect`.
- Shares the whole or part of a window by specifying `windowId`.

### Share the whole or part of a screen by specifying screenRect

Windows lays all its displays on a virtual screen. Developers need to get the relative position of the display on the virtual screen to get the view on the display. By obtaining the screen rect of the display, we can implement screen sharing on Windows with the following steps:

1. Get the screen rect for screen sharing.

	```c++
	bool result = true;
	int index;
	for (index = 0;; index++) {
			DISPLAY_DEVICE device;
			device.cb = sizeof(device);
			result = EnumDisplayDevices(NULL, index, &device, 0);
			if (!result) break;

			if (!(device.StateFlags & DISPLAY_DEVICE_ATTACHED_TO_DESKTOP)) {
					continue;
		 }

			DEVMODE device_mode;
			device_mode.dmSize = sizeof(DEVMODE);
			device_mode.dmDriverExtra = 0;
			EnumDisplaySettingsEx(device.DeviceName, ENUM_CURRENT_SETTINGS, &device_mode, 0);
			printf("device name:%ls\n", device.DeviceName);
			printf("x: %d\n", device_mode.dmPosition.x);
			printf("y: %d\n", device_mode.dmPosition.y);
			printf("w: %d\n", device_mode.dmPelsWidth);
			printf("h: %d\n", device_mode.dmPelsHeight);
	}
	```

	> For more information on screenRect, see [Microsoft EnumDisplayDevicesA](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-enumdisplaydevicesa).

2. Share the screen by specifying the screen rect.

	```c++
	// Specifies the screen or window.
	RECT rc;
	agora::rtc::Rectangle rcCap;
	agora::rtc::Rectangle screenRegion = { rc.left, rc.right, rc.right - rc.left, rc.bottom - rc.top };
	capParam.dimensions.width = rc.right - rc.left;
	capParam.dimensions.height = rc.bottom - rc.top;

	::GetWindowRect(hWnd, &rc);
	ScreenCaptureParameters capParam;
	capParam.dimensions.width = rc.right - rc.left;
	capParam.dimensions.height = rc.bottom - rc.top;

	VideoContentHint contentHint;
	agora::rtc::Rect rt;

	// Starts screen sharing.
	ret = m_lpAgoraEngine->startScreenCaptureByScreenRect(screenRegion, rcCap, capParam);

	// Updates the screen sharing parameters.
	m_lpAgoraEngine->updateScreenCaptureParameters(capParam);

	// Updates the screen sharing region.
	m_lpAgoraEngine->updateScreenCaptureRegion(screenRegion);

	// Sets the content hint for screen sharing.
	m_lpAgoraEngine->setScreenCaptureContentHint(contentHint);

	// Stops screen sharing.
	m_lpAgoraEngine->stopScreenCapture();
	```

### Share the whole or part of a window by specifying windowId

Windows assigns a unique window identifier (windowId) for each window with a data type of HWND. To achieve compatibility with the x86 and x64 operating systems, it is in the format of view_t. With this window ID, we can implement window sharing on Windows with the following steps:

1. Get the window ID for window sharing.

	```c++
	BOOL CALLBACK EnumProc(HWND hWnd, LPARAM IParam)
	{
			// Gets the windowId of the visible window. Popup and menu window is excludes.
			LONG IStyle = ::GetWindowLong(hWnd, GWL_STYLE);
			if ((IStyle&WS_VISIBLE) != 0 && (IStyle&(WAS_POPUP | WA_SYSMENU)) != 0) {
					HWND window_id = hWnd;
		 }
			return TRUE;
	}
	EnumWindows(&EnumProc, NULL);
	```

	> For more information on windowId, see [Microsoft EnumWindows](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-enumwindows).


2. Share the window by specifying the window ID.

	```c++
	// Specifies the screen or window.
	RECT rc;
	agora::rtc::Rectangle rcCap;
	agora::rtc::Rectangle screenRegion = { rc.left, rc.right, rc.right - rc.left, rc.bottom - rc.top };
	capParam.dimensions.width = rc.right - rc.left;
	capParam.dimensions.height = rc.bottom - rc.top;

	::GetWindowRect(hWnd, &rc);
	ScreenCaptureParameters capParam;
	capParam.dimensions.width = rc.right - rc.left;
	capParam.dimensions.height = rc.bottom - rc.top;

	VideoContentHint contentHint;
	agora::rtc::Rect rt;

	// Starts sharing the window.
	ret = m_lpAgoraEngine->startScreenCaptureByWindowId(hWnd, rcCap, capParam);

	// Updates the screen sharing parameters.
	m_lpAgoraEngine->updateScreenCaptureParameters(capParam);

	// Updates the screen sharing region.
	m_lpAgoraEngine->updateScreenCaptureRegion(screenRegion);

	// Sets the content hint for screen sharing.
	m_lpAgoraEngine->setScreenCaptureContentHint(contentHint);

	// Stops screen sharing.
	m_lpAgoraEngine->stopScreenCapture();
	```


### API Reference

* [`startScreenCaptureByWindowId`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)
* [`startScreenCaptureByScreenRect`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)
* [`updateScreenCaptureParameters`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)
* [`setScreenCaptureContentHint`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)
* [`updateScreenCaptureRegion`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ae2ab9c3ff28b64c601f938ab45644586)
* [`stopScreenCapture`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a77412ab7c8653289a28212e60bd00673)

## Enable both screen sharing and video

We provide an open-source [Agora-Screen-Sharing-Windows](https://github.com/AgoraIO/Advanced-Video/tree/master/Windows/Agora-Screen-Sharing-Windows) demo project on GitHub that implements screen sharing and publishing the local video stream. You can download it and refer to the source code.

## Considerations
- v2.4.0 deprecates the `startScreenCapture` method. You can still use it, but we no longer recommend it.
- Changing `AgoraScreenCaptureParameters` may affect your communication chargers. As of v2.4.1, if you set the `dimendions` parameter as default, Agora uses 1920 x 1080 to calculate the charges.
- Sharing the window of a QQ chat on Windows causes the black screen of the shared window.
