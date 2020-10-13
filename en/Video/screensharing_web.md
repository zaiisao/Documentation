
---
title: Share the Screen
description: 
platform: Web
updatedAt: Mon Oct 12 2020 07:33:33 GMT+0800 (CST)
---
# Share the Screen
## Introduction

Screen sharing enables the host of a video call or live interactive streaming to display what is on their screen to other users in the channel. This function has many obvious advantages for communicating information, particularly in the following scenarios:

- In a video conference, the speaker can share a local image, web page, or full presentation with other participants.
- In an online class, the teacher can share slides or notes with students.

<div class="alert note">The Agora RTC Web SDK supports screen sharing in the following <b>desktop</b> browsers:<li>Chrome 58 or later</li><li>Firefox 56 or later</li><li>Edge 80 or later on Windows 10+</li></div>

<div class="alert info">Click the <a href="https://webdemo.agora.io/agora-web-showcase/examples/Agora-Screen-Sharing-Web/">online demo</a> to try this feature out.</div>

## Working principles

In order to enable screen sharing on the web client, you need to call `createStream` to create a screen-sharing stream. You do this by setting the relevant properties when you create the stream. The property settings of a stream differ between browsers (as shown in [Implementation](#implementation)). After you call `createStream`, the web browser pops up a window to let the end user select which screens to share and then shares the selected screens.

The following are general guidelines:

- If you publish a screen-sharing stream only, you need to call `createClient` once to create one client object. Set `video` as `false`, and `screen` as `true` when calling `createStream`.
- If you publish both a camera stream and a screen-sharing stream, you need to call `createClient` twice to create two client objects:
  - A client object for publishing the camera stream. Set `video` as `true` and `screen` as `false` when calling `createStream`.
  - A client object for publishing the screen-sharing stream. Set `video` as `false` and `screen` as `true` when calling `createStream`.

As of v3.2.0, the Web SDK supports setting the transmission optimization strategy by using `optimizationMode`. For a screen-sharing stream, the default value of the `optimizationMode` property is `"detail"` and the SDK prioritizes clarity. This strategy is applicable to scenarios where the shared content is text or images. If the shared content is a movie or video clip, Agora recommends setting `optimizationMode` as `"motion"`. The SDK prioritizes smoothness.


## <a name="implementation"></a>Implementation

This section introduces how to share the screen on Chrome, Edge, Firefox, and Electron.

Before you begin, ensure that you understand how to [start a video call](../../en/Video/start_call_web.md) or [start interactive video streaming](../../en/Video/start_live_web.md).

### <a name="chrome"></a>Screen sharing on Chrome

The Web SDK supports screen sharing on Chrome 58 or later. There are two ways to enable screen sharing on Chrome: with or without using the Google Chrome Extension for Screen Sharing provided by Agora. 

Screen sharing without the extension requires the Agora Web SDK v2.6 or later, and Chrome 72 or later. Otherwise, you have to use the <a href="#ext">Google Chrome extension</a> to share the screen.

#### <a name="no-ext"></a>Screen sharing without the Google Chrome extension

To enable screen sharing without the extension, set `video` as `false`, and `screen` as `true` when calling `createStream` to create a screen-sharing stream.

```javascript
// Check if the browser supports screen sharing without an extension.
Number.tem = ua.match(/(Chrome(?=\/))\/?(\d+)/i);
if(parseInt(tem[2]) >= 72  && navigator.mediaDevices.getDisplayMedia ) {
 // Create the stream for screen sharing.
	screenStream = AgoraRTC.createStream({
		streamID: uid,
		audio: false,
		video: false,
		screen: true,
	});
}
```

#### <a name="ext"></a>Screen sharing with the Google Chrome extension

Perform the following steps to share the screen on Chrome using the extension:

1. Add the [Google Chrome Extension for Screen Sharing](../../en/Video/chrome_screensharing_plugin.md) provided by Agora and get an extension ID.

2. Set `video` as `false`, `screen` as `true`, and `extensionId` as the string you get in the previous step when calling `createStream` to create a screen-sharing stream.

   ```javascript
   screenStream = AgoraRTC.createStream({
   streamID: uid,
   audio: false,
   video: false,
   screen: true,
   //chrome extension ID
   extensionId: 'minllpmhdgpndnkomcoccfekfegnlikg'
   });
   ```

### Screen sharing on Edge

The Web SDK supports screen sharing on Edge 80 or later on Windows 10+. To enable screen sharing on Edge, set `video` as `false`, and `screen` as `true` when calling `createStream` to create a screen-sharing stream.

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
});
```

### <a name = "ff"></a>Screen sharing on Firefox

The Web SDK supports screen sharing on Firefox 56 or later. To enable screen sharing on Firefox, in addition to setting `video` as `false`, and `screen` as `true` when calling `createStream` to create a screen-sharing stream, you also need to set the `mediaSource` property to specify the screen-sharing mode:

- `screen`: Shares the whole screen.
- `application`: Shares all windows of an application.
- `window`: Shares a specific window of an application.

> Firefox on Windows does not support the application mode.

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
  mediaSource: 'screen' // 'screen', 'application', 'window'
});
```

