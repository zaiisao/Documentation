
---
title: 屏幕共享
description: 
platform: macOS
updatedAt: Mon Apr 20 2020 02:21:08 GMT+0800 (CST)
---
# 屏幕共享
## 功能简介
在视频通话或互动直播中进行屏幕共享，可以将说话人或主播的屏幕内容，以视频的方式分享给其他说话人或观众观看，以提高沟通效率。

屏幕共享在如下场景中应用广泛：

- 视频会议场景中，屏幕共享可以将讲话者本地的文件、数据、网页、PPT 等画面分享给其他与会人；
- 在线课堂场景中，屏幕共享可以将老师的课件、笔记、讲课内容等画面展示给学生观看。

## 实现方法

在实现屏幕共享前，请确保已在你的项目中实现基本的实时音视频功能。详见[开始音视频通话](../../cn/Video/start_call_mac.md)或[开始互动直播](../../cn/Video/start_live_mac.md)。

Agora 在 v2.4.0 对屏幕共享相关接口进行梳理，目前在 macOS 平台上支持：
- 通过 `displayId` 共享指定屏幕，或指定屏幕的部分区域
- 通过 `windowId` 共享指定窗口，或指定窗口的部分区域

### 共享指定屏幕

macOS 系统为每个屏幕分配一个 `displayId`，数据类型为 `CGDirectDisplayID`，32 位无符号整型。该 ID 对应唯一的 macOS 屏幕。我们可以按如下步骤在 macOS 平台上实现屏幕共享：

