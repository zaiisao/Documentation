
---
title: Recording-related Issues
description: 
platform: Recording-related Issues
updatedAt: Tue Apr 02 2019 09:16:31 GMT+0000 (UTC)
---
# Recording-related Issues
## Agora Recording SDK

### Why is there no recording file after starting the recording?

Check whether you enter the same `appID` and `channelProfile` (Communication or Live Broadcast) as the Agora Native SDK integrated into your app. Communication and Live Broadcast cannot interoperate.

### Why is there no audio when playing the recorded video?

The recorded audio and video files are independent; the audio is in AAC format, while the video is in MPEG-4 format. The video file does not contain any voice, you need to manually merge the audio and video files into one file. If the recorded files are already transcoded, contact Agora customer support.

### Why can’t I play the MPEG-4 file after the recording is complete?

This is usually because the player is not supported. Refer to the [Supported Players List](https://docs.agora.io/en/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#af8f3a6529f57ccfa3621014808d1283a).

### Why can’t I play the recorded video files after enabling encryption mode?

In encryption mode, if the encryption password is not entered or incorrect, the recorded file does not play. The audio output is corrupted because it is encrypted.

### Why is the recording still incorrect even when I configure the same password as the client?

Ensure that you set the encryption parameters the same as in the app. Ensure that the encryption mode is set as aes-128-xts or aes-256-xts in lowercase.

### How do I use a Channel Key?

See [Use Security Keys](../../en/recording/token.md).

### Why is there only one recording file with uid = 0 when audio mixing is enabled?

By default, when you enable audio mixing and multiple users join the channel, all voice record in one file, and uid = 0.

### How do I transcode the recorded file?

See [Record Voice and Video](../../en/recording/recording_voice_video.md). Contact Agora customer support for any issues.

### Can I configure the name and path of the recording file?

You can configure the root directory of the recording file, but not the name. See [Record Voice and Video](../../en/recording/recording_voice_video.md).

### How do I ensure that the number of channels recorded at the same time does not exceed the server capacity?

See [Set Up Your Environment](../../en/Recording/recording_env.md).


### Why can't the recording application log in while the client can, and why can the recording application send but not receive packets from the Agora servers?

Check the firewall configuration. See [Set Up Your Environment](../../en/Recording/recording_env.md). 

### Can I record the voice and video of a specified user?

This function is currently not supported.

### How can I tell whether or not the recording application left the channel?

The recording application left the channel if the SDK triggers the `onLeaveChannel` callback and returns the error code ERR_OK = 1.

The recording application failed to leave the channel If the SDK triggers the `onError` callback instead of the `onLeaveChannel` callback and returns the error code ERR_INTERNAL_FAILED = 3.

### What is the file format of the recorded files after transcoding?
See [Record Voice and Video](../../en/recording/recording_voice_video.md).

### Why is there a blank period at the beginning of the recorded and transcoded video during playback?

Possible reasons:

* Poor network conditions.
* The video recording is only created when the I frame of the video packet is received; which means that the previously received B and P frames are ignored.
* The size of each video frame is significantly larger than each voice frame. This means that the voice packet is transmitted quicker and received before the video packet. Once the video packet is received, the recording starts.

### Can I record rotated video?

The recording SDK only stores the original video passed from the client. When creating the MPEG-4 media file, the SDK adjusts the rotation once per rotational information found in the `uid_xxx.txt` file. In other words, no matter how many times the video rotates, the Recording SDK rotates it only once according to the first rotational information found in the `uid_xxx.txt` file. You can change the converting script according to the rotational information found in the `uid_xxx.txt` file to generate the rotated video. Agora plans to add a new feature that enables video rotation by a converting script in version 2.3.

## Recording files

### Why aren't there any files generated under the recording folder?

* Check if the recording process successfully joined the channel. Check if the App ID and channel name are valid. If the App Certificate is enabled, ensure that a valid Channel Key or Token is used when joining the channel. You can check the `appID`, channel, and `channelKey` parameters in the recording log, `recording_sys.log`.
* Check if there is any Native/Web user in the channel. At least one Native/Web SDK user is required in the channel for the recording client to generate a recording file. If there is a Native/Web user, ensure that the user sent a stream. If not, the recording file cannot be generated.

### Why is the recording duration of the recorded file incorrect?

If the Native/Web SDK and Recording SDK are in the same channel during the same time, contact Agora customer support.

### Why are there only audio files and no video files after recording?

This usually occurs when the Native/Web SDK and Recording SDK use different communication modes. Check the Argus SDK Client Role.

If the same communication mode is used, check the following parameters for the client sending the video and the receiver receiving the video: Video Receive Bitrate, Video Decoder In/Out Frame Rate, and ARS Received H264 Frame Num. If the parameters are fine, check the `recording_sys.log` file.

### Why is there a black screen when playing the recorded video, but the sound is normal?

This is usually because the player is not supported. Refer to the [Supported Players List](https://docs.agora.io/en/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#af8f3a6529f57ccfa3621014808d1283a).

### Why is the recorded video inverted?

Please upgrade to the latest official version. Contact Agora customer support for any issues.

### Why is the recorded file split?

This may be caused by:

* Different communication modes used between the Native/Web SDK and Recording SDK.
* Failure to enable `enableWebSdkInteroperability` in the Native SDK when the channel uses both the Native and Web SDK.

### What should I do if the audio and video are out of sync when I play the recording file? 

Please upgrade to the latest official version. Contact Agora customer support for any issues.

## Recording status

### Recording quit error

If Error: 3, with stat_code:16 is reported, the recording quits normally. You can determine why the recording quit by the leave_path code.

![](https://web-cdn.agora.io/docs-files/1542879822196)

* LEAVE_CODE_INIT(0)：Initialization failure.
* LEAVE_CODE_SIG(1<<1)：Signal triggered an exit.
* LEAVE_CODE_NO_USERS(1<<2)：No user is in the channel except for the recording application.
* LEAVE_CODE_TIMER_CATCH(1<<3)：Timer catch exit.
* LEAVE_CODE_CLIENT_LEAVE(1<<4)：The client leaves the channel.
 
> The code in the error log is a decimal number after a binary shift. For example, (1<<1) = 2; (1<<2) = 4. So "leave channel with code:12" is derived from (1<<2) = 4 + (1<<3) = 8.

Generally, the recording quits normally if the channel has no users. Check the `recording_sys.log` file for the "No users in channel" line to confirm.

### How do I know whether the recording crashed?

The recording crashes if the following scenarios occur:

* Video files cannot play.
* The `uid_xxx.txt` file has no close information about the MPEG-4 file.
* The recording_sys.log file contains the **MediaFile** keyword but not the **~MediaFile** keyword.
* The recorded event information in Argus has no quit flag.

### What should I do when the recording crashed?

Users of the Recording SDK v2.2.3+ can check the crash.log file generated in the same directory as AgoraCoreService.

Users of the Recording SDK versions earlier than v2.2.3 can check if there is a core file generated in the same directory as AgoraCoreService.

1. If you get the core file:
   a. Put the bin/AgoraCoreService and core file together, then execute the command line: gdb -c core_xxxx AgoraCoreService.
   b. Provide the core file, the result from step a, and the recording_sys.log file to Agora technical support.
2. If you cannot find the core file:
    a. If no directory is set for the core file, then the core file is usually found in the directory where the recorded AgoraCoreService file is located.
    b. If not, execute ulimit -c on Linux. If the output is 0, coredump is not open. Open coredump by executing: ulimit -c unlimited.
    c. After opening coredump, users can generate the core file after a crash.
    d. Provide the recording_sys.log file and the core file to Agora technical support.
		
### The recorded video files of the native client are in webm format when the Native SDK interoperates with the Web SDK.
To fix this issue, set the `codec` property as "h264" when calling the `createClient` method on the web client. If the `codec` property is "vp8", the recorded video files of the native client are in webm format.
