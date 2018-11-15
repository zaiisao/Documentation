
---
title: Share the Screen
description: 
platform: Web
updatedAt: Thu Nov 15 2018 09:40:40 GMT+0000 (UTC)
---
# Share the Screen
## Introduction

During a video call or live broadcast, **sharing the screen** enhances communication by bringing whatever is on the speaker's screen to the other speakers or audience in the channel.

Screen share has extensive application in the following scenarios:

- For a video conference, the speaker can share the image of the local file, web page, and PPT with other users in the channel.
- For an online class, the teacher can share the image of the slides or notes with the students.

## Implementations

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md) for details.

To enable screen sharing, you need to set relevant attributes when creating the video stream. The web browser will ask you to select which screens to share. The attribute settings of the Chrome and Firefox browser vary.

### <a name = "chrome"></a>Screen Sharing on Google Chrome

Before enabling screen sharing on Google Chrome, ensure that you have added the [Google Chrome Extension for Screen Sharing](../../en/Quickstart%20Guide/chrome_screensharing_plugin.md) provided by Agora.

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

> - Do not set the `video` and `screen` attributes as `true` at the same time.
> - Agora recommends that you set the `audio` attribute as `false` to avoid any echo during the call.

### <a name = "ff"></a>Screen Sharing on Firefox

To enable screen sharing on Firefox, set the `mediaSource` attribute to specify the sharing mode:

- `screen`：Shares the whole screen.
- `application`：Shares all the windows of an application.
- `window`：Shares a specific window of an application.

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

### <a name = "both"></a>Enabling Both Screen Sharing and Video

One client only sends one stream. If you want to enable both screen sharing and video on one host, you need to create two clients; one to send the screen-sharing stream, and the other to send the video stream.

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

If two clients of a host subscribe to each other, extra charges will occur:

<img alt="../_images/screensharing_streams.png" src="https://web-cdn.agora.io/docs-files/en/screensharing_streams.png" style="width: 500px; "/>

Agora recommends that you to save the returned `uid` when each client joins the channel. When the `stream-added` event occurs, first check if the joined client is a local stream, if `yes`, do not subscribe to the client.

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

### Sample Code

```javascript
var key = “********************************”;
var channel = “screen_video”;
var channelKey = null;

AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);

var localStreams = [];

var screenClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
screenClient.init(key, function() {
screenClient.join(channelKey, channel, null, function(uid) {
// Save the uid of the local stream.
localStreams.push(uid);
// Create the stream for screen sharing.
screenStream = AgoraRTC.createStream({
streamID: uid,
audio: false, // Set the audio attribute as false to avoid any echo during the call.
video: false,
screen: true,
// Google Chrome:
extensionId: 'minllpmhdgpndnkomcoccfekfegnlikg',
// Firefox:
mediaSource: 'window' // 'screen', 'application', 'window'
});
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
if(!localStreams.includes(uid)) {
  console.log('subscribe stream:' + uid);
  // Subscribe to the stream.
  screenClient.subscribe(stream);
  }
  })

}, function (err) {
  console.log(err);
});

}, function (err) {
  console.log(err);
})
});

var videoClient = AgoraRTC.createClient({mode: 'rtc', codec: 'vp8'});
videoClient.init(key, function() {
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
videoClient.on('stream-added', function(evt){
var stream = evt.stream;
var uid = stream.getId();
// Check if the stream is a local uid.
if(!localStreams.includes(uid)) {
console.log('subscribe stream:' + uid);
// Subscribe to the stream.
screenClient.subscribe(stream);
}
})
}, function (err) {
  console.log(err);
  });

  }, function (err) {
  console.log(err);
  })
});
```

## Considerations

- Do not set the UID of the screen sharing stream to a fixed value. Streams with the same UID can interfere with each other.
- **Do not subscribe to a locally published screen sharing stream**, or additional charge will occur.
- Ensure that the argument `video`/`audio` is set to `false` when creating the screen sharing stream.

## Working Principles

Screen sharing on the Web client is essentially enabled by creating a screen sharing stream.

- If you publish the screen sharing stream only, set the `video` argument as false, and the `screen` argument as true when creating a stream.
- If you publish both the local video stream and your screen sharing stream, you need to create two Client objects, one for sending the local stream, and the other the screen sharing stream. For the local video stream, set `video` as true and `screen` as false; for the screen sharing scree, set `video` as false and `screen` as true.
