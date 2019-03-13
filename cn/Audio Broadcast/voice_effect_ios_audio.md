
---
title: 调整音调、音色
description: How to adjust voice effect for iOS
platform: iOS
updatedAt: Wed Mar 13 2019 07:17:33 GMT+0000 (UTC)
---
# 调整音调、音色
## 功能描述
在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。Agora 提供了一系列的方法让开发者灵活定制自己想要的声音，比如设置音调、均衡和混响等。 
## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Audio%20Broadcast/ios_audio.md)。

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

- [`setLocalVoicePitch`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## 开发注意事项
以上方法都有返回值，返回值小于 0 表示方法调用失败。
