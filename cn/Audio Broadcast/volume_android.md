
---
title: 调整通话音量
description: How to adjust volume on Android
platform: Android
updatedAt: Tue Aug 18 2020 10:17:15 GMT+0800 (CST)
---
# 调整通话音量
## 功能描述

 在使用我们 SDK 时，开发者可以对 SDK 采集到的声音及 SDK 播放到声卡的声音音量进行调整，以满足产品在声音上的个性化需求。比如进行双人通话时，想实现静音操作，可以通过调整播放音量的接口将音量设置为 0。



本文梳理了在使用 SDK 从音频采集到播放各阶段中，用户可能需要调整音量的场景、各场景对应的 API 及其使用注意事项。

![](https://web-cdn.agora.io/docs-files/1577958825602)

## 实现方法
在调整通话音量前，请确保已在你的项目中实现基本的实时音视频功能。详见[实现音视频通话](../../cn/Audio%20Broadcast/start_call_android.md)或[实现互动直播](../../cn/Audio%20Broadcast/start_live_android.md)。

### 设置采集音量
**采集**是指音频信号由录音设备采集后传输到发送端的过程。

![](https://web-cdn.agora.io/docs-files/1577958939565)

你可以通过 `adjustRecordingSignalVolume` 方法直接调节录制声音的信号幅度，由此实现调节录音的音量。

该方法中 `volume` 参数表示录音信号的音量，取值范围为 [0, 400]：
- 0: 静音。
- 100: （默认值）原始音量，即不对信号做缩放。
- 400: 原始音量的 4 倍（把信号放大到原始信号的 4 倍）。

#### 示例代码

```java
int volume = 200;
// 将录音音量设置为原始音量的 200%。
rtcEngine.adjustRecordingSignalVolume(volume);
```

#### API 参考

- [`adjustRecordingSignalVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)

### 设置播放音量
**播放**是指音频信号从发送端进入到接收端，然后使用播放设备进行播放的过程。

![](https://web-cdn.agora.io/docs-files/1577959124720)

你可以通过 `adjustPlaybackSignalVolume` 方法或 `adjustUserPlaybackSignalVolume` 方法直接调节播放声音的信号幅度，由此实现调节播放的音量。

- `adjustPlaybackSignalVolume`：
  - 用于调节本地播放的所有远端用户混音后的音量。
  - 该方法中 `volume` 参数表示播放音量，取值范围为 [0, 400]。
- `adjustUserPlaybackSignalVolume`：
  - 用于调节本地播放的指定远端用户混音后的音量。你可以多次调用该方法来调节不同远端用户在本地播放的音量，或对某个远端用户在本地播放的音量调节多次。
  - 该方法中 `volume` 参数表示播放音量，取值范围为 [0, 100]。

<div class="alert note"><li>从 v2.3.2 开始，adjustPlaybackSignalVolume 接口仅支持调节所有远端用户的本地播放音量。如果你使用的是 v2.3.2 及之后版本的 Native SDK，静音本地音频请同时调用 adjustPlaybackSignalVolume(0) 和 adjustAudioMixingPlayoutVolume(0)。<li>adjustUserPlaybackSignalVolume 方法要在加入频道后调用。</li></div>

#### 示例代码

```java
int volume = 200;
// 将本地播放的所有远端用户音量设置为原始音量的 200%。
rtcEngine.adjustPlaybackSignalVolume(volume);
// 将本地播放的指定远端用户混音后的音量设置为原始音量的 200%。
rtcEngine.adjustUserPlaybackSignalVolume(uid, volume);
```

#### API 参考
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)
- [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aac9c5135996428d9a238fe8e66858268)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)

### 设置混音音量
**混音**是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。你可以参考[播放音效/混音](../../cn/Audio%20Broadcast/audio_effect_mixing_android.md)开启混音功能。

![](https://web-cdn.agora.io/docs-files/1577959275593)

你可以直接调用 `adjustAudioMixingVolume`，同时调节本地及远端用户听到的音乐文件混音音量。

该方法中 `volume` 参数表示音乐文件的音量，取值范围为 [0, 100]。默认值 100 表示原始文件音量，即不对信号做缩放。0 表示混音音乐文件播放静音。

#### 示例代码

```java
int volume = 50;
// 设置本地及远端用户听到的音乐文件音量为原始音量的 50%。
rtcEngine.adjustAudioMixingVolume(volume);
```

你也可以通过 `adjustAudioMixingPlayoutVolume` 方法和 `adjustAudioMixingPublishVolume` 方法分别调节混音音量。

- `adjustAudioMixingPlayoutVolume`：
  - 用于调节音乐文件在本地播放的混音音量。
  - 该方法中 `volume` 参数表示音乐文件在本地播放的混音音量，取值范围为 [0, 100]。
- `adjustAudioMixingPublishVolume`：
  - 用于调节音乐文件在远端播放的混音音量。
  - 该方法中 `volume` 参数表示音乐文件在远端播放的混音音量，取值范围为 [0, 100]。

<div class="alert note">adjustAudioMixingPlayoutVolume 方法和 adjustAudioMixingPublishVolume 方法要在加入频道后调用。</div>

#### 示例代码

```java
int volume = 50;
// 设置远端用户听到的音乐文件音量为原始音量的 50%。
rtcEngine.adjustAudioMixingPublishVolume(volume);
// 设置本地用户听到的音乐文件音量为原始音量的 50%。
rtcEngine.adjustAudioMixingPlayoutVolume(volume);
```

#### API 参考
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a)

### 设置音效音量
播放**音效**是指播放短小的音频，如鼓掌、子弹撞击的声音等。你可以参考[播放音效/混音](../../cn/Audio%20Broadcast/audio_effect_mixing_android.md)开启音效播放。

![](https://web-cdn.agora.io/docs-files/1577959440461)

使用播放设备播放音效音量时，你可以通过 `setEffectsVolume` 方法或 `setVolumeOfEffect` 方法设置音效文件在本地的播放音量。
- `setEffectsVolume`：
  - 用于设置所有音效文件的播放音量。
  - 该方法中 `volume` 参数表示音效文件的音量，取值范围为 [0, 100]。
- `setVolumeOfEffect`：
  - 用于设置指定音效文件的播放音量。
  - 该方法中 `volume` 参数表示音效文件的音量，取值范围为 [0, 100]。

#### 示例代码

```java
// 获取全局的音效管理类。
IAudioEffectManager manager = rtcEngine.getAudioEffectManager();
...
// 设置音效音量为原始音量的 50%。
double volume = 50.0
// 设置所有音效的播放音量。
manager.setEffectsVolume(volume);
// 设置单个音效的播放音量。
// soundId 是你在调用 playEffect 时设置的音效 ID。
manager.setVolumeOfEffect(soundId, volume);
```

#### API 参考
- [`setEffectsVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#ab758558563b3dd70771e5d44ba1a96f3)
- [`setVolumeOfEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#afcd8cd6d733703c0ba153b8e1ac81ec0)

### 设置耳返音量
在音频采集、混音、播放的整个过程中，你都可以使用 `setInEarMonitoringVolume` 调整耳返的音量。

![](https://web-cdn.agora.io/docs-files/1577959520909)

调节耳返音量的参数值范围是 [0, 100]，默认值 100 表示原始音效音量，即不对信号做缩放。0 表示耳返静音。

#### 示例代码

```java
// 开启耳返监听功能。
rtcEngine.enableInEarMoniroting(true);
int volume = 50;
// 设置耳返音量为原始音量的 50%。
rtcEngine.setInEarMonitoringVolume(volume);
```

#### API 参考
- [`setInEarMonitoringVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af71afdf140660b10c4fb0c40029c432d)

### 获取用户音量（回调）

在音频采集、混音、播放的整个过程中，你都可以使用下面的接口获取用户音量等信息。

- 瞬时说话声音音量提示。`onAudioVolumeIndication` 回调报告频道内瞬时音量最高的几个用户（即说话者）的用户 ID 及他们的音量。若返回的 uid 为 0，则表示返回的是本地用户的瞬时音量。

 <div class="alert note">你需要开启 enableAudioVolumeIndication 方法才能收到该回调。</div>
	
#### 示例代码
	
```java
// 获取瞬时说话音量最高的几个用户（即说话者）的用户 ID、他们的音量及本地用户是否在说话。
// @param speakers 为一个数组，包含说话者的用户 ID 、音量及本地用户人声状态。音量的取值范围为 [0, 255]。
// @param totalVolume 指混音后频道内的总音量，取值范围为 [0, 255]。
public void onAudioVolumeIndication(AudioVolumeInfo[] speakers, int totalVolume) {
}
```

- 监测到活跃用户提示。`onActiveSpeaker` 回调报告特定时间段内累积音量最高的用户 ID。如果返回的 uid 为 0，则默认为本地用户。

 <div class="alert note">你需要开启 enableAudioVolumeIndication 方法才能收到该回调。</div>
	
#### 示例代码
	
```java
// 获取当前时间段声音最大的用户 ID（仅 1 个）
public void onActiveSpeaker(int uid) {
}
```

#### API 参考
- [`onAudioVolumeIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a4d37f2b4d569fa787bb8c0e3ae8cd424)
- [`onActiveSpeaker`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a895e965178d808f9d33b387ab3e50300)


## 开发注意事项

由于硬件设备的限制，当使用调节信号设置音量的方法将音量设置过大时，在某些设备上可能会出现失真的声音效果。

## 相关链接

实现调整通话音量过程中，你还可以参考如下文档：

- [如何处理音量太小问题？](https://docs.agora.io/cn/faq/audio_low)
