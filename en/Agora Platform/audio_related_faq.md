
---
title: Audio-related Issues
description: 
platform: Audio-related Issues
updatedAt: Thu Nov 22 2018 07:57:52 GMT+0000 (UTC)
---
# Audio-related Issues
## Echo

An echo is usually caused by the remote user’s device, which in most cases can be fixed by using a headset.

### Step 1: Self Check

1. Ensure all users are in separated physical environments.
2. Check the SDK version:
    * Android/iOS: v1.6.0 or later
    * Windows/macOS: v1.7.0 or later
3. For Windows, ensure that the monitoring microphone checkbox is unchecked.
4. Use a headset.
    * In a one-to-one call, if you hear an echo, ask the other user to use a headset.
    * In a multi-user call, ask users to mute in turn to isolate who is causing the echo. Users who are causing the echo are recommended to use a headset or be on mute.

If the issue still exists, contact customer support.

### Step 2: Contact Customer Support

Report the issue to the customer support team. Submit Issue with the following information:

<table>
  <tr>
    <th>Information</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Mandatory</td>
    <td>The name of the channel where the echo occurred.<br>The uids of the users who heard the echo.<br>The uid of the user who caused the echo.<br>The recording files, if available.</td>
  </tr>
  <tr>
    <td>Additional</td>
    <td>The time when the users heard the echo.<br>Whether the issue remained after exiting and rejoining the channel.<br>Whether the issue remained after the users who caused the echo or who heard the echo switched the audio route. For example, plugged in a headphone.</td>
  </tr>
</table>

## Noise

### Step 1: Self Check

1. Ensure all users are in separated physical environments.
2. Try to locate which user the noise originated from.
3. Adjust or change the recording device. For example, check whether the headset is plugged in correctly, use another headset, or switch to another audio route.
4. Avoid communication in a noisy environment.

If the issue still exists, contact the customer support team.

### Step 2: Contact Customer Support

Report the issue to the customer support team. Submit Issue with the following information:

<table>
  <tr>
    <th>Priority</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Mandatory Information</td>
    <td><li>The name of the channel where the noise occurred.</li><li>The uid of the user who caused the noise.</li><li>The recording files, if available.</li></td>
  </tr>
  <tr>
    <td>Additional Information</td>
    <td><li>The time when the users heard the noise.<</li><li>Whether the issue remained after exiting and rejoining the channel.</li><li>Whether the device test result was normal on Apple or Windows.</li><li>Whether the noise always existed, or you heard it only when you spoke and there was no noise when the remote user spoke.</li></td>
  </tr>
</table>

## Low Volume

### Step 1: Self Check

1. Check whether the system volume of the receiver is turned up.
2. Change the headset to check whether it fixes this issue.
3. Check whether it is an issue caused by your device. Test the volume with another application on the same device.
4. Call the following APIs to adjust the volume on the required platform: `enableAudioVolumeIndication`, `adjustRecordingSignalVolume`, `adjustPlaybackSignalVolume`, and `adjustAudioMixingVolume`.
5. Check the callback `onAudioRouteChanged` to see whether the audio route is set to the earpiece or speaker. If it is set to the earpiece, call the API `setDefaultRouteToSpeakerphone` to modify the settings.

If the issue remains, contact customer support.

### Step 2: Contact Customer Support

Report the issue to the customer support team and Submit Issue with the following information:

* The name of the channel with users who encountered this issue.
* The uids of the users who found the volume to be too low.
* The time when the users found the volume to be too low.

## No Voice Communication

The local user cannot hear any voice communication from the remote users, or vice versa, or both parties cannot hear each other.

### Step 1: Self Check

1. Ensure you received the callback onJoinChannelSuccess, which means you have joined the channel successfully.

2. Check whether you put yourself or others on mute by mistake.

3. Check whether you called the API adjustRecordingSignalVolume or adjustPlaybackSignalVolume to mute the audio.

4. Check whether the recording device can record the audio content. You can call the API method startEchoTest to test it.

5. Confirm that the headset is working properly.

6. Check whether you have called the API setEnableSpeakphone. In speaker mode, there should be no output when plugging in the headset.

7. Check whether the headset is working properly if the audio output is set to the headset.

8. Test whether your device works properly by using the Agora Video Call demo which can be downloaded from Agora.io.

9. In Windows, if the users cannot hear any audio from the speaker or headset, do the following:
    
	a. Right click on the speaker icon and select Sounds.
    ![](https://web-cdn.agora.io/docs-files/1539335697243)
  
	b. Check whether the green bar of the speaker changes according to the volume. Otherwise, it means that the speaker does not work.
    ![](https://web-cdn.agora.io/docs-files/1539335709730)
		
	  If the green bar does not appear no matter which application you use, plug a headset in and out, or change the playback device, or mute and restart the playback device.
    If none of these methods work, check whether the recording device works according to the following step:
    If the green bar changes according to the volume, try whether you can hear any audio output using another application, if yes, then it should be your application’s issue.
   
	c. Go to the recording tab, and check whether the green bar will change according to the volume when you speak. Ensure that you select a proper device when multiple devices are available.
    ![](https://web-cdn.agora.io/docs-files/1539335720712)

If the issue remains, contact customer support.

### Step 2: Contact Customer Support

Report the issue to the customer support team and Submit Issue with the following information:

* The name of the channel with users who do not hear the noise.
* The uids of the users who cannot hear the voice communication in the channel.
* The time when users cannot hear any voice communication.

## Jitters

### Step 1: Self Check

Check whether the network is stable and in good condition. If not, switch to 4G or Wi-Fi.

If the issue remains, contact the customer support team.

### Step 2: Contact Customer Support

Report the issue to the customer support team. Submit Issue with the following information:

<table>
  <tr>
    <th>Information</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Mandatory</td>
    <td><li>The name of the channel in which users heard the jitters.</li><li>The uids of the users who heard the jitters.</li><li>The uid of the user who caused the jitters.</li><li>The recording files, if available.</li></td>
  </tr>
  <tr>
    <td>Additional</td>
    <td><li>In a live broadcast, whether the jitters came from the host.</li><li>If the channel was in video mode, whether the video was smooth and clear.</li></td>
  </tr>
</table>
