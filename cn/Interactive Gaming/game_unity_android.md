
---
title: 实现游戏语音功能
description: 
platform: Unity
updatedAt: Mon Apr 13 2020 15:26:52 GMT+0800 (CST)
---
# 实现游戏语音功能
<div class="alert note">互动游戏 Unity SDK 将不再维护，请使用 RTC Unity SDK 替代。详情见<a href="https://docs.agora.io/cn/Voice/start_call_audio_unity?platform=Unity">实现语音通话</a >。</div>

使用 Agora 的 `Hello-Unity3D-Agora` 代码示例可以演示以下功能:

-   创建/加入频道

-   自由发言

-   离开频道

本页分别展示如何使用 Unity3D 包在 Android 平台和 iOS 平台上实现上述功能。

## Android 平台实现
### 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Unity3D 软件包。软件包结构如下:

    ![](https://web-cdn.agora.io/docs-files/1548830935872)

> `Hello-Unity3D-Agora` 即为本文需要使用的代码示例。

2.  请确保已满足以下环境要求:

    -   Unity3D 5.5 或更高版本
    -   Android Studio 2.0 或更高版本
    -   两部或多部支持音频功能的 Android 真机 \(4.0 或更高版本\)
    -   一个 App ID，详见 [获取 App ID](../../cn/Interactive%20Gaming/token.md)

3.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


### 步骤 2: 创建新项目

创建一个新的 Unity3D 项目，如有必要请参考 [Unity 快速开始](https://docs.unity3d.com/2018.2/Documentation/Manual/GettingStarted.html)。如果你已有项目，则跳过这一步。


### 步骤 3: 添加 SDK

1. 在项目的根目录下，创建如下文件夹或路径：

	- `Assets/Plugins/Android/AgoraAudioKit.plugin/libs`
	- `Assets/Scripts`

2. 将下载下来的 SDK 包中的 `libs/Scripts/AgoraGamingSDK` 文件夹复制到你项目的 `Assets/Scripts` 路径下
3. 将 SDK 包中的 `libs/Android/agora-rtc-sdk.jar` 文件复制到你项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` 路径下
4. 将示例代码中的 `Assets/Plugins/Android/mainTemplate.gradle` 文件复制到你项目的 `Assets/Plugins/Android` 路径下
5. 将示例代码中的 `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` 文件复制到你项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin` 路径下
6. 将 SDK 包 `libs/Android` 路径下的 **armeabi-v7a** 和 **x86** 文件夹复制到你项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` 路径下

### 步骤 4：添加权限

在你的项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` 文件中，添加如下权限：

```C#
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

### 步骤 5：防止混淆代码

在你的项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/project.properties` 文件中添加如下行，防止混淆代码：

```
-keep class io.agora.**{*;}
```

### 步骤 6：调用 API 实现游戏语音

调用 [互动游戏 API](../../cn/API%20Reference/game_unity.md) 中的 API，实现你想要的功能。游戏语音包含两种模式：

- 自由发言模式
- 指挥模式

你可以调用 `setChannelProfile` 方法选择任意一种模式。

## iOS 平台实现

### 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Unity3D 软件包。软件包结构如下:

![](https://web-cdn.agora.io/docs-files/1548829843264)

-   **include**：头文件。通常不在 Unity3D 项目中使用，除非需要修改原始数据。

-   **libs**: 库文件。本文档只涉及 **Scripts** 文件夹中的 framework 文件和 C\# API 库文件。

-   **samples**: 示例代码

2.  环境要求:

    -   Unity3D 5.5 或更高版本

    -   Xcode 8.0 或更高版本

    -   两部或多部支持音频功能的 iOS 真机（9.0 或更高版本）

3. [获取一个 App ID](../../cn/Interactive%20Gaming/token.md)。

4. 请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


### 步骤 2: 创建一个新项目

创建一个新的 Unity3D 项目，如有必要请参考 [Unity 快速开始](https://docs.unity3d.com/2018.2/Documentation/Manual/GettingStarted.html)。如果你已有项目，则跳过这一步。


### 步骤 3: 添加 SDK

1.  在你的项目的根目录下，创建 `Assets/Scripts` 文件夹。

2.  将示例代码中的 `Assets/Scripts/AgoraGamingSDK` 文件夹复制到你的项目中的 `Assets/Scripts` 文件夹中。

3.  添加 `.framework` 文件：

    -   将示例代码中的 `Assets/Plugins/iOS/AgoraAudioKit.framework` 文件夹复制到你的项目中的 `Assets/Plugins/iOS` 目录下。

    -   将示例代码中的 `Assets/Plugins/iOS/libagoraSdkCWrapper.a` 文件复制到你的项目中的 `Assets/Plugins/iOS` 目录下。

    -   添加如下系统文件：`AgoraAudioKit.framework`，`AudioToolBox.framework`，`CoreTelephony.framework`，`AVFoundation.framework` 和 `libresolv.tbd`。

    ![](https://web-cdn.agora.io/docs-files/1543568568723)

### 步骤 4：添加权限

添加所需权限，例如麦克风权限。

![](https://web-cdn.agora.io/docs-files/1543568604885)


### 步骤 5：调用 API

调用 [互动游戏 API](../../cn/API%20Reference/game_unity.md) 中的 API，实现你想要的功能。游戏语音包含两种模式：

- 自由发言模式
- 指挥模式

你可以调用 `setChannelProfile` 方法选择任意一种模式。





