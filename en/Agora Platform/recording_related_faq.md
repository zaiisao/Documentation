
---
title: Recording-related Issues
description: 
platform: Recording-related Issues
updatedAt: Fri Nov 23 2018 06:39:22 GMT+0000 (UTC)
---
# Recording-related Issues
## Agora Recording SDK

### Why is there no recording file after starting the recording?

Check whether the `appID` and `channelProfile` (Communication or Live Broadcast) entered is the same as the `appID` and `channelProfile` configured for the Agora Native SDK integrated into your app. Communication and Live Broadcast cannot interoperate.

### Why is there no audio when playing the recorded video?

The recorded audio and video files are independent; the audio is in AAC format, while the video is in MPEG-4 format. The video file does not contain any voice, you need to manually merge the audio and video files into one file.

### Why can’t I play the MPEG-4 file after the recording is complete?

Before Agora SDK v1.1, the real-time video mixing function is not supported. You have to use the transcoding tools in the Agora Recording SDK to transcode first before you can play the MPEG-4 file.

After Agora SDK v1.1, you can play the MPEG-4 file if you have called the setVideoMixingLayout method to mix the video in real time. Otherwise, you need to use the transcoding tools in the Agora Recording SDK to transcode first before you can play the MPEG-4 file.

### Why can’t I play the recorded video files after enabling encryption mode?

In encryption mode, if the encryption password is not entered or incorrect, the recorded file will not play. The audio output is corrupted because it is encrypted.

### Why is the recording still incorrect even when I configured the same password as the client?

Ensure that the encryption parameters are set as the same as in the app. Ensure that the encryption mode is set as aes-128-xts or aes-256-xts in lowercase.

### Why is there no recording file even though I set the same App ID at the client and server?

When you call the `joinChannel` method and set the `channelProfile` parameter, ensure that the settings on the client and the recording server are the same. For example, if the client is in Live Broadcast mode while the recording server is in Communication mode, there will no recording file created after joining the channel.

### How do I use a Channel Key?

See [Use Security Keys](../../en/recording/token.md).

### Why is there only one recording file with uid = 0 when audio mixing is enabled?

By default, when audio mixing is enabled and multiple users have joined the channel, all voice will record in one file, and uid = 0.

### Why is the display blurred or green when I drag the playback progress bar of the recording file?

By default, you need to transcode the recorded video files. Otherwise, if you drag the progress bar, a blurred or green screen occurs.

### Is there any callback that indicates a recording failure? For example, the server is offline and the recording process is stuck.

A callback indicating a recording failure is available in Agora SDK v1.1+.

### How can I inform the recording app to leave a channel?

The `onLeaveChannel` callback informs the recording app to leave a channel and is available from v1.1.

Before v1.1, the Linux server informs the recording app to leave a channel and stop recording through your own signaling. The Recording SDK calls the `leaveChannel` method, and a `recording2_done.txt` file is created after the recording app leaves the channel.

### How can I inform the recording app that the user is offline?

When there is no user in the channel, recording stops automatically after the time set in `idleLimitSec expires` when calling the `joinChannel` method.

A `recording2_done.txt` file is created after the recording stops.

### How do I transcode the recorded file?

See [Record Voice and Video](../../en/recording/recording_voice_video.md). Contact Agora customer support for any issues.

### Can I configure the name and path of the recording file?

You can configure the root directory of the recording file, but not the name. See [Record Voice and Video](../../en/recording/recording_voice_video.md).

### How do I ensure that the number of channels being recorded simultaneously does not exceed the server capacity?

See [Record Voice and Video](../../en/recording/recording_voice_video.md).

### How do I estimate the size of the recording file?

The size of the recording file = bitrate x length of the recording.

The default bitrate of each recording video file is 400 Kbps. The file size is also related to the recording content and the environment. 

### When will you support other programming languages besides C++?

Currently, we only support C++ and command line execution for recording.

