
---
title: 实现语音直播
description: 
platform: iOS
updatedAt: Wed Oct 17 2018 02:25:44 GMT+0000 (UTC)
---
# 实现语音直播
# 实现语音直播

在本页你可以了解如何使用 Agora SDK 实现语音直播。

## 环境准备

1. 具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/ios_audio.md) 。
2. 参考 [Agora iOS Voice Live Tutorial Sample App](https://github.com/AgoraIO/Basic-Audio-Broadcasting/tree/master/OpenLive-Voice-Only-iOS) 了解如何从头创建一个示例项目。

## 快速开始

### 初始化 AgoraRtcEngineKit

进入视频通话频道之前，调用 `sharedEngineWithAppId` 方法创建一个 AgoraRtcEngine 实例。

在该方法中：

- 填入获取到的 [App ID](../../cn/Quickstart%20Guide/ios_audio.md) 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
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

在该方法中，将频道模式设置为直播模式。直播模式适用于互动直播场景。频道内有主播和观众两种角色，主播收发语音消息，观众只收不发。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。


```objective-c
//Objective-C
- (void)setChannelProfile() {
  [self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting]
}
```

```swift
//Swift
func setChannelProfile() {
  agoraKit.setChannelProfile(.channelProfile_LiveBroadcasting)
}
```

### 设置用户角色

直播频道分主播和观众两种用户角色。在将频道模式为直播后，调用 `setClientRole` 方法，并根据需要将用户设置为主播或观众。两者的区别在于：

- 主播：可以收听和发布语音消息。根据应用程序的实现，还可以使用语音与观众互动、指定观众连麦。同一直播频道内，主播只能听到自己以及连麦主播的声音。
- 观众：只能收听语音消息。根据应用程序的实现，还可以发布实时文字消息，与主播互动。同一直播频道内，所有观众都能听到主播以及连麦主播的声音。

> 在加入直播频道前，务必调用该方法设置用户角色。直播过程中，用户也可以再次调用该方法，改变设置的用户角色。

```objective-c
//Objective-C
- (void)setClientRole() {
  [self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
}
```

```swift
//Swift
func setClientRole() {
  agoraKit.setClientRole(.clientRole_Broadcaster)
}
```

### 加入频道

现在你可以加入语音直播频道了。 调用 `joinChannelByToken` 方法加入频道。

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

### 结束语音直播

语音直播结束时，调用 `leaveChannel` 方法离开频道。

不论当前是否还在直播频道中，调用该方法会把直播相关的所有资源释放掉。真正退出频道后，SDK 会触发 `didLeaveChannelWithStats` 回调。

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

现在你可以开始互动直播了。

## 参考

在上述语音直播过程中，我们使用了 Agora SDK 提供的部分 API 功能：

- 初始化：`sharedEngineWithAppId`
- 设置频道属性：`setChannelProfile`
- 设置用户角色：`setClientRole`
- 加入频道：`joinChannelByToken`
- 离开频道：`leaveChannel`

在实现语音直播的过程中，你还可能需要使用以下功能:

- [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)
- [选择加密方案](../../cn/Quickstart%20Guide/encryption_ios_agora.md)
- [修改裸数据](../../cn/Quickstart%20Guide/rawdata_ios.md)

更多功能实现，请参考 [互动直播 API](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/index.html) 中各 API 的功能及描述。
