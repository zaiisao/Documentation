
---
title: I cannot hear any voice in a call.
description: 
platform: All Platforms
updatedAt: Tue Oct 22 2019 15:10:16 GMT+0800 (CST)
---
# I cannot hear any voice in a call.
The issue of no voice may occur in the following scenarios:

* The local user cannot hear any voice from the remote user.
* The remote user cannot hear any voice from the local user.
* The local and remote users cannot hear each other.

## Step 1: Self-check

Check the following:

* Ensure that the app grants access to audio devices.
* Ensure that the app receives the `onJoinChannelSuccess` callback, which means users join the channel successfully.
* Check if you muted yourself or others.
* Check if you called the `adjustRecordingSignalVolume` or `adjustPlaybackSignalVolume` method to mute the audio.
* Check if the recording device is working. You can call the `startEchoTest` method to test it.
* Check if the headset is working.
* Call the `setEnableSpeakphone` method and ensure that you cannot hear any voice from the headset when the speaker mode is on.
* Check if the headset is working and if the audio output is set to the headset.
* Test your device with the Agora Video Call demo (downloaded from [Agora.io](https://docs.agora.io/en/Voice/downloads)).
* In Windows, if users cannot hear any voice from the speaker or headset:
	* Right-click on the speaker icon and select Sounds.
    ![](https://web-cdn.agora.io/docs-files/1543547283115)
  
	* On the Playback tab, check whether the green bar of the speaker changes according to the volume. If not, the speaker is not working.
    ![](https://web-cdn.agora.io/docs-files/1539335709730)
		If the green bar changes according to the volume, check if you can hear any audio output with another app. If yes, there is something wrong with your app. If the green bar does not appear regardless of the app, unplug and plug the headset, change the playback device, or stop and restart the playback device.
   
	* If none of these methods work, check the recording device by clicking on the Recording tab and checking whether the green bar changes according to your voice volume. Ensure that you select the correct device when multiple devices are available.
    ![](https://web-cdn.agora.io/docs-files/1539335720712)

## Step 2: Contact Agora Customer Support

If the issue persists, contact Agora customer support and submit the issue with the following information:

* The name of the channel where the users cannot hear the noise.
* The uids of the users who cannot hear any voice in the channel.
* The time frame during which the users cannot hear any voice.

## Step 3: Monitor the Quality of Experience in Agora Analytics in Console

You can check the statistics of every call in [Agora Analytics](https://dashboard.agora.io/analytics/call/search) in Console. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).
