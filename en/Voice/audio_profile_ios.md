
---
title: Set the Stereo/High-quality Audio Profile
description: How to set the high-quality audio for iOS and macOS
platform: iOS,macOS
updatedAt: Fri Nov 23 2018 08:42:57 GMT+0000 (UTC)
---
# Set the Stereo/High-quality Audio Profile
## Feature Description 

In some professional scenarios, the audio quality is essential for the user experience. For example, podcast applications require stereo and high-quality audio. The so-called high-quality audio refers to the audio profile of 48 KHz sampling rate and 192 Kbps bitrate, which suits the high-fidelity musical scenarios, such as online radio and singing competitions.

## Implementation
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Voice/ios_video.md) for more information.

Agora SDK provides the [setAudioProfile](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) method for developers to set appropriate audio profiles according to the scenarios. This method has two parameters:

- `profile` sets the sampling rate, bitrate, encode mode, and the number of channels.
- `scenario` sets the audio application scenario, for example entertainment, education, and live gaming. The SDK optimizes the audio fluency, noise control, and the audio quality based on the scenarios.

```swift
// swift
// Gaming
agoraKit.setAudioProfile(.speechStandard, scenario: .chatRoomGaming)
// Entertainment
agoraKit.setAudioProfile(.musicStandard, scenario: .chatRoomEntertainment)
// KTV
agoraKit.setAudioProfile(.musicHighQuality, scenario: .chatRoomEntertainment)
// FM high-qaulity
agoraKit.setAudioProfile(.musicHighQuality, scenario: .gameStreaming)
```

```objective-c
// objective-c
// Gaming
[agoraKit setAudioProfile: AgoraAudioProfileSpeechStandard scenario: AgoraAudioScenarioChatRoomGaming];
// Entertainment
[agoraKit setAudioProfile: AgoraAudioProfilemusicStandard, scenario: AgoraAudioScenarioChatRoomEntertainment];
// KTV
[agoraKit setAudioProfile: AgoraAudioProfileMusicHighQuality, scenario: AgoraAudioScenarioChatRoomEntertainment];
// FM high-quality
[agoraKit setAudioProfile: AgoraAudioProfilemusicHighQuality, scenario: AgoraAudioScenarioGameStreaming]
```

### API Reference
- [setAudioProfile](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:)

## Considerations

- Call this method before joining the channel.
- The `scenario`  parameter takes effect only when the channel profile is live broadcast.