1. 获取想要共享屏幕的 Display ID

	```swift
	// 获取屏幕列表
	let screens = NSScreen.screens
	for (index, screen) in screens.enumerated() {
		// 获取 displayId
		guard let displayId = screen.deviceDescription[NSDeviceDescriptionKey(rawValue: "NSScreenNumber")] as? CGDirectDisplayID else {
			continue
		}
	}
	```
	
	> 更多关于 displayId 的详情，请参考 [Apple NSScreen](https://developer.apple.com/documentation/appkit/nsscreen) 说明。

2. 通过 Display ID 共享屏幕

	```swift
	// swift
	// 开始共享屏幕
	// displayId = 0 表示共享整个屏幕
	let displayId = 0
	let rectangle = CGRect.zero
	let parameters = AgoraScreenCaptureParameters()
	parameters.dimensions = CGSize.zero
	parameters.frameRate = 15
	parameters.bitrate = 1000
	agoraKit.startScreenCapture(bydisplayId: displayId, rectangle: rectangle, parameters: parameters)

	// 更新屏幕共享编码参数
	let parameters = AgoraScreenCaptureParameters()
	parameters.dimensions = CGSize.zero
	parameters.frameRate = 15
	parameters.bitrate = 1000
	agoraKit.update(parameters)

	// 更新屏幕共享区域
	let region = CGRect.zero
	agoraKit.updateScreenCaptureRegion(region)

	// 设置屏幕共享内容类型
	agoraKit.setScreenCapture(.none)

	// 停止屏幕共享
	agoraKit.stopScreenCapture()
	```

	```objective-c
	// objective-c
	// 开始共享屏幕
	// displayId = 0 表示共享整个屏幕
	NSUInteger displayId = 0;
	CGRect rectangle = CGRectZero;
	AgoraScreenCaptureParameters *parameters = [[AgoraScreenCaptureParameters alloc] init];
	parameters.dimensions = CGSizeZero;
	parameters.frameRate = 15;
	parameters.bitrate = 1000;
	[self.agoraKit startScreenCaptureByDisplayId:displayId rectangle:rectangle parameters:parameters];

	// 更新屏幕共享编码参数
	AgoraScreenCaptureParameters *parameters = [[AgoraScreenCaptureParameters alloc] init];
	parameters.dimensions = CGSizeZero;
	parameters.frameRate = 15;
	parameters.bitrate = 1000;
	[self.agoraKit updateScreenCaptureParameters:parameters];

	// 更新屏幕共享区域
	CGRect region = CGRectZero;
	[self.agoraKit updateScreenCaptureRegion:region];

	// 设置屏幕共享内容类型
	[self.agoraKit setScreenCaptureContentHint:AgoraVideoContentHintNone];

	// 停止屏幕共享
	[self.agoraKit stopScreenCapture];
	```

### 共享指定窗口

macOS 为每个窗口分配一个 `windowId`，数据类型为 `CGWindowID`，32 位无符号整型。该 ID 对应唯一的 macOS 窗口。我们可以按如下步骤在 macOS 平台上实现窗口共享：

1. 获取想要共享窗口的 Window ID

```objective-c
// 获取窗口 ID
CFArrayRef window_list = CGWindowListCopyWindowInfo(kCGWindowListOptionOnScreenOnly | kCGWindowListExcludeDesktopElements, kCGNullWindowID);
if (window_list) {
    CFIndex count = CFArrayGetCount(window_array);
    for (CFIndex  i = 0; i < count; ++i) {
        CFDictionaryRef window = reinterpret_cast<CFDictionaryRef>(CFArrayAtIndex(window_array, i));
        CFStringRef window_title = reinterpret_cast<CFStringRef>(CFDictionaryGetValue(window, kCGWindowName));
        CFNumberRef window_id = reinterpret_cast<CFNumberRef>(CFDictionaryGetValue(window, kCGWindowNumber));
   }
}
```

更多关于 `windowId` 的详情，请参考 [Apple CGWindowListCopyWindowInfo(::) 说明](https://developer.apple.com/documentation/coregraphics/1455137-cgwindowlistcopywindowinfo)。

2. 通过 Window ID 共享窗口

	```swift
	// swift
	// 开始共享窗口
	// windowId = 0 表示共享整个窗口
	let windowId = 0
	let rectangle = CGRect.zero
	let parameters = AgoraScreenCaptureParameters()
	parameters.dimensions = CGSize.zero
	parameters.frameRate = 15
	parameters.bitrate = 1000
	agoraKit.startScreenCapture(byWindowId: windowId, rectangle: rectangle, parameters: parameters)

	// 更新屏幕共享编码参数
	let parameters = AgoraScreenCaptureParameters()
	parameters.dimensions = CGSize.zero
	parameters.frameRate = 15
	parameters.bitrate = 1000
	agoraKit.update(parameters)

	// 更新屏幕共享区域
	let region = CGRect.zero
	agoraKit.updateScreenCaptureRegion(region)

	// 设置屏幕共享内容类型
	agoraKit.setScreenCapture(.none)

	// 停止屏幕共享
	agoraKit.stopScreenCapture()
	```

	```objective-c
	// objective-c
	// 开始共享窗口
	// windowId = 0 表示共享整个窗口
	NSUInteger windowId = 0;
	CGRect rectangle = CGRectZero;
	AgoraScreenCaptureParameters *parameters = [[AgoraScreenCaptureParameters alloc] init];
	parameters.dimensions = CGSizeZero;
	parameters.frameRate = 15;
	parameters.bitrate = 1000;
	[self.agoraKit startScreenCaptureByWindowId:windowId rectangle:rectangle parameters:parameters];

	// 更新屏幕共享编码参数
	AgoraScreenCaptureParameters *parameters = [[AgoraScreenCaptureParameters alloc] init];
	parameters.dimensions = CGSizeZero;
	parameters.frameRate = 15;
	parameters.bitrate = 1000;
	[self.agoraKit updateScreenCaptureParameters:parameters];

	// 更新屏幕共享区域
	CGRect region = CGRectZero;
	[self.agoraKit updateScreenCaptureRegion:region];

	// 设置屏幕共享内容类型
	[self.agoraKit setScreenCaptureContentHint:AgoraVideoContentHintNone];

	// 停止屏幕共享
	[self.agoraKit stopScreenCapture];
	```

### API 参考
* [`startScreenCaptureByDisplayId`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)
* [`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)
* [`updateScreenCaptureParameters`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)
* [`setScreenCaptureContentHint`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)
* [`updateScreenCaptureRegion:`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureRegion:)
* [`stopScreenCapture`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopScreenCapture)

## 开发注意事项

- SDK 在 v2.4.0 版本中废弃了原有的屏幕共享接口 `startScreenCapture`，你仍然可以使用，但 Agora 不再推荐。
- 视频共享编码属性 `AgoraScreenCaptureParameters` 类中各参数的设置可能会影响计费。从 v2.4.1 版本起，如果你将 `dimensions` 参数设为默认值，会按照 1920 x 1080 进行计费。
