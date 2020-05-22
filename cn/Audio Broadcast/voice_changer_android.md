
---
title: 变声与混响
description: How to adjust voice effect for Android
platform: Android
updatedAt: Tue May 19 2020 06:20:55 GMT+0800 (CST)
---
# 变声与混响
## 功能描述
在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。Agora 提供多种预置的变声和混响效果，你也可以灵活定制自己想要的声音，比如设置音调、均衡和混响等。

## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。 详见[开始音视频通话](../../cn/Audio%20Broadcast/start_call_android.md)或[开始互动直播](../../cn/Audio%20Broadcast/start_live_android.md)。
### 使用预置效果

通过 `setLocalVoiceChanger` 可以选择以下预设的语音变声效果：

- 老男孩
- 小男孩
- 小女孩
- 猪八戒
- 空灵
- 绿巨人

```java
// 设置变声效果为老男孩
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OLDMAN);

// 关闭变声效果
mRtcEngine.setLocalVoiceChanger(VOICE_CHANGER_OFF);
```

通过 `setLocalVoiceReverbPreset` 可以选择以下预设的语音混响效果：

- 流行
- R&B
- 摇滚
- 嘻哈
- 演唱会
- KTV
- 录音棚

```java
// 设置混响效果为流行
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_POPULAR);

// 关闭混响效果
mRtcEngine.setLocalVoiceReverbPreset(AUDIO_REVERB_OFF);
```

### 定制变声和混响效果

如果预置效果无法满足你的需求，你也可以自行调整音调、均衡和混响设置。

你可以根据以下方法把原始声音变成绿巨人霍克的声音。

```java
// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示原始音调。
double pitch = 0.5;
rtcEngine.setLocalVoicePitch(pitch);
  
// 设置本地语音均衡波段的中心频率
// 第1个参数为频谱子带索引，取值范围 [0, 9]，分别代表 10 个频带，对应的中心频率是 [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// 第2个参数为每个频率区间的增益值，取值范围 [-15,15]，单位 dB, 默认值为 0
rtcEngine.setLocalVoiceEqualization(0, -15);
rtcEngine.setLocalVoiceEqualization(1, 3);
rtcEngine.setLocalVoiceEqualization(2, -9);
rtcEngine.setLocalVoiceEqualization(3, -8);
rtcEngine.setLocalVoiceEqualization(4, -6);
rtcEngine.setLocalVoiceEqualization(5, -4);
rtcEngine.setLocalVoiceEqualization(6, -3);
rtcEngine.setLocalVoiceEqualization(7, -2);
rtcEngine.setLocalVoiceEqualization(8, -1);
rtcEngine.setLocalVoiceEqualization(9, 1);
  
// 原始声音强度，即所谓的 dry signal，取值范围 [-20, 10]，单位为 dB
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_DRY_LEVEL, 10);
  
// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20, 10]，单位为 dB
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_WET_LEVEL, 7);
  
// 所需混响效果的房间尺寸，一般房间越大，混响越强，取值范围 [0, 100]
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_ROOM_SIZE, 6);
  
// Wet signal 的初始延迟长度，取值范围 [0, 200]，单位为 ms
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_WET_DELAY, 124);
  
// 混响持续的强度，取值范围为 [0, 100]，值越大，混响越强
rtcEngine.setLocalVoiceReverb(Constants.AUDIO_REVERB_STRENGTH, 78);
```

### API 参考

- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ade6883c7878b7a596d5b2563462597dd)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a10dd25bc8e129512cd6727133b7fc42f)
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a41b525f9cbf2911594bcda9b20a728c9)
- [`setLocalVoiceEqualization`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e3aa79f0d6d8f2ea81907543506d960)
- [`setLocalVoiceReverb`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a4afc32ba68e997e90ba3f128317827fa)

## 开发注意事项

以上方法都有返回值，返回值小于 0 表示方法调用失败。
