
---
title: 美声与音效
description: How to set voice effects for iOS and macOS
platform: iOS,macOS
updatedAt: Sat Aug 08 2020 14:30:59 GMT+0800 (CST)
---
# 美声与音效
## 功能描述

在社交娱乐应用中，为增添场景的趣味性并提升互动体验，通常需要美化人声或为人声增添丰富的音效。例如在语聊房场景中，用户可以选择音效来增添自己声音的立体感。Agora RTC SDK 提供预设的人声效果，也支持通过音调、声音均衡和混响等设置自定义人声效果。你可以通过 Agora 提供的[在线 Demo](https://www.agora.io/cn/audio-demo) 体验 SDK 预设的人声效果。

## 实现方法


开始前请确保已在你的项目中实现基本的实时音视频功能。详见以下文档：

- iOS: [实现音视频通话](../../cn/Video/start_call_ios.md)或[实现互动直播](../../cn/Video/start_live_ios.md)
- macOS: [实现音视频通话](../../cn/Video/start_call_mac.md)或[实现互动直播](../../cn/Video/start_live_mac.md)


### 使用预设的人声效果

为满足不同场景中对人声效果的需求，SDK 预设了以下人声效果：

<table>
  <tr>
    <th colspan="2">人声效果</th>
    <th>适用场景</th>
  </tr>
  <tr>
    <td rowspan="2">美声</td>
    <td>语聊美声</td>
    <td>以说话声为主的音视频场景：<li>语音相亲</li><li>情感电台</li><li>语音连麦直播</li><li>语音 PK 直播</li><li>语聊房</li><li>开黑语音</li></td>
  </tr>
  <tr>
    <td>音色变换</td>
    <td>以说话声或歌声为主的音视频场景：<li>语音连麦直播</li><li>语音 PK 直播</li><li>语聊房</li><li>开黑语音</li><li>语音相亲</li><li>K 歌房</li><li>FM 电台</li></td>
  </tr>
  <tr>
    <td rowspan="3">音效</td>
    <td>变声音效</td>
    <td>以说话声为主的音视频场景：<li>语音连麦直播</li><li>语音 PK 直播</li><li>语聊房</li><li>开黑语音</li></td>
  </tr>
  <tr>
    <td>曲风音效</td>
    <td>以歌声为主的音视频场景：<li>K 歌房</li><li>音乐电台</li><li>直播秀场</li></td>
  </tr>
  <tr>
    <td>空间塑造</td>
    <td>以说话声或歌声为主的音视频场景：<li>语音连麦直播</li><li>语音 PK 直播</li><li>语聊房</li><li>开黑语音</li><li>语音相亲</li><li>K 歌房</li><li>FM 电台</li></td>
  </tr>
 </table>

通过调用 `setLocalVoiceChanger` 或 `setLocalVoiceReverbPreset` 方法，你可以使用 SDK 预设的人声效果。


<div class="alert note"><li>调用该方法前，你需要将 <tt>setAudioProfile</tt> 的 <tt>profile</tt> 参数设置为 <tt>AgoraAudioProfileMusicHighQuality(4)</tt> 或 <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>，且 <tt>scenario</tt> 参数设置为 <tt>AgoraAudioScenarioGameStreaming(3)</tt>。</li><li><tt>setLocalVoiceChanger</tt> 和 <tt>setLocalVoiceReverbPreset</tt> 中预设的人声效果互斥。如果先后调用这两个方法，则先调用的方法会不生效。</li> </div>


#### 语聊美声

语聊美声是指在不改变原声辨识度的前提下，根据男女声各自的特点美化说话声。

通过 `setLocalVoiceChanger` 中以下枚举值，你可以实现语聊美声效果：

| 枚举值                               | 描述                                                         |
| :----------------------------------- | :----------------------------------------------------------- |
| `AgoraAudioGeneralBeautyVoiceMaleMagnetic`   | 磁性（男）。请确保使用该枚举美化男声，否则音频可能会产生失真。 |
| `AgoraAudioGeneralBeautyVoiceFemaleFresh`    | 清新（女）。请确保使用该枚举美化女声，否则音频可能会产生失真。 |
| `AgoraAudioGeneralBeautyVoiceFemaleVitality` | 活力（女）。请确保使用该枚举美化女声，否则音频可能会产生失真。 |

```swift
// swift
// 预设人声效果为磁性（男）。
agoraKit.setLocalVoiceChanger(.generalBeautyVoiceMaleMagnetic)
// 关闭人声效果。
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设人声效果为磁性（男）。
[self.agoraKit setLocalVoiceChanger: AgoraAudioGeneralBeautyVoiceMaleMagnetic];
// 关闭人声效果。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### 音色变换

音色变换可以让人声的音色朝特定的方向改变。根据不同的听众或用户声音的特点，用户可以选择最合适自己的效果。

通过 `setLocalVoiceChanger` 中以下枚举值，你可以实现音色变换效果：

| 枚举值                  | 描述   |
| :---------------------- | :----- |
| `AgoraAudioVoiceBeautyVigorous`   | 浑厚。 |
| `AgoraAudioVoiceBeautyDeep`       | 低沉。 |
| `AgoraAudioVoiceBeautyMellow`     | 圆润。 |
| `AgoraAudioVoiceBeautyFalsetto`   | 假音。 |
| `AgoraAudioVoiceBeautyFull`       | 饱满。 |
| `AgoraAudioVoiceBeautyClear`      | 清澈。 |
| `AgoraAudioVoiceBeautyResounding` | 高亢。 |
| `AgoraAudioVoiceBeautyRinging`    | 嘹亮。 |

```swift
// swift
// 预设人声效果为浑厚。
agoraKit.setLocalVoiceChanger(.voiceBeautyVigorous)
// 关闭人声效果。
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设人声效果为浑厚。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceBeautyVigorous];
// 关闭人声效果。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### 变声音效

