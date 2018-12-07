
---
title: Share the Screen
description: 
platform: iOS
updatedAt: Fri Dec 07 2018 19:57:35 GMT+0000 (UTC)
---
# Share the Screen
## Introduction
During a video call or live broadcast, **sharing the screen** enhances communication by bringing whatever is on the speaker's screen to the other speakers or audience in the channel.

Screen share has extensive application in the following scenarios:

- For a video conference, the speaker can share the image of the local file, web page, and PPT with other users in the channel.
- For an online class, the teacher can share the image of the slides or notes with the students.

## Implementation

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md) for details.

Screen sharing on the iOS platform is implemented with the following steps:

- Create a process with Broadcast Upload Extension.
- Record the screen with Apple ReplayKit.
- Use the Agora SDK to transfer the video stream.

```
// swift
override func processSampleBuffer(_ sampleBuffer: CMSampleBuffer, with sampleBufferType: RPSampleBufferType) {
        DispatchQueue.main.async {
            switch sampleBufferType {
            case RPSampleBufferType.video:
                AgoraUploader.sendVideoBuffer(sampleBuffer)
                break
            case RPSampleBufferType.audioApp:
                AgoraUploader.sendAudioAppBuffer(sampleBuffer)
                break
            case RPSampleBufferType.audioMic:
                AgoraUploader.sendAudioMicBuffer(sampleBuffer)
                break
            }
        }
    }
```

```objective-c
// objective-c
- (void)processSampleBuffer:(CMSampleBufferRef)sampleBuffer withType:(RPSampleBufferType)sampleBufferType {
    dispatch_async(dispatch_get_main_queue(), ^{
    	switch (sampleBufferType) {
        case RPSampleBufferTypeVideo:
            [agoraUpload sendVideoBuffer:sampleBuffer];
             NSLog(@"RPSampleBufferTypeVideo App~~~~");
            break;
        case RPSampleBufferTypeAudioApp:
            [agoraUpload sendAudioAppBuffer:sampleBuffer];
            NSLog(@"RPSampleBufferTypeAudio App+++");
            break;
        case RPSampleBufferTypeAudioMic:
            [agoraUpload sendMicAppBuffer:sampleBuffer];
            break;
    	}
    })
}
```

You can also refer to the [Agora Screen Sharing](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-iOS) Sample Code to implement screen sharing on Android.
