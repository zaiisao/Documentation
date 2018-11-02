
---
title: Web SDK-related Issues
description: 
platform: Web SDK-related Issues
updatedAt: Fri Nov 02 2018 04:17:55 GMT+0000 (UTC)
---
# Web SDK-related Issues
### Why can’t I set the low stream in H.264 mode when joining a session with the Firefox browser on a macOS device?

On a macOS device, if you join a session in H.264 mode with the Firefox browser, you may not be able to set the low stream parameter, and the resolution is the same as that of the high stream. This does not occur in VP8 mode. Agora is investigating this issue.

### Why can’t I see the remote video when joining a session with the Firefox browser (v59.01) on a macOS device?

On a macOS device, when you join a session with the Firefox browser (v59.01), you can only see the local video but not the remote video. This problem is fixed by the Web SDK 2.1 hot fix.

### Why aren’t the billings calculated based on the video resolution I set for the low-video stream?

Due to limitations of some devices and browsers, the resolution you set may fail to take effect and get adjusted by the browser. In this case, billings will be calculated based on the actual resolution.

### Why does the cellphone freeze when joining two desktops, and is irresponsive when pressing the Home button?

In a video session participated by Safari, Google Chrome, and a cellphone, the cellphone may freeze. This is caused by the H.264 video codec from Google Chrome. For more information, see:
http://bugs.webkit.org/show_bug.cgi?id=176439
http://bugs.webkit.org/show_bug.cgi?id=178357

### Why can’t I connect with the Safari browser when using Google Chrome for Android?

Safari uses the H.264 video codec for video streaming and all users in the Safari session must support this codec. Google Chrome for Android prevents the device from streaming H.264 video to other users, causing a black screen on Android devices. This issue will be fixed in a later version of Google Chrome. For more information, see：
https://bugs.chromium.org/p/chromium/issues/detail?id=761336

You may encounter another issue on Google Chrome for Android where you will receive only voice, but no video. The H.264 codec support was added to Google Chrome for Android in version 57. Only Qualcomm (KitKat and later) and Samsung Exynos (Lollipop and later) chipsets support the H.264 video codec. For more information, see:
https://groups.google.com/forum/#!msg/discuss-webrtc/xXjeKbW_JYI/LIXzVrKWCwAJ

### Why can’t I send any voice communication when I resume a web call after a QQ call?

If a third-party application, such as QQ, takes over the audio device during a web call, your device may fail to send out any voice communication once you return to your web session. Agora recommends that you start a new session.

### Why does the image on a macOS device freeze when two participants are using Google Chrome browsers on a macOS device and a Windows device, and the Windows device switches to another Wi-Fi network?

In a call session joined by Google Chrome on a macOS and a Windows device, if the Windows device switches Wi-Fi networks, the Mac screen may freeze until you refresh the page. Agora is working on a fix for this issue.

### Why does the iOS window turn black and the remote user does not receive any video from Safari in iOS if I enable dualStream during a voice/video communication or live broadcast?

Safari in iOS does not support dual-stream mode.

### Why can’t I join the channel and there is a WebSocket error and DDoS-like attacks when the joinChannelparameters have changed?

In Web SDK versions later than v1.12, the join() attribute of ChannelKey was added: https://docs.agora.io/en/2.3.1/product/Video/API%20Reference/web_API_video?platform=Web

The Web SDK versions before v1.12 have no such attribute: https://docs.agora.io/cn/1.8/user_guide/API/webrtc_api.html

### Why can’t the receiver switch to the low stream in h264_interop mode if the Firefox browser is the publisher?

The setting of stream is affected by the browser, resolution, and codec type. Generally, browsers have an internal algorithm to adjust the stream, therefore, chances are that the stream is not in absolute accordance with your settings.

### During a live broadcast, why is there no video if the host does not set the transcoding stream inAgoraRTC.createClient({mode:’interop’}) mode?

In `AgoraRTC.createClient({mode:’interop’})` mode, the host (if he/she is the only host in the channel) needs to transcode before publishing the stream, or there will be no video.

To push the stream directly, you can switch to `AgoraRTC.createClient({mode:’h264_interop’})` mode.

Possible impact: Transcoding charges an extra fee.

### Why is the audio is affected if the Safari browser plays third-party audio content during a live broadcast and switches back to the live broadcast afterwards?

During a live broadcast, if the Safari browser plays music with a third-party application, and then resumes the voice and video communications, the communications is compromised. This is caused by Safari updates. For more information, see:
https://bugs.webkit.org/show_bug.cgi?id=179964

### Why can’t I set the video profile with the Firefox browser?

When using the Firefox browser, you may fail to set the video profile because of incompatibility between your computer and the browser. This issue occurs on the following devices:

* MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
* Windows 10 (MI)

If you encounter this problem, please contact Agora’s technical support.

