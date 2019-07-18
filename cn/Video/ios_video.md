
---
title: 集成客户端
description: 
platform: iOS
updatedAt: Tue Jun 04 2019 09:29:54 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora SDK for iOS 进行通话/直播前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

## 前提条件

- Xcode 10.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。
- 请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。

> 请使用真机运行示例代码。模拟机可能会因为功能缺失而无法运行示例代码。

## <a name = "appid-ios"></a>创建项目并获取 App ID

1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号、创建项目。
2. 点击 **Dashboard** 左侧的**项目管理**页，查看你所创建的项目详情。

	![](https://web-cdn.agora.io/docs-files/1562926227232)

3. 在项目详情页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1562926236498)


## 添加 Agora SDK 到项目中

你可以选择两种方式将 Agora SDK 添加到你的项目中：

- [自动添加库](#auto-add)：该方法无需下载 Agora SDK，直接使用 CocoaPods 将 SDK 库添加到项目中。
- [手动添加库](#man-add)：该方法需要下载 Agora SDK，并手动将 SDK 的库添加到项目中。

### <a name = "auto-add"></a>自动添加库

1. 安装 CocoaPods。在 Terminal 里输入以下命令行：

	```
	brew install cocoapods
	```

> - 如果你已在系统中预置了 CocoaPods 和 Homebrew，可以跳过这一步。
> - 如果 Terminal 显示 `-bash: brew: command not found`，则需要先安装 Homebrew，再输入该命令行。详见 [Homebrew 安装方法](https://brew.sh/index.html)。

2. 创建 Podfile 文件。在 Terminal 里进入项目所在路径，然后输入以下命令行。输入后，项目路径下会出现一个 Podfile 文本文件。

	```
	pod init
	```

3. 添加 Agora SDK 的引用。打开 Podfile 文本文件，修改文件为如下内容。注意 **“Your App”** 是你的 Target 名称。

	```
	platform :ios, '9.0'
	use_frameworks!

	target 'Your App' do
		pod 'AgoraRtcEngine_iOS'
	end
	```

4. 更新本地 CocoaPods 库。在 Terminal 内输入以下命令更新本地库版本：

	```
	pod update
	```

5. 安装 Agora SDK。在 Terminal 内输入以下命令进行安装：

	```
	pod install
	```

	如果 Terminal 显示 `Pod installation complete!`，则表示自动添加库已完成。点击打开项目的 **YourApp.xcworkspace** 文件，或输入以下命令行进行打开，注意 **“Your App”** 是你的 Target 名称。

	```
	open YourApp.xcworkspace
	```

### <a name = "man-add"></a>手动添加库

1. 下载 [Agora Video SDK for iOS](https://docs.agora.io/cn/Agora%20Platform/downloads) ，并解压。
2. 使用 Xcode 打开你想要运行的项目，然后选中当前 Target。

	<img alt="../_images/ios_video_2.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_2.jpg" />

3. 打开 **Build Phases** 页签，展开 **Link Binary with Libraries** 项并添加如下库。点击 **+** 图标开始添加

	<img alt="../_images/ios_video_3.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_3.jpg" />
 
   - `AgoraRtcEngineKit.framework`
   - `Accelerate.framework`
   - `SystemConfiguration.framework`
   - `libc++.tbd`
   - `libresolv.tbd`
   - `CoreMedia.framework`
   - `VideoToolbox.framework`
   - `AudioToolbox.framework`
   - `CoreTelephony.framework`
   - `AVFoundation.framework`
   - `CoreML.framework`

 其中，`AgoraRtcEngineKit.framework` 位于下载下来的 SDK 包 **libs** 文件夹下。因此点击 + 后，还需要点击 **Add Other…** ，然后进入到 SDK 的 **libs** 路径下，点击并添加 `AgoraRtcEngineKit.framework`。

	<img alt="../_images/ios_video_5.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_5.jpg" />

> 如需支持 **iOS 9** 或更低版本的设备，请在 **XCode** 中将对 `CoreML.framework` 的依赖设为 **Optional**。

## 授权使用 Agora SDK

使用 Agora SDK 前，需要对设备的麦克风进行授权。打开 `info.plist` ，点击 **+** 图标开始添加：

- **Privacy - Microphone Usage Description**，并填入使用麦克风的目的，例如：**Video Chat**。
- **Privacy - Camera Usage Description**，并填入使用摄像头的目的，例如：**Video Chat**。

**添加前：**

<img alt="../_images/ios_video_6.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_6.jpg" />

**添加后：**

<img alt="../_images/ios_video_7.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_7.jpg" />

## 访问库

根据你的项目所对应的编程语言，选择使用 [Objective-C](#oc) 或 [Swift](#swift) 进行访问。

### <a name = "oc"></a>Objective-C

在项目需要使用 Agora Voice SDK API 的文件里，填入 `#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>`。

> SDK 提供的库是 Fat Image，包含 32/64 位模拟器、32/64 位真机版本。

### <a name = "swift"></a>Swift

在项目需要使用 Agora Voice SDK API 的文件里，填入 `import AgoraRtcEngineKit`。

<img alt="../_images/ios_video_8.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_8.jpg" />

## 其他设置

你还可以根据实际项目需要进行如下项的设置：

- 设置后台模式。后台模式允许 iOS 设备退回后台后，依然可以运行相关功能。 选中当前 Target，在 **Capabilities** 下点击并展开 **Background Modes** 项，打开启用开关，然后勾选 **Audio，AirPlay and Picture in Picture** 。

  <img alt="../_images/ios_video_9.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_9.jpg" />

- 启用或禁用 Bitcode。使用 Bitcode 开发的 App 在上传 App Store 后，App Store 会对其进行优化或瘦身。 选中当前 Target，在 **Build Settings** 下，根据实际需要启用或禁用 Bitcode。

  <img alt="../_images/ios_video_10.jpg" src="https://web-cdn.agora.io/docs-files/cn/ios_video_10.jpg" />
	
## 相关文档

完成了客户端集成后，你可以使用 Agora SDK，依次实现左侧《快速开始》菜单栏下的步骤，进行通话/直播：

- 初始化
- 加入频道
- 发布和订阅流
