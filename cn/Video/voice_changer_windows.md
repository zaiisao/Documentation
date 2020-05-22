
---
title: 变声与混响
description: How to adjust the voice effect on Windows
platform: Windows
updatedAt: Tue May 19 2020 06:36:42 GMT+0800 (CST)
---
# 变声与混响
## 功能描述

在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。Agora 提供多种预置的变声和混响效果，你也可以灵活定制自己想要的声音，比如设置音调、均衡和混响等。

## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。 详见[开始音视频通话](../../cn/Video/start_call_windows.md)或[开始互动直播](../../cn/Video/start_live_windows.md)。

### 使用预置效果

通过 `setLocalVoiceChanger` 可以选择以下预设的语音变声效果：

- 老男孩
- 小男孩
- 小女孩
- 猪八戒
- 空灵
- 绿巨人

通过 `setLocalVoiceReverbPreset` 可以选择以下预设的语音混响效果：

- 流行
- R&B
- 摇滚
- 嘻哈
- 演唱会
- KTV
- 录音棚

```c++
VOICE_CHANGER_PRESET voiceChanger;
AUDIO_REVERB_PRESET reverbPreset;

// 设置变声效果为老男孩
voiceChanger = VOICE_CHANGER_OLDMAN;
rtcEngine.setLocalVoiceChanger(voiceChanger);

// 关闭变声效果
voiceChanger = VOICE_CHANGER_OFF;
rtcEngine.setLocalVoiceChanger(voiceChanger);

// 设置混响效果为流行
reverbPreset = AUDIO_REVERB_POPULAR;
rtcEngine.setLocalVoiceReverbPreset(reverbPreset);

// 关闭混响效果
reverbPreset = AUDIO_REVERB_OFF;
rtcEngine.setLocalVoiceReverbPreset(reverbPreset);
```

### 定制变声和混响效果

如果预置效果无法满足你的需求，你也可以自行调整音调、均衡和混响设置。

你可以根据以下方法把原始声音变成绿巨人霍克的声音。

```c++
// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示不需要修改音调。
int nRet = rtcEngine.setLocalVoicePitch(0.5);

// 设置本地语音均衡波段的中心频率
// 第 1 个参数为频谱子带索引，取值范围 [0, 9]，分别代表 10 个频带，对应的中心频率是 [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// 第 2 个参数为每个频率区间的增益值，取值范围 [-15, 15]，单位 dB, 默认值为 0
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_31, -15);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_62, 3);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_125, -9);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_250, -8);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_500, -6);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_1K, -4);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_2K, -3);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_4K, -2);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_8K, -1);
nRet = rtcEngine.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_16K, 1);

// 原始声音强度，即所谓的 dry signal，取值范围 [-20, 10]，单位为 dB
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_DRY_LEVEL, 10);

// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20, 10]，单位为 dB
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_WET_LEVEL, 7);

// 所需混响效果的房间尺寸，一般房间越大，混响越强，取值范围 [0, 100]
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_ROOM_SIZE, 6);

// Wet signal 的初始延迟长度，取值范围 [0, 200]，单位为 ms
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_WET_DELAY, 124);

// 混响持续的强度，取值范围为 [0, 100]，值越大，混响越强
nRet = rtcEngine.setLocalVoiceReverb(AUDIO_REVERB_STRENGTH, 78);
```

### API 参考

- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a99dc3d74202422436d40f6d7aa6e99dc)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a51a429a5a848b2ad591220aa6c24a898)
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a43616f919e0906279dff5648830ce31a)
- [`setLocalVoiceEqualization`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8c75994eb06ab26a1704715ec76e0189)
- [`setLocalVoiceReverb`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4d1d1309f97f3c430a1aa2d060bb7316)

## 开发注意事项

以上方法都有返回值，返回值小于 0 表示方法调用失败。