变声音效是指将说话声朝着特定的方向进行调整，以达到区别原声的效果。

通过 `setLocalVoiceChanger` 和 `setLocalVoiceReverbPreset` 中以下枚举值，你可以实现变声音效：

<table>
  <tr>
    <th>方法</th>
		<th>枚举值</th>
    <th>描述</th>
  </tr>
  <tr>
    <td rowspan="5"><tt>setLocalVoiceChanger</tt></td>
		<td><tt>AgoraAudioVoiceChangerOldMan</tt></td>
    <td>老男人，建议用于处理男声。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerBabyBoy</tt></td>
    <td>小男孩，建议用于处理男声。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerBabyGirl</tt></td>
    <td>小女孩，建议用于处理女声。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerZhuBaJie</tt></td>
    <td>猪八戒。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerHulk</tt></td>
    <td>绿巨人。 </td>
  </tr>
	<tr>
    <td rowspan="2"><tt>setLocalVoiceReverbPreset</tt></td>
		<td><tt>AgoraAudioReverbPresetFxUncle</tt></td>
    <td>大叔，建议用于处理男声。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetFxSister</tt></td>
    <td>少女，建议用于处理女声。</td>
  </tr>
 </table>

```swift
// swift
// 预设人声效果为绿巨人。
agoraKit.setLocalVoiceChanger(.voiceChangerHulk)
// 关闭人声效果。
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设人声效果为绿巨人。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerHulk];
// 关闭人声效果。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

#### 曲风音效

曲风音效可以在演唱特定风格的歌曲时，使歌声与伴奏更加契合。

通过 `setLocalVoiceReverbPreset` 中以下枚举值，你可以实现曲风音效：

<div class="alert note">为达到更好的人声效果，Agora 推荐使用以 <tt>AgoraAudioReverbPresetFx</tt> 为前缀的枚举值。</div>

| 枚举值                  | 描述           |
| :---------------------- | :------------- |
| `AgoraAudioReverbPresetFxPopular` | 流行。         |
| `AgoraAudioReverbPresetPopular`    | 流行（旧版）。 |
| `AgoraAudioReverbPresetFxRNB`     | R&B。          |
| `AgoraAudioReverbPresetRnB`        | R&B（旧版）。  |
| `AgoraAudioReverbPresetRock`       | 摇滚。         |
| `AgoraAudioReverbPresetHipHop`     | 嘻哈。         |

```swift
// swift
// 预设人声效果为流行。
agoraKit.setLocalVoiceReverbPreset(.fxPopular)
// 关闭人声效果。
agoraKit.setLocalVoiceReverbPreset(.off)
```

```objective-c
// objective-c
// 预设人声效果为流行。
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetFxPopular];
// 关闭人声效果。
[self.agoraKit setLocalVoiceReverbPreset: AgoraAudioReverbPresetOff];
```

#### 空间塑造

空间塑造是指通过空间混响效果营造一定的空间氛围，让人声仿佛从特定的场地中传出。

通过 `setLocalVoiceChanger` 和 `setLocalVoiceReverbPreset` 中以下枚举值，你可以实现空间塑造效果：

<div class="alert note"><li>为达到更好的人声效果，Agora 推荐使用以 <tt>AgoraAudioReverbPresetFx</tt> 为前缀的枚举值。</li><li>Agora 推荐在使用 <tt>AgoraAudioReverbPresetVirtualStereo</tt> 前将 <tt>setAudioProfile</tt> 的 <tt>profile</tt> 参数设置为 <tt>AgoraAudioProfileMusicHighQualityStereo(5)</tt>。</li></div>

<table>
  <tr>
    <th>方法</th>
		<th>枚举值</th>
    <th>描述</th>
  </tr>
  <tr>
    <td rowspan="2"><tt>setLocalVoiceChanger</tt></td>
		<td><tt>AgoraAudioVoiceBeautySpacial</tt></td>
    <td>空旷。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioVoiceChangerEthereal</tt></td>
    <td>空灵。</td>
  </tr>
  <tr>
		<td rowspan="8"><tt>setLocalVoiceReverbPreset</tt></td>
    <td><tt>AgoraAudioReverbPresetFxVocalConcert</tt></td>
    <td>演唱会。</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetVocalConcert</tt></td>
    <td>演唱会（旧版）。</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetFxKTV</tt></td>
    <td>KTV。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetKTV</tt></td>
    <td>KTV（旧版）。</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetFxStudio</tt></td>
    <td>录音棚。</td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetStudio</tt></td>
    <td>录音棚（旧版）。 </td>
  </tr>
  <tr>
    <td><tt>AgoraAudioReverbPresetFxPhonograph</tt></td>
    <td>留声机。</td>
  </tr>
	<tr>
		<td><tt>AgoraAudioReverbPresetVirtualStereo</tt></td>
		<td>虚拟立体声。虚拟立体声是指将单声道的音轨渲染出立体声的效果，使频道内所有用户听到有空间感的人声效果。</td>
  </tr>
 </table>

```swift
// swift
// 预设人声效果为空旷。
agoraKit.setLocalVoiceChanger(.voiceBeautySpacial)
// 关闭人声效果。
agoraKit.setLocalVoiceChanger(.voiceChangerOff)
```

```objective-c
// objective-c
// 预设人声效果为空旷。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceBeautySpacial];
// 关闭人声效果。
[self.agoraKit setLocalVoiceChanger: AgoraAudioVoiceChangerOff];
```

### 自定义人声效果

如果预设效果无法满足你的需求，你还可以通过 `setLocalVoicePitch`、`setLocalVoiceEqualization` 和 `setLocalVoiceReverb` 自行调整音调、均衡和混响效果，获取想要的人声效果。

以下示例代码展示如何把原始人声变成绿巨人霍克的声音：

```swift
// swift
// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示不需要修改音调。
agoraKit.setLocalVoicePitch(1)

