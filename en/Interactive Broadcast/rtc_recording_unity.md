
---
title: Record the Audio on the Client
description: 
platform: Unity
updatedAt: Wed Mar 18 2020 10:02:18 GMT+0800 (CST)
---
# Record the Audio on the Client
## Introduction

You can record the audio of all the users in a call and save it on the client for future replays. 

The Agora Unity SDK supports recording the audio of all the users in a channel and saves the recording into one file in the following formats: 

- WAV: Large file (lossless compression)
- AAC: Small file (lossy compression)

## Implementation

Before proceeding, ensure that you implement a basic call or live interactive streaming in your project. See [Start a Video Call](../../en/Interactive%20Broadcast/start_call_audio_unity.md) or [Start Live Interactive Video Streaming](../../en/Interactive%20Broadcast/start_live_audio_unity.md) for details.

To start audio recording, call the `StartAudioRecording` method after joining a channel.

```c#
// Starts audio recording.
mRtcEngine.StartAudioRecording(
    // Local path of the recording file specified by the user, including the filename and format. For example: "/sdcard/emulated/0/recording.acc".
	"filePath",
    // Sample rate (Hz) of the recording file.
    32000,
	// Audio quality of the recording: LOW, MEDIUM, and HIGH.
	AUDIO_RECORDING_QUALITY_TYPE.AUDIO_RECORDING_QUALITY_HIGH 
);

// Stops audio recording.
mRtcEngine.StopAudioRecording();
```

### API reference

- [`StartAudioRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a077834840aee46f9fb8352e0a810bf1a)
- [`StopAudioRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a69e3ff25b224e257a5a37aa7532b7d35)

## Considerations

- Start the recording after joining a channel.
- The recording automatically stops if the local user leaves the channel. 