### <a name = "electron"></a>Screen sharing on Electron

To enable screen sharing on Electron, you need to draw the UI for your users to select which screen or window to share.

For quick integration, perform the following steps to use the default UI provided by Agora:

1. Call `AgoraRTC.createStream`, and set `video` as `false`, and `screen` as `true` to create a screen-sharing stream.

   ```javascript
   localStream = AgoraRTC.createStream({
       streamID: UID,
       audio: false,
       video: false,
       screen: true
   });
   ```

2. Call `localStream.init` to initialize the stream.

   ```javascript
   localStream.init(function(stream) {})
   ```

   The SDK provides a default UI for screen source selection, as the following figure shows:

   ![](https://web-cdn.agora.io/docs-files/1571629977600)

If you want to customize the UI, perform the following steps:

1. Call `AgoraRTC.getScreenSources` to get sources of the screens to share.

   ```javascript
   AgoraRTC.getScreenSources(function(err, sources) {
   	console.log(sources)
   })
   ```

   The `sources` parameter is an array of the `source` object. A  `source` object contains the following properties of the screen source:

   ![img](https://web-cdn.agora.io/docs-files/1547455349613)

   - `id`: The `sourceId`.
   - `name`: The name of the screen source.
   - `thumbnail`: The thumbnail of the screen source.

2. Based on the properties of `source`, draw the UI (by HTML and CSS) for the end user selecting the screen source to share.

   The following figure shows the properties' corresponding elements on the UI for screen source selection:

   ![img](https://web-cdn.agora.io/docs-files/1547456888707)

3. Get the `sourceId` of the source selected by the end user.

4. Set `sourceId` when creating the screen-sharing stream.

   ```javascript
   localStream = AgoraRTC.createStream({
       streamID: UID,
       audio: false,
       video: false,
       screen: true,
       sourceId: sourceId
   });
   localStream.init(function(stream) {})
   ```

> - The `getScreenSources` method is a wrapper of `desktopCapturer.getSources` provided by Electron. See [desktopCapturer](https://electronjs.org/docs/api/desktop-capturer).
> - Only Electron requires the `sourceId` parameter.

### <a name = "screenAudio"></a>Share audio

As of v3.0.0, the Web SDK supports sharing the local audio playback when sharing a screen on Windows Chrome 74 or later. To share the audio, set the `screen` and `screenAudio` properties as `true` and the `audio` property as `false` when calling `createStream` to create a screen-sharing stream.

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
  screenAudio: true
});
```

<div class="alert note">Note:
	<ul>
	<li>The <code>audio</code> and <code>screenAudio</code> properties are different：
			<ul>
				<li><code>audio</code> controls whether the stream contains the audio captured by the local audio input device.</li>
				<li><code>screenAudio</code> controls whether the stream contains the local audio playback.</li>
			</ul>
		Agora recommends setting <code>audio</code> as <code>false</code> in the screen-sharing stream. If you set both <code>audio</code> and <code>screenAudio</code>  as <code>true</code>, the stream contains the local audio playback only.</li>
	<li>For the audio sharing to take effect, the user must check <b>Share audio</b> on the pop-up window when sharing a screen.</li>
	<li>Audio sharing is disabled when the user shares a single application window.</li>
	</ul>
</div>

![](https://web-cdn.agora.io/docs-files/1574232834184)


### Sample code

The following sample code shows the complete steps to enable screen sharing.

<div class="alert info">The following code uses <code>isFirefox</code> and <code>isCompatibleChrome</code> to determine the browser type, and you need to define the functions. See the code in <a href="https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Screen-Sharing-Web-Webpack/src/common.js#L29"><code>common.js</code></a> for an example.</div>

```javascript
//TODO: Fill in your App ID.
var appID = "<yourAppID>";
var channel = "screen_video";
var channelKey = null;

AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);

var screenClient = AgoraRTC.createClient({
    mode: 'rtc',
    codec: 'vp8'
});
screenClient.init(appID, function() {
    screenClient.join(channelKey, channel, null, function(uid) {
        // Create the stream for screen sharing.
        const streamSpec = {
            streamID: uid,
            audio: false,
            video: false,
            screen: true
          }
          // Set relevant properties according to the browser.
          // Note that you need to implement isFirefox and isCompatibleChrome.
          if (isFirefox()) {
            streamSpec.mediaSource = 'window';
          } else if (!isCompatibleChrome()) {
            streamSpec.extensionId = 'minllpmhdgpndnkomcoccfekfegnlikg';
          }
        screenStream = AgoraRTC.createStream(streamSpec);
        // Initialize the stream.
        screenStream.init(function() {
            // Play the stream.
            screenStream.play('Screen');
            // Publish the stream.
            screenClient.publish(screenStream);

        }, function(err) {
            console.log(err);
        });

    }, function(err) {
        console.log(err);
    })
});
```

Additionally, we provide an open-source GitHub [sample project](https://github.com/AgoraIO/Advanced-Video/tree/master/Web/Agora-Screen-Sharing-Web-Webpack). You can [try it online](https://webdemo.agora.io/agora-web-showcase/examples/Agora-Screen-Sharing-Web) and refer to the code in  [`rtc-client.js`](https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Screen-Sharing-Web-Webpack/src/rtc-client.js) and [`index.js`](https://github.com/AgoraIO/Advanced-Video/blob/master/Web/Agora-Screen-Sharing-Web-Webpack/src/index.js).

## <a name = "both"></a>Enable both screen sharing and video

One client only sends one stream. If you want to enable both screen sharing and video capturing on one host, you need to create two clients:

- A client for sending the screen-sharing stream.
- A client for sending the video stream captured by a local camera.

```javascript
// Create the client to send the screen-sharing stream.
var screenClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
screenClient.init(key, function() {
 screenClient.join(channelKey, channel, null, function(uid) {
  // Create the screen-sharing stream, screenStream.
  ...
  screenClient.publish(screenStream);
 }
  }

// Create the client to send the video stream.
var videoClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
videoClient.init(key, function() {
videoClient.join(channelKey, channel, null, function(uid) {
  // Create the video stream, videoStream.
  ...
  videoClient.publish(videoStream);
 }
}
```

**Billing issues**: Two clients of a host subscribing to each other incurs extra charges, as shown in the following image:

<img alt="../_images/screensharing_streams.png" src="https://web-cdn.agora.io/docs-files/en/screensharing_streams.png" style="width: 500px; "/>

Agora recommends that you save the returned `uid` when each client joins the channel and handle the subscription as follows:
- For the camera client, when the `stream-added` event occurs, first check if the joined `uid` belongs to a local stream. If yes, do not subscribe to the stream.
- For the screen-sharing client, do not subscribe to any stream.

```javascript
var localStreams = [];
...

screenClient.join(channelKey, channel, null, function(uid) {
 // Save the uid of the local stream.
 localStreams.push(uid);
}
...

videoClient.on('stream-added', function(evt) {
 var stream = evt.stream;
 var uid = stream.getId()
 // When the 'stream-added' event occurs, check if the stream is a local uid.
 if(!localStreams.includes(uid)) {
  console.log('subscribe stream: ' + uid);
  // Subscribe to the stream.
  videoClient.subscribe(stream);
 }
})
```

## Set the screen profile

The default video profile for screen sharing is a resolution of 1920 × 1080 (width × height) and a frame rate of 5 fps. To use a different profile, call `Stream.setScreenProfile` to set the screen profile, as in the following example:

```javascript
// After creating a stream for screen sharing
screenStream.setScreenProfile("720p_1");
```

The SDK supports the following screen profiles:

| Screen profile | Resolution (width × height) | Frame rate |
| :------------- | :-------------------------- | :--------- |
| `480p_1`       | 640 × 480                   | 5 fps      |
| `480p_2`       | 640 × 480                   | 30 fps     |
| `720p_1`       | 1280 × 720                  | 5 fps      |
| `720p_2`       | 1280 × 720                  | 30 fps     |
| `1080p_1`      | 1920 × 1080                 | 5 fps      |
| `1080p_2`      | 1920 × 1080                 | 30 fps     |

<div class="alert note">Note:<br/>
<li> Ensure that you set the screen profile before initializing (<code>Stream.init</code>) the screen-sharing stream.</li>
<li>Setting the screen profile may affect your billing. Due to limitations of some devices and browsers, the resolution you set may fail to take effect and is adjusted by the browser. In this scenario, billing is calculated based on the actual resolution.</li></div>

## Considerations

- Set the `video` property as `false` when creating a screen-sharing stream.
- If you need to create multiple streams, we recommend setting the `audio` property as `true` for only one local stream.
- Do not set the UID of the screen-sharing stream to a fixed value. Streams with the same UID can interfere with each other.
- **Do not subscribe to a locally published screen-sharing stream**, else additional charges incur.
- Sharing the window of a QQ chat on Windows causes a black screen.

## Reference

When implementing the screen sharing, you can also refer to the following articles:

- [How can I switch between the screen-sharing stream and the camera stream?](https://docs.agora.io/en/faq/switch_screen_camera_web)
