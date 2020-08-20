
---
title: Which browsers does the Agora Web SDK support?
description: 
platform: Web
updatedAt: Thu Aug 20 2020 14:26:28 GMT+0800 (CST)
---
# Which browsers does the Agora Web SDK support?
The Agora Web SDK supports all mainstream browsers. 

<table>
  <tr>
    <th>Platform</th>
    <th>Chrome 58 or later</th>
    <th>Firefox 56 or later</th>
    <th>Safari 11 or later</th>
    <th>Opera 45 or later</th>
    <th>QQ Browser 10.5 or later</th>
    <th>360 Secure Browser</th>
    <th>WeChat Built-in Browser</th>
  </tr>
   <tr>
    <td>Android 4.1 or later</td>
		 <td><font color="green">✔<sup>[1]</sup></td>
    <td><font color="red">✘</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11 or later</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
		<td><font color="green">✔<sup>[2]</sup></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7 or later</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

<div class="alert warning">[1] Support for H.264 relies on hardware, and some Android devices do not support the H.264 codec.<br>[2] Agora does not recommend using the Web SDK on iOS Safari. See <a href="../../en/faq/browser_support.md">iOS Safari</a> for known issues and limitations. For better support on iOS, use the <a href="https://docs.agora.io/en/Interactive%20Broadcast/downloads">Agora iOS SDK</a>.</div>
<div class="alert warning"> Upgrade to the latest Agora Web SDK in the following scenarios:
	<li>Safari on iOS 12.1.4 or later.</li>
	<li>Safari 12.1 or later on macOS.</li>
</div>

Other browser support:

- The Agora Web SDK v2.5 or later supports Chrome 49 on Windows XP (supports the VP8 codec only, and cannot interop with the Native SDK).
- The Agora Web SDK v2.7 or later supports Edge on Windows 10, see <a href="https://docs.agora.io/en/faq/browser_support#edge">Edge support</a> for details.
- The Agora Web SDK theoretically supports 360 Extreme Browser, but we do not guarantee full support.

Due to the various browser engine implementations, support for some features may vary by browser and platform. The following are known issues and limitations.

## Limitation

On Chrome 81 or later, Safari, and Firefox, device IDs are only available after the user has granted permissions to use the media device. See [Why can't I get device ID on Chrome 81?](https://docs.agora.io/en/faq/empty_deviceId)
	
### Chrome

The Agora Web SDK is based on WebRTC and works best on Chrome.
- The Agora Web SDK supports Chrome 58 or later.
- On some Android devices, Chrome does not support the H.264 codec.
- Some APIs require later versions of Chrome, see the API Reference for details.
- On all AMD-based and some Intel-based devices with the Windows operating system, if Chrome uses the H.264 codec, the video transmission bitrate may be lower than the set value. To resolve it, you can set the browser to use the VP8 codec or try to disable hardware acceleration.

### Safari

<a name="ios"></a>**iOS Safari**

Known issues and limitations of Safari on iOS:

- The audio routing may change randomly: Sometimes, the audio is routed to the speakerphone when a headset is connected, or to the earpiece when no headset is connected.
- The volume of a remote user may change randomly on iOS 13.
- Safari does not support the `getAudioLevel` method on iOS.
- If you call `getUserMedia` twice to get two tracks of the same media type, the first track goes muted or black.
- Occasional: On iOS 13, after a user switches to other apps that use the microphone or camera (such as Siri or Skype) and then switches back, the audio sampling or video capture fails.
- Occasional: After the audio session is interrupted, for example, the local user mutes or unmutes the audio, uses Siri, or answers an incoming call, the user can no longer hear any remote users.

**Other issues**

The following lists other known issues and limitations of Safari on iOS and macOS:

- Safari 11 only supports the video resolution of 480P and higher.
- Safari 12.1 or earlier only supports the H.264 codec.
- Safari 13 users may not be able to hear other users.
- Device permission limitations:
  - Safari does not support getting the output device information, so it does not support the `getPlayoutDevices` and `setAudioOutput` methods.
  - If **Auto-Play** is not enabled on Safari (as the following figure shows), the stream playback has no audio. You have to call the `navigator.mediaDevices.getUserMedia` method to get the device permissions before playing a stream.
    ![](https://web-cdn.agora.io/docs-files/1591079062644)
- Safari does not support the `addTrack` and `removeTrack` methods.
- Safari does not support enabling [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#dual-stream).
- On Safari, when calling APIs to get quality statistics, the values of some properties are 0.For example, when calling `getLocalAudioStats` to get the audio statistics of the local stream, the values of `RecordingLevel` and `SendLevel` are 0.

### Firefox

- When the Web SDK on Firefox communicates with the SDK on some devices, the video on Firefox is rotated.
- Firefox does not support changing the frame rate (30 fps by default).
- Setting the video profile on Firefox does not take effect on the following devices:
  - MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
  - Windows 10 (MI)
- On Firefox, when calling APIs to quality statistics, the values of some properties are 0.For example, when calling `getLocalAudioStats` to get the audio statistics of the local stream, the values of `RecordingLevel` 和 `SendLevel` are 0.

### Edge

The Agora Web SDK v2.7 or later supports the Microsoft Edge browser. Due to browser limitations, the Agora Web SDK supports the following functions:

- Communicates with the Agora Native/Web SDK in audio calls, video calls, and live interactive streaming.
- Gets the connection statistics by calling the `getStats` method.
- Gets the audio level by calling the `getAudioLevel` method.
- Disables/Enables the audio track by calling the `muteAudio`/`unmuteAudio` method.
- Disables/Enables the video track by calling the `muteVideo`/`unmuteVideo` method.

## Reference
[How to use Agora Web SDK on mobile devices?](https://docs.agora.io/en/faq/web_on_mobile)
