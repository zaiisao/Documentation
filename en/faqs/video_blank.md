
---
title: How can I solve black screen issues?
description: 视频黑屏
platform: All Platforms
updatedAt: Wed Mar 25 2020 22:44:41 GMT+0800 (CST)
---
# How can I solve black screen issues?
## Issue description

A user may encounter black screen issue in the following scenarios:
- Black screen on the local side
- Black screen on the remote side
- Black screen on the local and remote sides

## Reason
Common reasons for black screens are as follows:
* Network failure: If the local network connection is poor or interrupted, the user cannot see other users. If any user in the call has network issues, none of the other users in the call can see this user.
* The user disabled the video. 

## Solution

### Step 1: Self-check

**Black screen on the local side**

This may be caused by a video capture failure on the local side. The camera does not work properly or is used by another application.

1. Check the camera hardware. Start the built-in video camera to test the recording function.
2. Check if the camera access permission is enabled. Both Android and iOS systems have a runtime access permission function under System Settings. Additionally, security software on Android may control camera access permissions.
3. Check if another app is using the camera. Close all apps, restart your phone, and try again.
4. If the app enabled the External Source Mode, check the data collected from the external video sources.

**Black screen on the remote side**

This may be caused by a video capture failure on the remote side or slow downlink network on the local side.

1. Check if the user disabled the remote video.
2. Switch to 4G or another Wi-Fi network to ensure that the problem is not caused by poor Internet connections.
3. Check whether the remote user uses the [channel encryption](https://docs.agora.io/en/Video/channel_encryption_windows?platform=Windows) function but the local user doesn't.

**Black screen on the local and remote sides**

This problem occurs when the video is not rendered correctly or the video function is not enabled.

1. Check if the app calls the `enableVideo` method to enable the video.
2. Check if the video is enabled on both local and remote sides.
3. Check the rendering type in the SDK log in Windows. If the rendering type is D2D, ensure that you update to the latest graphics card driver. If the issue persists after updating the driver, switch to GDI rendering, which means the app calls the following function before the user joins the channel:
	- 	`AParameter apm(*pRTCEngine)`;
	- 	`nRet = apm->setInt("che.video.renderer.type", 9)`;
4. If the app enables the Other Rendering Method Mode, check for any rendering issue.

### Step 2 Contact Agora Customer Support
If the issue persists, contact Agora customer support and [submit the issue](https://agora-ticket.agora.io/) with the following information:
* The uid of the user whose screen goes black.
* The time frame during which the black screen appears.
* SDK logs and screen recording files of the user.

### Step 3: Monitor the Quality of Experience in Agora Analytics in Console

You can check the statistics of every call in [Agora Analytics](https://dashboard.agora.io/analytics/call/search) in Console. For more information, see [Agora Analytics Tutorial](https://docs.agora.io/en/Agora%20Platform/aa_guide?platform=All%20Platforms).
