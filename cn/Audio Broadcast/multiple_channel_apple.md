
---
title: 加入多频道
description: 加入多频道v3.0首次上线
platform: iOS,macOS
updatedAt: Thu Jun 04 2020 08:43:05 GMT+0800 (CST)
---
# 加入多频道
## 功能描述

为方便用户同时加入多个频道，接收多个频道的音视频流，Agora Native SDK 自 v3.0 起新增支持多频道管理，且频道数量无限制。

该功能可应用于类似超级小班课的场景：将一个互动大班里的学生分到不同的小班，学生可以在小班内进行实时音视频互动。根据场景需要，你还可以给每个小班可以配备一名助教老师。

## 实现方法
Native SDK 通过一个 `AgoraRtcChannel` 类和 `AgoraRtcChannelDelegate` 类实现多频道控制。你可以从如下任一种方法实现该功能：

### 方法一：仅使用 RtcChannel 类实现

参考如下步骤在项目中实现多频道功能：

1. 调用 `AgoraRtcEngineKit` 类的 `createRtcChannel` 方法，通过 `channelId` 创建一个 `AgoraRtcChannel` 对象。
2. 调用 `AgoraRtcChannel` 对象中的 `joinChannelByToken` 方法加入频道。
3. 重复步骤 2、3，创建多个 `AgoraRtcChannel` 对象，然后分别调用这些 `AgoraRtcChannel` 对象中的 `joinChannelByToken` 方法，同时加入多个频道。

<div class="alert note">
	<li>加入多个频道后，你可以调用各 <code>AgoraRtcChannel</code> 对象中的 <code>publish</code> 方法在对应的频道中发布音视频流。但需要注意，一次只能在一个 <code>AgoraRtcChannel</code> 对象的频道里发布。
	<li>调用频道一内的 <code>publish</code> 方法发布流后，必须先调用频道一的 <code>unpublish</code> 方法结束发布，才能调用频道二的 <code>publish</code> 方法。</div>

该方法适用于需求频繁切换发布音、视频流频道的场景。

**API 调用时序**

参考如下步骤，在你的项目中使用 `AgoraRtcChannel` 类实现多频道功能。

