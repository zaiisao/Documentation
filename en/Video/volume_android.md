
---
title: Adjust the Volume
description: How to adjust volume for Android
platform: Android
updatedAt: Mon Jul 06 2020 09:19:48 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction

The Agora RTC SDK enables you to manage the volume of the recorded audio or of the audio playback according to your actual scenario. For example, to mute a remote user in a one-to-one call, you can set the audio playback volume as 0.

This article provides the APIs and additional information relating to audio recording, audio mixing, audio playback and in-ear monitoring volume settings.

![](https://web-cdn.agora.io/docs-files/1578559042677)

## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Video/start_call_android.md) or [Start Live Interactive Streaming](../../en/Video/start_live_android.md).

### Adjust the recording volume

**Recording** is the process of sampling audio by a recording device and transmitting the recorded signal to the sender. To adjust the recording volume, you can **set the volume of the recorded signal**.

![](https://web-cdn.agora.io/docs-files/1578559122611)

Call `adjustRecordingSignalVolume` to set the volume of the recorded signal.
The `volume` parameter represents the audio level of the recorded signal, which ranges between 0 and 400:
- 0: Mute.
- 100: (Default) The original volume.
- 400: Four times the original volume (amplifying the audio signals by four times).

#### Sample code

```java
int volume = 200;
// Sets the volume of the recorded signal as 200% of the original volume.
rtcEngine.adjustRecordingSignalVolume(volume);
```

#### API reference

- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)

### Adjust the playback volume

**Playback** is the process of playing the received audio signal on the local playback device. To adjust the playback volume, you can **set the volume of the audio signal**.

![](https://web-cdn.agora.io/docs-files/1578559415146)

You can use `adjustPlaybackSignalVolume` or `adjustUserPlaybackSignalVolume` to set the volume of the audio signal.
- `adjustPlaybackSignalVolume`：
  - Universally sets the playback audio level of all remote users after audio mixing.
  - The `volume` parameter represents the playback audio level, which ranges between 0 and 400. 
- `adjustUserPlaybackSignalVolume`：
  - Adjusts the playback audio level of a specified remote user after audio mixing. Call this method as many times as necessary to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.
  - The `volume` parameter represents the playback audio level, which ranges between 0 and 100. 

<div class="alert note"><li>As of v2.3.2, to mute the local audio playback, you must call both adjustPlaybackSignalVolume and adjustAudioMixingVolume, and set the volume parameter as 0.<li>Call adjustUserPlaybackSignalVolume after joining a channel.</li></div>

#### Sample code

```java
int volume = 200;
// Sets the playback audio level of all remote users as 200% of the original volume..
rtcEngine.adjustPlaybackSignalVolume(volume);
// Sets the playback audio level of a specified remote user as 50% of the original volume..
rtcEngine.adjustUserPlaybackSignalVolume(uid, volume);
```

#### API reference

- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)
- [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aac9c5135996428d9a238fe8e66858268)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a)

### Adjust the audio mixing volume

**Audio mixing** is the process of combining local or online music files with the local stream so that all the remote users in the channel can hear the host and the music at the same time. See [Audio Effects/Mixing](../../en/Video/audio_effect_mixing_android.md) for more information about enabling this function.

![](https://web-cdn.agora.io/docs-files/1578559833929)

The `adjustAudioMixingVolume` method adjusts the volume of the music file for both the local user and the remote users.

The `volume` parameter represents the audio level of the music, ranging between 0 and 100.
- 0: Mute.
- 100: (Default) The original volume.

Sample code

```java
int volume = 50;
// Sets the audio mixing volume of the music for the local user and remote users as 50% of original volume..
rtcEngine.adjustAudioMixingVolume(volume);
```

You can also call `adjustAudioMixingPlayoutVolume` and `adjustAudioMixingPublishVolume` to set the audio mixing volume, respectively.
- `adjustAudioMixingPlayoutVolume`:
  - Sets the audio mixing volume of the music for the local users.
  - The `volume` parameter represents the audio mixing volume of the music for the local users, ranging between 0 and 100.
- `adjustAudioMixingPublishVolume`：
  - Sets the audio mixing volume of the music for the remote users.
  - The `volume` parameter represents the audio mixing volume of the music for the remote users, ranging between 0 and 100.

<div class="alert note">Call adjustAudioMixingPlayoutVolume and adjustAudioMixingPublishVolume after joining a channel.</div>

Sample code

```java
int volume = 50;
// Sets the audio mixing volume of the music for the remote users as 50% of the original volume..
rtcEngine.adjustAudioMixingPublishVolume(volume);
// Sets the audio mixing volume of the music for the local user as 50% of the original volume..
rtcEngine.adjustAudioMixingPlayoutVolume(volume);
```

#### API reference

- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a)

