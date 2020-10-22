
---
title: 实现语音通话
description: 
platform: iOS
updatedAt: Tue Sep 22 2020 02:00:16 GMT+0800 (CST)
---
# 实现语音通话
本文介绍如何使用 Agora 语音通话 SDK 快速实现语音通话。

## 快速跑通示例项目

如果你是第一次使用声网的服务，我们推荐观看下面的视频，了解关于声网服务的基本信息以及如何快速跑通示例项目。

<div class="alert info">点击参与<a href="https://www.wenjuan.com/s/7FbeEz6/" target="_blank">视频教程问卷调查</a>，帮助我们改进体验。</div>

<video src="https://web-cdn.agora.io/docs-files/1593742244429" poster="https://web-cdn.agora.io/docs-files/1597911607719"   controls width = 100% height = auto>你的浏览器不支持 <code>video</code> 标签。</video>

<div class="alert note">视频中展示的 UI 可能有部分调整更新，请以当前最新版为准。</div>

## 示例项目

Agora 在 GitHub 上提供开源的实时语音通话示例项目 [Agora-iOS-Voice-Tutorial-Swift-1to1](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/One-to-One-Voice/Agora-iOS-Voice-Tutorial-Swift-1to1)。在实现相关功能前，你可以下载并查看源代码。

## 前提条件

