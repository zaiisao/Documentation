
---
title: 设置音频属性
description: How to set audio profile for iOS
platform: iOS
updatedAt: Fri Aug 16 2019 06:33:43 GMT+0800 (CST)
---
# 设置音频属性
## 功能描述
 在一些比较专业的场景里，用户对声音的效果尤为敏感，比如语音电台，此时就需要对双声道和高音质的支持。
 所谓的高音质指的是我们提供采样率为 48 Khz、码率 192 Kbps 的能力，帮助用户实现高逼真的音乐场景，这种能力在语音电台、唱歌比赛类直播场景中应用较多。
## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Audio%20Broadcast/ios_audio.md)。

Agora SDK 提供 [setAudioProfile](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) 方法给开发者根据场景需求灵活配置适合的音质属性。这个方法有 2 个参数：

- `profile` 代表不同的音频参数配置，比如采样率、码率和编码模式等。
- `scenario` 侧重于不同的使用场景，如娱乐、教学和游戏直播等。流畅度、噪声控制、音质等频道特性会根据不同的场景做出优化。

```swift
// swift
// 开黑聊天室
agoraKit.setAudioProfile(.speechStandard, scenario: .chatRoomGaming)
// 娱乐聊天室
agoraKit.setAudioProfile(.musicStandard, scenario: .chatRoomEntertainment)
// K 歌房
agoraKit.setAudioProfile(.musicHighQuality, scenario: .chatRoomEntertainment)
// FM 超高音质
agoraKit.setAudioProfile(.musicHighQuality, scenario: .gameStreaming)
```



```objective-c
// objective-c
// 开黑聊天室
[agoraKit setAudioProfile: AgoraAudioProfileSpeechStandard scenario: AgoraAudioScenarioChatRoomGaming];
// 娱乐聊天室
[agoraKit setAudioProfile: AgoraAudioProfilemusicStandard, scenario: AgoraAudioScenarioChatRoomEntertainment];
// K 歌房
[agoraKit setAudioProfile: AgoraAudioProfileMusicHighQuality, scenario: AgoraAudioScenarioChatRoomEntertainment];
// FM 超高音质
[agoraKit setAudioProfile: AgoraAudioProfilemusicHighQuality, scenario: AgoraAudioScenarioGameStreaming]
```

### API 参考

- [setAudioProfile](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) 

## 开发注意事项

- 该方法必须在加入频道之前调用
- `scenario` 参数仅在频道模式设为直播时有效
