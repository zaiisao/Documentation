
---
title: 美声与音效
description: How to set voice effects for iOS and macOS
platform: iOS,macOS
updatedAt: Fri Jun 19 2020 13:39:57 GMT+0800 (CST)
---
# 美声与音效
## 功能描述
在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。Agora 提供多种预置的变声和混响效果，你也可以灵活定制自己想要的声音，比如设置音调、均衡和混响等。

Agora 提供[在线 Demo](https://www.agora.io/cn/audio-demo)，你可以体验变声与混响效果。

## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。详见快速开始文档：

- iOS: [实现音视频通话](../../cn/Interactive%20Broadcast/start_call_ios.md)/[实现互动直播](../../cn/Interactive%20Broadcast/start_live_ios.md)
- macOS: [实现音视频通话](../../cn/Interactive%20Broadcast/start_call_mac.md)/[实现互动直播](../../cn/Interactive%20Broadcast/start_live_mac.md)

### 预设声音效果

#### 变声音效

通常在语聊场景中，通过 `setLocalVoiceChanger` 中以下枚举值，你可以实现变声音效：

| 枚举值                 | 变声音效 |
| :--------------------- | :------- |
| AgoraAudioVoiceChangerOldMan   | 老年男性   |
| AgoraAudioVoiceChangerBabyBoy  | 小男孩   |
| AgoraAudioVoiceChangerBabyGirl | 小女孩   |
| AgoraAudioVoiceChangerZhuBaJie | 猪八戒   |
| AgoraAudioVoiceChangerEthereal | 空灵     |
| AgoraAudioVoiceChangerHulk     | 绿巨人   |

<div class="alert note">同一时间，只能实现变声音效、美音、语聊美声、混响音效中的一种。</div>

```swift
// swift
// 预设变声音效为老年男性
agoraKit.setLocalVoiceChanger(.voiceChangerOldMan)
// 关闭效果
agoraKit.setLocalVoiceReverbPreset(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设变声音效为老年男性
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOldMan];
// 关闭效果
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

####  美音

通常在语聊或歌唱场景中，通过 `setLocalVoiceChanger` 中以下枚举值，你可以实现美音效果：

| 枚举值                  | 美音效果 |
| :---------------------- | :------- |
| AgoraAudioVoiceBeautyVigorous   | 浑厚     |
| AgoraAudioVoiceBeautyDeep       | 低沉     |
| AgoraAudioVoiceBeautyMellow     | 圆润     |
| AgoraAudioVoiceBeautyFalsetto   | 假音     |
| AgoraAudioVoiceBeautyFull       | 饱满     |
| AgoraAudioVoiceBeautyClear      | 清澈     |
| AgoraAudioVoiceBeautyResounding | 高亢     |
| AgoraAudioVoiceBeautyRinging    | 嘹亮     |
| AgoraAudioVoiceBeautySpacial    | 空旷     |

<div class="alert note">同一时间，只能实现变声音效、美音、语聊美声、混响音效中的一种。</div>

```swift
// swift
// 预设美音效果为浑厚
agoraKit.setLocalVoiceChanger(.voiceBeautyVigorous)
// 关闭效果
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设美音效果为浑厚
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceBeautyVigorous];
// 关闭效果
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### 语聊美声

通常在语聊场景中，通过 `setLocalVoiceChanger` 中以下枚举值，你可以实现语聊美声效果：

| 枚举值                               | 语聊美声效果 |
| :----------------------------------- | :----------- |
| AgoraAudioGeneralBeautyVoiceMaleMagnetic   | 磁性（男）   |
| AgoraAudioGeneralBeautyVoiceFemaleFresh    | 清新（女）   |
| AgoraAudioGeneralBeautyVoiceFemaleVitality | 活力（女）   |

<div class="alert warning">该功能主要细化了男声和女声各自的特点，请确保使用 <tt>AgoraAudioGeneralBeautyVoiceMaleMagnetic</tt> 处理男声，使用 <tt>AgoraAudioGeneralBeautyVoiceFemaleFresh</tt> 或 <tt>AgoraAudioGeneralBeautyVoiceFemaleVitality</tt> 处理女声，否则音频可能会产生失真。</div>

<div class="alert note">同一时间，只能实现变声音效、美音、语聊美声、混响音效中的一种。</div>

