
---
title: Share the Screen
description: 
platform: Web
updatedAt: Thu Sep 19 2019 10:28:13 GMT+0800 (CST)
---
# Share the Screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances the communication experience by displaying the speaker's screen on the display of other speakers or audience members in the channel.

You can use screen sharing in the following scenarios:

- Video conference: the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- Online class: the teacher can share slides or notes with students.

### Working principles

Screen sharing on the web client is enabled by creating a screen-sharing stream.

- If you publish the screen-sharing stream only, set `video` as `false`, and `screen` as `true` when creating a stream.
- If you publish both the local video stream and your screen-sharing stream, you need to create two client objects:
  - A client object for sending the local stream: Set `video` as true and `screen` as false.
  - A client object for sending the screen-sharing stream: Set `video` as false and `screen` as true.


## Implementation

Ensure that you [set up the development environment](https://docs.agora.io/en/Video/start_call_web?platform=Web#set-up-the-development-environment).

To enable screen sharing, you need to set relevant attributes when creating the video stream. The web browser asks you to select which screens to share. The attribute settings are different in Google Chrome and Firefox.

### <a name = "chrome"></a>Screen sharing on Google Chrome

#### Screen sharing with the Google Chrome extension

Ensure that you add the [Google Chrome Extension for Screen Sharing](../../en/Quickstart%20Guide/chrome_screensharing_plugin.md) provided by Agora.

Fill in the `extensionId` when you create the stream.

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

#### Screen sharing without an extension

From v2.6.0, Agora Web SDK supports screen sharing without an extension on Chrome 72 or later, and you do not need to set the `extensionId` parameter.

```javascript
// Check if the browser supports screen sharing without an extension
Number.tem = ua.match(/(Chrome(?=\/))\/?(\d+)/i);
if(parseInt(tem[2]) >= 72  && navigator.mediaDevices.getDisplayMedia ) {
 // Create the stream for screensharing
	screenStream = AgoraRTC.createStream({
		streamID: uid,
		audio: false,
		video: false,
		screen: true,
	});
}
```

> - Do not set the `video` and `screen` attributes as `true` at the same time.
> - Agora recommends that you set the `audio` attribute as `false` to avoid any echo during the call.

### <a name = "electron"></a>Screen sharing on Electron

Screen sharing on Electron does not require any plugin, but you need to draw the UI for screen-source selection. Electron only provides interfaces for sharing the screen. 

Follow these steps to implement screen sharing on Electron:

1. Call the `AgoraRTC.getScreenSources` method to get sources of the screens to share. 

   ```javascript
   AgoraRTC.getScreenSources(function(err, sources) {
   	console.log(sources)
   }
   ```

   The `sources` parameter is an array of the `source` objects. The  `source` object contains the following properties of the screen source:

   ![img](https://web-cdn.agora.io/docs-files/1547455349613)

   - `id`: `sourceId`.
   - `name`: name of the screen source.
   - `thumbnail`: thumbnail of the screen source.

2. Based on the properties of `source`, draw the UI (by HTML and CSS) for selecting the screen source to be shared. For quick integration, Agora provides a default UI.

   The following figure shows the properties' corresponding elements on the UI for screen source selection in Google Chrome:

   ![img](https://web-cdn.agora.io/docs-files/1547456888707)

3. Get the `sourceId` of the source selected by the user.

4. Fill in `sourceId` when creating the screen-sharing stream.

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

   If you leave `sourceId` empty, the Agora SDK uses the default UI for screen source selection when calling the `localStream.init` method. The following figure shows the default UI:

   ![img](https://web-cdn.agora.io/docs-files/1547455511311)

> - The `getScreenSources` method is a wrapper of the `desktopCapturer.getSources` method provided by Electron. See [desktopCapturer](https://electronjs.org/docs/api/desktop-capturer).
> - The `sourceId` parameter only takes effect on Electron.

### <a name = "ff"></a>Screen sharing on Firefox

Set the `mediaSource` attribute to specify the screen-sharing mode:

- `screen`：shares the whole screen.
- `application`：shares all windows of an application.
- `window`：shares a specific window of an application.

```javascript
screenStream = AgoraRTC.createStream({
  streamID: uid,
  audio: false,
  video: false,
  screen: true,
  mediaSource: 'screen' // 'screen', 'application', 'window'
});
```

> - Do not set the `video` and `screen` attributes as `true` at the same time.
> - Agora recommends that you set the `audio` attribute as `false` to avoid echo during the call.
> - Firefox on Windows does not support the application mode.

### <a name = "both"></a>Enable both screen sharing and video

One client only sends one stream. If you want to enable both screen sharing and video on one host, you need to create two clients: 

- A client to send the screen-sharing stream.
- A client to send the video stream.

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

If two clients of a host subscribe to each other, extra charges incur.

<img alt="../_images/screensharing_streams.png" src="https://web-cdn.agora.io/docs-files/en/screensharing_streams.png" style="width: 500px; "/>

Agora recommends that you save the returned `uid` when each client joins the channel. When the `stream-added` event occurs, first check if the joined client is a local stream, if yes, do not subscribe to the client.

```javascript
var localStreams = [];
...

screenClient.join(channelKey, channel, null, function(uid) {
 // Save the uid of the local stream.
 localStreams.push(uid);
}
...

screenClient.on('stream-added', function(evt) {
 var stream = evt.stream;
 var uid = stream.getId()
 // When the 'stream-added' event occurs, check if the stream is a local uid.
 if(!localStreams.includes(uid)) {
  console.log('subscribe stream: ' + uid);
  // Subscribe to the stream.
  screenClient.subscribe(stream);
 }
})
```

### Sample code

The following sample code implements screen sharing and publishing the local video stream. Meanwhile, we provide an open-source GitHub [demo project](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-Web-Webpack). You can [try it online](https://webdemo.agora.io/agora-web-showcase/examples/Agora-Screen-Sharing-Web) and refer to the code in  [`rtc-client.js`](https://github.com/AgoraIO/Advanced-Video/blob/master/Screensharing/Agora-Screen-Sharing-Web-Webpack/src/rtc-client.js) and [`index.js`](https://github.com/AgoraIO/Advanced-Video/blob/master/Screensharing/Agora-Screen-Sharing-Web-Webpack/src/index.js) .

<div class="alert note">The following code uses <code>isFirefox</code> and <code>isCompatibleChrome</code> to determine the browser type, and you need to define the functions. See the code in <a href="https://github.com/AgoraIO/Advanced-Video/blob/master/Screensharing/Agora-Screen-Sharing-Web-Webpack/src/common.js#L28"><code>common.js</code></a> for an example.</div>

```javascript
//TODO: Fill in your App ID
var appID = "<yourAppID>";
var channel = "screen_video";
var channelKey = null;

AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);

var localStreams = [];

var screenClient = AgoraRTC.createClient({
    mode: 'rtc',
    codec: 'vp8'
});
screenClient.init(appID, function() {
    screenClient.join(channelKey, channel, null, function(uid) {
        // Save the uid of the local stream.
        localStreams.push(uid);
        // Create the stream for screen sharing.
        const streamSpec = {
            streamID: uid,
            audio: false,
            video: false,
            screen: true
          }
          // Set relevant attributes according to the browser
          // Note you need to implement isFirefox and isCompatibleChrome
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

            // Listen to the 'stream-added' event.
            screenClient.on('stream-added', function(evt) {
                var stream = evt.stream;
                var uid = stream.getId()

                // Check if the stream is a local uid.
                if (!localStreams.includes(uid)) {
                    console.log('subscribe stream:' + uid);
                    // Subscribe to the stream.
                    screenClient.subscribe(stream);
                }
            })

        }, function(err) {
            console.log(err);
        });

    }, function(err) {
        console.log(err);
    })
});

var videoClient = AgoraRTC.createClient({
    mode: 'rtc',
    codec: 'vp8'
});
videoClient.init(appID, function() {
    videoClient.join(channelKey, channel, null, function(uid) {
        // Save the uid of the local stream.
        localStreams.push(uid);

        // Create the video stream.
        videoStream = AgoraRTC.createStream({
            streamID: uid,
            audio: true,
            video: true,
            screen: false
        });
        return;

        // Initialize the stream.
        videoStream.init(function() {
            // Play the stream.
            videoStream.play('Video');
            // Publish the stream.
            videoClient.publish(videoStream);
            // Listen to the 'stream-added' event.
            videoClient.on('stream-added', function(evt) {
                var stream = evt.stream;
                var uid = stream.getId();
                // Check if the stream is a local uid.
                if (!localStreams.includes(uid)) {
                    console.log('subscribe stream:' + uid);
                    // Subscribe to the stream.
                    screenClient.subscribe(stream);
                }
            })
        }, function(err) {
            console.log(err);
        });

    }, function(err) {
        console.log(err);
    })
});
```

## Considerations

- Do not set the uid of the screen-sharing stream to a fixed value. Streams with the same uid can interfere with each other.
- **Do not subscribe to a locally published screen-sharing stream**, else additional charges incur.
- Ensure that `video`/`audio` is set as `false` when creating the screen-sharing stream.
- Sharing the window of a QQ chat on Windows causes the black screen.
