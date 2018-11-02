
---
title: Recording-related Issues
description: 
platform: Recording-related Issues
updatedAt: Thu Nov 01 2018 09:22:30 GMT+0000 (UTC)
---
# Recording-related Issues
# Recording-related Issues

## Recording Solutions

### Should I use the Agora Recording Server, Agora Recording SDK, or CDN Recording?

Agora Recording SDK is the recommended solution for all users.

## Migration from the Agora Recording Server to the Agora Recording SDK

### Do I need to configure the ARS IP in the backend dashboard?

No.

### Do I need a recording Key to record?

No.

### Is the recording trigger controlled by the server?

Yes.

### Do I need to call the API startRecordingServer to trigger the recording?

No.

### How can the server know when to join a channel and what the channel details are since the recording is triggered by the server?

The server decides when to join a channel to start recording or leave a channel to stop recording through your own signaling. When the 
Agora Recording SDK joins a channel, the API behavior is equivalent to any other Agora Native SDK joining a channel at the client side.

## Agora Recording SDK

### Why is there no recording file after starting the recording?

Check whether the App ID and channelProfile (Communication or Live Broadcast) entered is the same as the ones configured for the Agora Native SDK integrated in your app. Communication and live broadcast cannot interoperate.

### Why is there no audio when playing the recorded video?

The recorded files are divided into video.mp4 and audio.aac. The video file does not contain any audio, and you need manually merge the video and audio into one file.

### Why can’t I play the MP4 file after the recording is complete?

Before Agora SDK v1.1, the real-time video mixing function is not supported, and you have to use the transcoding tools in the Agora Recording SDK to transcode first before you can play the MP4 file. After Agora SDK v1.1, you can play the MP4 file if you have called setVideoMixingLayout to mix the video in realtime. Otherwise, you need to use the transcoding tools in the Agora Recording SDK to transcode first before you can play the MP4 file.

### Why can’t I play the recorded video files after enabling encryption mode?

In encryption mode, if the password is not entered or incorrect, the recorded file will not play. The audio output is corrupted because it is encrypted.

### Why is the recording still incorrect even when I configured the same password as the client side?

Ensure the encryption parameter is set as the same as the app, in aes-128-xts or aes-256-xts, in lowercase.

###  Why is there no recording file even though I set the same App ID at the client and server side?

When you call joinChannel() and set the parameter channelProfile, ensure that the settings on the client and the recording server are the same. For example, if the client is in Live Broadcast mode while the recording server is in Communications mode, there will not be any recording file created after joining the channel.

### How do I use a Channel Key?

See [Security Keys](../../en/Agora%20Platform/token.md).

### Why is there only one recording file with uid = 0 when audio mixing is enabled?

By default, when audio mixing is enabled and multiple users have joined the channel, it will record all voice as one file, and uid = 0.

### Why is the display blurred or green when I drag the playback progress of the recording file?

By default, you need to transcode the recorded video files. Otherwise, if you drag the progress bar, a blurred or green screen occurs.

### Is there any callback function to indicate a recording failure? For example, the server is offline, and the recording process gets stuck.

Yes, a callback function to indicate a recording failure will be available in Agora SDK v1.1 and later.

### How can I inform the recording app of leaving a channel?

The callback function to inform the recording app of leaving a channel will be available from v1.1.

Before v1.1, the Linux server informs the recording app of leaving a channel to stop recording through your own signaling. The 

Recording SDK calls the API leaveChannel, and a recording2_done.txt file is created after leaving the channel.

### How do I inform the recording app that the user is offline?

When there is no user in the channel, recording stops automatically after the duration set in idleLimitSec expires when calling the API `joinChannel`.

A recording2_done.txt files is created after the recording is stopped.

### How do I transcode the recorded file?

Refer to [Recording Voice and Video](../../en/Advance%20Guide/recording_voice_video.md).

Contact customer support for any issues.

### Can I configure the path and name of the recording file?

Yes, you can configure the root directory of the recording file, but not the name. See [Recording Voice and Video](../../en/Advance%20Guide/recording_voice_video.md).

### How do I ensure that the number of the channels being recorded simultaneously does not exceed the server capacity?

See [Recording Voice and Video](../../en/Advance%20Guide/recording_voice_video.md).

### How do I estimate the size of the recording file?

The default configuration of each video recording file is 400 kbps. The file size is related to the recording content and environment.

### When will you support other programming languages besides C++?

Currently, we only support C++ and command line execution for recording.

### Why is it invalid when I type “–appId = f4637604af81440596a54254d53ade20 –uid = 222 –channel = ttt” in the command line?

```
--appId f4637604af81440596a54254d53ade20
–uid 222 –channel ttt
```

### Why does the recording app fail to login while the client can, and why can the recording app send but not receive packets from the Agora servers?

The recording app randomly selects a port to communicate with the Agora server. If the environment of the recording app is limited to the port, it may fail to connect.

If you need to specify a port, you can write a configuration file, agorasdk.json, in the same directory as the executable with the following content: agorasdk.json { “rtc.udp_port_range”: [ 40000, 40004 ] }

You can change the port number with a range of: high - low > = 4. This method only supports one channel of recording. If you support multiple recording channels, it is recommended that the server environment not limit the UDP port.

### When will the recording be stopped when I use the command line to record?

The recording will be stopped under one of the following situations when you use the command line to record:

1. Press Ctrl + C.
2. When you set the idle value to configure the channel idle duration, and If no user in the channel succeeds the predefined duration, the channel will be disconnected and the recording will be stopped automatically. The default value is 300 seconds.

### Can I record the voice and video of a specified user?

This function is currently not supported.

### How can I tell whether it is a normal or abnormal exit from the channel?

It is a normal exit if the SDK triggers the onLeaveChannel callback and returns the error code ERR_INTERNAL_FAILED = 3. It is an abnormal exit If the SDK triggers the onError callback instead of the onLeaveChannel callback and returns the error code ERR_INTERNAL_FAILED = 3.

### What is the file format after transcoding the recorded files?

See [Recording Voice and Video](../../en/Advance%20Guide/recording_voice_video.md).

### Why doesn’t the recording app start when I use the command line and only enter the parameters marked with a ‘must’?

Be sure that you only point to the video_recorder folder instead of the file when you set the appliteDir parameter.

### Why is there a blank period at the beginning of the recorded and transcoded video during playback?

The following are possible reasons:

* Poor network conditions.
* The video recording is only created when the I frame of the video packet is received; which means that the previously received B and P frames will be ignored.
* The size of each video frame is significantly larger than each voice frame. This means that the voice packet is transmitted quicker and received before the video packet. Once the video packet is received, it will start the recording.

### Can I record the rorated video?

The recording SDK only stores the original video passed from the Client’s side. When creating the mp4 media file, the SDK adjusts the rotation once per the rotation information noted in the uid_xxx.txt file. In other words, no matter how many times the video rotates, the Recording SDK rotates it once only according to the first rotation information in the uid_xxx.txt file. You can change the converting script according to the rotation information in the uid_xxx.txt file to generate the rotated video. Agora plans to add a new feature which enables video rotation by the converting script in version 2.3.



