
---
title: Record the Audio on the Client
description: 
platform: Windows
updatedAt: Wed Nov 13 2019 08:34:17 GMT+0800 (CST)
---
# Record the Audio on the Client
## Introduction

You can record the audio of all the users in a call and save it on the client for future replays. 

The Agora Native SDK supports recording the audio of all the users in a channel and saves the recording into one file in the following formats: 

- WAV: Large file (lossless compression)
- AAC: Small file (lossy compression)

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Audio%20Broadcast/start_call_windows.md) or [Start a Live Broadcast](../../en/Audio%20Broadcast/start_live_windows.md) for details.

```C++
// Initialize the RtcEngineParameters object.
RtcEngineParameters rep(*lpRtcEngine);

// Start local audio recording. 
#ifdef UNICODE
 CHAR aFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, aFilePath, MAX_PATH, NULL, NULL);
int nRet = rep.startAudioRecording(aFilePath, // Valid path to the local recording file.
	AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH // AUDIO_RECORDING_QUALITY_HIGH|MEDIUM|LOW
	);
#else
int nRet = rep.startAudioRecording(filePath, AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH);
#endif

// Stop audio recording. 
int nRet = rep.stopAudioRecording();
```

### API reference

* [`startAudioRecording`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a3c05d82c97a9d63ebda116b9a1e5ca3f)
* [`stopAudioRecording`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adb392026425663e5b9f90fe90130e5a5)

## Considerations

- Start the recording after joining a channel.
- The recording automatically stops if the local user leaves the channel. 
