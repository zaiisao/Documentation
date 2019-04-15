
---
title: 实现视频通话
description: 
platform: iOS
updatedAt: Mon Apr 15 2019 06:45:38 GMT+0800 (CST)
---
# 实现视频通话
在本页你可以了解如何使用 Agora SDK 实现视频通话。

## 环境准备

1. 具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/ios_video.md) 。
2. 参考 [Agora iOS Video Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-iOS-Tutorial-Swift-1to1) 了解如何从头创建一个示例项目。

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

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。

```objective-c
//Objective-C
- (void)setChannelProfile() {
  [self.agoraKit setChannelProfile:AgoraChannelProfileCommunication]
}
```

```swift
//Swift
func setChannelProfile() {
  agoraKit.setChannelProfile(.channelProfile_Communication)
}
```

### 打开视频模式

调用 `enableVideo` 方法打开视频模式。在 Agora SDK 中，音频功能是默认打开的，因此在加入频道前，或通话过程中，你都可以调用该方法开启视频。

- 如果在加入频道前打开，则进入频道后直接加入视频通话。
- 如果在通话过程中打开，则由纯音频通话切换为视频通话。

```objective-c
//Objective-C
- (void)enableVideo {
  [self.agoraKit enableVideo];
  // Default mode is disableVideo
}
```

```swift
//Swift
func enableVideo() {
  agoraKit.enableVideo()  //Default mode is disableVideo
}
```

### 设置视频编码属性

打开视频模式后，调用 `setVideoEncoderConfiguration` 方法设置视频的编码属性。

在该方法中，指定你想要的视频编码的分辨率、帧率、码率以及视频编码的方向模式。详细的视频编码参数定义，参考 **设置本地视频属性** (`setVideoEncoderConfiguration`)中的描述。

> - 该方法设置的参数为理想情况下的最大值。当视频引擎因网络等原因无法达到设置的分辨率、帧率或码率值时，会取最接近设置值的最大值。
> - 如果设备的摄像头无法支持定义的视频属性，SDK 会为摄像头自动选择一个合理的分辨率。该行为对视频编码没有影响，编码时 SDK 仍沿用该方法中定义的分辨率。

```objective-c
//Objective-C
AgoraVideoEncoderConfiguration *configuration =
[[AgoraVideoEncoderConfiguration alloc]
initWithSize:AgoraVideoDimension640x360
frameRate:AgoraVideoFrameRateFps15 bitrate:400
orientationMode:AgoraVideoOutputOrientationModeFixedPortrait];

[self.agoraKit setVideoEncoderConfiguration:configuration];
```

```swift
//Swift
let configuration = AgoraVideoEncoderConfiguration(size:
AgoraVideoDimension640x360, frameRate: .fps15, bitrate: 400,
orientationMode: .fixedPortrait)
agoraKit.setVideoEncoderConfiguration(configuration);
```

### 设置本地视频视图

本地视频视图，是指用户在本地设备上看到的本地视频流的视图。

在进入频道前调用 `setupLocalVideo` 方法，使应用程序绑定本地视频流的显示视窗，并设置本地看到的本地视频视图。

在该方法中：

- 选择你想要看到的本地视频流的视窗。
- 指定你想要的视频显示模式。共有两种模式可选：Hidden 和 Fit 模式。两种模式下视频尺寸都是等比缩放，区别在于：
  - Hidden 模式：优先保证视窗被填满。如果缩放后的视频尺寸与显示视窗尺寸不一致，多出的视频将被截掉。
  - Fit 模式：优先保证视频内容全部显示。如果缩放后的视频尺寸与显示视窗尺寸不一致，未被填满的视窗区域将使用黑色填充。
- 传入本地用户的 UID。该值默认为 0，可以不设置。

```objective-c
//Objective-C
- (void)setupLocalVideo {
  AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
  videoCanvas.uid = 0;

  videoCanvas.view = self.localVideo;
  videoCanvas.renderMode = AgoraVideoRenderModeHidden;
  [self.agoraKit setupLocalVideo:videoCanvas];
  // Bind local video stream to view
}
```

```swift
//Swift
func setupLocalVideo() {
  let videoCanvas = AgoraRtcVideoCanvas()
  videoCanvas.uid = 0
  videoCanvas.view = localVideo
  videoCanvas.renderMode = .hidden
  agoraKit.setupLocalVideo(videoCanvas)
}
```

### 加入频道

现在你可以加入频道开始视频通话了。 调用 `joinChannelByToken` 方法加入频道。同一个频道内的用户可以互相通话；多个用户加入同一个频道，可以群聊。

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

### 设置远端视频视图

远端视频视图，是指用户在本地设备上看到的远端视频流的视图。

在进入频道后，调用 `setupRemoteVideo` 方法设置本地看到的远端用户的视频视图。

在该方法中：

- 选择你想要看到的远端视频流的视窗。
- 指定你想要的视频显示模式。共有两种模式可选：Hidden 和 Fit 模式。两种模式下视频尺寸都是等比缩放，区别在于：
  - Hidden 模式：优先保证视窗被填满。如果缩放后的视频尺寸与显示视窗尺寸不一致，多出的视频将被截掉。
  - Fit 模式：优先保证视频内容全部显示。如果缩放后的视频尺寸与显示视窗尺寸不一致，未被填满的视窗区域将使用黑色填充。
- 传入你想要看到的远端视图的用户 UID。如果不知道想要指定的远端用户 UID，也可以在收到其他用户进入频道的回调事件 `didJoinedOfUid` 中使用该方法。

```objective-c
//Objective-C
- (void)setupRemoteVideo {
  AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
  videoCanvas.uid = uid;

  videoCanvas.view = self.remoteVideo;
  videoCanvas.renderMode = AgoraVideoRenderModeFit;
  [self.agoraKit setupRemoteVideo:videoCanvas];
  // Bind remote video stream to view
}
```

```swift
//Swift
func setupRemoteVideo() {
    let videoCanvas = AgoraRtcVideoCanvas()
    videoCanvas.uid = 1
    videoCanvas.view = remoteVideo
    videoCanvas.renderMode = .fit
    agoraKit.setupRemoteVideo(videoCanvas)
}
```

### 结束视频通话

视频通话结束时，调用 `leaveChannel` 方法离开频道。

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

在上述视频通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

- 初始化：`sharedEngineWithAppId`
- 设置频道属性：`setChannelProfile`
- 打开视频模式：`enableVideo`
- 设置本地视频属性：`setVideoEncoderConfiguration`
- 设置本地视频显示属性：`setupLocalVideo`
- 加入频道：`joinChannelByToken`
- 设置远端视频显示属性：`setupRemoteVideo`
- 离开频道：`leaveChannel`

在实现视频通话的过程中，你还可能需要使用以下功能:

- [视频采集旋转](../../cn/Quickstart%20Guide/rotation_guide_ios.md)
- [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)
- [选择加密方案](../../cn/Quickstart%20Guide/encryption_ios_agora.md)
- [修改裸数据](../../cn/Quickstart%20Guide/rawdata_ios.md)

更多功能实现，请参考 [视频通话 API](https://docs.agora.io/cn/Video/API%20Reference/oc/index.html) 中各 API 的功能及描述。
