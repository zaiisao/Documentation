
---
title: 调整通话音量
description: How to adjust volume on Android
platform: Android
updatedAt: Tue Aug 13 2019 06:41:09 GMT+0800 (CST)
---
# 调整通话音量
## 功能描述

 在使用我们 SDK 时，开发者可以对 SDK 采集到的声音及 SDK 播放到声卡的声音音量进行调整，以满足产品在声音上的个性化需求。比如进行双人通话时，想实现静音操作，可以通过调整播放音量的接口将音量设置为 0。



本文梳理了在使用 SDK 从音频采集到播放各阶段中，用户可能需要调整音量的场景、各场景对应的 API 及其使用注意事项。

![](https://web-cdn.agora.io/docs-files/1548728949895)
## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Video/android_video.md)。

### 设置采集音量
**采集**是指音频信号由录音设备采集后传输到发送端的过程。这个过程中你可以使用 SDK 的接口直接调整录制声音的信号幅度，以调整录音的音量。

调节音量的参数值范围是 0 - 400，默认值 100 表示原始音量，即不对信号做缩放，400 表示原始音量的 4 倍（把信号放大到原始信号的 4 倍）。

```java
int volume = 200;
// 设置录音信号音量
rtcEngine.adjustRecordingSignalVolume(volume);
```

#### API 参考

- [`adjustRecordingSignalVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)

### 设置播放音量
**播放**是指音频信号从发送端进入到接收端，然后使用播放设备进行播放的过程。这个过程中你可以使用 SDK 的接口直接调整播放声音的信号幅度，以调整播放的音量。

调节音量的参数值范围是 0 - 400，默认值 100 表示原始音量，即不对信号做缩放，400 表示原始音量的 4 倍（把信号放大到原始信号的 4 倍）。

```java
int volume = 200;
// 设置播放信号音量
rtcEngine.adjustPlaybackSignalVolume(volume);
```

#### API 参考
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)

### 设置混音音量
**混音**是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。你可以参考[音乐混音](../../cn/Video/effect_mixing_android.md)开启混音功能。

调节混音音量的参数值范围是 0 - 100，默认值 100 表示原始文件音量，即不对信号做缩放。0 表示混音文件播放静音。

```java
int volume = 50;
// 设置远端用户听到的音乐文件音量
rtcEngine.adjustAudioMixingPublishVolume(volume);
// 设置本地用户听到的音乐文件音量
rtcEngine.adjustAudioMixingPlayoutVolume(volume);
```

你也可以直接调用 `adjustAudioMixingVolume`，同时设置本地及远端用户听到的音乐文件音量。

```java
int volume = 50;
// 设置本地及远端用户听到的音乐文件音量
rtcEngine.adjustAudioMixingVolume(volume);
```

#### API 参考
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a)

### 设置音效音量
播放**音效**是指播放短小的音频，如鼓掌、子弹撞击的声音等。你可以参考[播放音效](../../cn/Video/effect_mixing_android.md)开启音效播放。

调节音效音量的参数值范围是 0.0 - 100.0，默认值 100.0 表示原始音效音量，即不对信号做缩放。0.0 表示音效文件播放静音。

```java
// 获取全局的音效管理类
IAudioEffectManager manager = rtcEngine.getAudioEffectManager();
...
// 设置音效音量为原始音量的 50%
double volume = 50.0
// 设置所有音效的播放音量
manager.setEffectsVolume(volume);
// 设置单个音效的播放音量
// soundId 是你在调用 playEffect 时设置的音效 ID
manager.setVolumeOfEffect(soundId, volume);
```

#### API 参考
- [`setEffectsVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#ab758558563b3dd70771e5d44ba1a96f3)
- [`setVolumeOfEffect`](https://docs.agora.io/cn/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#afcd8cd6d733703c0ba153b8e1ac81ec0)

### 设置耳返音量
在音频采集、混音、播放的整个过程中，你都可以使用 `setInEarMonitoringVolume` 调整耳返的音量。

调节耳返音量的参数值范围是 0 - 100，默认值 100 表示原始音效音量，即不对信号做缩放。0 表示耳返静音。

```java
// 开启耳返监听功能
rtcEngine.enableInEarMoniroting(true);
int volume = 50;
// 设置耳返音量为原始音量的 50%
rtcEngine.setInEarMonitoringVolume(volume);
```

#### API 参考
- [`setInEarMonitoringVolume`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af71afdf140660b10c4fb0c40029c432d)

### 获取用户音量（回调方法）

在音频采集、混音、播放的整个过程中，你都可以使用下面的接口获取用户音量。

- 瞬时说话声音音量提示。如下回调获取瞬时说话音量最大的用户 ID，及音量大小。如果返回的用户 ID 为 0，则表示瞬时说话音量最大的是本地用户。
	
```java
/**
 * 获取瞬时说话音量最大的几个用户 ID
 * @param speakers 为一个数组，包含说话者的用户 ID 及音量，音量范围为 0 - 255
 * @param totalVolume 指混音后频道内的总音量，范围为 0 - 255
 */
public void onAudioVolumeIndication(AudioVolumeInfo[] speakers, int totalVolume) {
}
```

- 当前时间内累积音量最大者。如下回调获取获取特定时间段内，累积音量最大的用户 ID。如果返回的 uid 是 0，则默认为本地用户。
	
```java
// 获取当前时间段声音最大的用户 ID（仅 1 个）
public void onActiveSpeaker(int uid) {
}
```

#### API 参考
- [`onAudioVolumeIndication`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a4d37f2b4d569fa787bb8c0e3ae8cd424)
- [`onActiveSpeaker`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a895e965178d808f9d33b387ab3e50300)


## 开发注意事项

- 所有相关的方法都有返回值。返回值小于 0 表示方法调用失败。
- 如果信号音量设置太大，由于硬件设备的限制，在某些设备上可能会出现失真的声音效果。
