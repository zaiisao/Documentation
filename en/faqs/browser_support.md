
---
title: Which browsers does the Agora Web SDK support?
description: 
platform: Web
updatedAt: Tue Jun 02 2020 11:50:09 GMT+0800 (CST)
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
    <td><font color="green">✔</td>
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
    <td><font color="green">✔</td>
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

<div class="alert info">Other browser support:
	<li>The Agora Web SDK v2.5 or later supports Chrome 49 on Windows XP (supports the VP8 codec only, and cannot interop with the Native SDK).</li>
	<li>The Agora Web SDK v2.7 or later supports Edge on Windows 10, see <a href="https://docs.agora.io/en/faq/browser_support#edge">Edge support</a> for details.</li>
	<li>The Agora Web SDK theoretically supports 360 Extreme Browser, but we do not guarantee full support.</li>
</div>
<div class="alert note"> Upgrade to Agora Web SDK v2.6 or later in the following scenarios:
	<li>Safari on iOS 12.1.4 or later.</li>
	<li>Safari 12.1 or later on macOS.</li>
</div>

Due to the various browser engine implementations, support for some features may vary by browser and platform. The following are known issues and limitations.

## Limitation

On Chrome 81 or later, Safari, and Firefox, device IDs are only available after the user has granted permissions to use the media device. See [Why can't I get device ID on Chrome 81?](https://docs.agora.io/en/faq/empty_deviceId)
	
### Chrome

The Agora Web SDK is based on WebRTC and works best on Chrome.
- The Agora Web SDK supports Chrome 58 or later.
- On some Android devices, Chrome does not support the H.264 codec.
- Some APIs require later versions of Chrome, see the API Reference for details.

### Safari

- Safari 12.1 or earlier only supports the H.264 codec.
- Device permissions
  - Safari does not support getting the output device information, so it does not support the `getPlayoutDevices` and `setAudioOutput` methods.
  - If **Auto-Play** is not enabled on Safari, the stream playback has no audio. You have to call the `navigator.mediaDevices.getUserMedia` method to get the device permissions before playing a stream.
- Safari does not support the `addTrack` and `removeTrack` methods.
- Safari does not support the [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#dual-stream).
- Safari does not support the `getAudioLevel` method on iOS.
- Occasional audio routing issues on iOS Safari: The audio is routed to the speakerphone when a headset is connected, or the audio is routed to the earpiece when no headset is connected.

### Firefox

- When the Firefox browser interoperates with the Native SDK for iOS, the video in the Firefox browser is rotated.
- Firefox does not support changing the frame rate (30 fps by default).
- Setting the video profile on Firefox does not take effect on the following devices:
  - MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
  - Windows 10 (MI)

### Edge

The Agora Web SDK v2.7 or later supports the Microsoft Edge browser. Due to browser limitations, the Agora Web SDK supports the following functions:

- Communicates with the Agora Native/Web SDK in audio/video calls and live broadcasts.
- Gets the connection statistics by calling the `getStats` method.
- Gets the audio level by calling the `getAudioLevel` method.
- Disables/Enables the audio track by calling the `muteAudio`/`unmuteAudio` method.
- Disables/Enables the video track by calling the `muteVideo`/`unmuteVideo` method.

## Reference
[How to use Agora Web SDK on mobile devices?](https://docs.agora.io/en/faq/web_on_mobile)