![](https://web-cdn.agora.io/docs-files/1575879041885)

**示例代码**

你还可以参考如下示例代码。

```Objective-C
- (BOOL)JoinChannelWithAgoraRtcChannel:(NSString *)channelId token:(NSString *)token info:(NSString *)info uid:(NSUInteger)uid {
    
    if (channelId.length <= 0) return NO;

    // 1. Create agoraRtcEngineKit
    AgoraRtcEngineKit *kit = [AgoraRtcEngineKit sharedEngineWithAppId:@"AppId" delegate:self];
    
    // 2. Create agoraRtcChannel
    AgoraRtcChannel *agoraRtcChannel = [kit createRtcChannel:channelId];
    if (agoraRtcChannel == nil) return NO;
    
    // 3. Set rtcChannelDelegate
    [agoraRtcChannel setRtcChannelDelegate:self];
    
    // 4. Configurate mediaOptions
    AgoraRtcChannelMediaOptions *mediaOptions = [AgoraRtcChannelMediaOptions new];
    mediaOptions.autoSubscribeAudio = YES;
    mediaOptions.autoSubscribeVideo = YES;
    
    // 5. Join channel
    [agoraRtcChannel joinChannelByToken:token info:info uid:uid options:mediaOptions];
    
  	return YES;
}
```

### 方法二：混用 AgoraRtcChannel 和 AgoraRtcEngineKit 类

参考如下步骤在项目中实现多频道功能：

1. 调用 `AgoraRtcEngineKit` 类下的 `joinChannelByToken` 方法加入第一个频道。
2. 调用 `AgoraRtcEngineKit` 类的 `createRtcChannel` 方法，通过 `channelId` 创建一个 `AgoraRtcChannel` 对象。
3. 调用 `AgoraRtcChannel` 对象中的 `joinChannelByToken` 方法加入第二个频道。
4. 重复步骤 2、3，创建多个 `AgoraRtcChannel` 对象，然后分别调用这些 `AgoraRtcChannel` 对象中的 `joinChannelByToken` 方法，加入更多频道。

该方法适用于只需要将音、视频流固定发布到 RtcEngine 频道的场景。对于已经使用 Agora SDK 实现实时音视频功能的开发者，该方法不涉及原有代码逻辑变动。

**API 调用时序**

参考如下步骤，在你的项目中使用 `AgoraRtcEngineKit` 和 `AgoraRtcChannel` 类实现多频道功能。

![](https://web-cdn.agora.io/docs-files/1575879666498)

我们在 GitHub 提供一个实现了多频道功能的开源示例项目 [Breakout Class](https://github.com/AgoraIO-Usecase/Breakout-Class/tree/master/breakout-ios)，你可以前往下载体验，了解其中的源代码。

### API 参考

- [`createRtcChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/createRtcChannel::)
- [`AgoraRtcChannel`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcChannel.html) 类
- [`AgoraRtcChannelDelegate`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) 类

## 开发注意事项

### 精细订阅限制

如果你在调用 `AgoraRtcChannel` 对象中的 `joinChannelByToken` 方法加入频道时将 `autoSubscribeAudio` 或 `autoSubscribeVideo` 设为 `false`，选择不自动订阅音频流或视频流，那么加入频道后，你不能再调用 `muteRemoteAudioStream`(false) 或 `muteRemoteVideoStream`(false) 接收指定用户的音频流或视频流。如想实现上述操作，我们建议在加入频道前调用  `setDefaultMuteAllRemoteAudioStreams`(true) 或 `setDefaultMuteAllRemoteVideoStreams`(true) 设置默认不接收所有音频流或所有视频流。

### 设置远端视图

在视频场景中，如果远端用户是通过 `AgoraRtcChannel` 加入频道的，那么在设置远端视图时，还需要在 [AgoraRtcVideoCanvas](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v3.0.0/Classes/AgoraRtcVideoCanvas.html)  中指定该远端用户所在频道的 channel ID，否则会无法渲染出远端视频画面。

### 多频道发流限制

SDK 仅支持用户同一时间在一个频道内发布音、视频流。因此只要你在一个频道内正在发布音、视频流，就无法在另一个频道发布。

由于通过 `AgoraRtcEngineKit` 类和 `AgoraRtcChannel` 类下的 `joinChannelByToken` 方法加入频道后，SDK 的默认行为有以下差异：

- `AgoraRtcEngineKit` 类下，SDK 默认发布本地音视频流到本频道
- `AgoraRtcChannel` 类下，SDK 默认不发布本地音视频流到本频道

因此，以下操作会导致 SDK 返回 `AgoraErrorCodeRefused(5)` 错误码：

- 通过 `AgoraRtcChannel` 类加入多频道：
  - 调用频道一的 `publish` 方法后，未调用频道一的 `unpublish` 方法结束发布，就调用频道二的 `publish` 方法；
- 通过混用 `AgoraRtcEngineKit` 类和 `AgoraRtcChannel` 类加入多频道：
  - 通信场景下，通过 `AgoraRtcEngineKit` 类加入频道一，然后通过 `AgoraRtcChannel` 类加入频道二后，试图调用 `AgoraRtcChannel` 类的 `publish` 方法；
  - 直播场景下，通过 `AgoraRtcEngineKit` 类加入频道一，然后通过 `AgoraRtcChannel` 类加入频道二后，试图调用 `AgoraRtcChannel` 类的 `publish` 方法；
  - 直播场景下，加入多个频道后，试图以观众身份调用 `AgoraRtcChannel` 类的 `publish` 方法；
  - 调用 `AgoraRtcChannel` 类的 `publish` 方法后，未调用对应 `AgoraRtcChannel` 类的 `unpublish` 方法，就试图通过 `AgoraRtcEngineKit` 类的 `joinChannelByToken` 方法加入频道。



