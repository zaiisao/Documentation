
---
title: 加入频道
description: macOS平台加入频道
platform: macOS
updatedAt: Thu Dec 13 2018 07:06:06 GMT+0800 (CST)
---
# 加入频道
在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Interactive%20Broadcast/mac_video.md)。

## 实现方法
App 在加入频道前，需要先设置频道模式，再加入频道。

### 设置频道模式为直播
创建实例后，调用 `setChannelProfile` 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。

在该方法中，将频道模式设置为直播模式。直播模式适用于互动直播场景。频道内有主播和观众两种角色，主播收发音视频消息，观众只收不发。

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

### 加入直播频道
调用 `joinChannelByToken` 方法加入频道。

在该方法中：

- 传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 nil。正式生产环境下，我们推荐你使用 Token。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Interactive%20Broadcast/token.md)。
- 传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。
- 传入能标识用户身份的用户 UID。请确保频道内每个用户的 UID 必须是独一无二的。如果想要从不同的设备同时接入同一个频道，请确保每个设备上使用的 UID 是不同的。

> 如果已在通话中，则必须调用 `leaveChannel` 方法退出当前通话，才能进入下一个频道。

```objective-c
//Objective-C
- (void)joinChannel {
  [self.agoraKit joinChannelByToken: "token" channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
    // Join channel "demoChannel1"
  }];
}
```

```swift
//Swift
func joinChannel() {
  agoraKit.joinChannel(by Token: "token", channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
      // Join channel "demoChannel1"
  }
}
```

## 相关文档

成功加入频道后，你可以使用 Agora SDK，实现如下功能进行互动直播：

- [切换用户角色](../../cn/Interactive%20Broadcast/role_mac.md)
- [发布和订阅音视频流](../../cn/Interactive%20Broadcast/publish_mac_live.md)

在直播过程中，如果对音量、音效、视频分辨率等有特殊需求，你还可以：

- [调整通话音量](../../cn/Interactive%20Broadcast/volume_mac.md)
- [播放音效/音乐混音](../../cn/Interactive%20Broadcast/effect_mixing_mac.md)
- [调整音调、音色](../../cn/Interactive%20Broadcast/voice_effect_mac.md)
- [设置视频属性](../../cn/Interactive%20Broadcast/videoProfile_mac.md)
