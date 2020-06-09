
---
title: 产品概述
description: 
platform: All Platforms
updatedAt: Thu May 28 2020 08:55:44 GMT+0800 (CST)
---
# 产品概述
语音通话可以实现纯语音的一对一单聊和多人群聊，不具备视频通话功能，包体积更小，适用于各种语音社交、语音会议等场景。

语音通话不同于[语音互动直播](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio?platform=All%20Platforms)。语音通话，不区分主播和观众，所有用户都可发言，默认流畅和低延时优先，典型场景如多人语音会议；语音直播，用户分为主播和观众，只有主播可以自由发言，默认高音质优先，典型场景如在线音乐直播。

## 功能和场景

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>
| 主要功能          | 功能描述                                                     | 典型适用场景         |
| ----------------- | ------------------------------------------------------------ | -------------------- |
| 伴奏混音          | 将本地或在线的音频和用户声音，同时发送并播放给频道内其他用户 | <li>在线合唱<li>音乐互动课堂 |
| 播放音效文件      | 可以播放指定的音效文件，支持设置音效的音调和空间位置。       | 棋牌游戏             |
| 变声和混响        | 提供多种预置的变声和混响效果，同时支持灵活调整用户声音的音调、均衡及混响效果。 | <li>一起 KTV<li>语音聊天室变声 |
| 听声辨位          | 设置远端用户声音出现的位置，增加游戏角色的方位感，还原真实游戏场景。 | 吃鸡游戏                       |
| 使用双声道/高音质 | 支持高音质、双声道的音频设置。                               | <li>音乐教学<li>FM 音频电台   |
| 修改原始音频数据  | 可支持变声，支持获取媒体引擎的原始语音，对原始数据进行处理   | 语音聊天室变声       |

## 关键特性

| 特性         | Agora 音频通话指标                                           |
| ------------ | ------------------------------------------------------------ |
| SDK 包体积   | 3.69 M - 7.75 M                                              |
| 音频属性     | <li>音频采样率：16 kHz - 48 kHz <li>支持单、双声道           |
| 音频抗丢包率 | 上下行抗丢包率 70%                                           |

### 平台兼容

语音通话支持 iOS、Android、Windows、macOS、Electron、Unity、Web、小程序，并支持平台间互通，具体的兼容性要求见下表。

| 平台       | 支持版本                                                     |
| ---------- | ------------------------------------------------------------ |
	| Android    | <p>4.1+</p><p>Android SDK 支持如下 ABI：</p><ul><li>armeabi-v7a<li>arm64-v8a<li>x86<li>x86_64                                                         |
| iOS        | 8.0+                                                         |
	| Windows    | <p>Windows 7+</p><p>Windows SDK 支持如下架构：<p><ul><li>x86<li>x64                                                      |
| macOS      | 10.0+                                                        |
| Unity      | <p>2017+</p><p>Unity SDK 支持如下平台：<p><ul><li>Android (armeabi-v7a、arm64-v8a、x86)<li>iOS<li>Windows (x86、x86_64)<li>macOS                                                         |
| 微信小程序 | 支持                                                         |
| Web        | <li>Chrome 58+ <li>Chrome 49（仅 Windows XP）<li>Firefox 56+ <li>Safari 11+ <li>Opera 45+ <li>QQ 10+ <li>360 安全浏览器 9.1+ |

<div class="alert note">Web 平台的支持情况还与设备型号及系统版本等有关，详见 <a href="https://docs.agora.io/cn/faq/browser_support">Agora Web SDK 支持哪些浏览器？</a></div>

## Demo 体验

### 语音通话应用 - Beckon

在应用市场下载 Beckon，快速体验跨国高清语音通话质量。

- [Android](http://dl3.beckon.cc/android/beckon/beckon-release-352.apk)
- [iOS](https://itunes.apple.com/cn/app/id927792759)
	
## 相关链接

 [Agora RTC SDK 最多支持多少人同时在线？ ](https://docs.agora.io/cn/faq/capacity)
