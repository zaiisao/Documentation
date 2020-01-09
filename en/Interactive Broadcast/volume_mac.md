
---
title: Adjust the Volume
description: How to adjust volume on macOS
platform: macOS
updatedAt: Fri Jan 03 2020 04:27:09 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction

The Agora RTC SDK enables you to manage the volume of the recorded audio or of the audio playback according to your actual scenario. For example, to mute a remote user in a one-to-one call, you can set the audio playback volume as 0.

This article describes the scenarios when you need to adjust the volume, the corresponding APIs and considerations in the process from audio recording to playing. 

![](https://web-cdn.agora.io/docs-files/1548729191542)

## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Interactive%20Broadcast/start_call_mac.md) or [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_mac.md).

### Set the recording volume

**Recording** is the process in which audio signals are captured by recorders and transported to signal senders. You can change the recording volume by adjusting the **volume of the recording device** or the **volume of the audio signal**. On macOS, Agora recommends adjusting the recording volume with related APIs.

#### Adjust the volume of the recording device

The value of the volume ranges between 0 and 255. 0 represents the recording device is muted, and 255 is the maiximum volume.

Sample code

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

#### Adjust the volume of the recording signal 

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the recording signals, which enables adjusting the recording volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the recording signals by four times).

Sample code

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

#### API reference
- [`setDeviceVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setDeviceVolume:volume:)
- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)

### Set the playback volume

**Playback** is the process in which audio signal are transported from signal senders to receivers and then to the players. During this process, you can adjust the volume by changing the **volume of the playback device** or the **volume of the playback signal**. Agora recommends adjusting the playback volume with related APIs. 

#### Adjust the volume of the playback device
The value of the volume ranges between 0 and 255. 0 represents the recording device is muted, and 255 is the maiximum volume.

Sample code

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

#### Adjust the volume of the playback signal

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the playback signals, which enables adjusting the playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

**Note**: 
Since v2.3.2, the [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:) method adjusts only the playback volume of the voice. If you use the Naive SDK v2.3.2 or later, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods to mute the local audio playback.

Sample code

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

#### API reference

- [`setDeviceVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setDeviceVolume:volume:)
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)

### Set the audio mixing volume

**Audio mixing** is playing local or online music while speaking, so that other users in the channel can hear the speaker and the music simultaneously. See [Audio Effects/Mixing](../../en/Interactive%20Broadcast/effect_mixing_mac.md) for enabling this function.

The value of the audio mixing volume ranges between 0 and 100. 100 (default) represents the original volume, and 0 means the audio mixing is muted.

Sample code

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

Sample code

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

#### API reference

- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)

### Set the audio effects volume

 **Audio effects** are certain short-time sounds such as clapping and gunshots. See [Audio Effects/Mixing](../../en/Interactive%20Broadcast/effect_mixing_mac.md) for enabling this function.

The value of the audio effects volume ranges between 0.0 and 100.0. 100 .0 (default) represents the original volume, and 0.0 means the audio effect is muted.

#### Sample code

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

#### API reference

- [`setEffectsVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [`setVolumeOfEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVolumeOfEffect:withVolume:)

### Get the data of the loudest speaker (callback method)

In audio recording, mixing and playing, you can use the following APIs to get the data of the loudest speaker in the channel.

- The speakers with the highest instant volume

Sample code

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

Sample code

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

#### API reference

- [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:4)
- [`activeSpeaker`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:activeSpeaker:)

## Considerations

- The API methods have return values. If the method call fails, the return value is < 0.
- If the volume of the audio signal is set too high, noise may occur on some devices.