### Why is it invalid when I type “–appId = f4637604af81440596a54254d53ade20 –uid = 222 –channel = ttt” in the command line?

The equal sign (=) is not supported. The correct command should be:
--appId f4637604af81440596a54254d53ade20 --uid 222 --channel ttt

### Why can't the recording app login while the client can, and why can the recording app send but not receive packets from the Agora servers?

The recording app randomly selects a port to communicate with the Agora server. If the recording app runs in an environment with restrictions on the port, it may fail to connect.

If you need to specify a port, you can write a configuration file, agorasdk.json, in the same directory as the executable with the following content:
`agorasdk.json { “rtc.udp_port_range”: [ 40000, 40004 ] }`

You can change the port number with a range of: high - low > = 4. This method only supports one channel of recording. If you support multiple recording channels, the server environment must not have restrictions on the UDP port.

### When will the recording stop when I use the command line to record?

The recording stops in one of the following scenarios when you use the command line to record:
* Press **Ctrl + C**.
* When you set the idle value to configure the channel idle duration, and If no user in the channel succeeds the predefined duration, the channel will be disconnected and the recording will be stopped automatically. The default value is 300 seconds.

### Can I record the voice and video of a specified user?

This function is currently not supported.

### How can I tell whether or not the recording application left the channel?

The recording application left the channel if the SDK triggers the `onLeaveChannel` callback and returns the error code ERR_OK = 1.

The recording application failed to leave the channel If the SDK triggers the `onError` callback instead of the `onLeaveChannel` callback and returns the error code ERR_INTERNAL_FAILED = 3.

### What is the file format of the recorded files after transcoding?
See [Record Voice and Video](../../en/recording/recording_voice_video.md).

### Why doesn’t the recording app start when I use the command line with the "must" parameters?

Ensure that you specify the video_recorder folder instead of the file when you set the appliteDir parameter.

### Why is there a blank period at the beginning of the recorded and transcoded video during playback?

Possible reasons:

* Poor network conditions.
* The video recording is only created when the I frame of the video packet is received; which means that the previously received B and P frames will be ignored.
* The size of each video frame is significantly larger than each voice frame. This means that the voice packet is transmitted quicker and received before the video packet. Once the video packet is received, the recording starts.

### Can I record rotated video?

The recording SDK only stores the original video passed from the client. When creating the MPEG-4 media file, the SDK adjusts the rotation once per rotational information found in the `uid_xxx.txt` file. In other words, no matter how many times the video rotates, the Recording SDK rotates it only once according to the first rotational information found in the `uid_xxx.txt` file. You can change the converting script according to the rotational information found in the `uid_xxx.txt` file to generate the rotated video. Agora plans to add a new feature that enables video rotation by a converting script in version 2.3.

## Recording files

### Why aren't there any files generated under the recording folder?

* Check if the recording process has successfully joined the channel. Check if the App ID and channel name are valid. If the App Certificate is enabled, make sure that a valid Channel Key or Token is used when joining the channel. You can check the `appID`, channel, and `channelKey` parameters in the recording log, `recording_sys.log`.
* Check if there is any Native/Web user in the channel. At least one Native/Web SDK user is required in the channel for the recording client to generate a recording file. If there is a Native/Web user, ensure that the user has sent a stream. If not, the recording file cannot be generated.

### Why is the recording duration of the recorded file incorrect?

* Check whether the Argus duration is the same as the recorded file's recording duration. If not, ask the user to provide the `recording_sys.log` file under the recording folder.
* Compare the recording durations of the audio and video of the same user. If the durations are not the same, ask the user to provide the `recording.sys.log` file under the recording folder, the corresponding audio and video recording file, and the `uid_xxx.txt` file.

### Why are there only audio files and no video files after recording?

This usually occurs when the Native/Web SDK and Recording SDK use different communication modes. Check the Argus SDK Client Role.

