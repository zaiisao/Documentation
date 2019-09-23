
---
title: Watch Live Video on Mobile Phones
description: 
platform: Web
updatedAt: Mon Sep 23 2019 05:55:25 GMT+0800 (CST)
---
# Watch Live Video on Mobile Phones
## Introduction

The Agora Web SDK adds an HTML5 plugin for playing streams on mobile webpages. Users can now share a live broadcast link within social apps for audiences to watch.

### HTML5 support

#### **iOS**

iOS WeChat uses the system WebView, so support for the HTML5 plugin depends on the iOS version.

<table>
  <tr>
    <th>iOS WeChat</th>
    <th>iOS Safari</th>
  </tr>
  <tr>
    <td bgcolor="grey"><font color="white">No support on iOS versions earlier than iOS 9.0</font></td>
    <td bgcolor="grey"><font color="white">No support on iOS versions earlier than iOS 9.0</font></td>
  </tr>
  <tr>
    <td bgcolor="#3ab7f8"><font color="white">Supported on iOS 9.0 or later</font></td>
    <td bgcolor="#3ab7f8"><font color="white">Supported on iOS 9.0 or later</font></td>
  </tr>
</table>

#### **Android**

Support for the HTML5 plugin depends on the app version of Android WeChat, because it uses a customized version of WebView.

<table>
  <tr>
    <th>Android WeChat</th>
    <th>Android Chrome</th>
    <th>Android WebView</th>
  </tr>
  <tr>
    <td bgcolor="grey"><font color="white">No support on WeChat versions earlier than WeChat 6.0</font></td>
    <td bgcolor="grey"><font color="white">No support on Chrome versions earlier than Chrome 33.0</font></td>
    <td bgcolor="grey"><font color="white">No support on Android versions earlier than 5.0</font></td>
  </tr>
  <tr>
    <td bgcolor="#3ab7f8"><font color="white">Supported on WeChat 6.0 or later</font></td>
    <td bgcolor="#3ab7f8"><font color="white">Supported on Chrome 33.0 or later</font></td>
    <td bgcolor="#3ab7f8"><font color="white">Supported on Android 5.0 or later</font></td>
  </tr>
</table>

#### **Host**

The HTML5 plugin has the following requirements for the host client:

- If the host also uses the Agora Web SDK, ensure that you set `codec` as `"h264"` in the `createClient` method.
- We do not recommend setting the video resolution to higher than 480p.

### Working principles

The Agora Web SDK is based on WebRTC and relies on the browser's support for WebRTC.

Though the built-in browser of WeChat supports WebRTC, the supported codecs vary dependant on the platform:

|       | Android    | iOS                          |
| :---- | :--------- | :--------------------------- |
| H.264 | No support | Supported on 12.1.4 or later |
| VP8   | Supported  | Supported on 12.2 or later   |

The HTML5 plugin provides a software-based decoding alternative for those platforms that do not support the H.264 codec. The plugin decodes H.264 streams for versions of Android and iOS 12.1.4 or earlier.

We recommend using the HTML5 plugin on platforms that do not directly support the H.264 codec.

## Implementation

