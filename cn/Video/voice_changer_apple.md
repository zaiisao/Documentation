
---
title: 变声与混响
description: How to set voice effects for iOS and macOS
platform: iOS,macOS
updatedAt: Tue May 19 2020 08:22:16 GMT+0800 (CST)
---
# 变声与混响
## 功能描述
在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。Agora 提供多种预置的变声和混响效果，你也可以灵活定制自己想要的声音，比如设置音调、均衡和混响等。

## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。详见快速开始文档：

- iOS: [实现音视频通话](../../cn/Video/start_call_ios.md)/[实现互动直播](../../cn/Video/start_live_ios.md)
- macOS: [实现音视频通话](../../cn/Video/start_call_mac.md)/[实现互动直播](../../cn/Video/start_live_mac.md)

### 使用预置效果

通过 `setLocalVoiceChanger` 可以选择以下预设的语音变声效果：

- 老男孩
- 小男孩
- 小女孩
- 猪八戒
- 空灵
- 绿巨人

```swift
// swift
// 设置变声效果为老男孩
agoraKit.setLocalVoiceChanger(.oldMan)

// 关闭变声效果
agoraKit.setLocalVoiceChanger(.off)
```

```objective-c
// objective-c
// 设置变声效果为老男孩
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOldMan];

// 关闭变声效果
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

通过 `setLocalVoiceReverbPreset` 可以选择以下预设的语音混响效果：

- 流行
- R&B
- 摇滚
- 嘻哈
- 演唱会
- KTV
- 录音棚

```swift
// swift
// 设置混响效果为流行
agoraKit.setLocalVoiceReverbPreset(.popular)

// 关闭混响效果
agoraKit.setLocalVoiceReverbPreset(.off)
```

```objective-c
// objective-c
// 设置混响效果为流行
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetPopular];

// 关闭混响效果
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetOff];
```

### 定制变声和混响效果

如果预置效果无法满足你的需求，你也可以自行调整音调、均衡和混响设置。

你可以根据以下方式把原始声音变 FM 的效果。

```swift
// swift
// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示原始音调。
agoraKit.setLocalVoicePitch(1)

// 设置本地语音均衡波段的中心频率
// 第1个参数为频谱子带索引，取值范围 [0-9]，分别代表 10 个频带，对应的中心频率是 [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// 第2个参数为每个频率区间的增益值，取值范围 [-15,15]，单位 dB, 默认值为 0
agoraKit.setLocalVoiceEqualizationOf(.band31, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band62, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band125, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band250, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band500, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band1K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band2K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band4K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band8K, withGain: 0)
agoraKit.setLocalVoiceEqualizationOf(.band16K, withGain: 0)
    
// 原始声音强度，即所谓的 dry signal，取值范围 [-20, 10]，单位为 dB
agoraKit.setLocalVoiceReverbOf(.dryLevel, withValue: -1)

// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20, 10]，单位为 dB
agoraKit.setLocalVoiceReverbOf(.wetLevel, withValue: -7)

// 所需混响效果的房间尺寸，一般房间越大，混响越强，取值范围 [0, 100]
agoraKit.setLocalVoiceReverbOf(.roomSize, withValue: 57)

// Wet signal 的初始延迟长度，取值范围 [0, 200]，单位为 ms
agoraKit.setLocalVoiceReverbOf(.wetDelay, withValue: 135)

// 混响持续的强度，取值范围为 [0, 100]，值越大，混响越强
agoraKit.setLocalVoiceReverbOf(.strength, withValue: 45)
```



```objective-c
// objective-c
// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示原始音调。
[agoraKit setLocalVoicePitch: 1];

// 设置本地语音均衡波段的中心频率
// 第1个参数为频谱子带索引，取值范围 [0-9]，分别代表 10 个频带，对应的中心频率是 [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// 第2个参数为每个频率区间的增益值，取值范围 [-15,15]，单位 dB, 默认值为 0
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand31 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand62 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand125 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand250 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand500 withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand1K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand2K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand4K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand8K withGain: 0];
[agoraKit setLocalVoiceEqualizationOfBandFrequency:AgoraAudioEqualizationBand16K withGain: 0];
    
// 原始声音强度，即所谓的 dry signal，取值范围 [-20, 10]，单位为 dB
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbDryLevel withValue: -1];

// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20, 10]，单位为 dB
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbWetLevel withValue: -7];

// 所需混响效果的房间尺寸，一般房间越大，混响越强，取值范围 [0, 100]
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbRoomSize withValue: 57];

// Wet signal 的初始延迟长度，取值范围 [0, 200]，单位为 ms
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbWetDelay withValue: 135];

// 混响持续的强度，取值范围为 [0, 100]，值越大，混响越强
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbStrength withValue: 45];
```

### API 参考

- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## 示例项目

我们在 GitHub 提供实现了变声与混响功能的 iOS 开源示例项目。你可以下载体验 [Agora-RTC-With-Voice-Changer-iOS (Swift)](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/Agora-RTC-With-Voice-Changer-iOS)，并参考 [`EffectViewController.swift`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/Agora-RTC-With-Voice-Changer-iOS/Agora-RTC-With-Voice-Changer-iOS/EffectViewController.swift)。

## 开发注意事项
以上方法都有返回值，返回值小于 0 表示方法调用失败。
