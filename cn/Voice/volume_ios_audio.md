
---
title: 调整通话音量
description: How to adjust volume for iOS audio SDK
platform: iOS
updatedAt: Tue Aug 13 2019 07:19:16 GMT+0800 (CST)
---
# 调整通话音量
## 功能描述

 在使用我们 SDK 时，开发者可以对 SDK 采集到的声音及 SDK 播放到声卡的声音音量进行调整，以满足产品在声音上的个性化需求。比如进行双人通话时，想实现静音操作，可以通过调整播放音量的接口将音量设置为 0。



本文梳理了在使用 SDK 从音频采集到播放各阶段中，用户可能需要调整音量的场景、各场景对应的 API 及其使用注意事项。

![](https://web-cdn.agora.io/docs-files/1548729657947)

## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/ios_audio.md)。

### 设置采集音量

**采集**是指音频信号由录音设备采集后传输到发送端的过程。这个过程中你可以使用 SDK 的接口直接调整录制声音的信号幅度，以调整录音的音量。

调节音量的参数值范围是 0 - 400，默认值 100 表示原始音量，即不对信号做缩放，400 表示原始音量的 4 倍（把信号放大到原始信号的 4 倍）。

```swift
// swift
// 设置录音信号音量
agoraKit.adjustRecordingSignalVolume(50)
```

```objective-c
//objective-c
// 设置录音信号音量
[agoraKit adjustRecordingSignalVolume: 50];
```

#### API 参考

- [`adjustRecordingSignalVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)

### 设置播放音量

**播放**是指音频信号从发送端进入到接收端，然后使用播放设备进行播放的过程。这个过程中你可以使用 SDK 的接口直接调整播放声音的信号幅度，以调整播放的音量。

调节音量的参数值范围是 0 - 400，默认值 100 表示原始音量，即不对信号做缩放，400 表示原始音量的 4 倍（把信号放大到原始信号的 4 倍）。

```swift
// swift
// 设置播放信号音量
agoraKit.adjustPlaybackSignalVolume(50)
```

```objective-c
// objective-c
// 设置播放信号音量
[agoraKit adjustPlaybackSignalVolume: 50];
```

**Note**: 
从 v2.3.2 开始，[`adjustPlaybackSignalVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:) 接口仅支持调整人声的播放音量。如果你使用的是 v2.3.2 及之后版本的 Native SDK，静音本地音频请同时调用 `adjustPlaybackSignalVolume(0)` 和 `adjustAudioMixingPlayoutVolume(0)`。

#### API 参考

- [`adjustPlaybackSignalVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)

### 设置混音音量

混音是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。你可以参考[音乐混音](../../cn/Voice/effect_mixing_ios_audio.md)开启混音功能。

调节混音音量的参数值范围是 0 - 100，默认值 100 表示原始文件音量，即不对信号做缩放。0 表示混音文件播放静音。

```swift
// swift
// 设置远端用户听到的音乐文件音量
agoraKit.adjustAudioMixingPublishVolume(50)
// 设置本地用户听到的音乐文件音量
agoraKit.adjustAudioMixingPlayoutVolume(50)
```

```objective-c
// objective-c
// 设置远端用户听到的音乐文件音量
[agoraKit adjustAudioMixingPublishVolume: 50];
// 设置本地用户听到的音乐文件音量
[agoraKit adjustAudioMixingPlayoutVolume: 50];
```

你也可以直接调用 `adjustAudioMixingVolume`，同时设置本地及远端用户听到的音乐文件音量。

```swift
// swift
// 设置本地及远端用户听到的音乐文件音量
agoraKit.adjustAudioMixingVolume(50)
```

```objective-c
// objective-c
// 设置本地及远端用户听到的音乐文件音量
[agoraKit adjustAudioMixingVolume: 50];
```

#### API 参考

- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)

### 设置音效音量

播放音效是指播放短小的音频，如鼓掌、子弹撞击的声音等。你可以参考[播放音效](../../cn/Voice/effect_mixing_ios_audio.md)开启音效播放。

调节音效音量的参数值范围是 0.0 - 100.0，默认值 100.0 表示原始音效音量，即不对信号做缩放。0.0 表示音效文件播放静音。

```swift
// swift
// 设置所有音效文件的播放音量
agoraKit.setEffectsVolume(50.0)
// 设置单个音效文件的播放音量
// soundId 是你在调用 playEffect 时设置的音效 ID
agoraKit.setVolumeOfEffect(soundId:"1", 50.0)
```

```objective-c
// objective-c
// 设置所有音效文件的播放音量
[agoraKit setEffectsVolume: 50.0];
// 设置单个音效文件的播放音量
// soundId 是你在调用 playEffect 时设置的音效 ID
[agoraKit setVolumeOfEffect: soundId:@"1" volume:50.0];
```

#### API 参考

- [`setEffectsVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [`setVolumeOfEffect`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVolumeOfEffect:withVolume:)

### 设置耳返音量

在音频采集、混音、播放的整个过程中，你都可以使用 `setInEarMonitoringVolume` 调整耳返的音量。

调节耳返音量的参数值范围是 0 - 100，默认值 100 表示原始音效音量，即不对信号做缩放。0 表示耳返静音。

```swift
// swift
// 开启耳返监听功能
agoraKit.enableInearMonitoring(true)
// 设置耳返音量
agoraKit.setInEarMonitoringVolume(50)
```

```objective-c
// objective-c
// 开启耳返监听功能
[agoraKit enableInEarMonitoring(true)];
// 设置耳返音量
[agoraKit setInEarMonitoringVolume: 50];
```

#### API 参考

- [`enableInEarMonitoring`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableInEarMonitoring:)
- [`setInEarMonitoringVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setInEarMonitoringVolume:)

### 获取用户音量（回调方法）

在音频采集、混音、播放的整个过程中，你都可以使用下面的接口获取用户音量。

- 瞬时说话声音音量提示。如下回调获取瞬时说话音量最大的用户 ID，及音量大小。如果返回的用户 ID 为 0，则表示瞬时说话音量最大的是本地用户。

```swift
// swift
func rtcEngine(_ engine: AgoraRtcEngineKit, reportAudioVolumeIndicationOfSpeakers speakers:
[AgoraRtcAudioVolumeInfo], totalVolume: Int) {
// 获取瞬时说话音量最大的几个用户 ID
// speakers 为一个数组，包含说话者的用户 ID 及音量，音量范围为 0 - 255
// totalVolume 指混音后频道内的总音量，范围为 0 - 255
}
```

```objective-c
// objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine reportAudioVolumeIndicationOfSpeakers:(NSArray<AgoraRtcAudioVolumeInfo*> *_Nonnull)speakers totalVolume:(NSInteger)totalVolume {
// 获取瞬时说话音量最大的几个用户 ID
// speakers 为一个数组，包含说话者的用户 ID 及音量，音量范围为 0 - 255
// totalVolume 指混音后频道内的总音量，范围为 0 - 255
}
```

- 当前时间内累积音量最大者。如下回调获取获取特定时间段内，累积音量最大的用户 ID。如果返回的 uid 是 0，则表示当前时间段内累积音量最大的是本地用户。

```swift
// swift
func rtcEngine(_ engine: AgoraRtcEngineKit, activeSpeaker speakerUid: UInt) {
// 获取当前时间段声音最大的用户 ID（仅 1 个）
}
```

```objective-c
// objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine activeSpeaker:(NSUInteger)speakerUid {
// 获取当前时间段声音最大的用户 ID（仅 1 个）
}
```

#### API 参考
- [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:)
- [`activeSpeaker`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:activeSpeaker:)


## 开发注意事项

- 所有相关的方法都有返回值。返回值小于 0 表示方法调用失败
- 如果信号音量设置太大，由于硬件设备的限制，在某些设备上可能会出现声音不自然的效果。
