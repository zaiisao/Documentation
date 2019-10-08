
---
title: 变声与混响
description: How to adjust the voice effect on Windows
platform: Windows
updatedAt: Sun Sep 29 2019 08:19:03 GMT+0800 (CST)
---
# 变声与混响
## 功能描述

在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。Agora 提供多种预置的变声和混响效果，你也可以灵活定制自己想要的声音，比如设置音调、均衡和混响等。

## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。 详见[开始音视频通话](../../cn/Audio%20Broadcast/start_call_windows.md)或[开始互动直播](../../cn/Audio%20Broadcast/start_live_windows.md)。

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
// 初始化参数对象
RtcEngineParameters rep(*m_lpAgoraEngine);
VOICE_CHANGER_PRESET voiceChanger;
AUDIO_REVERB_PRESET reverbPreset;

// 设置变声效果为老男孩
voiceChanger = VOICE_CHANGER_OLDMAN;
rep.setLocalVoiceChanger(voiceChanger);

// 关闭变声效果
voiceChanger = VOICE_CHANGER_OFF;
rep.setLocalVoiceChanger(voiceChanger);

// 设置混响效果为流行
reverbPreset = AUDIO_REVERB_POPULAR;
rep.setLocalVoiceReverbPreset(reverbPreset);

// 关闭混响效果
reverbPreset = AUDIO_REVERB_OFF;
rep.setLocalVoiceReverbPreset(reverbPreset);
```

### 定制变声和混响效果

如果预置效果无法满足你的需求，你也可以自行调整音调、均衡和混响设置。

你可以根据以下方法把原始声音变成绿巨人霍克的声音。

```c++
// 初始化参数对象
RtcEngineParameters rep(*lpAgoraEngine);

// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示不需要修改音调。
int nRet = rep.setLocalVoicePitch(0.5);

// 设置本地语音均衡波段的中心频率
// 第 1 个参数为频谱子带索引，取值范围 [0, 9]，分别代表 10 个频带，对应的中心频率是 [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// 第 2 个参数为每个频率区间的增益值，取值范围 [-15, 15]，单位 dB, 默认值为 0
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_31, -15);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_62, 3);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_125, -9);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_250, -8);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_500, -6);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_1K, -4);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_2K, -3);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_4K, -2);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_8K, -1);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_16K, 1);

// 原始声音强度，即所谓的 dry signal，取值范围 [-20, 10]，单位为 dB
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_DRY_LEVEL, 10);

// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20, 10]，单位为 dB
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_WET_LEVEL, 7);

// 所需混响效果的房间尺寸，一般房间越大，混响越强，取值范围 [0, 100]
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_ROOM_SIZE, 6);

// Wet signal 的初始延迟长度，取值范围 [0, 200]，单位为 ms
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_WET_DELAY, 124);

// 混响持续的强度，取值范围为 [0, 100]，值越大，混响越强
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_STRENGTH, 78);
```

### API 参考

- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b)
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fef48b6aa3954d7e76164a43d660b94)
- [`setLocalVoiceEqualization`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a3de79ba906e6b254b997eda4d395d052)
- [`setLocalVoiceReverb`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#aa00e903b1cc6f2752373afbe556ef456)

## 开发注意事项

以上方法都有返回值，返回值小于 0 表示方法调用失败。
