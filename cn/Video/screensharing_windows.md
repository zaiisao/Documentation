
---
title: 屏幕共享
description: 
platform: Windows
updatedAt: Thu Jun 11 2020 09:39:09 GMT+0800 (CST)
---
# 屏幕共享
## 功能简介
在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法
在实现屏幕共享前，请确保已在你的项目中实现基本的实时音视频功能。详见[开始音视频通话](../../cn/Video/start_call_windows.md)或[开始互动直播](../../cn/Video/start_live_windows.md)。

Agora 在 v2.4.0 对屏幕共享相关接口进行梳理，目前在 Windows 平台上支持：
- 通过 `screenRect` 共享指定屏幕，或指定屏幕的部分区域
- 通过 `windowId` 共享指定窗口，或指定窗口的部分区域

### 共享指定屏幕

Windows 系统的所有屏幕（display）都画在一整张 virtual screen 上。对用户来说，只有拿到该屏幕在整张 virtual screen 上的相对位置，才能获得该屏幕的画面。通过获取该画面的 `screenRect`，我们可以按如下步骤在 Windows 平台上实现屏幕共享：

1. 获取想要共享窗口的 `screenRect`
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
> 更多关于 screenRect 的详情，请参考 [Microsoft EnumDisplayDevicesA 说明](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-enumdisplaydevicesa)。

2. 通过 `screenRect` 共享屏幕

	```c++
	// 指定共享的屏幕或窗口
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

	// 开始共享屏幕
	ret = m_lpAgoraEngine->startScreenCaptureByScreenRect(screenRegion, rcCap, capParam);

	// 更新屏幕共享编码参数
	m_lpAgoraEngine->updateScreenCaptureParameters(capParam);

	// 更新屏幕共享区域
	m_lpAgoraEngine->updateScreenCaptureRegion(screenRegion);

	// 设置屏幕共享内容类型
	m_lpAgoraEngine->setScreenCaptureContentHint(contentHint);

	// 停止屏幕共享
	m_lpAgoraEngine->stopScreenCapture();
	```

### 共享指定窗口

Windows 系统为每个窗口分配一个 windowId，数据类型为 HWND。该 ID 对应唯一的 Windows 窗口。为了兼容 x86 和 x64 系统，使用 view_t 类型。

通过获取该 windowId，我们可以按如下步骤在 Windows 平台上实现窗口共享：

1. 获取想要共享窗口的 Window ID
```
BOOL CALLBACK EnumProc(HWND hWnd, LPARAM IParam)
{
    // 仅获取可视窗口 ID，忽略弹出窗口及目录窗口
    LONG IStyle = ::GetWindowLong(hWnd, GWL_STYLE);
    if ((IStyle&WS_VISIBLE) != 0 && (IStyle&(WAS_POPUP | WA_SYSMENU)) != 0) {
        HWND window_id = hWnd;
   }
    return TRUE;
}
EnumWindows(&EnumProc, NULL);
```
> 更多关于 windowId 的详情，请参考 [Microsoft EnumWindows 说明](https://docs.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-enumwindows)。

2. 通过 Window ID 共享窗口

	```cpp
	// 指定共享的屏幕或窗口
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

	// 开始共享窗口
	ret = m_lpAgoraEngine->startScreenCaptureByWindowId(hWnd, rcCap, capParam);

	// 更新屏幕共享编码参数
	m_lpAgoraEngine->updateScreenCaptureParameters(capParam);

	// 更新屏幕共享区域
	m_lpAgoraEngine->updateScreenCaptureRegion(screenRegion);

	// 设置屏幕共享内容类型
	m_lpAgoraEngine->setScreenCaptureContentHint(contentHint);

	// 停止屏幕共享
	m_lpAgoraEngine->stopScreenCapture();
	```

### API 参考
* [`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)
* [`startScreenCaptureByScreenRect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)
* [`updateScreenCaptureParameters`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)
* [`setScreenCaptureContentHint`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)
* [`updateScreenCaptureRegion`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ae2ab9c3ff28b64c601f938ab45644586)
* [`stopScreenCapture`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a77412ab7c8653289a28212e60bd00673)

## 同时共享屏幕和开启视频

我们在 GitHub 提供一个实现同时发布屏幕共享流和用户视频流功能的开源示例项目。你可以前往 [Agora-Screen-Sharing-Windows](https://github.com/AgoraIO/Advanced-Video/tree/dev/win-screenshare/Screensharing/Agora-Screen-Sharing-Windows) 下载体验。

## 开发注意事项
- SDK 在 v2.4.0 版本中废弃了原有的 `startScreenCapture` 接口。你可以继续使用，但 Agora 不再推荐。
- 视频共享编码属性 `ScreenCaptureParameters` 类中各参数的设置可能会影响计费。从 v2.4.1 版本起，如果你将 `dimensions` 参数设为默认值，则 Agora 使用 1920 x 1080 进行计费。
- 在 Windows 平台上进行屏幕共享时，如果共享的是 QQ 聊天窗口会导致共享窗口黑屏。
