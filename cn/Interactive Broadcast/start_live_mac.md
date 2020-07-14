
---
title: 实现视频直播
description: 
platform: macOS
updatedAt: Fri Jul 03 2020 01:58:56 GMT+0800 (CST)
---
# 实现视频直播
本文介绍如何使用 Agora SDK 快速实现互动直播。

互动直播和实时通话的区别就在于，直播频道的用户有角色之分。你可以将角色设置为主播 Broadcaster，或者观众 Audience，其中主播可以收、发流，观众只能收流。

## 快速跑通 Demo

如果你是第一次使用声网的服务，我们推荐观看下面的视频，了解关于声网服务的基本信息以及如何快速跑通 demo。

<div class="alert info">点击参与<a href="https://www.wenjuan.com/s/7FbeEz6/" target="_blank">视频教程问卷调查</a>，帮助我们改进体验。</div>

<video src="https://web-cdn.agora.io/docs-files/1593741447106" poster="https://web-cdn.agora.io/docs-files/1584610484891"   controls width = 100% height = auto>你的浏览器不支持 <code>video</code> 标签。</video>

<div class="alert note">视频中展示的 UI 可能有部分调整更新，请以当前最新版为准。</div>

## 示例项目

Agora 在 GitHub 上提供开源的互动直播示例项目 [OpenLive-macOS-Objective-C](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-macOS-Objective-C) 或 [OpenLive-macOS-Swift](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-macOS)。在实现相关功能前，你可以下载并查看源代码。

## 前提条件