- Xcode 9.0 或以上版本
- 支持 iOS 8.0 或以上版本的 iOS 设备
- 有效的 [Agora 账户](https://docs.agora.io/cn/Agora%20Platform/sign_in_and_sign_up) 和 [App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#getappid)

<div class="alert note">如果你的网络环境部署了防火墙，请根据<a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">应用企业防火墙限制</a>打开相关端口。</div>

## 准备开发环境

本节介绍如何创建项目，并将 Agora SDK 集成至你的项目中。

### 创建 iOS 项目

参考以下步骤创建一个 iOS 项目。如果已有 iOS 项目，可以直接查看[集成 SDK](#IntegrateSDK)。

<details>
	<summary><font color="#3ab7f8">创建 iOS 项目</font></summary>
	
1. 打开 **Xcode** 并点击 **Create a new Xcode project**。
2. 选择项目类型为 **Single View App**，并点击 **Next**。
3. 输入项目信息，如项目名称、开发团队信息、组织名称和语言，并点击 **Next**。
 
	**Note**：如果你没有添加过开发团队信息，会看到 **Add account…** 按钮。点击该按钮并按照屏幕提示登入 Apple ID，完成后即可选择你的账户作为开发团队。
4. 选择项目存储路径，并点击 **Create**。
5. 将你的 iOS 设备连接至电脑。
6. 进入 **TARGETS > Project Name > General > Signing** 菜单，选择 **Automatically manage signing**，并在弹出菜单中点击 **Enable Automatic**。
	
	![](https://web-cdn.agora.io/docs-files/1568803609379)
</details>
	
<a name="IntegrateSDK"></a>
### 集成 SDK

选择如下任意一种方式将 Agora SDK 集成到你的项目中。

<div class="alert note">自 3.0.1 版本起，下载的 SDK 内仅包含动态库包 <tt>AgoraRtcKit.framework</tt>。如果你将旧版本 SDK 升级至 3.0.1 版本，请参考<a href="../../cn/Voice/migration_apple.md">升级指南</a >。</div>

**方法一：使用 CocoaPods 自动集成**

1. 开始前确保你已安装 **Cocoapods**。参考 [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) 安装说明。
2. 在 **Terminal** 里进入项目根目录，并运行 `pod init` 命令。项目文件夹下会生成一个 `Podfile` 文本文件。
3. 打开 `Podfile` 文件，修改文件为如下内容。注意将 `Your App` 替换为你的 Target 名称，并将 `version` 替换为你需集成的 SDK 版本。

 
```
# platform :ios, '9.0' use_frameworks!
target 'Your App' do
    pod 'AgoraAudio_iOS', '~> version'
end
```


4. 在 **Terminal** 内运行 `pod update` 命令更新本地库版本。
5. 运行 `pod install` 命令安装 Agora SDK。成功安装后，**Terminal** 中会显示 `Pod installation complete!`，此时项目文件夹下会生成一个 `xcworkspace` 文件。
6. 打开新生成的 `xcworkspace` 文件。

**方法二：手动复制 SDK 文件**

1. 前往 [SDK 下载页面](https://docs.agora.io/cn/Agora%20Platform/downloads)，获取最新版的 Agora SDK，然后解压。SDK 包中有两种 `AgoraRtcKit.framework` 和 `AgoraRtcCryptoLoader.framework`，区别如下：
 - `libs` 文件夹内的 `AgoraRtcKit.framework` 和 `AgoraRtcCryptoLoader.framework` 包含 armv7 和 arm64 架构，不支持模拟器。集成该库后，app 可以直接上架 App Store。
 - `ALL_ARCHITECTURE` 文件夹内的 `AgoraRtcKit.framework` 和 `AgoraRtcCryptoLoader.framework` 包含 armv7、arm64 、x86_64 和 i386 架构，支持模拟器。集成该库后，app 不能直接上架 App Store，你需要在上架前手动移除库中的 x86_64 和 i386 架构。
 ![](https://web-cdn.agora.io/docs-files/1591775772856)
2. 将 `AgoraRtcKit.framework` 复制到项目文件夹下。 
3. 打开 **Xcode**（以 Xcode 11.0 为例），进入 **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**  菜单，点击 **+**，再点击 **Add Other…** 添加 `AgoraRtcKit.framework`。添加完成后，项目会自动链接其他系统库。为保证动态库的签名和 app 的签名一致，你需要将动态库的 <b>Embed</b> 属性设置为 <b>Embed & Sign</b>。

<details>
	<summary><font color="#3ab7f8">如需集成 3.0.0 以下版本的 SDK，点击查看操作步骤。</font></summary>

1. 解压 Agora SDK。
2. 将 `libs` 文件夹内的 `AgoraRtcEngineKit.framework` 文件复制到项目路径下。
3. 打开 Xcode，进入 **TARGETS > Project Name > Build Phases > Link Binary with Libraries** 菜单，点击 **+** 添加如下库。在添加 `AgoraRtcEngineKit.framework` 文件时，还需在点击 **+** 后点击 **Add Other…** ，找到本地文件并打开。

 - AgoraRtcEngineKit.framework
 - Accelerate.framework
 - AudioToolbox.framework
 - AVFoundation.framework
 - CoreMedia.framework
 - CoreTelephony.framework
 - libc++.tbd
 - libresolv.tbd
 - SystemConfiguration.framework


</details>

 <div class="alert warning">根据 Apple 官方要求，app 的 Extension 不允许包含动态库。如果工程中的 Extension 需要集成 SDK，则集成动态库时需将文件状态改为 <b>Do Not Embed</b>。</div>

  <div class="alert note">如需使用媒体流加密功能，需添加 <tt>AgoraRtcCryptoLoader.framework</tt>。添加后 app 体积会增大。</div>
 
**动态库添加前**：
 
![](https://web-cdn.agora.io/docs-files/1583329514061)
 
**动态库添加后**：
 
![](https://web-cdn.agora.io/docs-files/1584688934447)

### 添加媒体设备权限

根据场景需要，在 **info.plist** 文件中，点击 **+** 图标开始添加如下内容，获取相应的设备权限：


| Key | Type | Value |
| ---------------- | ---------------- | ---------------- |
| Privacy - Microphone Usage Description      | String      | 使用麦克风的目的，例如：for a call or live interactive streaming。      |


<div class="alert note">iOS 14.0 版本新增了 <b>Privacy - Local Network Usage Description</b> 权限。如果使用 3.1.2 之前版本的 SDK，你需要添加该权限。详见 <a href="https://docs.agora.io/cn/faq/local_network_privacy">FAQ</a >。</div>
	
**添加前**：
 
![](https://web-cdn.agora.io/docs-files/1584604864457)
 
**添加后**：
 
 ![](https://web-cdn.agora.io/docs-files/1584604875770) 

## 实现语音通话

本节介绍如何实现语音通话。语音通话的 API 调用时序见下图：

![](https://web-cdn.agora.io/docs-files/1584590910617)

### 1. 创建用户界面

根据场景需要，为你的项目创建语音通话的用户界面。若已有用户界面，可以直接查看[导入类](#ImportClass)。

在语音通话中，我们推荐你添加如下 UI 元素：

- 语音通话窗口
- 退出频道按钮
	
当你使用示例项目中的 UI 设计时，你将会看到如下界面：

![](https://web-cdn.agora.io/docs-files/1584592390009)
	
### <a name="ImportClass"></a>2. 导入类

在项目中导入 AgoraRtcKit 类：

```objective-c
// 自 3.0.0 版本，SDK 使用 AgoraRtcKit 类。
#import <AgoraRtcKit/AgoraRtcEngineKit.h>
// 在 3.0.0 版本以前，SDK 使用 AgoraRtcEngineKit 类。
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>
```

```swift
// 自 3.0.0 版本，SDK 使用 AgoraRtcKit 类。
import AgoraRtcKit
// 在 3.0.0 版本以前，SDK 使用 AgoraRtcEngineKit 类。
import AgoraRtcEngineKit
```

<div class="alert note">Agora Native SDK 默认使用 libc++ (LLVM)，如需使用 libstdc++ (GNU)，请联系 sales@agora.io。SDK 提供的库是 Fat Image，包含 32/64 位模拟器、32/64 位真机版本。</div>


### 3. 初始化 AgoraRtcEngineKit

在调用其他 Agora API 前，需要创建并初始化 `AgoraRtcEngineKit` 对象。

调用 `sharedEngineWithAppId` 方法，传入获取到的 App ID，即可初始化 `AgoraRtcEngineKit`。

你还可以根据场景需要，在初始化时注册想要监听的回调事件，如远端用户加入频道或离开频道等。

```objective-c
// Objective-C
- (void)initializeAgoraEngine {
    // 输入 App ID 并初始化 AgoraRtcEngineKit 类。
    self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:appID delegate:self];
}
```

```swift
// Swift
func initializeAgoraEngine() {
   // 输入 App ID 并初始化 AgoraRtcEngineKit 类。
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: AppID, delegate: self)
}
```


### 4. 加入频道

完成初始化后，你就可以调用 `joinChannelByToken` 方法加入频道。你需要在该方法中传入如下参数：
- channelId: 传入能标识频道的频道 ID。输入频道 ID 相同的用户会进入同一个频道。
- token: 传入能标识用户角色和权限的 Token。你可以设置如下值：
	- `nil`。
	-控制台中生成的临时 Token。一个临时 Token 的有效期为 24 小时，详情见[获取临时 Token](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#%E8%8E%B7%E5%8F%96%E4%B8%B4%E6%97%B6-token)。
	- 你的服务器端生成的正式 Token。适用于对安全要求较高的生产环境，详情见[生成 Token](../../cn/Voice/token_server.md)。

<div class="alert note">若项目已启用 App 证书，请使用 Token。</div>

- uid: 本地用户的 ID。数据类型为整型，且频道内每个用户的 `uid` 必须是唯一的。若将 `uid` 设为 0，则 SDK 会自动分配一个 `uid`，并在 `joinSuccessBlock` 回调中报告。
- joinSuccessBlock：成功加入频道回调。`joinSuccessBlock` 优先级高于 `didJoinChannel`，2 个同时存在时，`didJoinChannel` 会被忽略。 需要有 `didJoinChannel` 回调时，请将 `joinSuccessBlock` 设置为 `nil`。

更多的参数设置注意事项请参考 [joinChannelByToken](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) 接口中的参数描述。

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
func joinChannel() {
    // 加入频道。
    agoraKit.joinChannel(byToken: Token, channelId: "demoChannel1", info:nil, uid:0) { [unowned self] (channel, uid, elapsed) -> Void in}
    self.logVC?.log(type: .info, content: "did join channel")
    }
    isStartCalling = true
}
```
		
### 5. 离开频道

根据场景需要，如结束通话、关闭 app 或 app 切换至后台时，调用 `leaveChannel` 离开当前通话频道。

```objective-c
// Objective-C
- (void)leaveChannel {
    // 离开频道。
    [self.agoraKit leaveChannel:^(AgoraChannelStats *stat)
}
```

```swift
// Swift
func leaveChannel() {
        // 离开频道。
        agoraKit.leaveChannel(nil)
        isStartCalling = false
        self.logVC?.log(type: .info, content: "did leave channel")
    }
```

## 运行项目
你可以在 iOS 设备中运行此项目。当成功开始语音通话时，你和远端用户可听到彼此的声音。

## 相关链接

如果你需要实现一对多群聊场景，可以前往 GitHub 下载以下示例项目，或查看源代码。

- Swift 项目: [OpenVoiceCall-iOS](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/Group-Voice-Call/OpenVoiceCall-iOS)，参考 [RoomViewController.swift](https://github.com/AgoraIO/Basic-Audio-Call/blob/master/Group-Voice-Call/OpenVoiceCall-iOS/OpenVoiceCall/RoomViewController.swift) 中的代码。
- Objective-C 项目: [OpenVoiceCall-iOS-Objective-C](https://github.com/AgoraIO/Basic-Audio-Call/tree/master/Group-Voice-Call/OpenVoiceCall-iOS-Objective-C)，参考 [RoomViewController.m](https://github.com/AgoraIO/Basic-Audio-Call/blob/master/Group-Voice-Call/OpenVoiceCall-iOS-Objective-C/OpenVoiceCall-iOS-Objective-C/RoomViewController.m) 中的代码。

使用 Agora 语音通话 SDK 开发过程中，你还可以参考如下文档：

- [如何设置日志文件？](https://docs.agora.io/cn/faq/logfile)
- [为什么在运行集成 RTC SDK 的 iOS app 时会看到查找本地网络设备的弹窗提示？](https://docs.agora.io/cn/faq/local_network_privacy)
