
---
title: Record the Audio from the Client
description: 
platform: Android
updatedAt: Wed Feb 20 2019 06:48:34 GMT+0000 (UTC)
---
# Record the Audio from the Client
## Introduction

You can record the audio of all users in a call and save it on the client, just like using the recording function on your cell phone to record a call and save it for future replays. 

Agora's Native SDK supports audio recording at the client. You can record the audio of all users in a channel and generate one recording file with the following format: 

- wav: Large file (lossless compression)
- aac: Smaller file (lossy compression)

## Implementation


```Java
	// Start audio recording.
	rtcEngine.startAudioRecording(
		"path/to/file",              // Local path of the recording file
		                             //  specified by the user, 
								     // including the filename and format.
		AUDIO_RECORDING_QUALITY_HIGH // Audio quality of the recording: 
		                             // LOW, MEDIUM, and HIGH.
	);

	// Stop audio recording.
	rtcEngine.stopAudioRecording();
```

### API Reference

- [`startAudioRecording()`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a44744695d723b7d18c704a57f828cddb)
- [`stopAudioRecording()`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a2d751055a21611b3cf99fe39d24bb1a0)

## Considerations

- Only after joining a channel can you start recording the audio.
- Client audio recording is automatically stopped once you leave the channel. 