// 设置本地人声均衡波段的中心频率
// 第 1 个参数为频谱子带索引，取值范围 [0,9]，分别代表 10 个频带，对应的中心频率是 [31,62,125,250,500,1000,2000,4000,8000,16000] Hz
// 第 2 个参数为每个频率区间的增益值，取值范围 [-15,15]，单位 dB, 默认值为 0
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

// 原始人声强度，即所谓的 dry signal，取值范围 [-20,10]，单位为 dB
agoraKit.setLocalVoiceReverbOf(.dryLevel, withValue: -1)

// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20,10]，单位为 dB
agoraKit.setLocalVoiceReverbOf(.wetLevel, withValue: -7)

// 所需混响效果的房间尺寸，一般房间越大，混响效果越强。取值范围 [0,100]
agoraKit.setLocalVoiceReverbOf(.roomSize, withValue: 57)

// Wet signal 的初始延迟长度，取值范围 [0,200]，单位为 ms
agoraKit.setLocalVoiceReverbOf(.wetDelay, withValue: 135)

// 混响效果持续的强度，取值范围为 [0,100]，值越大，混响效果越强
agoraKit.setLocalVoiceReverbOf(.strength, withValue: 45)
```

```objective-c
// objective-c
// 设置音调。可以在 [0.5, 2.0] 范围内设置。取值越小，则音调越低。默认值为 1.0，表示不需要修改音调。
[agoraKit setLocalVoicePitch: 1];

// 设置本地人声均衡波段的中心频率
// 第 1 个参数为频谱子带索引，取值范围 [0,9]，分别代表 10 个频带，对应的中心频率是 [31,62,125,250,500,1000,2000,4000,8000,16000] Hz
// 第 2 个参数为每个频率区间的增益值，取值范围 [-15,15]，单位 dB, 默认值为 0
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

// 原始人声强度，即所谓的 dry signal，取值范围 [-20,10]，单位为 dB
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbDryLevel withValue: -1];

// 早期反射信号强度，即所谓的 wet signal，取值范围 [-20,10]，单位为 dB
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbWetLevel withValue: -7];

// 所需混响效果的房间尺寸，一般房间越大，混响效果越强。取值范围 [0,100]
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbRoomSize withValue: 57];

// Wet signal 的初始延迟长度，取值范围 [0,200]，单位为 ms
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbWetDelay withValue: 135];

// 混响效果持续的强度，取值范围为 [0,100]，值越大，混响效果越强
[agoraKit setLocalVoiceReverbOfType:AgoraAudioReverbStrength withValue: 45];
```

## API 参考

**预设的人声效果：**
- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)

**自定义人声效果：**
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setLocalVoiceEqualizationOfBandFrequency`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceEqualizationOfBandFrequency:withGain:)
- [`setLocalVoiceReverbOfType`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbOfType:withValue:)

## 开发注意事项

- 本文所有方法对人声的处理效果最佳，Agora 不推荐调用这类方法处理含音乐的音频数据。
- Agora 建议不要一起使用预设的人声效果和自定义人声效果相关方法，否则可能会发生未定义行为。如需一起使用，你需要在调用预设人声效果相关方法后调用自定义人声效果相关方法，否则自定义人声效果相关方法会不生效。