### Why can’t I call getAudioLevel to get the volume level on iOS?

On the iOS Safari browser, you cannot get the volume level by calling getAudioLevel. This is caused by the browser.

### What resolutions does Safari support?

Safari supports the following resolutions and corresponding frame rates:

<table>
  <tr>
    <th>Video Profile</th>
    <th>Resolution (width x height)</th>
    <th>Frame Rate</th>
    <th>Bitrate</th>
  </tr>
  <tr>
    <td>4K</td>
    <td>3840 x 2160</td>
    <td>30 fps</td>
    <td>8910</td>
  </tr>
  <tr>
    <td>4K_1</td>
    <td>3840 x 2160</td>
    <td>30 fps</td>
    <td>8910</td>
  </tr>
  <tr>
    <td>4K_3</td>
    <td>3840 x 2160</td>
    <td>60 fps</td>
    <td>13500</td>
  </tr>
  <tr>
    <td>1440P</td>
    <td>2560 x 1440</td>
    <td>30 fps</td>
    <td>4850</td>
  </tr>
  <tr>
    <td>1440P_1</td>
    <td>2560 x 1440</td>
    <td>30 fps</td>
    <td>4850</td>
  </tr>
  <tr>
    <td>1440P_2</td>
    <td>2560 x 1440</td>
    <td>60 fps</td>
    <td>7350</td>
  </tr>
  <tr>
    <td>1080P</td>
    <td>1920 x 1080</td>
    <td>15 fps</td>
    <td>2080</td>
  </tr>
  <tr>
    <td>1080P_1</td>
    <td>1920 x 1080</td>
    <td>15 fps</td>
    <td>2080</td>
  </tr>
  <tr>
    <td>1080P_2</td>
    <td>1920 x 1080</td>
    <td>30 fps</td>
    <td>3000</td>
  </tr>
  <tr>
    <td>1080P_3</td>
    <td>1920 x 1080</td>
    <td>30 fps</td>
    <td>3150</td>
  </tr>
  <tr>
    <td>1080_5</td>
    <td>1920 x 1080</td>
    <td>60 fps</td>
    <td>4780</td>
  </tr>
  <tr>
    <td>720P</td>
    <td>1280 x 720</td>
    <td>15 fps</td>
    <td>1130</td>
  </tr>
  <tr>
    <td>720P_1</td>
    <td>1280 x 720</td>
    <td>15 fps</td>
    <td>1130</td>
  </tr>
  <tr>
    <td>720P_2</td>
    <td>1280 x 720</td>
    <td>15 fps</td>
    <td>2080</td>
  </tr>
  <tr>
    <td>720P_3</td>
    <td>1280 x 720</td>
    <td>30 fps</td>
    <td>1710</td>
  </tr>
  <tr>
    <td>480P</td>
    <td>640 x 480</td>
    <td>15 fps</td>
    <td>500</td>
  </tr>
  <tr>
    <td>480P_1</td>
    <td>640 x 480</td>
    <td>15 fps</td>
    <td>500</td>
  </tr>
  <tr>
    <td>480P_4</td>
    <td>640 x 480</td>
    <td>30 fps</td>
    <td>750</td>
  </tr>
</table>

### Does Web SDK support iFrame? Why can't Stream init successfully in iFrame when I use Chrome?

The Web SDK supports iFrame. According to the Chrome policy, iFrame requires additional permissions. For example: `<iframe src="..." allow="microphone; camera"></iframe>.`

### Why does the black screen appear after entering the channel using Huawei P10 (VTR-AL00)?

Because Huawei P10 (VTR-AL00) does not support the Web RTC.


### Blank Screen

When users join the live broadcast channel with the Agora Native SDK and Agora Web SDK (WebRTC), users from PC or Mobile applications can see both the local and remote videos, but users using web browsers cannot see the remote videos.

Call the API method enableWebSdkInteroperability in the Agora Native SDK to enable the interoperability function with WebRTC.

### Error Message

The following lists the most frequently found errors:

#### Device Limitation

The SDK returns “Media access: Overconstrained getUserMedia failed Object { type=”error”, msg=”Overconstrained”/”CONSTRAINT_NOT_SATISFIED”.

This issue is usually caused by the device limitation:

1.  Call the API method setVideoProfile to adjust the resolution. For example, if the camera cannot capture 720p or 1080p video.
2.  Update your browser.

#### Missing Camera

The SDK returns “Uncaught TypeError: Cannot read property ‘0’ of null”.

The issue is usually caused by a device with no camera. Set the parameter video as false when calling the API method createStream.

#### Missing Libraries

The SDK returns “Uncaught ReferenceError: ** is not defined”.

Reference the extension libraries.

#### Browser Issues

The SDK returns “getUserMedia() no longer works on insecure origins. To use this feature, consider switching your application to a secure origin, such as HTTPS”.

Use https instead of http to deploy the website.



