
---
title: Adjust the Volume
description: How to adjust volume
platform: Windows
updatedAt: Fri Oct 23 2020 10:39:27 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction

The Agora RTC SDK enables you to manage the volume of the recorded audio or of the audio playback according to your actual scenario. For example, to mute a remote user in a one-to-one call, you can set the audio playback volume as 0.

This article provides the APIs and additional information relating to audio recording, audio mixing, and audio playback volume settings.

![](https://web-cdn.agora.io/docs-files/1578902005463)

## Sample project
We provide an open-source demo project that implements [adjusting audio volume](https://github.com/AgoraIO/API-Examples/tree/dev/3.2.0/windows/APIExample/APIExample/Advanced/AudioVolume) on GitHub. You can try the demo and view the source code.

## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_windows.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_windows.md).

### Adjust the recording volume

**Recording** is the process of sampling audio by a recording device and transmitting the recorded signal to the sender. To adjust the recording volume, you can **set the volume of the recording device** or **set the volume of the recorded signal**. We recommend using audio device-related APIs to adjust the recording volume.
![](https://web-cdn.agora.io/docs-files/1578902023400)

#### Set the volume of the recording device

Call `setRecordingDeviceVolume` to set the volume of your recording device. 

<div class="alert note">This method sets the global volume of your recording device.</div>

The `volume` parameter represents the audio level of the recording device, ranging between 0 and 255:
- 0: Mute.
- 255: The maximum volume of the device.

As the following screenshot shows, the volume value corresponds to the audio level of your audio recording device.
![](https://web-cdn.agora.io/docs-files/1577201229190)

Sample code

```cpp
// Sets the volume of your recording device as 50.
int setRecordingDeviceVolume(50);
```

#### Set the volume of the recorded signal 

If `setRecordingDeviceVolume` does not suffice, you can call `adjustRecordingSignalVolume` to set the volume of the recorded signal.

The `volume` parameter represents the audio level of the recorded signal, ranging between 0 and 400:
- 0: Mute.
- 100: (Default) The original volume.
- 400: Four times the original volume (amplifying the audio signals by four times).

Sample code

```cpp
// Sets the volume of the recorded signal as 200.
int ret = rtcEngine.adjustRecordingSignalVolume(200);
```

#### API reference
- [`setRecordingDeviceVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac24424e86ded2727a532df739ebf8086)
- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acf94e9e0122f09d0450475d7c5809036)

### Adjust the playback volume

**Playback** is the process of playing the received audio signal on the local playback device. To adjust the playback volume, you can **set the volume of the playback device**, or **set the volume of the audio signal**.
![](https://web-cdn.agora.io/docs-files/1578902038036)

#### Set the volume of the playback device

To set the volume on the playback device directly, call `setPlaybackDeviceVolume`. 

<div class="alert note">This method sets the global volume of your playback device.</div>

The `volume` parameter represents the audio level of the playback device, ranging between 0 and 255:
- 0: Mute.
- 255: The maximum volume of the device.

As the following screenshot shows, the volume value corresponds to the audio level of your audio playback device.
![](https://web-cdn.agora.io/docs-files/1577201545034)

Sample code

```cpp
// Sets the volume of the playback device as 50.
int setPlaybackDeviceVolume(50);
```

#### Set the volume of the playback signal

If `setPlaybackDeviceVolume` does not suffice, you can use `adjustPlaybackSignalVolume` or `adjustUserPlaybackSignalVolume` to set the volume of the audio signal.
- `adjustPlaybackSignalVolume`：
  - Universally sets the playback audio level of all remote users after audio mixing.
  - The `volume` parameter represents the playback audio level, ranging between 0 and 400. 
- `adjustUserPlaybackSignalVolume`：
  - Adjusts the playback audio level of a specified remote user after audio mixing. Call this method as many times as necessary to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.
  - The `volume` parameter represents the playback audio level, ranging between 0 and 100. 

<div class="alert note"><li>As of v2.3.2, to mute the local audio playback, you must call both adjustPlaybackSignalVolume and adjustAudioMixingPlayoutVolume, and set the volume parameter as 0.<li>Call adjustUserPlaybackSignalVolume after joining a channel.</li></div>

Sample code

```cpp
// Sets the volume of the local playback of all remote users as 200.
int ret = rtcEngine.adjustPlaybackSignalVolume(200);
// Sets the volume of the local playback of a specified remote user as 50.
int ret = rtcEngine.adjustUserPlaybackSignalVolume(uid, 50);
```

#### API reference

- [`setPlaybackDeviceVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac14a1238e83303abed2f36e02fcc9366)
- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a98919705c8b2346811f91f9ce5e97a79)
- [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a609e74c9a6e8df205543326f2ca6a965)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8677c3f3160927d25d9814a88ab06da6)


### Get the data of the loudest speaker (callback)

When recording, mixing, or playing audio, you can use the following methods to get the data of the loudest speaker in the channel.

- Reports users with the highest peak volumes. The `onAudioVolumeIndication` callback reports the user IDs the corresponding volumes of the currently loudest speakers in the channel.

 <div class="alert note">You must call enableAudioVolumeIndication to be able to receive this callback.</div>
 
 Sample code

 ```cpp
// Gets the user IDs of the users with the highest peak volume, the corresponding volumes, as well as whether the local user is speaking.
// @param speakers is an array containing the user IDs and volumes of the local and the remote users. The volume parameter ranges between 0 and 255.
// @param speakerNumber refers to the number of the speakers, ranging between 0 and 3.
// @param totalVolume refers to the total volume after audio mixing, ranging between 0 and 255.
void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume)  {
}
 ```

- Reports the user with the highest average volume. The onActiveSpeaker callback reports the user ID with the highest average volume during a certain period of time.

 <div class="alert note">You must call enableAudioVolumeIndication to be able to receive this callback.</div>

 Sample code

 ```cpp
 // Gets the user ID of the user with the highest average volume during a certain period of time. A uid of 0 indicates the local user.
void onActiveSpeaker(uid_t uid) {
}
 ```

#### API reference

- [`onAudioVolumeIndication`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aab1184a2b276f509870c055a9ff8fac4)
- [`onActiveSpeaker`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ae643c9dbf94360a23a8b3a56c93f90bc)

## Considerations

Setting the audio level too high may cause audio distortion on some devices.

## Reference

When adjusting the audio volume, you can also refer to the following articles:

- [How to solve the problem of low volume?](https://docs.agora.io/en/faq/audio_low)
