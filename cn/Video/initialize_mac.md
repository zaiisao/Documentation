
---
title: 初始化 AgoraRtcEngineKit
description: macOS平台初始化
platform: macOS
updatedAt: Thu Dec 06 2018 09:38:51 GMT+0000 (UTC)
---
# 初始化 AgoraRtcEngineKit
进入频道之前，调用 `sharedEngineWithAppId` 方法创建一个 AgoraRtcEngine 实例。

在该方法中：

- 填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
- 指定一个 delegate 对象。SDK 通过指定的 delegate 通知应用程序 SDK 的运行事件，如：加入或离开频道、新用户加入频道等。

```objective-c
//Objective-C
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>

...

- (void)initializeAgoraEngine {
  self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:@"Your App ID" delegate:self];
}
```

```swift
//Swift
import AgoraRtcEngineKit

...

func initializeAgoraEngine() {
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: "Your App ID", delegate: self)
}
```
