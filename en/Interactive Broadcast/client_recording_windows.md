
---
title: Record the Audio from the Client
description: 
platform: Windows
updatedAt: Tue Dec 11 2018 01:45:25 GMT+0000 (UTC)
---
# Record the Audio from the Client
## Introduction

Just like you can use the recording function of your cellphone to record a call and save it for future replay, you can record the audio of all participants in a call and save it to the local.

Agora's native SDK supports audio recording at the client. It allows recording the audio of all users in a channel and generating one recording file, the format of which can be: 

- wav: large file (lossless compression)
- aac: smaller file (lossy compression)

## Implementation

````C++
// Initialize the RtcEngineParameters object.
RtcEngineParameters rep(*lpRtcEngine);

// Start local audio recording. 
#ifdef UNICODE
 CHAR aFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, aFilePath, MAX_PATH, NULL, NULL);
int nRet = rep.startAudioRecording(aFilePath, // Valid path to the local recording file.
	AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH // AUDIO_RECORDING_QUALITY_HIGH|MEDIUM|LOW
	);
#else
int nRet = rep.startAudioRecording(filePath, AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH);
#endif

// Stop audio recording. 
int nRet = rep.stopAudioRecording();

````

## API Methods

* [startAudioRecording](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#acb567614081900eaaf94d02b7c809af5)
* [stopAudioRecording](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#ac5f5a19d5f32d7f7d7d2765caafcdaec)

## Considerations

- Only after joining a channel can you start recording the audio.
- Client audio recording is automatically stopped once you leave the channel. 
