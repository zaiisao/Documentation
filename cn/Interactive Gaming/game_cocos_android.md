
---
title: 实现游戏语音功能
description: 
platform: Cocos
updatedAt: Thu Nov 14 2019 03:10:37 GMT+0800 (CST)
---
# 实现游戏语音功能
使用 Agora 的 `Hello-Cocos2d-Agora` 代码示例可以实现以下功能:

-   创建/加入频道
-   自由发言
-   离开频道

本页分别展示如何使用 Cocos2d 包在 Android 平台和 iOS 平台上实现上述功能。

## Android 平台实现

### 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Cocos2d 软件包。软件包结构如下:

    <img alt="../_images/AMG-SDK-structure-full-Cocos2d.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-SDK-structure-full-Cocos2d.png" style="width: 500.0px;"/>

> `Hello-Cocos2d-Agora` 即为本文需要使用的代码示例。你也可以直接从 [GitHub](https://github.com/AgoraIO/Hello-Cocos2d-Agora/)下载。

2.  请确保已满足以下环境要求:

    -   Cocos2d 3.0 或更高版本

    -   Android Studio 2.0 或更高版本

    -   两部或多部支持音频功能的 Android 真机\(4.0 或更高版本\)

    -   一个 App ID， 详见 [获取 App ID](../../cn/Agora%20Platform/token.md)。

3.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


### 步骤 2: 编译代码示例

1.  用 **Android Studio** 打开项目 `proj.android-studio` 。

2.  修改 `app/src/org/cocos2d/cpp/AGApplication.java` 当中的 `appId`。

3.  连接好真机，点击 **Run** 按钮编译代码示例。

### 步骤 3: 演示游戏语音

演示游戏语音至少需要两部或多部 Android 真机，本文仅以两部手机为例进行演示。

1.  在两部手机上输入相同频道名称。

    <img alt="../_images/AMG-Voice-Cocos2d-Hello-World-Running.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Voice-Cocos2d-Hello-World-Running.png" style="width: 500.0px;"/>

2.  点击 **Join Channel** 即可加入同一频道。

你现在可以在频道内自由发言了！请观察该页面的文字提示消息来确定演示程序的运行状况。

## iOS 平台实现 

### 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Cocos2d 软件包。软件包结构如下:

    <img alt="../_images/AMG-SDK-structure-full-Cocos2d.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-SDK-structure-full-Cocos2d.png" style="width: 500.0px;"/>

> `Hello-Cocos2d-Agora` 即为本文需要使用的代码示例。你也可以直接从 [GitHub](https://github.com/AgoraIO/Hello-Cocos2d-Agora/)下载。

1.  请确保已满足以下环境要求:

    -   Cocos2d 3.0 或更高版本

    -   Xcode 8.0 或更高版本

    -   两部或多部支持音频功能的 iOS 真机\(9.0 或更高版本\)

    -   一个 App ID， 详见 [获取 App ID](../../cn/Agora%20Platform/token.md)

2.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


### 步骤 2: 编译代码示例

1.  使用 **Xcode** 打开项目 `proj.ios\mac` 进行编译。

2.  修改 `ios/AppController.mm` 当中的 `appId` 。

3.  连接好真机，点击 **Run** 进行编译。


### 步骤 3: 演示游戏语音

演示游戏语音至少需要两部或多部 iOS 真机，本文仅以两部手机为例进行演示。

1.  在两部手机上输入相同频道名称。

    <img alt="../_images/AMG-Voice-Cocos2d-Hello-World-Running.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Voice-Cocos2d-Hello-World-Running.png" style="width: 500.0px;"/>

2.  点击 **Join Channel** 即可加入同一频道。

你现在可以在频道内自由发言了！请观察该页面的文字提示消息来确定演示程序的运行状况。