### Integrate the SDK
Download the [SDK containing the HTML5 plugin](https://download.agora.io/sdk/release/rts-v2.8.0.400.zip) and integrate the SDK on the audience's side. See [Start a Live Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/start_live_web?platform=Web) for details.

<div class="alert info">We provide a demo project in the SDK package. You can load the <code>index.html</code> file through a local web server to try the demo. <br/>Note that directly opening the HTML file in a browser does not work. For instructions on how to run the web app through a local server, see <a href="https://docs.agora.io/en/Interactive%20Broadcast/start_live_web?platform=Web#run-your-app">Run your app<a>.</div>

The SDK package includes the following files:

- `AgoraRTC.js`: The JavaScript file of the Agora Web SDK.
- `AgoraRTS.js`: The JavaScript file of the HTML5 plugin that relies on `AgoraRTC.js`.
- `AgoraRTS.wasm`: The binary-coded decoding library file.
- `AgoraRTS.asm`: The JavaScript-based decoding library file. 

The SDK automatically loads the appropriate decoding library according to the web browser being used. Software decoding in a web browser requires [WebAssembly](https://webassembly.org/), which loads a binary-coded decoding library for the webpage. 

- If the browser supports WebAssembly, the HTML5 plugin uses `AgoraRTS.wasm` for decoding.
- If the browser does not support WebAssembly, the HTML5 plugin uses `AgoraRTS.asm`  to decode H.264 for older systems. However,  `AgoraRTS.asm` has a larger file size and higher CPU consumption than `AgoraRTS.wasm`.

### Check the system requirements

Check system support for the HTML5 plugin.

```javascript
AgoraRTS.checkSystemRequirements()
```

This method call returns `true` if the system supports the RTS plugin.

### Import the HTML5 plugin

To import the HTML5 plugin, call  `AgoraRTS.init` and `AgoraRTS.proxy` after creating the client. 

```javascript
var client = AgoraRTC.createClient({ mode: "live", codec: "h264"});
AgoraRTS.init(AgoraRTC, {
  /**
   * Set the online filepath for the decoding libraries.
   * The SDK requests library files according to this setting.
   * We recommend enabling the HTTP cache on your server.
   */
  wasmDecoderPath: "./xxx/AgoraRTS.wasm",
  asmDecoderPath: "./xxx/AgoraRTS.asm",
});
AgoraRTS.proxy(client);
```

> Ensure that you set `mode` to `"live"` and codec to `"h264"` in the `createClient` method.

After you import the HTML5 plugin, in all the events related with subscribing to remote streams, the SDK returns  `rtsStream` objects. For example:

- In the `stream-added` event

  ```javascript
  client.on("stream-added", function(e) {
    var stream = e.stream; // This is a `rtsStream` object
    client.subscribe(stream, { video: true, audio: true});
  });
  ```

- In the `stream-subscribed` event

  ```javascript
  client.on("stream-subscribed", function (e) {
    var stream = e.stream; // This is a `rtsStream` object
  });
  ```

The `rtsStream` object receives and plays software decoded streams. It works differently than the original Agora Web SDK Stream object in the following ways:

- The Stream object is actually a [`MediaStream`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream) object.
- The `rtsStream` object is a software decoded stream and does not support the methods of MediaStream.

The `rtsStream` object only supports methods related to the remote streams, see [Considerations](#note) for details.

### Subscribe to rtsStream

Call the `subscribe` method to subscribe to an `rtsStream` object.

```javascript
client.subscribe(stream, { video: true, audio: true }, console.log);
```

<div class="alert note">Ensure that you set the <code>options</code> parameter in the <code>subscribe</code> method.</div> 

## <a name="note"></a>Considerations

- The audience can subscribe to a maximum of two 480p streams, or four 240p streams.
- After you import the HTML5 plugin, the `"active-speaker"` event will not be triggered.
- Do not call methods that block the main thread, such as `Window.alert()`.
- Due to web browser [autoplay policy changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes), audio does not automatically play on iOS webpages and Android Chrome 70 or later. We recommend triggering stream playback at the user gesture.
- How `rtsStream` objects differ from Stream objects:
  - `rtsStream` methods do not trigger any event.
  - `rtsStream` objects do not support the `getTrack` and `enableAudioVolumeIndicator` methods.
  - `rtsStream` objects support the following methods:
    - `init`
    - `play`
    - `stop`
    - `isPlaying`
    - `unmuteAudio`
    - `muteAudio`
    - `hasAudio`
    - `getAudioLevel`
    - `setAudioVolume`
    - `unmuteVideo`
    - `muteVideo`
    - `hasVideo`
    - `getId`
    - `getStats`
  - `rtsStream` objects are played by Canvas + AudioContext, and require an implementation of the player controls.
