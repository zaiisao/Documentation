
---
title: 产品概述
description: 
platform: All Platforms
updatedAt: Wed Apr 08 2020 07:10:10 GMT+0800 (CST)
---
# 产品概述
Agora Interactive Gaming SDK，是 Agora 针对游戏开发者 （Unity，Cocos）提供的音视频通话软件开发包。其主要目的是帮助游戏开发者在游戏中快速集成音视频通话的功能。

在游戏中集成音视频通话的功能时，用户可以选择使用 C# 的 SDK 和 Wrapper 在 Unity 的开发环境下直接接入语音或者视频功能，也可以选择使用 C++ 的 SDK 和 Wrapper 在 Cocos 的开发环境下接入语音功能。 如果用户需要使用原生的 Objective-C 和 Java 接口，也可以使用 Native SDK 来做接入。


## 功能和场景

Agora Interactive Gaming SDK 应用丰富，主要适用于使用 Unity 和 Cocos 游戏引擎需要实时音视频功能的应用，也可以用 Native SDK 在 iOS 和 Android 上进行原生开发。

| 主要功能   | 功能描述                                                     | 典型适用场景           |
| ---------- | ------------------------------------------------------------ | ---------------------- |
| 音视频互通 | 实现游戏中的实时音视频互通，可以在游戏中实现开黑语音，实时视频传输的功能 | 开黑工具               |
| 电台语音   | 可在游戏中加入语音电台功能，支持 44.1k 采样率超高音质，也支持观众与主播连麦，实现电台双向互通 | <li>MMO<li>RPG         |
| 听声辨位   | 支持游戏音效 180° 听声辨位，增加游戏角色的方位感，还原真实场景 | FPS                    |
| 趣味变声   | 支持性别变声，迷惑对手，增加游戏互动趣味性                   | <li>MOBA<li>二次元游戏 |

更多的玩法 ，点击查看示例代码：

* [在线语音聊天室](https://github.com/AgoraIO-Usecase/Chatroom)
* [剧本杀](https://github.com/AgoraIO-Usecase/Murder-Mystery-Game)

## 关键特性

Agora Interactive Gaming SDK 主要有以下特性：


| 特性       | Agora 互动游戏指标                                           |
| ---------- | ------------------------------------------------------------ |
| 最低影响   | <li>语音过程不影响游戏 FPS<li>游戏音效完美兼容（FMOD，WWISE）<li>安装包体积优化<li>不占 CPU<li>耗电低 |
| 专注质量   | <li>高音质（32 kHz 超宽带）<li>低延时（全球平均延时 76 ms）<li>无干扰（回声消除，降噪）<li>不卡顿（超强抗丢包） |
| 为游戏而生 | <li>自由模式 + 指挥模式<li>趣味变声功能<li>听声辨位功能<li>模拟器支持 |
| 立体声     | 支持立体声采集播放，提供最佳游戏直播体验                     |


### 平台兼容
	
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
<td>C++</td>
</tr>
<tr><td>iOS Audio</td>
<td>iOS</td>
<td>Objective-C、 Swift、C++</td>
</tr>
<tr><td>Android Audio</td>
<td>Android</td>
<td>Java、C++</td>
</tr>
</tbody>
</table>

## Demo 体验

* [游戏音频](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming)
* [游戏视频](https://github.com/AgoraIO/Video-Call-for-Mobile-Gaming)


