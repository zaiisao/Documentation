
---
title: The audio volume of other users is too low. 
description: 
platform: All Platforms
updatedAt: Tue Oct 22 2019 15:09:03 GMT+0800 (CST)
---
# The audio volume of other users is too low. 
## Step 1: Self-check

Check the following:
* Turn up the system volume of the receiver.
* Check whether the issue is caused by the device. You can change your playback device or try other VoIP services on the same device.
* Check whether the user opens another app during the call, which may change the audio settings or routing.
* Call the following methods to adjust the volume: `enableAudioVolumeIndication`, `adjustRecordingSignalVolume`, `adjustPlaybackSignalVolume`, and `adjustAudioMixingVolume`.
* Check the `onAudioRouteChanged` callback to see whether the audio route is set to the headset or speaker. If the audio route is set to the headset, call the `setDefaultRouteToSpeakerphone` method and switch the audio route to the speaker.

## Step 2: Contact Agora Customer Support

If the issue persists, contact Agora customer support and submit the issue with the following information:
* The name of the channel where the users encounter this issue.
* The uids of the users whose volume is too low.
* The time frame during which the volume is too low.

## Step 3: Monitor the Quality of Experience in Agora Analytics in Console

You can check the statistics of every call in [Agora Analytics](https://dashboard.agora.io/analytics/call/search) in Console. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).
