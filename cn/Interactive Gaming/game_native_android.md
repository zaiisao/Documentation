
---
title: 实现游戏语音功能
description: 
platform: Android
updatedAt: Thu Sep 13 2018 23:35:13 GMT+0800 (CST)
---
# 实现游戏语音功能
# 实现游戏语音功能

使用 Agora 的 `Hello-Gaming-Agora-Android` 代码示例可以演示以下功能:

-   创建/加入频道

-   自由发言

-   离开频道


## 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/2.2/download) 最新的 Interactive Gaming SDK 软件包(Android)。软件包结构如下:

    <img alt="../_images/AMG-SDK-structure-full-Native-Android.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-SDK-structure-full-Native-Android.png" style="width: 283.0px; height: 193.0px;"/>

> `Hello-Gaming-Agora-Android` 即为本文需要使用的代码示例。你也可以直接从 [Github](https://github.com/AgoraIO/Hello-Gaming-Agora-Android/)下载。

1.  请确保已满足以下环境要求:

    -   Android Studio 2.0 或更高版本

    -   两部或多部支持音频功能的 Android 真机\(4.0 或更高版本\)

    -   一个 App ID，详见 [获取 App ID](../../cn/Agora%20Platform/token.md) 。

2.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


## 步骤 2: 编译代码示例

1.  用 Android Studio 打开项目进行编译。

2.  修改 `app/src/main/java/io/agora/amg/AGApplication.java` 当中的 `appId` 。

3.  连接好真机，点击 **Run** 编译代码示例。

## 步骤 3: 演示游戏语音

演示游戏语音至少需要两部或多部 Android 真机，本文仅以两部手机为例进行演示。

1.  在两部手机上输入相同频道名称。

    <img alt="../_images/AMG-Voice-Android-Hello-World-Running.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Voice-Android-Hello-World-Running.png" style="width: 324.0px; height: 576.0px;"/>

2.  点击 **JOIN** 即可加入同一频道。

你现在可以在频道内自由发言了！请观察该页面的文字提示消息来确定演示程序的运行状况。