### Adjust the audio effects volume

**An audio effects** here refers to a sound clip which plays a brief sound effect such as clapping or gunshots. See [Audio Effects/Mixing](../../en/Video/audio_effect_mixing_android.md) for more information about enabling this function.

![](https://web-cdn.agora.io/docs-files/1578560140888)

You can call `setEffectsVolume` or `setVolumeOfEffect` to set the audio effects volume.
- `setEffectsVolume`：
  - Sets the volume of all audio effects.
  - The `volume` parameter represents the volume of the audio effects, ranging between 0 and 100.
- `setVolumeOfEffect`：
  - Sets the volume of a specified audio effect.
  - The `volume` parameter represents the volume of the audio effects, ranging between 0 and 100.

#### Sample code

```java
// Gets the global audio effect manager.
IAudioEffectManager manager = rtcEngine.getAudioEffectManager();
...
// Sets the audio effect volume as 50% of the the original volume.
double volume = 50.0
// Sets the volume of all audio effects.
manager.setEffectsVolume(volume);
// Sets the volume of a specified audio effect.
// soundId is ID of the audio effect when you call playEffect.
manager.setVolumeOfEffect(soundId, volume);
```

#### API reference

- [`setEffectsVolume`](https://docs.agora.io/en/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#ab758558563b3dd70771e5d44ba1a96f3)
- [`setVolumeOfEffect`](https://docs.agora.io/en/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#afcd8cd6d733703c0ba153b8e1ac81ec0)

### Adjust the in-ear monitoring volume

In audio recording, mixing and playing, you can call `setInEarMonitoringVolume` to adjust the volume of the in-ear monitoring.

![](https://web-cdn.agora.io/docs-files/1578560373700)

The `volume` parameter represents the volume of the in-ear monitoring, ranging between 0 and 100.
- 0: Mute.
- 100: (Default) The original volume.

#### Sample code

```java
// Enables in-ear monitoring.
rtcEngine.enableInEarMoniroting(true);
int volume = 50;
// Sets the in-ear monitoring volume as 50% of the original volume.
rtcEngine.setInEarMonitoringVolume(volume);
```

#### API reference

- [`setInEarMonitoringVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af71afdf140660b10c4fb0c40029c432d)

### Get the data of the loudest speaker (callback)

When recording, mixing, or playing audio, you can use the following methods to get the data of the loudest speaker in the channel.

- Reports users with the highest peak volumes. The `onAudioVolumeIndication` callback reports the user IDs the corresponding volumes of the currently loudest speakers in the channel.

	 <div class="alert note">You must call enableAudioVolumeIndication to be able to receive this callback.</div>

Sample code

```java
// Gets the the user IDs of the users with the highest peak volume, the corresponding volumes, as well as whether the local user is speaking.
// @param speakers is an array containing the user IDs and volumes of the local and the remote users. The volume parameter ranges between 0 and 255.
// @param totalVolume refers to the total volume after audio mixing, ranging between 0 and 255.
public void onAudioVolumeIndication(AudioVolumeInfo[] speakers, int totalVolume) {
}
```

- Reports the user with the highest average volume. The `onActiveSpeaker` callback reports the user ID with the highest average volume during a certain period of time.
	
	 <div class="alert note">You must call enableAudioVolumeIndication to be able to receive this callback.</div>
	 
Sample code

```java
// Gets the user ID of the user with the highest average volume during a certain period of time. A uid of 0 indicates the local user.
public void onActiveSpeaker(int uid) {
}
```

#### API reference
- [`onAudioVolumeIndication`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a4d37f2b4d569fa787bb8c0e3ae8cd424)
- [`onActiveSpeaker`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a895e965178d808f9d33b387ab3e50300)

## Considerations

Setting the audio level too high may cause audio distortion on some devices.

## Reference

When adjusting the audio volume, you can also refer to the following articles:

- [How to solve the problem of low volume?](https://docs.agora.io/en/faq/audio_low)
