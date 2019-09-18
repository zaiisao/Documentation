
---
title: Watch Live Video on Mobile Phones
description: 
platform: Web
updatedAt: Wed Sep 18 2019 02:05:08 GMT+0800 (CST)
---
# Watch Live Video on Mobile Phones
## Introduction

The Agora Web SDK adds the HTML5 plugin to support playing streams on mobile webpages. This feature enables sharing the link of a live broadcast in social apps for the audience to directly watch the live video inside the app.

### HTML5 support

#### **iOS**

On iOS, WeChat uses the system WebView, so the support for the HTML5 plugin depends on the iOS version.

<table>
  <tr>
    <th>iOS WeChat</th>
    <th>iOS Safari</th>
  </tr>
  <tr>
    <td bgcolor="grey"><font color="white">No support on iOS versions earlier than iOS 11.0</font></td>
    <td bgcolor="grey"><font color="white">No support on iOS versions earlier than iOS 11.0</font></td>
  </tr>
  <tr>
    <td bgcolor="#3ab7f8"><font color="white">Support on iOS 11.0 or later</font></td>
    <td bgcolor="#3ab7f8"><font color="white">Support on iOS 11.0 or later</font></td>
  </tr>
</table>

#### **Android**

Android WebView supports customization, and Android WeChat uses a self-developed WebView. Therefore, the support for the HTML5 plugin depends on the app version.

<table>
  <tr>
    <th>Android WeChat</th>
    <th>Android Chrome</th>
  </tr>
  <tr>
    <td bgcolor="grey"><font color="white">No support on WeChat versions earlier than WeChat 7.0</font></td>
    <td bgcolor="grey"><font color="white">No support on Chrome versions earlier than Chrome 58.0</font></td>
  </tr>
  <tr>
    <td bgcolor="#3ab7f8"><font color="white">Support on WeChat 7.0 or later</font></td>
    <td bgcolor="#3ab7f8"><font color="white">Support on Chrome 58.0 or later</font></td>
  </tr>
</table>

#### **Host**

The HTML5 plugin has the following requirements for the host client:

- Supports the Agora Native SDK and the Agora Web SDK on Chrome. Do not use other browsers such as Safari and Firefox.
- Ensure that the host uses the live-broadcast channel profile.
- If the host also uses the Agora Web SDK, ensure that `codec` is set as `"h264"` in the `createClient` method.
- If the host uses the Agora Native SDK v2.3.2 or later, ensure that the `setParameters` method is called before joining a channel:

  ```cpp
  setParameters("{\"che.hardware_encoding\":0}")
  setParameters("{\"che.video.h264Profile\":66}")
  ```
- Do not set the video resolution to be higher than 480p.

## Implementation

### Integrate the SDK
Download the [SDK with the HTML5 plugin](http://download.agora.io/sdk/release/rts-v2.8.0.zip) and integrate the SDK on the audience side. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md) for details.

### Check the system requirements

Check if the system supports the HTML5 plugin.

```javascript
AgoraRTS.checkSystemRequirements()
```

This method call returns `true` if the system supports the RTS plugin.

### Import the RTS plugin

To import the RTS plugin, add the following code after creating the client. 

```javascript
var client = AgoraRTC.createClient({ mode: "live", codec: "h264"});
AgoraRTS.init(AgoraRTC);
AgoraRTS.proxy(client);
```

> Ensure that you set `mode` as `"live"` and codec as `"h264"` in the `createClient` method.

After importing the HTML5 plugin, all the streams that you subscribe to become rtsStream objects. For example:

- When a remote stream is added:

  ```javascript
  client.on("stream-added", function(e) {
    var stream = e.stream; // This is a rtsStream object
    client.subscribe(stream, { video: true, audio: true});
  });
  ```

- When a remote stream is subscribed to:

  ```javascript
  client.on("stream-subscribed", function (e) {
    var stream = e.stream; // This is a rtsStream object
  });
  ```

The rtsStream object is for receiving and playing software decoded streams. It is different from the Stream object of the original Agora Web SDK.

- The Stream object is actually a [MediaStream](https://developer.mozilla.org/us-EN/docs/Web/API/MediaStream) object.
- The rtsStream object is a software decoded stream and does not support the methods of MediaStream.

The rtsStream object only supports methods related to the remote streams, see [Considerations](#note) for details.

### Subscribe to rtsStream

When subscribing to an rtsStream object, ensure that you set the `options` parameter in the [`subscribe`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.client.html#subscribe) method. For example:

```javascript
client.subscribe(stream, { video: true, audio: true }, console.log);
```

## <a name="note"></a>Considerations

- The audience can subscribe to two 480p streams or four 240p streams at most.
- After importing the HTML5 plugin, the `"active-speaker"` event will not be triggered.
- Do not call methods that block the main thread, such as `Window.alert()`.
- Due to web browser [autoplay policy changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes), audio is not automatically played on iOS webpages and Android Chrome 70 or later. We recommend  triggering the stream playback by the user's gesture.
- rtsStream objects are different from Stream objects:
  - rtsStream methods do not trigger any event.
  - rtsStream objects do not support the `getTrack` and `enableAudioVolumeIndicator` methods.
  - rtsStream objects support the following methods:
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
  - rtsStream objects are played by Canvas + AudioContext, and you need to implement the player controls.
