
---
title: Record the Audio from the Client
description: 
platform: iOS,macOS
updatedAt: Wed Feb 20 2019 06:49:05 GMT+0800 (CST)
---
# Record the Audio from the Client
## Introduction

You can record the audio of all the users in a call and save it on the client for future replays. 

The Agora Native SDK supports recording the audio of all the users in a channel and saves the recording into one file in the following formats: 

- WAV: Large file (lossless compression)
- AAC: Small file (lossy compression)

## Implementation

```swift
// Swift
// Start audio recording.
// Local path of the recording file specified by the user, including the filename and format.
// Audio quality of the recording: LOW, MEDIUM, and HIGH.
agoraKit.startAudioRecording("recording file path", quality: .high)

// Stop audio recording.
agoraKit.stopAudioRecording()
```

```oc
// Objective-C
// Start audio recording.
// Local path to the recording file specified by the user, including the filename and format.
// Audio quality of the recording: LOW, MEDIUM, and HIGH.
[agoraKit startAudioRecording:@"recording file path", quality: AgoraAudioRecordingQualityHigh];

// Stop audio recording.
[agoraKit stopAudioRecording];
```

### API Reference

- [`startAudioRecording:quality:`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:quality:)
- [`stopAudioRecording`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioRecording)

## Considerations

- Start the recording after joining a channel.
- The recording automatically stops if the local user leaves the channel. 