- Xcode 9.0 或以上版本
- 支持 macOS 10.10 或以上版本的 macOS 设备
- 有效的 [Agora 账户](https://docs.agora.io/cn/Agora%20Platform/sign_in_and_sign_up) 和 [App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#%E8%8E%B7%E5%8F%96-app-id)

<div class="alert note">如果你的网络环境部署了防火墙，请根据<a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">应用企业防火墙限制</a>打开相关端口。</div>

## 准备开发环境

本节介绍如何创建项目，并将 Agora SDK 集成至你的项目中。

### 创建 macOS 项目

参考以下步骤创建一个 macOS 项目。若已有 macOS 项目，可以直接查看[集成 SDK](#IntegrateSDK)。
<details>
	<summary><font color="#3ab7f8">创建 macOS 项目</font></summary>
	
1. 打开 **Xcode** 并点击 **Create a new Xcode project**。
2. 选择项目类型为 **Cocoa App**，并点击 **Next**。
3. 输入项目信息，如项目名称、开发团队信息、组织名称和语言，并点击 **Next**。
 
	<div class="alert note">如果你没有添加过开发团队信息，会看到 <b>Add account…</b> 按钮。点击该按钮并按照屏幕提示登入 Apple ID，完成后即可选择你的账户作为开发团队。</div>
	
4. 选择项目存储路径，并点击 **Create**。
5. 进入 **TARGETS > Project Name > General > Signing** 菜单，选择 **Automatically manage signing**，并在弹出菜单中点击 **Enable Automatic**。
	
	![](https://web-cdn.agora.io/docs-files/1568803584472)
</details>
	
### <a name="IntegrateSDK"></a>集成 SDK

选择如下任意一种方式将 Agora SDK 集成到你的项目中。

<div class="alert note">自 3.0.1 版本起，下载的 SDK 内仅包含动态库包 <tt>AgoraRtcKit.framework</tt>。如果你将旧版本 SDK 升级至 3.0.1 版本，请参考<a href="../../cn/Interactive%20Broadcast/migration_apple.md">升级指南</a >。</div>

**方法一：使用 CocoaPods 自动集成**

1. 开始前确保你已安装 **Cocoapods**。参考 [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) 安装说明。
2. 在 **Terminal** 里进入项目根目录，并运行 `pod init` 命令。项目文件夹下会生成一个 **Podfile** 文本文件。
3. 打开 **Podfile** 文件，修改文件为如下内容。注意将 `Your App` 替换为你的 Target 名称，并将 `version` 替换为你需集成的 SDK 版本。
 ```
# platform :macOS, '10.11' use_frameworks!
 target 'Your App' do
     pod 'AgoraRtcEngine_macOS', '~> version'
 end
```
4. 在 **Terminal** 内运行 `pod update` 命令更新本地库版本。
5. 运行 `pod install` 命令安装 Agora SDK。成功安装后，**Terminal** 中会显示 `Pod installation complete!`，此时项目文件夹下会生成一个 **xcworkspace** 文件。
6. 打开新生成的 **xcworkspace** 文件。

**方法二：手动复制 SDK 文件**

1. 前往 [SDK 下载页面](https://docs.agora.io/cn/Agora%20Platform/downloads)，获取最新版的 Agora SDK，然后解压。
2. 将 `libs` 文件夹内的 `AgoraRtcKit.framework` 文件复制到项目文件夹下。
3. 打开 **Xcode**（以 Xcode 11.0 为例），进入 **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**  菜单，点击 **+**，再点击 **Add Other…** 添加 `AgoraRtcKit.framework` 文件。添加完成后，项目会自动链接其他系统库。为保证动态库的签名和 app 的签名一致，你需要将动态库的 **Embed** 属性设置为 **Embed & Sign**。

<details>
	<summary><font color="#3ab7f8">如需集成 3.0.0 以下版本的 SDK，点击查看操作步骤。</font></summary>

1. 解压 Agora SDK。
2. 将 `libs` 文件夹内的 `AgoraRtcEngineKit.framework` 文件复制到项目路径下。
3. 打开 Xcode，进入 **TARGETS > Project Name > Build Phases > Link Binary with Libraries** 菜单，点击 **+** 添加如下库。在添加 `AgoraRtcEngineKit.framework` 文件时，还需在点击 **+** 后点击 **Add Other…**，找到本地文件并打开。

 - AgoraRtcEngineKit.framework
 - Accelerate.framework
 - CoreWLAN.framework
 - libc++.tbd
 - libresolv.9.tbd
 - SystemConfiguration.framework
 - VideoToolbox.framework

</details>

  <div class="alert warning">根据 Apple 官方要求，app 的 Extension 不允许包含动态库。如果工程中的 Extension 需要集成 SDK，则集成动态库时需将文件状态改为 <b>Do Not Embed</b>。</div>

 **动态库添加前**：
 
 ![](https://web-cdn.agora.io/docs-files/1583330350955)
 
 **动态库添加后**：
 
 ![](https://web-cdn.agora.io/docs-files/1584688978762)

### 添加媒体设备权限

1. 根据场景需要，打开 **Xcode** ，在 **info.plist** 文件中，点击 **+** 图标开始添加如下内容，获取相应的设备权限：


| Key | Type | Value |
| ---------------- | ---------------- | ---------------- |
| Privacy - Microphone Usage Description      | String      | 使用麦克风的目的，例如：for a call or live interactive streaming。      |
| Privacy - Camera Usage Description      | String      | 使用摄像头的目的，例如：for a call or live interactive streaming。      |


**添加前**：
 
![](https://web-cdn.agora.io/docs-files/1584691351348)
 
**添加后**：
 
 ![](https://web-cdn.agora.io/docs-files/1584604886884) 

2. 若你的项目已启用 **App Sandbox** 或 **Hardened Runtime** 设置，则需勾选如下内容，获取相应的设备权限：

<table>
    <tr>
        <td><b>菜单</b></td>
        <td><b>类型</b></td>
        <td><b>内容</b></td>
    </tr>
    <tr>
        <td rowspan="4">App Sandbox</td>
        <td rowspan="2">Network</td>
        <td>Incoming Connections (Server)</td>
    </tr>
    <tr>
        <td>Incoming Connections (Client)</td>
    </tr>
	    <tr>
        <td rowspan="2">Hardware</td>
        <td>Camera</td>
    </tr>
    <tr>
        <td>Audio Input</td>
    </tr>
	<tr>
        <td rowspan="2">Hardened Runtime</td>
        <td rowspan="2">Resource Access</td>
        <td>Camera</td>
    </tr>
    <tr>
        <td>Audio Input</td>
</table>
 
 
<div class="alert note"><li>根据 Apple 官方要求：<ul><li>对于在 Mac App Store 发布的软件，需要启用 <b>App Sandbox</b> 设置。详见 <a href="https://developer.apple.com/app-sandboxing/">Apple 官方声明</a>。<li>对于不在 Mac App Store 发布的软件，需要启用 <b>Hardened Runtime</b> 设置。详见 <a href="https://developer.apple.com/news/?id=09032019a">Apple 官方声明</a>。</li></ul><li><b>Hardened Runtime</b> 设置中的 <b>Library Validation</b> 会阻止 app 加载框架、插件或库，除非框架、插件或库是由 Apple 或是与 app 相同的团队 ID 签名的。当遇到需要取消该安全限制的场景（例如无法枚举到第三方虚拟摄像头）时，请勾选 <b>Hardened Runtime -> Runtime Exceptions -> Disable Library Validation</b>。</li></div>

## 实现互动直播

本节介绍如何实现互动直播。互动直播的 API 调用时序见下图：

![](https://web-cdn.agora.io/docs-files/1570604677337)

### 1. 创建用户界面

根据场景需要，为你的项目创建互动直播的用户界面。若已有用户界面，可以直接查看[导入类](#ImportClass)。

如果你想实现一个视频直播，我们推荐你添加如下 UI 元素：

- 主播视频窗口
- 退出频道按钮
	
当你使用示例项目中的 UI 设计时，你将会看到如下界面：

![](https://web-cdn.agora.io/docs-files/1568802427977)

### <a name="ImportClass"></a>2. 导入类

在项目中导入 AgoraRtcKit 类：

```objective-c
// Objective-C
// 自 3.0.0 版本，SDK 使用 AgoraRtcKit 类。
#import <AgoraRtcKit/AgoraRtcEngineKit.h>
// 在 3.0.0 版本以前，SDK 使用 AgoraRtcEngineKit 类。
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>
```

```swift
// Swift
// 自 3.0.0 版本，SDK 使用 AgoraRtcKit 类。
import AgoraRtcKit
// 在 3.0.0 版本以前，SDK 使用 AgoraRtcEngineKit 类。
import AgoraRtcEngineKit
```

<div class="alert note">Agora Native SDK 默认使用 libc++ (LLVM)，如需使用 libstdc++ (GNU)，请联系 sales@agora.io。SDK 提供的库是 Fat Image，包含 32/64 位模拟器、32/64 位真机版本。</div>

### 3. 初始化 AgoraRtcEngineKit

在调用其他 Agora API 前，需要创建并初始化 AgoraRtcEngineKit 对象。

调用 `sharedEngineWithAppId` 方法，传入获取到的 App ID，即可初始化 `AgoraRtcEngineKit`。

你还可以根据场景需要，在初始化时注册想要监听的回调事件，如本地用户加入频道，及解码远端用户视频首帧等。

```
// Objective-C
- (void)initializeAgoraEngine {
    // 输入 App ID 并初始化 AgoraRtcEngineKit 类。
    self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:appID delegate:self];
}
```

```
// Swift
func initializeAgoraEngine() {
   // 输入 App ID 并初始化 AgoraRtcEngineKit 类。
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: AppID, delegate: self)
}
```

### 4. 设置频道场景

初始化结束后，调用 `setChannelProfile` 方法，将频道场景设为直播。

一个 `AgoraRtcEngineKit` 只能使用一种频道场景。如果想切换为其他模式，需要先调用 `destroy` 方法释放当前的 `AgoraRtcEngineKit` 实例，然后使用 `sharedEngineWithAppId` 方法创建一个新实例，再调用 `setChannelProfile` 设置新的频道场景。

```objective-c
// Objective-C
// 设置频道场景。
[self.rtcEngine setChannelProfile:AgoraChannelProfileLiveBroadcasting];
```

```swift
// Swift
// 设置频道场景。
agoraKit.setChannelProfile(.liveBroadcasting)
```

### 5. 设置用户角色

直播频道有两种用户角色：主播和观众，其中默认的角色为观众。设置频道场景为直播后，你可以在 App 中参考如下步骤设置用户角色：

1. 让用户选择自己的角色是主播还是观众
2. 调用 `setClientRole` 方法，然后使用用户选择的角色进行传参

注意，直播频道内的用户，只能看到主播的画面、听到主播的声音。加入频道后，如果你想切换用户角色，也可以调用 `setClientRole` 方法。

```objective-c
// Objective-C
 if (self.isBroadcaster) {
        self.clientRole = AgoraClientRoleAudience;
        if (self.fullSession.uid == 0) {
            self.fullSession = nil;
        }
    } else {
        self.clientRole = AgoraClientRoleBroadcaster;
    }
    // 设置用户角色。
    [self.rtcEngine setClientRole:self.clientRole];
```

```swift
// Swift
// 设置用户角色。
agoraKit.setClientRole(.audience)
agoraKit.setClientRole(.broadcaster)
```

### 6. 设置本地视图

如果你想实现一个语音直播，可以直接查看[加入频道](#JoinChannel)。

成功初始化 AgoraRtcEngineKit 对象后，需要在加入频道前设置本地视图，以便在通话中看到本地图像。参考以下步骤设置本地视图：

- 调用 `enableVideo` 方法启用视频模块。
- 调用 `setupLocalVideo` 方法设置本地视图。

```objective-c
// Objective-C
// 启用视频模块。
[self.agoraKit enableVideo];
- (void)setupLocalVideo {
    AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
    videoCanvas.uid = 0;
    videoCanvas.view = self.localVideo;
    videoCanvas.renderMode = AgoraVideoRenderModeHidden;
    // 设置本地视图。
    [self.agoraKit setupLocalVideo:videoCanvas];
}
```

```swift
// Swift
// 启用视频模块。
agoraKit.enableVideo()
func addLocalSession() {
    let localSession = VideoSession.localSession()
    videoSessions.append(localSession)
    // 设置本地视图。
    agoraKit.setupLocalVideo(localSession.canvas) 
    let mediaInfo = MediaInfo(dimension: settings.dimension,
                                  fps: settings.frameRate.rawValue)
    localSession.mediaInfo = mediaInfo
    }
```

### <a name="JoinChannel"></a>7. 加入频道

完成初始化和设置本地视图后（视频通话场景），你就可以调用 `joinChannelByToken` 方法加入频道。你需要在该方法中传入如下参数：

- channelId: 传入能标识频道的频道 ID。输入频道 ID 相同的用户会进入同一个频道。

- token：传入能标识用户角色和权限的 Token。可设为如下一个值：

	- `nil`
	-  临时 Token。临时 Token 服务有效期为 24 小时。你可以在控制台里生成一个临时 Token，详见[获取临时 Token](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#%E8%8E%B7%E5%8F%96%E4%B8%B4%E6%97%B6-token)。
	-  在你的服务器端生成的 Token。在安全要求高的场景下，我们推荐你使用此种方式生成的 Token，详见[生成 Token](../../cn/Interactive%20Broadcast/token_server_cpp.md)。

 <div class="alert note">若项目已启用 App 证书，请使用 Token。</div>

- uid: 本地用户的 ID。数据类型为整型，且频道内每个用户的 `uid` 必须是唯一的。若将 `uid` 设为 0，则 SDK 会自动分配一个 `uid`，并在 `joinSuccessBlock` 回调中报告。
- joinSuccessBlock：成功加入频道回调。`joinSuccessBlock` 优先级高于 `didJoinChannel`，2 个同时存在时，`didJoinChannel` 会被忽略。 需要有 `didJoinChannel` 回调时，请将 `joinSuccessBlock` 设置为 `nil`。

更多的参数设置注意事项请参考 [joinChannelByToken](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) 接口中的参数描述。

<div class="alert note">对于 v3.0.0 之前的 SDK，如果频道中有 Web SDK，需要调用<code>enableWebSdkInteroperability</code> 开启和Web SDK 的互通。v3.0.0 及之后的 SDK 在通信和直播场景下均自动开启了与 Web SDK 的互通。</div>

```objective-c
// Objective-C
- (void)joinChannel {
    // 加入频道。
    [self.agoraKit joinChannelByToken:token channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
    }];
}
```

```swift
// Swift
// 加入频道。
agoraKit.joinChannel(byToken: KeyCenter.Token, channelId: channelId, info: nil, uid: 0, joinSuccess: nil)
```

### 8. 设置远端视图

视频通话中，通常你也需要看到其他用户。在加入频道后，可通过调用 `setupRemoteVideo` 方法设置远端用户的视图。

远端用户成功加入频道后，SDK 会触发 `firstRemoteVideoDecodedOfUid` 回调，该回调中会包含这个远端用户的 `uid` 信息。在该回调中调用 `setupRemoteVideo` 方法，传入获取到的 `uid`，设置远端用户的视图。

```objective-c
// Objective-C
// 监听 firstRemoteVideoDecodedOfUid 回调。
// SDK 接收到第一帧远端视频并成功解码时，会触发该回调。
// 可以在该回调中调用 setupRemoteVideo 方法设置远端视图。
- (void)rtcEngine:(AgoraRtcEngineKit *)engine firstRemoteVideoDecodedOfUid:(NSUInteger)uid size: (CGSize)size elapsed:(NSInteger)elapsed {
    if (self.remoteVideo.hidden) {
        self.remoteVideo.hidden = NO;
    }
    AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
    videoCanvas.uid = uid;
    videoCanvas.view = self.remoteVideo;
    videoCanvas.renderMode = AgoraVideoRenderModeHidden;
    // 设置远端视图。
    [self.agoraKit setupRemoteVideo:videoCanvas];
}
```

```swift
// Swift
// 监听 firstRemoteVideoDecodedOfUid 回调。
// SDK 接收到第一帧远端视频并成功解码时，会触发该回调。
// 可以在该回调中调用 setupRemoteVideo 方法设置远端视图。
func rtcEngine(_ engine: AgoraRtcEngineKit, firstRemoteVideoDecodedOfUid uid: UInt, size: CGSize, elapsed: Int) {
    let userSession = videoSession(of: uid)
    userSession.updateMediaInfo(resolution: size)
    // 设置远端视图。
    agoraKit.setupRemoteVideo(userSession.canvas)
    }
```

### 9. 离开频道

根据场景需要，如结束通话、关闭 App 或 App 切换至后台时，调用 `leaveChannel` 离开当前通话频道。

```objective-c
// Objective-C
[self.rtcEngine setupLocalVideo:nil];
    // 离开频道。
    [self.rtcEngine leaveChannel:nil];
    if (self.isBroadcaster) {
        [self.rtcEngine stopPreview];
    }
```

```swift
// Swift
func leaveChannel() {       
    setIdleTimerActive(true)
    agoraKit.setupLocalVideo(nil)
    // 离开频道。
    agoraKit.leaveChannel(nil)
    if settings.role == .broadcaster {
       agoraKit.stopPreview()
    }
    navigationController?.popViewController(animated: true)
    }
```

## 运行项目

你可以在 macOS 设备中运行此项目。当成功开始视频直播时，主播可以看到自己的画面；观众可以看到主播的画面。

## 相关链接

- [直播场景下，如何监听远端观众用户加入/离开频道的事件？](https://docs.agora.io/cn/faq/audience_event)
- [如何设置日志文件？](https://docs.agora.io/cn/faq/logfile)
- [如何处理视频黑屏问题？](https://docs.agora.io/cn/faq/video_blank)

