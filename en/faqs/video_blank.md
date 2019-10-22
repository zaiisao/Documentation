
---
title: Black screen occurs in a call.
description: 视频黑屏
platform: All Platforms
updatedAt: Tue Oct 22 2019 15:12:55 GMT+0800 (CST)
---
# Black screen occurs in a call.
Common reasons for black screens:
* Network failure: If the local network connection is poor or interrupted, the user cannot see other users. If any user in the call has network issues, none of the other users in the call can see this user.
* The user disabled the video. 

## Step 1: Self-check

Black screen scenarios:

### Black screen on the local side

This may be caused by a video capture failure on the local side. The camera does not work properly or is used by another application.

Check the following:
* Check the camera hardware. Start the built-in video camera to test the recording function.
* Check if the camera access permission is enabled. Both Android and iOS systems have a runtime access permission function under System Settings. Additionally, security software on Android may control camera access permissions.
* Check if another app is using the camera. Close all apps, restart your phone, and try again.
* If the app enabled the External Source Mode, check the data collected from the external video sources.

### Black screen on the remote side

This may be caused by a video capture failure on the remote side or slow downlink on the local side.

Check the following:

* Check if the user disabled the remote video.
* Switch to 4G or another Wi-Fi network to ensure that the problem is not caused by poor Internet connections.

### Black screen on the local and remote sides

This problem occurs when the video is not rendered correctly or the video function is not enabled.

Check the following:
* Check if the app calls the enableVideo method to enable the video.
* Check if the video is enabled on the local and remote sides.
* Check the rendering type in the SDK log in Windows. If the rendering type is D2D, ensure that you update to the latest graphics card driver. If the issue persists after updating the driver, switch to GDI rendering, which means the app calls the following function before the user joins the channel:
	* `AParameter apm(*pRTCEngine)`;
	* `nRet = apm->setInt("che.video.renderer.type", 9)`;
* If the app enables the Other Rendering Method Mode, check for any rendering issue.

## Step 2 Contact Agora Customer Support
If the issue persists, contact Agora customer support and submit the issue with the following information:
* The uid of the user whose screen goes black.
* The time frame during which the black screen appears.
* SDK logs and screen recording files of the user.

## Step 3: Monitor the Quality of Experience in Agora Analytics in Dashboard

You can check the statistics of every call in [Agora Analytics](https://dashboard.agora.io/analytics/call/search) in Dashboard. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).
