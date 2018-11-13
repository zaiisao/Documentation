
---
title: 产品概述
description: 
platform: All Platforms
updatedAt: Tue Nov 13 2018 08:44:07 GMT+0000 (UTC)
---
# 产品概述
本文为你展示 Agora Interactive Gaming SDK 的产品简介、适用场景、产品特性，以及相关文档。

## 产品简介

Agora Interactive Gaming SDK，是 Agora 针对游戏开发者 （Unity，Cocos）提供的音视频通话软件开发包。其主要目的是帮助游戏开发者在游戏中快速集成音视频通话的功能。

在游戏中集成音视频通话的功能时，用户可以选择使用 C\# 的 SDK 和 Wrapper 在 Unity 的开发环境下直接接入语音或者视频功能，也可以选择使用 C++、Lua 或 JS 的 SDK 和 Wrapper 在 Cocos 的开发环境下接入语音功能。 如果用户需要使用原生的 Objective-C 和 Java 接口，也可以使用 Native SDK 来做接入。

Agora Interactive Gaming SDK 一共包含了 5 个 SDK。 支持的系统和开发语言如下：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>SDK</td>
<td>支持系统</td>
<td>开发语言</td>
</tr>
<tr><td>Unity Video</td>
<td>iOS、 Android、 Windows</td>
<td>C#</td>
</tr>
<tr><td>Unity Audio</td>
<td>iOS、 Android、 Windows</td>
<td>C#</td>
</tr>
<tr><td>Cocos Audio</td>
<td>iOS、 Android</td>
<td>C++、 Lua、 JS</td>
</tr>
<tr><td>iOS Audio</td>
<td>iOS</td>
<td>Objective-C、 Swift</td>
</tr>
<tr><td>Android Audio</td>
<td>Android</td>
<td>Java</td>
</tr>
</tbody>
</table>



## 功能描述

Agora Interactive Gaming SDK 具有以下主要功能：

-   音视频互通：实现游戏中的实时音视频互通，可以在游戏中实现开黑语音，实时视频传输的功能

-   电台语音：可在游戏中加入语音电台功能，支持 44.1k 采样率超高音质，也支持观众与主播连麦，实现电台双向互通

-   听声辩位：支持游戏音效 180° 听声辩位，增加游戏角色的方位感，还原真实场景

-   趣味变声：支持性别变声，迷惑对手，增加游戏互动趣味性


## 适用场景

Agora Interactive Gaming SDK 应用丰富，主要适用于使用 Unity 和 Cocos 游戏引擎需要实时音视频功能的应用，也可以用 native SDK 在 iOS 和 Android 上进行原生开发。

## 产品特性

Agora Interactive Gaming SDK 主要有以下特性：

-   最低影响：语音过程不影响游戏 FPS，游戏音效完美兼容（FMOD，WWISE），安装包体积优化，不占 CPU，耗电低

-   专注质量：高音质（32 KHz 超宽带）、低延时（全球平均延时 76 ms）、无干扰（回声消除，降噪）、不卡顿（超强抗丢包）

-   为游戏而成：自由模式 + 指挥模式，趣味变声功能，听声辨位功能，模拟器支持

-   支持立体声采集播放，提供最佳游戏直播体验


## 相关文档

Agora 为你提供了如下文档，方便在使用中查阅：

-   [实现游戏语音功能](../../cn/Quickstart%20Guide/game_native_android.md) 展示了如何从零开始完成 Agora Interactive Gaming SDK 的部署及使用，包括环境搭建、集成方法、编译代码等内容。

-   [游戏 API](../../cn/API%20Reference/game_android.md) 展示了使用 Agora Interactive Gaming SDK 过程中你可以调用的各 API，以及调用这些 API 能实现的功能、以及会收到的回调等内容。



