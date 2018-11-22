
---
title: Record the Audio from the Client
description: 
platform: iOS,macOS
updatedAt: Thu Nov 22 2018 09:17:22 GMT+0000 (UTC)
---
# Record the Audio from the Client
## Introduction

Just like you can use the recording function of your cellphone to record a call and save it for future replay, you can record the audio of all participants in a call and save it to the local.

Agora's native SDK supports audio recording at the client. It allows recording the audio of all users in a channel and generating one recording file, the format of which can be: 

- wav: large file (lossless compression)
- aac: smaller file (lossy compression)

## Implementation

```swift
// Swift
// Start audio recording
// local path to the recording file. Specified by the user. Accurate to the file name and format.
// Audio quality of the recording. Is classified into LOW, MEDIUM, and HIGH.
agoraKit.startAudioRecording("recording file path", quality: .high)

// Stop audio recording
agoraKit.stopAudioRecording()
```

```oc
// Objective-C
// Start audio recording
// local path to the recording file. Specified by the user. Accurate to the file name and format.
// Audio quality of the recording. Is classified into LOW, MEDIUM, and HIGH.
[agoraKit startAudioRecording:@"recording file path", quality: AgoraAudioRecordingQualityHigh];

// Stop audio recording
[agoraKit stopAudioRecording];
```

## API Reference

- [startAudioRecording:quality:](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:quality:)
- [stopAudioRecording](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioRecording)

## Considerations

- Only after joining a channel can you start recording the audio.
- Client audio recording is automatically stopped once you leave the channel. 
