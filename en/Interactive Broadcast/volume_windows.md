
---
title: Adjust the Volume
description: How to adjust volume
platform: Windows
updatedAt: Wed Nov 13 2019 07:53:08 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, you can adjust the audio recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume as 0.

This article describes the scenarios when you need to adjust the volume, the corresponding APIs and considerations in the process from audio recording to playing. 

![](https://web-cdn.agora.io/docs-files/1548729217285)

## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Interactive%20Broadcast/start_call_windows.md) or [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_windows.md).

### Set the recording volume

**Recording** is the process in which audio signals are captured by recorders and transported to signal senders. You can change the recording volume by adjusting the **volume of the recording device** or the **volume of the recording signal**. On Windows, Agora recommends adjusting the recording volume with related APIs.

#### Adjust the volume of the recording device

The value of the volume ranges between 0 and 255. 0 represents the recording device is muted, and 255 is the maiximum volume.

Sample code

```cpp
// Sets the volume of the recording device.
int setRecordingDeviceVolume(int volume);
```

The value of `volume` corresponds to the **microphone array** on Windows, and 255 is the maximum value, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1545634908852)

#### Adjust the volume of the recording signal 

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the recording signals, which enables adjusting the recording volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

Sample code

```cpp
// Initializes the parameter engine.
RtcEngineParameters rep(*lpAgoraEngine);
  
// Sets the volume of the recording signal to 200.
int ret = rep.adjustRecordingSignalVolume(200);
```

#### API reference
- [`setRecordingDeviceVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac24424e86ded2727a532df739ebf8086)
- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acf94e9e0122f09d0450475d7c5809036)

### Set the playback volume

**Playback** is the process in which audio signal are transported from signal senders to receivers and then to the players. During this process, you can adjust the volume by changing the **volume of the playback device** or the **volume of the playback signal**. Agora recommends adjusting the playback volume with related APIs. 

#### Adjust the volume of the playback device
The value of the volume ranges between 0 and 255. 0 represents the recording device is muted, and 255 is the maiximum volume.

Sample code

```cpp
// Sets the volume of the playback device.
int setPlaybackDeviceVolume(int volume);
```

The value of `volume` corresponds to the **louderspearker/headset** on Windows, and 255 is the maximum value, as shown in the following screenshot.

![](https://web-cdn.agora.io/docs-files/1545635853077)

#### Adjust the volume of the playback signal

If the above methods do not meet your requirements, the Agora SDK provides methods to adjust the volume of the playback signals, which enables adjusting the playback volumes.
The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

**Note**: 
Since v2.3.2, the `adjustPlaybackSignalVolume` method adjusts only the playback volume of the voice. If you use the Naive SDK v2.3.2 or later, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods to mute the local audio playback.

Sample code

```cpp
// Initializes the parameter engine.
RtcEngineParameters rep(*lpAgoraEngine);

// Sets the volume of the playback signal to 200.
int ret = rep.adjustPlaybackSignalVolume(200);
```

#### API reference

- [`setPlaybackDeviceVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac14a1238e83303abed2f36e02fcc9366)
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a98919705c8b2346811f91f9ce5e97a79)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a544aee96b789ac5a57d26b61b7e1a5fa)

### Set the audio mixing volume

**Audio mixing** is playing local or online music while speaking, so that other users in the channel can hear the speaker and the music simultaneously. See [Audio Effects/Mixing](../../en/Interactive%20Broadcast/audio_effect_mixing_windows.md) for enabling this function.

The value of the audio mixing volume ranges between 0 and 100. 100 (default) represents the original volume, and 0 means the audio mixing is muted.

Sample code

```cpp
// Initializes the parameter engine.
RtcEngineParameters rep(*lpAgoraEngine);

// Sets the audio mixing volume for the remote users.
int ret = rep.adjustAudioMixingPublishVolume(50);
// Sets the audio mixing volume for both local users. 
int ret = rep.adjustAudioMixingPlayoutVolume(50);
```

You can also call the API `adjustAudioMixingVolume` to set the volume of audio playing for both remote users and local users.

Sample code

```cpp
// Initializes the parameter engine.
RtcEngineParameters rep(*lpAgoraEngine);

// Sets the audio mixing volume for both local and remote users.
int ret = rep.adjustAudioMixingVolume(50);
```

#### API reference

- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8677c3f3160927d25d9814a88ab06da6)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a544aee96b789ac5a57d26b61b7e1a5fa)

### Set the audio effects volume

 **Audio effects** are certain short-time sounds such as clapping and gunshots. See [Audio Effects/Mixing](../../en/Interactive%20Broadcast/audio_effect_mixing_windows.md) for enabling this function.

The value of the audio effects volume ranges between 0.0 and 100.0. 100 .0 (default) represents the original volume, and 0.0 means the audio effect is muted.

#### Sample code

```cpp
// Initializes the parameter engine.
RtcEngineParameters rep(*lpAgoraEngine);

// Sets the volume of all audio effect files.
int ret = rep.setEffectsVolume(50);
// Sets the volume of a single audio effect file.
int ret = rep.setVolumeOfEffect(soundId, 50);
```

#### API reference

- [`setEffectsVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#add9a7fd856700acd288d47ff3c7da19d)
- [`setVolumeOfEffect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a08287428f277b7bf24d51a86ef61799b)

### Get the data of the loudest speaker (callback method)

In audio recording, mixing and playing, you can use the following APIs to get the data of the loudest speaker in the channel.

- The speakers with the highest instant volume

Sample code

```cpp
void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume)  {
// Gets the ID of the speakers with the highest instant volume. A user ID of 0 indicates it is the local user.
// speakers is an array that contains uid and volumne of the speaker, volume ranging between 0 and 255.
// speakerNumber shows the size of the speakers array.
// totalVolume is the toal volume after audio mixing, ranging between 0 to 255.
}
```

- The speaker with the highest accumulative volume during a certain period

Sample code

```cpp
void onActiveSpeaker(uid_t uid) {
// Gets the ID of the speaker with the highest accumulative volume during a certain period. A user ID of 0 indicates it is the local user.
}
```

#### API reference

- [`onAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aab1184a2b276f509870c055a9ff8fac4)
- [`onActiveSpeaker`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae643c9dbf94360a23a8b3a56c93f90bc)

## Considerations

- The API methods have return values. If the method call fails, the return value is < 0.
- If the volume of the audio signal is set too high, noise may occur on some devices.

