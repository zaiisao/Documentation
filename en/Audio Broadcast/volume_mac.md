
---
title: Adjust the Volume
description: How to adjust volume on macOS
platform: macOS
updatedAt: Fri Sep 20 2019 04:22:48 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, you can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume as 0.

This article describes the scenarios when you need to adjust the volume, the corresponding APIs and considerations in the process from audio recording to playing. 

![](https://web-cdn.agora.io/docs-files/1548729191542)

## Implementation
Before proceeding, ensure that you prepare the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/mac_video.md).

### Set the Recording Volume

**Recording** is the process in which audio signals are captured by recorders and transported to signal senders. You can change the recording volume by adjusting the **volume of the recording device** or the **volume of the audio signal**. On macOS, Agora recommends adjusting the recording volume with related APIs.

#### Adjust the Volume of the Recording Device

The value of the volume ranges between 0 and 255. 0 represents the recording device is muted, and 255 is the maiximum volume.

```swift
// swift
// Sets the recording volume.
agoraKit.setDeviceVolume(.audioRecording, volume: 50)
```

```objective-c
// objective-c
// Sets the recording volume.
[agoraKit setDeviceVolume: AgoraMediaDeviceTypeAudioRecording volume: 50];
```

The value of `volume` corresponds to the **Input Volume** on macOS, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1545632794842)

#### Adjust the Volume of the Recording Signal 

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the recording signals, which enables adjusting the recording volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the recording signals by four times).

```swift
// swift
// Sets the volume of the recording signal.
agoraKit.adjustRecordingSignalVolume(50)
```

```objective-c
// objective-c
// Sets the volume of the recording signal to 50.
[agoraKit adjustRecordingSignalVolume: 50];
```

#### API Reference
- [`setDeviceVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setDeviceVolume:volume:)
- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)

### Set the Playback Volume

**Playback** is the process in which audio signal are transported from signal senders to receivers and then to the players. During this process, you can adjust the volume by changing the **volume of the playback device** or the **volume of the playback signal**. Agora recommends adjusting the playback volume with related APIs. 

#### Adjust the Volume of the Playback Device
The value of the volume ranges between 0 and 255. 0 represents the recording device is muted, and 255 is the maiximum volume.

```swift
// swift
// Sets the playback device volume.
agoraKit.setDeviceVolume(.audioPlayout, volume: 50)
```

```objective-c
// objective-c
// Sets the playback device volume.
[agoraKit setDeviceVolume: AgoraMediaDeviceTypeAudioPlayout volume: 50];
```

The value of `volume` corresponds to the **Output Volume** on macOS, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1545633859976)

#### Adjust the Volume of the Playback Signal

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the playback signals, which enables adjusting the playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```swift
// swift
// Sets the volume of the playback signal.
agoraKit.adjustRecordingSignalVolume(50)
```

```objective-c
// objective-c
// Sets the volume of the playback signal.
[agoraKit adjustRecordingSignalVolume: 50];
```

**Note**: 
Since v2.3.2, the [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:) method adjusts only the playback volume of the voice. If you use the Naive SDK v2.3.2 or later, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods to mute the local audio playback.

#### API Reference

- [`setDeviceVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setDeviceVolume:volume:)
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)

### Set the Audio Mixing Volume

**Audio mixing** is playing local or online music while speaking, so that other users in the channel can hear the speaker and the music simultaneously. See [Audio Mixing](../../en/Audio%20Broadcast/effect_mixing_mac.md) for enabling this function.

The value of the audio mixing volume ranges between 0 and 100. 100 (default) represents the original volume, and 0 means the audio mixing is muted.

```swift
// swift
// Sets the audio mixing volume for remote users.
agoraKit.adjustAudioMixingPublishVolume(50)
// Sets the audio mixing volume for local users.
agoraKit.adjustAudioMixingPlayoutVolume(50)
```

```objective-c
// objective-c
// Sets the audio mixing volume for remote users.
[agoraKit adjustAudioMixingPublishVolume: 50]；
// Sets the audio mixing volume for local users.
[agoraKit adjustAudioMixingPlayoutVolume: 50];
```

You can also call the API `adjustAudioMixingVolume` to set the volume of audio playing for both remote users and local users.

```swift
// swift
// Sets the audio mixing volume for both local and remote users.
agoraKit.adjustAudioMixingVolume(50)
```

```objective-c
// objective-c
// Sets the audio mixing volume for both local and remote users.
[agoraKit adjustAudioMixingVolume: 50];
```

#### API Reference

- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)

### Set the Audio Effects Volume

 **Audio effects** are certain short-time sounds such as clapping and gunshots. See [Audio Effects](../../en/Audio%20Broadcast/effect_mixing_mac.md) for enabling this function.

The value of the audio effects volume ranges between 0.0 and 100.0. 100 .0 (default) represents the original volume, and 0.0 means the audio effect is muted.

```swift
// swift
// Sets the volume of all audio effect files.
agoraKit.setEffectsVolume(50.0)
// Sets the volume of a single audio effect file.
// soundId is ID of the audio effect when you call playEffect.
agoraKit.setVolumeOfEffect(soundId:"1", 50.0)
```

```objective-c
// objective-c
// Sets the volume of all audio effect files.
[agoraKit setEffectsVolume: 50.0];
// Sets the volume of a single audio effect file.
// soundId is ID of the audio effect when you call playEffect.
[agoraKit setVolumeOfEffect: soundId:@"1" volume:50.0];
```

#### API Reference

- [`setEffectsVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [`setVolumeOfEffect`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVolumeOfEffect:withVolume:)

### Get the Data of the Loudest Speaker (Callback Method)

In audio recording, mixing and playing, you can use the following APIs to get the data of the loudest speaker in the channel.

- The speakers with the highest instant volume

```swift
// swift
// Gets the ID of the speakers with the highest instant volume. A user ID of 0 indicates it is the local user.
// speakers is an array that contains uid and volumne of the speaker, volume ranging between 0 and 255.
// totalVolume is the toal volume after audio mixing, ranging between 0 to 255.
func rtcEngine(_ engine: AgoraRtcEngineKit, reportAudioVolumeIndicationOfSpeakers speakers:
[AgoraRtcAudioVolumeInfo], totalVolume: Int) {
}
```

```objective-c
// objective-c
// Gets the ID of the speakers with the highest instant volume. A user ID of 0 indicates it is the local user.
// speakers is an array that contains uid and volumne of the speaker, volume ranging between 0 and 255.
// totalVolume is the toal volume after audio mixing, ranging between 0 to 255.
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine reportAudioVolumeIndicationOfSpeakers:(NSArray<AgoraRtcAudioVolumeInfo*> *_Nonnull)speakers totalVolume:(NSInteger)totalVolume {
}
```

- The speaker with the highest accumulative volume during a certain period

```swift
// swift
// Gets the ID of the speaker with the highest accumulative volume during a certain period. A user ID of 0 indicates it is the local user.
func rtcEngine(_ engine: AgoraRtcEngineKit, activeSpeaker speakerUid: UInt) {
}
```

```objective-c
// objective-c
// Gets the ID of the speaker with the highest accumulative volume during a certain period. A user ID of 0 indicates it is the local user.
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine activeSpeaker:(NSUInteger)speakerUid {
}
```

#### API Reference

- [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:4)
- [`activeSpeaker`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:activeSpeaker:)

## Considerations

- The API methods have return values. If the method call fails, the return value is < 0.
- If the volume of the audio signal is set too high, noise may occur on some devices.