```swift
// swift
// 预设语聊美声效果为磁性（男）
agoraKit.setLocalVoiceChanger(.generalBeautyVoiceMaleMagnetic)
// 关闭效果
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设语聊美声效果为磁性（男）
[self.agoraKit setLocalVoiceChanger: AgoraAudioGeneralBeautyVoiceMaleMagnetic];
// 关闭效果
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### 混响音效

通过 `setLocalVoiceReverbPreset`，你可以实现以下混响效果：

| 枚举值                        | 描述                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| AgoraAudioReverbPresetPopular          | 流行                                                         |
| AgoraAudioReverbPresetRnB              | R&B                                                          |
| AgoraAudioReverbPresetRock             | 摇滚                                                         |
| AgoraAudioReverbPresetHipHop           | 嘻哈                                                         |
| AgoraAudioReverbPresetVocalConcert    | 演唱会                                                       |
| AgoraAudioReverbPresetKTV              | KTV                                                          |
| AgoraAudioReverbPresetStudio           | 录音棚                                                       |
| AgoraAudioReverbPresetFxKTV           | KTV（增强版）                                                |
| AgoraAudioReverbPresetFxVocalConcert | 演唱会（增强版）                                             |
| AgoraAudioReverbPresetFxUncle         | 大叔                                                         |
| AgoraAudioReverbPresetFxSister        | 小姐姐                                                       |
| AgoraAudioReverbPresetFxStudio        | 录音棚（增强版）                                             |
| AgoraAudioReverbPresetFxPopular       | 流行（增强版）                                               |
| AgoraAudioReverbPresetFxRNB           | R&B（增强版）                                                |
| AgoraAudioReverbPresetFxPhonograph    | 留声机                                                       |
| AgoraAudioReverbPresetVirtualStereo          | 虚拟立体声。虚拟立体声是指将单声道的音轨渲染出立体声的效果，使频道内所有用户听到有空间感的声音效果。 |

<div class="alert warning">当使用以 <tt>AgoraAudioReverbPresetFx</tt> 为前缀的枚举值时，请确保在调用该方法前将 <tt>setAudioProfile</tt> 的 <tt>profile</tt> 参数设置为 <tt>AgoraAudioProfileMusicHighQuality(4)</tt> 或 <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>，否则该方法设置无效。</div>

<div class="alert note"><li>同一时间，只能实现变声音效、美音、语聊美声、混响音效中的一种。<li>为达到更好的混响音效，Agora 推荐使用以 <tt>AgoraAudioReverbPresetFx</tt> 为前缀的枚举值。<li>当使用 <tt>AgoraAudioReverbPresetVirtualStereo</tt> 设置虚拟立体声时，Agora 推荐在调用该方法前将 <tt>setAudioProfile</tt> 的 <tt>profile</tt> 参数设置为 <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>。</div>

```swift
// swift
// 预设混响音效为演唱会（增强版 ）
agoraKit.setLocalVoiceChanger(.fxVocalConcert)
// 关闭混响效果
agoraKit.setLocalVoiceReverbPreset(.off)
```

```objective-c
// objective-c
// 预设混响音效为演唱会（增强版 ）
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetFxVocalConcert];
// 关闭混响效果
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetOff];
```

### 定制声音效果

如果预设效果无法满足你的需求，你也可以通过 `setLocalVoicePitch`、`setLocalVoiceEqualization` 和 `setLocalVoiceReverb` 自行调整音调、均衡和混响效果。

你可以根据以下示例代码把原始声音变成绿巨人霍克的声音。

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

- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## 示例项目

我们在 GitHub 提供实现了变声与混响功能的 iOS 开源示例项目。你可以下载体验 [Agora-RTC-With-Voice-Changer-iOS (Swift)](https://github.com/AgoraIO/Advanced-Audio/tree/master/iOS%26macOS/Agora-RTC-With-Voice-Changer-iOS)，并参考 [`EffectViewController.swift`](https://github.com/AgoraIO/Advanced-Audio/blob/master/iOS%26macOS/Agora-RTC-With-Voice-Changer-iOS/Agora-RTC-With-Voice-Changer-iOS/EffectViewController.swift)。

## 开发注意事项

- 本文所有方法对人声的处理效果最佳，Agora 不推荐调用这类方法处理含人声和音乐的音频数据。
- Agora 建议不要同时使用预设声音效果相关方法和定制声音效果相关方法，否则可能会发生未定义行为。若需同时使用，请在调用预设声音效果相关方法后调用定制声音效果相关方法，否则定制声音效果相关方法会不生效。
- Agora 建议不要同时使用 `setLocalVoiceChanger` 和 `setLocalVoiceReverbPreset` 方法，否则先调用的方法会不生效。
- 为达到更好的声音效果，Agora 推荐在调用 `setLocalVoiceChanger` 或 `setLocalVoiceReverbPreset` 前将 `setAudioProfile` 的 `profile` 参数设置为 `AgoraAudioProfileMusicHighQuality(4)` 或 `AgoraAudioProfileMusicHighQualityStereo(5)`。
