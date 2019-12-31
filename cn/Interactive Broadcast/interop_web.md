
---
title: 移动、桌面、Web 端互通
description: 
platform: Web
updatedAt: Tue Dec 31 2019 08:53:33 GMT+0800 (CST)
---
# 移动、桌面、Web 端互通
## 功能简介

开启移动端、桌面端、Web 端**互通**，是指开发者可以将不同平台的 SDK 集成到自己的 App 中，实现实时音视频通话服务。用户无论是从移动端（iOS 或  Android 设备）、PC 端（Windows 或 macOS 设备），还是 Web 端（浏览器或 Web App）都能使用该 App 进入同一个频道进行互通。

## 实现方法

在开启互通前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Interactive%20Broadcast/web_prepare.md)。

Agora SDK 的移动端和 Web 端互通，需要在移动端和 Web 端同时进行配置。

> 该功能仅在直播模式下需要，通信模式下默认互通是打开的。

* 移动端/桌面端：调用 `enableWebSdkInteroperability` API 方法。

	```java
	// java
	// 移动端调用 enableWebSdkInteroperability 方法开启与 Web SDK 的互通
	rtcEngine.enableWebSdkInteroperability(true);
	```

	```swift
	// swift
	// 移动端/桌面端调用 enableWebSdkInteroperability 方法开启与 Web SDK 的互通
	agoraKit.enableWebSdkInteroperability(true)
	```

	```objective-c
	// objective-c
	// 移动端/桌面端调用 enableWebSdkInteroperability 方法开启与 Web SDK 的互通
	[agoraKit enableWebSdkInteroperability: YES];
	```

	```cpp
	// cpp
	// 桌面端调用 enableWebSdkInteroperability 方法开启与 Web SDK 的互通
	lpAgoraEngine->enableWebSdkInteroperability
	```

* Web 端：将 `createClient` 方法中的 `mode` 设置为 `'live'` 实现互通。

	```javascript
	// javascript
	// Web 端在创建客户端时，选择正确的 mode 和 codec 参数
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	```

## 开发注意事项

* 移动端/桌面端和 Web 端必须同时设置，才能实现互通。
* Agora SDK 在通信模式下，默认互通是打开的。因此只有在直播模式下，才需要打开互通
* 根据已知实验经验，如果场景中有 Safari 浏览器，则建议在 Web 端将 `codec` 设为 `h264`；如果没有，则建议设为 `vp8`