If the same communication mode is used, check the following parameters for the client sending the video and the receiver receiving the video: Video Receive Bitrate, Video Decoder In/Out Frame Rate, and ARS Received H264 Frame Num. If the parameters are fine, check the `recording_sys.log` file.

### Why is there a black screen when playing the recorded video, but the sound is normal?

This is usually because the player is not supported. Refer to the following player support list.

![](https://web-cdn.agora.io/docs-files/1542879634290)

### Why is the recorded video inverted?

This is usually due to the recording user keep switching rotations (switching 180 degrees in landscape mode or switching front and rear cameras). The Native client automatically rotates to display the correct orientation.

In the Recording SDK versions earlier than v2.0, the uid_xxx.txt file does not contain any real-time rotational information, only the first rotational information.

In the Recording SDK v2.0 to v2.2, the `uid_xxx.txt` file contains real-time rotational information and users can process the video based on this information. 

In the Recording SDK v2.3+, recording supports automatic rotation.

### Why is the recorded file split?

This may be caused by:

* Different communication modes used between the Native/Web SDK and Recording SDK.
* Failure to enable `enableWebSdkInteroperability` in the Native SDK when the channel uses both the Native and Web SDK.

### What should I do if the audio and video are out of sync when I play the recording file? 

* The Recording SDK v2.1.1+ fixed the issue of fast recorded video playback with normal recorded audio playback.
* The Recording SDK v2.3 added the AVsyncMode method to fix the issues of occasional out of sync audio and video and channel not synchronized within 15 seconds.
* If the audio and video are often out of sync, check the recording server's CPU usage and VOQA data. You may need to adjust the number of concurrent recordings on the network or server.

## Recording status

### Recording quit error

If Error: 3, with stat_code:16 is reported, the recording quits normally. You can determine why the recording quit by the leave_path code.

![](https://web-cdn.agora.io/docs-files/1542879822196)

* LEAVE_CODE_INIT(0)：Initialization failure.
* LEAVE_CODE_SIG(1<<1)：Signal triggered exit.
* LEAVE_CODE_NO_USERS(1<<2)：No user is in the channel except for the recording application.
* LEAVE_CODE_TIMER_CATCH(1<<3)：Timer catch exit.
* LEAVE_CODE_CLIENT_LEAVE(1<<4)：The client leaves the channel.
 
> * The code in the error log is a decimal number after a binary shift. For example, (1<<1) = 2; (1<<2) = 4. So "leave channel with code:12" is derived from (1<<2) = 4 + (1<<3) = 8.
> * Generally, the recording quits normally if the channel has no users. Check the `recording_sys.log` file for the "No users in channel" line to confirm.

### How do I know whether the recording crashed?

* Video files cannot play.
* The `uid_xxx.txt` file has no close information about the MPEG-4 file.
* The recording_sys.log file contains the **MediaFile** keyword but not the **~MediaFile** keyword.
* The recorded event information in Argus has no quit flag.

If several of the above scenarios occur at the same time, the recording crashed.

### What should I do when the recording crashed?

Users of the Recording SDK v2.2.3+ can check the crash.log file generated in the same directory as AgoraCoreService.

Users of the Recording SDK versions earlier than v2.2.3 can check if there is a core file generated in the same directory as AgoraCoreService.

1. If you get the core file, do the following:
   a. Put the bin/AgoraCoreService and core file together, then execute the command line: gdb -c core_xxxx AgoraCoreService.
   b. Provide the core file, the result from step a, and the recording_sys.log file to Agora technical support.
2. If you cannot find the core file:
    a. If no directory is set for the core file, then the core file is usually found in the directory where the recorded AgoraCoreService file is located.
    b. If not, execute ulimit -c on Linux. If the output is 0, coredump is not open. Open coredump by executing: ulimit -c unlimited.
    c. Run the create_core.sh script to open coredump.
    d. After opening coredump, users can generate the core file after a crash.
    e. Collect the recording_sys.log file and the core file.
