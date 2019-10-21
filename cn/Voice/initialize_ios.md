
---
title: 初始化 AgoraRtcEngineKit
description: iOS平台初始化
platform: iOS
updatedAt: Fri Jul 19 2019 08:23:35 GMT+0800 (CST)
---
# 初始化 AgoraRtcEngineKit
## 前提条件

在初始化 AgoraRtcEngineKit 前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/ios_audio.md)。

初始化过程中，你需要传入一个的 App ID。因此需要现在 Agora Dashboard 注册项目并获取 App ID。

1. 进入 [控制台](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


## 实现方法

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

> 请确保在调用其他 API 前先调用 `initializeEngineWithAppId` 方法创建并初始化 AgoraRtcEngine。

## 相关文档
完成创建实例后，你可以使用 Agora SDK，依次实现如下功能进行语音通话：
* [加入频道](../../cn/Voice/join_communication_ios.md)
* [发布和订阅音频流](../../cn/Voice/publish_ios_audio.md)

如果对网络或音质有特殊的需求，你还可以在加入频道前：
* [进行通话前网络质量监测](../../cn/Voice/lastmile_ios.md)
* [使用双声道/高音质](../../cn/Voice/audio_profile_ios_audio.md)
