
---
title: 实现视频直播
description: 
platform: iOS
updatedAt: Tue Oct 16 2018 10:40:51 GMT+0800 (CST)
---
# 实现视频直播
# 实现视频直播

在本页你可以了解如何 Agora SDK 实现视频直播。

## 环境准备

1. 具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/ios_video.md) 。
2. 参考 [Agora iOS Video Live Tutorial Sample App](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-iOS) 了解如何从头创建一个示例项目。

## 快速开始

### 初始化 AgoraRtcEngineKit

进入视频直播频道之前，调用 `sharedEngineWithAppId` 方法创建一个 AgoraRtcEngine 实例。

在该方法中：

- 填入获取到的 [App ID](../../cn/Quickstart%20Guide/ios_video.md) 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
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

### 设置用户角色

直播频道分主播和观众两种用户角色。在将频道模式为直播后，调用 `setClientRole` 方法，并根据需要将用户设置为主播或观众。两者的区别在于：

- 主播：可以收听和发布音视频消息。根据应用程序的实现，还可以与观众互动、指定观众连麦。同一直播频道内，主播只能听到和看到自己以及连麦主播的音视频。
- 观众：只能收听主播的音视频消息。根据应用程序的实现，还可以发布实时文字消息，与主播互动。同一直播频道内，所有观众都能听到和看到主播以及连麦主播的音视频。

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

在该方法中，指定你想要的视频编码的分辨率、帧率、码率以及视频编码的方向模式。详细的视频编码参数定义，参考 **设置本地视频属性**(`setVideoEncoderConfiguration`)中的描述。

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

现在你可以加入视频直播频道了。 调用 `joinChannelByToken` 方法加入频道。

在该方法中：

- 传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 nil。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。
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

端视频视图，是指用户在本地设备上看到的远端视频流的视图。

在进入频道后，调用 `setupRemoteVideo` 方法设置本地看到的远端用户的视频视图。用户需要在该方法中指定想要看到的远端视图的用户 UID。

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

### 结束视频直播

视频直播结束时，调用 `leaveChannel` 方法离开频道。

不论当前是否还在直播中，调用该方法会把会话相关的所有资源释放掉。真正退出频道后，SDK 会触发 `didLeaveChannelWithStats` 回调。

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

在上述视频直播过程中，我们使用了 Agora SDK 提供的部分 API 功能：

- 初始化： `sharedEngineWithAppId`
- 设置频道属性：`setChannelProfile`
- 设置用户角色：`setClientRole`
- 打开视频模式：`enableVideo`
- 设置本地视频属性：`setVideoProfile`
- 设置本地视频显示属性：`setupLocalVideo`
- 加入频道：`joinChannelByToken`
- 设置远端视频显示属性：`setupRemoteVideo`
- 离开频道：`leaveChannel`

在实现视频直播的过程中，你还可能需要使用以下功能:

- [视频采集旋转](../../cn/Quickstart%20Guide/rotation_guide_ios.md)
- [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)
- [选择加密方案](../../cn/Quickstart%20Guide/encryption_ios_agora.md)
- [修改裸数据](../../cn/Quickstart%20Guide/rawdata_ios.md)

更多功能实现，请参考 [互动直播 API](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/index.html) 中各 API 的功能及描述。
