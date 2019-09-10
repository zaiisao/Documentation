
---
title: Web SDK-related Issues
description: 
platform: All Platforms
updatedAt: Thu Aug 29 2019 10:05:43 GMT+0800 (CST)
---
# Web SDK-related Issues
### In the H.264 mode, when I join a channel using the Firefox browser on macOS and enable the low-stream video, why do I get the high stream?
If you join a channel in the H.264 mode using the Firefox browser on macOS, you cannot set the low-stream parameter and the resolution is the same as that of the high stream. This does not occur in the VP8 mode.

### Why can't I see the remote video when I join a channel using the Firefox browser (v59.01) on macOS?
This issue is fixed by the Web SDK 2.1 Hotfix.

### When I use my cell phone to call two people using desktops, why does my cell phone freeze and become irresponsive even if I press the Home button?
During a video call made by a user with a cell phone to users on Safari and Google Chrome, the cell phone may freeze due to Google Chrome's H.264 video codec issue.

For more information, see:
http://bugs.webkit.org/show_bug.cgi?id=176439
http://bugs.webkit.org/show_bug.cgi?id=178357

### I pick up a QQ call during a web call. Why can't I send any audio when I resume the web call? 
A third-party application, such as QQ, takes over the audio device during a web call and your device may fail to send out voice after resuming your web call. In this case, Agora suggests starting a new call.

### I make a call with Google Chrome in Windows to a user using Google Chrome on macOS. When I switch my Wi-Fi, why does the image on macOS freeze?
This is a known issue and the macOS user needs to refresh the page. Agora is working on a fix for this issue.

### Why can't I join the channel and why is there a WebSocket error and DDoS-like attacks?
The `joinChannel` parameters of the Web SDK vary in different versions:
- The join attribute of ChannelKey is added in Web SDK v1.12+. 
- Web SDK versions earlier than v1.12 do not have this attribute.

### Why can't the receiver switch to the low-stream video in h264_interop mode if the sender is using a Firefox browser?
The low-stream or high-stream video setting is dependent on the browser, resolution, and codec type. Web browsers use an internal algorithm to set the stream. Therefore, the stream setting may not be according to your setting.

### During a live broadcast, why is there no video if the host does not set the transcoding stream in the AgoraRTC.createClient({mode:’interop’}) mode?
In the `AgoraRTC.createClient({mode:’interop’})` mode, if the host is the sole host in the channel, the host needs to transcode before publishing the stream. Otherwise, there will be no video. To push the stream to the CDN, enable the `AgoraRTC.createClient({mode:’h264_interop’})` mode.

Note: Agora charges an extra fee for transcoding.

### Using Safari, after a user plays third-party audio content during a live broadcast, why is the live broadcast audio abnormal?
This is an issue caused by recent Safari updates. For more information, see https://bugs.webkit.org/show_bug.cgi?id=179964.

### Using Firefox, why can't I join the channel after setting up the video profile?
This issue occurs on the following devices due to incompatibility issues between your computer and the browser:
* MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
* Windows 10 (MI)

If you encounter this issue, please contact Agora technical support.

### On iOS, why can't I call the getAudioLevel method to get the volume?
This is a limitation of the iOS Safari browser.

### Users join a live broadcast channel with the Agora Native SDK and Agora Web SDK (WebRTC) respectively. Users on PC or mobile applications see both the local and remote videos. Why can't users with web browsers see the remote videos?
You need to call the `enableWebSdkInteroperability` method in the Agora Native SDK to enable the interoperability functionality with WebRTC.

### Why aren't the charges calculated based on the video resolution that I set for the low-stream video?
Due to device and web browser limitations, the resolution you set may not take effect and may be set by the browser instead. In this case, the charges are calculated based on the actual resolution.

### When the Firefox browser interoperates with the Native SDK, the video in the Firefox browser is rotated in the communication channel. How can I fix that?
Due to the browser limitations, Agora recommends you use the Chrome browser to interoperate with the Native SDK in the communication channel.

### Why can't I subscribe to the remote streams on Firefox v59.0.2?
This is a known issue of the Firefox browser, please use a later version of Firefox.

### Error `Media access:SecurityError` when calling `stream.init`?
Please use HTTPS for your web app.

### Error `Media access: NotFoundError` when calling `stream.init`?
Cannot find the specified media track. Please ensure your media device is working.

### There is no audio on Safari if the user subscirbes to remote streams without publishing any stream?
If autoplay is disabled in the Safari settings, you need to call `navigator.mediaDevices.getUserMedia` before `Stream.play`, otherwise the audio is not played.

