
---
title: 实现游戏语音功能
description: 
platform: Unity
updatedAt: Wed Jan 30 2019 09:55:15 GMT+0000 (UTC)
---
# 实现游戏语音功能
使用 Agora 的 `Hello-Unity3D-Agora` 代码示例可以演示以下功能:

-   创建/加入频道

-   自由发言

-   离开频道

本页分别展示如何使用 Unity3D 包在 Android 平台和 iOS 平台上实现上述功能。

## Android 平台实现
### 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Unity3D 软件包。软件包结构如下:

    <img alt="../_images/AMG-SDK-structure-full-Unity3D.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-SDK-structure-full-Unity3D.png" style="width: 840px;"/>

> `Hello-Unity3D-Agora` 即为本文需要使用的代码示例。你也可以直接从 [Github](https://github.com/AgoraIO/Hello-Unity3D-Agora/)下载。

2.  请确保已满足以下环境要求:

    -   Unity3D 5.5 或更高版本

    -   Android Studio 2.0 或更高版本

    -   两部或多部支持音频功能的 Android 真机 \(4.0 或更高版本\)

    -   一个 App ID，详见 [获取 App ID](../../cn/Agora%20Platform/token.md)

3.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


### 步骤 2: 编译代码示例

1.  使用 Unity 打开该项目。

    <img alt="../_images/AMG-Voice-Unity3D-Hello-World.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Voice-Unity3D-Hello-World.png" />

2.  修改 `Assets/HelloUnity3D.cs` 当中的 `appId` 。

3.  按照下图顺序选择 **Build And Run** \(导出并安装到设备\)。

    ![](https://web-cdn.agora.io/docs-files/1543804584356)

### 步骤 3: 演示游戏语音

演示游戏语音至少需要两部或多部 Android 真机，本文仅以两部手机为例进行演示。

1.  在两部手机上输入相同频道名称。

   ![](https://web-cdn.agora.io/docs-files/1543570993172)


2.  点击 **Join Channel** 即可加入同一频道。


你现在可以在频道内自由发言了！请观察该页面的文字提示消息来确定演示程序的运行状况。

## iOS 平台实现

### 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Unity3D 软件包。软件包结构如下:

    <img alt="../_images/AMG-SDK-structure-full-Unity3D.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-SDK-structure-full-Unity3D.png" style="width: 840px;"/>

`Hello-Unity3D-Agora` 即为本文需要使用的代码示例。你也可以直接从 [Github](https://github.com/AgoraIO/Hello-Unity3D-Agora/)下载。

2.  请确保已满足以下环境要求:

    -   Unity3D 5.5 或更高版本

    -   Xcode 8.0 或更高版本

    -   两部或多部支持音频功能的 iOS 真机 \(9.0 或更高版本\)

    -   一个 App ID，详见 [获取 App ID](../../cn/Agora%20Platform/token.md)。

3.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


### 步骤 2: 编译代码示例

1.  使用 Unity 打开该项目。

    <img alt="../_images/AMG-Voice-Unity3D-Hello-World.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Voice-Unity3D-Hello-World.png"/>

2.  修改 `Assets/HelloUnity3D.cs` 当中的 `appId` 。

3.  按照下图顺序选择 **Build And Run**。

    ![](https://web-cdn.agora.io/docs-files/1543804619166)

4.  连接好真机，点击 **Run** 编译代码示例。

如在编译过程中遇到任何问题，请参考 [游戏语音 FAQs](https://docs.agora.io/cn/2.2/faq/faq/game)。

### 步骤 3: 演示游戏语音

演示游戏语音至少需要两部或多部 iOS 真机，本文仅以两部手机为例进行演示。

1.  在两部手机上输入相同频道名称。

    ![](https://web-cdn.agora.io/docs-files/1543571014803)

2.  点击 **Join Channel** 即可加入同一频道。

你现在可以在频道内自由发言了！请观察该页面的文字提示消息来确定演示程序的运行状况。


