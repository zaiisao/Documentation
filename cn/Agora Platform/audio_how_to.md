
---
title: 音频相关
description: 
platform: 音频相关
updatedAt: Fri Jun 14 2019 01:41:51 GMT+0800 (CST)
---
# 音频相关
### iOS 端集成 H5 游戏音量低

**问题现象**：用 layabox 自带的引擎播放 WkWebview，主播加入声网的频道，游戏里面的声音很低。该问题仅在 iOS 端发现，Android 端表现正常。

**问题原因**：WKWebView 加载的 H5 是另外一个进程通过 AVAudioSession 播放声音，不是通过 SDK 播放的。

**解决方案**：
`self.agoraEngine setAudioProfile:(AgoraRtc_AudioProfile_Default) scenario:(AgoraRtc_AudioScenario_GameStreaming);`  //选择 `GameStreaming` 模式。

**方案缺点**：

* 采用 `GameStreaming` 模式可能会出现一点回声，理论不影响使用，如有必现或大概率复现的情况可联系技术支持。
* H5 的声音不是通过 SDK 播放的，无法消除回声。

### Android 9 设备上应用退到后台或锁屏后，采集不到声音或视频

**问题现象**：Android 9 设备锁屏 1 分钟内，音频无声或看不到视频。

**问题原因**：从 Android 官网来看，这是系统强制限制。原文如下：

> **Limited access to sensors in background**
> Android 9 limits the ability for background apps to access user input and sensor data. If your app is running in the background on a device running Android 9, the system applies the following restrictions to your app:
> * Your app cannot access the microphone or camera.
> * Sensors that use the continuous reporting mode, such as accelerometers and gyroscopes, don't receive events.
> * Sensors that use the on-change or one-shot reporting modes don't receive events.
> If your app needs to detect sensor events on devices running Android 9, use a foreground service.


详见 [Android 行为变更](https://developer.android.com/about/versions/pie/android-9.0-changes-all)。


**解决方案：** 目前 Android 官网没有明确说明后台采集声音或视频应如何处理，但使用**前台服务**可以让应用正常工作。

如果 Android 9 设备用户有锁屏后采集音频或视频的需求，可以在锁屏或退至后台前起一个 Service，并在退出锁屏或返回前台前终止 Service。
关于如何起 Service，请参考 https://developer.android.com/reference/android/app/Service 。

### Android 设备进入频道后，耳机无声/语音路由不正常

**问题现象**：进频道后安卓设备上耳机无声，路由功能不正常等。

**问题原因**：

有些 Agora 的 Android 示例程序通过创建 WorkerThread 线程来维护一个全局的 RtcEngine 实例。WorkerThread 的生命周期与示例程序的生命周期是一致的。当应用程序进程销毁（调用 `destroy` 方法），WorkerThread 也随之消亡。

如果开发者没能正确退出 WorkerThread，就有可能会导致耳机无声，或语音路由不正常等问题。

在实际的应用开发中，开发者会通过操控 WorkerThread 来管理 RtcEngine 实例的生命周期。在创建引擎和加入频道时这么做是可以的。但是开发者在离开 WorkerThread 时，没有销毁 RtcEngine 实例。如果 WorkerThreader 和应用程序的生命周期不一致，这就很容易产生问题。

RtcEngine 调用 `destroy` 方法会移除所有注册过的系统 Listener （对于音频来说是 PhoneStateListener），这些监听可能会引用线程的 Looper。如果在退出 WorkerThread 时不移除系统 Listener，注册过的 Listener 会持续监听，但其引用的 Looper 已经随着 WorkerThread 线程的停止而被置为无效或清空。最终会以 Dead Binder 的形式出错。

**解决方案**：

Agora 建议你选择如下一种方法解决该问题：

- 全局只维护一个 WorkerThread 线程
- 退出频道时，还需要调用 `destroy` 方法销毁 RtcEngine

### Android 设备使用第三方录音 App 时，用户加入频道后无法发送本地音频流，也未收到 SDK 的错误或警告信息

**问题现象**：第三方录音应用使用时，用户加入频道后，无法发送本地音频，也没有收到 SDK 的任何错误信息

**问题原因**：第三方录音应用占用音频设备

**解决方案**：请参考以下逻辑部署代码：在用户进入频道前，先使用 Android 原生方法判断 Audio Recorder 的状态，当状态为 Available 时，如果用户加入频道 6 秒内连续收到警告码 WARN_ADM_RECORD_AUDIO_LOWLEVEL(1031)，或者错误码 ERR_ADM_RECORD_AUDIO_IS_ACTIVE(1033)，则判定录音设备已被占用。请提示用户关闭第三方录音应用。

<a id="audioScenario"></a>
### 音量无法调节至 0

**背景信息**：通话音量无法调节至 0，媒体音量可以。因此需要区分系统音量走的是通话音量还是媒体音量

**问题现象**：使用 SDK 后，音量无法调节至 0

**问题原因**：由于系统限制，媒体音量可以调整到 0，而通话音量不可以。

**解决方案**：

一般而言，通话音量指的是进行语音、视频通话时的音量；媒体音量指的是播放音乐、视频或游戏的音效、背景音的音量。在实际使用中，两者的差异在于，通话音量有较好的回声消除，媒体音量有较好的声音表现力。

系统音量走通话音量，是指当你在设备上调整音量时，调整的是通话音量。媒体音量同理。

SDK 在 `setAudioProfile` 中提供 6 种不同的 Audio Scenario，包括：`DEFAULT`、`CHATROOM_ENTERTAINMENT`、`EDUCATION`、`GAME_STREAMING`、`SHOWROOM` 和 `CHATROOM_GAMING`。其中：

- `GAME_STREAMING` 场景下，通信时使用媒体音量；直播时无论观众还是主播也都使用媒体音量
- `DEFAULT`、`EDUCATION` 和 `SHOWROOM` 场景下，通信时使用通话音量；直播时观众使用媒体音量，连麦后使用通话音量
- `CHATROOM_ENTERTAINMENT` 和 `CHATROOM_GAMING` 场景下，通信时使用通话音量；直播时无论观众还是主播也都使用通话音量

由于系统限制，媒体音量可以调整到 0，而通话音量不可以。如果需要将音量调整到 0，建议尝试使用媒体音量控制的 Audio Scenario。
