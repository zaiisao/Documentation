
---
title: 音频相关
description: 
platform: 音频相关
updatedAt: Mon Dec 17 2018 08:37:09 GMT+0000 (UTC)
---
# 音频相关
### iOS 端集成 H5 游戏音量低

**背景信息：**iOS SDK 2.2.0 集成 H5 游戏；

**问题现象：**用 layabox 自带的引擎播放 WkWebview，主播加入声网的频道，游戏里面的声音很低。该问题仅在 iOS 端发现，Android 端表现正常。

**问题原因：**WKWebView 加载的 H5 是另外一个进程通过 AVAudioSession 播放声音，不是通过 SDK 播放的。

**解决方案：**
`self.agoraEngine setAudioProfile:(AgoraRtc_AudioProfile_Default) scenario:(AgoraRtc_AudioScenario_GameStreaming);`  //选择 `GameStreaming` 模式。

**方案缺点：**

* 采用 `GameStreaming` 模式可能会出现一点回声，理论不影响使用，如有必现或大概率复现的情况可联系技术支持。
* H5 的声音不是通过 SDK 播放的，无法消除回声。
