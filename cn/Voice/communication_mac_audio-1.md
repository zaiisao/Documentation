
---
title: 实现语音通话
description: 
platform: macOS
updatedAt: Fri Nov 02 2018 04:00:16 GMT+0800 (CST)
---
# 实现语音通话
## 环境准备

1. 具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见  [设置开发环境](../../cn/voice/mac_video.md)。
2. 参考 [Agora macOS Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-macOS-Tutorial-Swift-1to1) 了解如何从头创建一个示例项目。

## 快速开始

### 初始化 AgoraRtcEngineKit

进入语音通话频道之前，调用 `sharedEngineWithAppId` 方法创建一个 AgoraRtcEngine 实例。

在该方法中：

- 填入获取到的 [App ID](../../cn/voice/mac_video.md) 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
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

### 设置频道模式

创建实例后，调用 `setChannelProfile` 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。

```
//Objective-C
- (void)setChannelProfile() {
  [self.agoraKit setChannelProfile:AgoraChannelProfileCommunication]
}
```

```
//Swift
func setChannelProfile() {
  agoraKit.setChannelProfile(.channelProfile_Communication)
}
```

### 加入频道

现在你可以加入频道开始语音通话了。 调用 `joinChannelByToken` 方法加入频道。同一个频道内的用户可以互相通话；多个用户加入同一个频道，可以群聊。

在该方法中：

- 传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。
- 传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。
- 传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。如果想要从不同的设备同时接入同一个频道，请确保每个设备上使用的 UID 是不同的。

> 如果已在通话中，则必须调用 `leaveChannel` 方法退出当前通话，才能进入下一个频道。

```objective-c
//Objective-C
- (void)joinChannel {
  [self.agoraKit joinChannelByToken:nil channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
    // Join channel "demoChannel1"
  }];
}
```

```swift
//Swift
func joinChannel() {
  agoraKit.joinChannel(by Token: nil, channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
      // Join channel "demoChannel1"
  }
}
```

### 结束语音通话

语音通话结束时，调用 `leaveChannel` 方法离开频道。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 `didLeaveChannelWithStats` 回调。

> 如果在调用 `leaveChannel` 方法后立即使用 `destroy` ，则退出频道会被打断，SDK 也不会触发 `didLeaveChannelWithStats` 回调。

```objective-c
//Objective-C
-(void)leaveChannel {
  [self.agoraKit leaveChannel:nil];
```

```swift
//Swift
func leaveChannel() {
    agoraKit.leaveChannel(nil)
}
```

## 结论

现在你可以开始一个一对一通话或多人群聊了。

## 参考

在上述语音通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

- 初始化：`sharedEngineWithAppId`
- 设置频道属性：`setChannelProfile`
- 加入频道：`joinChannelByToken`
- 离开频道：`leaveChannel`

在实现语音通话的过程中，你还可能需要使用以下功能:

- [录制音视频](../../cn/recording/recording_voice_video.md)
- [进选择加密方案](../../cn/voice/encryption_mac_agora.md)
- [修改裸数据](../../cn/voice/rawdata_ios.md)

更多功能实现，请参考 [语音通话 API](https://docs.agora.io/cn/Voice/API%20Reference/oc/index.html) 中各 API 的功能及描述。
