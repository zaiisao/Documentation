
---
title: Agora Web SDK API
description: 
platform: Web
updatedAt: Tue Sep 25 2018 12:53:29 GMT+0800 (CST)
---
# Agora Web SDK API
# Agora Web SDK API

The Agora Web SDK \(Web SDK\), includes the following classes:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>AgoraRTC</td>
<td>The object that creates client(s) and stream objects.</td>
</tr>
<tr><td>Client</td>
<td>The web client objects that provide access to the core AgoraRTC functionality.</td>
</tr>
<tr><td>Stream</td>
<td>The local or remote media stream in a session.</td>
</tr>
</tbody>
</table>



## AgoraRTC Methods

You can use the AgoraRTC client to create client\(s\) and stream objects.

### Check the Web Browser Compatibility \(checkSystemRequirements\)

```
checkSystemRequirements()
```

This method checks the compatibility between the Web SDK and the current web browser.

**Note:** 

Use this method before calling createClient to check the compatibility between the system and the web browser.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>Return value</td>
<td>Boolean</td>
<td><ul>
<li>true: The Web SDK is compatible with the current web browser.</li>
<li>false: The Web SDK is not compatible with the current web browser.</li>
</ul>
</td>
</tr>
</tbody>
</table>



**Note:** 

Agora has yet to conduct comprehensive tests on Chromium kernel browsers, such as QQ and 360. Agora will gradually achieve compatibility on most mainstream browsers in subsequent versions of the Web SDK.

### Create a Client Object \(createClient\)

```
createClient(<config>)
```

This method creates and returns a client object. You can only call this method once each call session.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>config</td>
<td>Object</td>
<td><p>The object contains the following properties:</p>
<ul>
<li>mode: The channel mode. <ul>
<li>live: Sets the channel mode as live broadcast (interop with the Native SDK).</li>
<li>rtc: Sets the channel mode as communication (non-interop with the Native SDK).</li>
</ul>
</li>
<li>codec: The codec the Web browser uses for encoding and decoding. <ul>
<li>vp8: Sets the browser to use VP8 for encoding and decoding</li>
<li>h264: Sets the browser to use H264 for encoding and decoding</li>
</ul>
</li>
<li>proxyServer: (Optional) Enterprise users with a company firewall can use this parameter to pass signaling messages to the Agora SD-RTN through the Nginx Server. See <a href="../../en/Voice/live_video_web.html.md"><span>Set the Proxy Server</span></a> for the sample code</li>
<li>turnServer: (Optional) Enterprise users with a company firewall can use this parameter to pass voice and video data to the Agora SD-RTN through the TURN Server. See <a href="../../en/Voice/live_video_web.html.md"><span>Set the Proxy Server</span></a> for the sample code</li>
</ul>
</td>
</tr>
</tbody>
</table>

For users who use the Web SDKs earlier than v2.3, please refer to the compatibility relationship between the old and new versions :

- `createClient({mode: "interop"})`= `createClient({mode: "live", codec: "vp8"})`
- `createClient({mode: "h264_interop"})` = `createClient({mode: "live", codec: "h264"})`

**Note:** 

- Ensure that you do not leave the `mode` parameter as empty.
- Set the `codec` parameter as `h264` as long as Safari is involved in the session.
- Set the `mode` parameter as `live` if you want to communicate with the Native SDK.
- If you set the mode parameter as `rtc`, the Agora Recording SDK will not be supported.
- Call `enableWebSdkInteroperability` at the Native SDK in a live broadcast if you set the mode parameter as `live`.

#### Set the Proxy Server

```
var config = {
  mode: 'live',
  codec: 'vp8',
  proxyServer: 'YOUR NGINX PROXY SERVER IP',
  turnServer: {
      turnServerURL: 'YOUR TURNSERVER URL',
      username: 'YOUR USERNAME',
      password: 'YOUR PASSWORD',
      udpport: 'THE UDP PORT YOU WANT TO ADD',
      tcpport: 'THE TCP PORT YOU WANT TO ADD',
      forceturn: false
  }
}
var client = AgoraRTC.createClient(config);
```

**Note:** 

- Proxy services by different service providers may result in slow performance if you are using the Firefox browser. Therefore, Agora recommends using the same service provider for the proxy services. If you use different service providers, Agora recommends not using the Firefox browser.
- Ensure that you set the parameters before calling [Join an AgoraRTC Channel \(join\)](../../en/API%20Reference/live_video_web.md) .

For a tutorial on deploying the Proxy server on a Web client, see [Advanced: Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy_web.md) .

### Enumerate the Platform Devices \(getDevices\)

```
var getDevices = function(onSuccess, onFailure)
```

This method enumerates the platform camera or microphone devices.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>Method call succeeded</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>Method call failed</td>
</tr>
</tbody>
</table>



If this method succeeds, the SDK returns the microphone and camera object, and each object contains the following attributes:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Attribute</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>deviceId</td>
<td>String</td>
<td>Unique ID of the device.</td>
</tr>
<tr><td>label</td>
<td>String</td>
<td>Device name. If the user has not granted access to the camera and microphone, label will be set as an empty string.</td>
</tr>
<tr><td>kind</td>
<td>String</td>
<td>Device type: <em>audioInput</em> or <em>videoInput</em>.</td>
</tr>
</tbody>
</table>



Sample Code

```
AgoraRTC.getDevices (function(devices) {
var devCount = devices.length;

var id = devices[0].deviceId;
});
```

### Create a Stream Object \(createStream\)

```
createStream(<spec>)
```

This method creates and returns a stream object.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>spec</td>
<td>Object</td>
<td><p>This object contains the following properties:</p>
<ul>
<li>streamID: Number type. The stream ID; normally you can set the stream ID as the uid, which can be retrieved from the client.join callback.</li>
<li>audio: Boolean type; marks whether this stream contains an audio track.</li>
<li>video: Boolean type; marks whether this stream contains a video track. <a href="#f1" id="id1">[1]</a></li>
<li>screen: Boolean type; marks whether this stream contains a screen sharing track. <a href="#f1" id="id2">[1]</a></li>
<li>audioSource: Object type, specifies the audio source of the stream.</li>
<li>videoSource: Object type, specifies the video source of the stream.</li>
<li>mirror: Boolean type; marks whether the video image of the publisher is mirrored on the publisher’s webpage. The default value is <em>true</em> (except in the screen-share mode). Agora recommends enabling this function when using the front camera, and disabling it when using the rear camera.</li>
<li>mediaSource: (optional) If you are using the Firefox browser, setting this property specifies the screen-sharing mode. See <a href="../../en/Voice/live_video_web.html.md"><span>Enable Screen-sharing on the Firefox Web Browser</span></a> for the detailed method. This property contains the following parameters:<ul>
<li>windows: Share a specified window of an app</li>
<li>screen: Share the current screen</li>
<li>application: Share all windows of an app</li>
</ul>
</li>
<li>extensionId: String type. ID obtained after installing a Google Chrome extension. For details, see <a href="../../en/Quickstart%20Guide/chrome_screensharing_plugin.html.md"><span>3. Implement the extension and get the extension ID</span></a>. Contact <a href="mailto:sales-us%40agora.io">sales-us<span>@</span>agora<span>.</span>io</a>. for the source code of the extension library.</li>
<li>audioProcessing: Boolean type, marks whether to enable audio processing. This property contains the following parameters:<ul>
<li>AGC: Boolean type; marks whether to enable audio gain control. The default value is <em>true</em> (enable). If you wish not to enable the audio gain control, set <code><span>AGC</span></code> as <em>false</em>, that is, <code><span>audioProcessing:</span> <span>{AGC:</span> <span>false}</span></code></li>
<li>AEC: Boolean type; marks whether to enable acoustic echo cancellation. The default value is <em>true</em> (enable). If you wish not to enable the acoustic echo cancellation, set <code><span>AEC</span></code> as <em>false</em>, that is, <code><span>audioProcessing:</span> <span>{AEC:</span> <span>false}</span></code></li>
<li>ANS: Boolean type; marks whether to enable automatic noise suppression. The default value is <em>true</em> (enable). If you wish not to enable the automatic noise suppression, set <code><span>ANS</span></code> as <em>false</em>, that is, <code><span>audioProcessing:</span> <span>{ANS:</span> <span>false}</span></code></li>
</ul>
</li>
<li>attributes: (Optional) This method contains the following attributes:<ul>
<li>resolution: Resolution.</li>
<li>minFrameRate: Minimum frame rate.</li>
<li>maxFrameRate: Maximum frame rate.</li>
</ul>
</li>
<li>(Optional) cameraId: The camera device ID retrieved from the getDevices method.</li>
<li>(Optional) microphoneId: The microphone device ID retrieved from the getDevices method.</li>
</ul>
</td>
</tr>
</tbody>
</table>

**Note:** Do not set video and screen as *true* at the same time.

**Sample Code**

#### Create a Stream

You have two options to create an audio/video stream:

##### Set the audio, video, and screen properties

```
var stream = AgoraRTC.createStream({
  streamID: uid,
  audio:true,
  video:true,
  screen:false
});
```

##### Set the audioSource and videoSource properties

Compared with the first option, the audioSource and videoSource properties can specify the audio and video tracks for the stream. Use this option if you need to process the audio and video before creating the stream.

Use the mediaStream method to get the audio and video tracks from MediaStreamTrack, and then set audioSource and videoSource:

```
navigator.mediaDevices.getUserMedia(
    {video: true, audio: true}
).then(function(mediaStream){
    var videoSource = mediaStream.getVideoTracks()[0];
    var audioSource = mediaStream.getAudioTracks()[0];
    // After processing videoSource and audioSource
    var localStream = AgoraRTC.createStream({
        videoSource: videoSource,
        audioSource: audioSource
    });
    localStream.init(function(){
        client.publish(localStream, function(e){
            //...
        });
    });
});
```

**Note:** 

- MediaStreamTrack refers to the MediaStreamTrack object supported by the browser. See [MediaStreamTrack API](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack)
- Currently this option only supports the Chrome brower.

#### Enable Screen-sharing on the Chrome Web Browser

```
var stream = AgoraRTC.createStream({
  streamID: uid,
  audio:false,
  video:false,
  screen:true,
  extensionId:"minllpmhdgpndnkomcoccfekfegnlikg"});
```

#### Enable Screen-sharing on the Firefox Web Browser

```
localStream = AgoraRTC.createStream({
     streamID: uid,
     audio: false,
     video: false,
     screen: true,
     mediaSource: 'screen',
   });
```

For a tutorial on screen-sharing on a website, see [Advanced: Screen Sharing on the Web](../../en/Quickstart%20Guide/screensharing_web.md).

**Note:** 

- Do not set video and screen as *true* at the same time.
- To enable screen-sharing on the Firefox browser, ensure that the screen parameter is set to *true*, and the mediaSource parameter has been set to specify a certain sharing mode.

### Enable Log Upload \(Logger.enableLogUpload\)

Call this method to enable log upload to Agora’s server.

The log-upload function is disabled by default, if you need to enable this function, please call this method before all the other methods.

**Note:** 

If the user fails to join the channel, the log information will not be available on Agora’s server.

**Sample Code**

```
AgoraRTC.Logger.enableLogUpload();
```

### Disable Log Upload \(Logger.disableLogUpload\)

This method disables log upload.

By default, the log-uplaod function is disabled. If you have used [Enable Log Upload \(Logger.enableLogUpload\)](#), call this method when you need to stop uploading the log.

**Sample Code**

```
AgoraRTC.Logger.disableLogUpload();
```

### Set the Log Level \(setLogLevel\)

```
AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.(logLevel));
```

This method sets the output log level.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>logLevel</td>
<td>Number</td>
<td><p>The log filter level set by the developer:</p>
<blockquote>
<div><ul>
<li>logLevel = NONE: Output no log.</li>
<li>logLevel = ERROR: Output logs of the ERROR level.</li>
<li>logLevel = WARN: Output logs of the WARN and ERROR levels.</li>
<li>logLevel = INFO: Output logs of the WARN and ERROR levels, together with other relative information.</li>
<li>logLevel = DEBUG: Output all API logs.</li>
</ul>
</div></blockquote>
</td>
</tr>
</tbody>
</table>



This method sets the output log level of the SDK. The log level follows the sequence of *NONE*, *ERROR*, *WARNING*, *INFO*, and *DEBUG*. Choose a level, and you can see logs that precede that level.

For example, if you set the log level as AgoraRTC.Logger.setLogLevel\(AgoraRTC.Logger.INFO\);, then you can see logs in levels *ERROR* and *WARNING*.

## Client Methods and Callbacks

The web client object that provides access to the core AgoraRTC functionality.

### Client Methods

#### Initialize a Client Object \(init\)

```
init(<appId>, <onSuccess>, <onFailure>)
```

This method initializes the client object.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>appId</td>
<td>String</td>
<td>App ID for your project. To get your App ID, see <a href="../../en/Agora%20Platform/key_web.html.md"><span>Get an App ID</span></a>.</td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The returned callback when the method succeeds.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>

Sample Code

```
client.init(appId, function() {
    console.log("client initialized");
    // Join a channel
    //……
}, function(err) {
    console.log("client init failed ", err);
    // Error handling
});
```

#### Join an AgoraRTC Channel \(join\)

```
join(<token/channelKey>, <channel>, <uid>, <onSuccess>, <onFailure>)
```

This method joins an AgoraRTC channel.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>token/channelKey</td>
<td>String</td>
<td><ul>
<li>Low security requirements: Pass <em>null</em> as the parameter value.</li>
<li>High security requirements: Pass the string of the Token or Channel Key as the parameter value. See <a href="../../en/Agora%20Platform/token.html.md"><span>Security Keys</span></a> for details.</li>
</ul>
</td>
</tr>
<tr><td>channel</td>
<td>String</td>
<td>A string that provides a unique channel name for the Agora session.</td>
</tr>
<tr><td>uid</td>
<td>Integer</td>
<td>The user ID: Integer. Ensure that this integer is unique. If you set the integer to <em>null</em>, the server assigns one and returns it in the onSuccess callback.</td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The returned callback when the method succeeds. The server returns the uid which represents the identity of the user.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>



Sample Code

```
client.join(<token>, '1024', null, function(uid) {
    console.log("client" + uid + "joined channel");
    // Create a local stream
    //……
}, function(err) {
    console.error("client join failed ", err);
    // Error handling
});
```

#### Leave an AgoraRTC channel \(leave\)

```
leave(<onSuccess>, <onFailure>)
```

This method enables a user to leave a channel.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The returned callback when the method succeeds.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>



Sample Code

```
client.leave(function() {
    console.log("client leaves channel");
    //……
}, function(err) {
    console.log("client leave failed ", err);
    //error handling
});
```

#### Publish a local stream \(publish\)

```
publish(<stream>, <onFailure>)
```

This method publishes a local stream to the SD-RTN.

**Note:** 

In a live broadcast, whoever calls this API is the host.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>Stream object, which represents the local stream.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>

Sample Code

```
client.publish(stream, function(err) {
    console.log("stream published");
    //……
})
```

#### Unpublish the Local Stream \(unpublish\)

```
unpublish(<stream>, <onFailure>)
```

This method unpublishes the local stream.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>Stream object, which represents the local stream.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>

Sample Code

```
client.unpublish(stream, function(err) {
    console.log("stream unpublished");
    //……
})
```

#### Subscribe to a Remote Stream \(subscribe\)

```
subscribe(<stream>, <onFailure>)
```

This method enables a user to subscribe to a remote stream.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>Stream object, which represents the remote stream.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The function to be called when the method fails.</td>
</tr>
</tbody>
</table>



Sample Code

```
client.subscribe(stream, function(err) {
    console.error("stream subscribe failed", err);
    //……
})
```

#### Unsubscribe from the Remote Stream \(unsubscribe\)

```
unsubscribe(stream, onFailure)
```

This method enables the user to unsubscribe from the remote stream.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>Stream object, which represents the remote stream.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>

Sample Code

```
client.unsubscribe(stream, function(err) {
    console.log("stream unsubscribed");
    //……
});

```

#### Enable Dual Stream \(enableDualStream\)

```
enableDualStream(<onSuccess>, <onFailure>)
```

This method enables the dual-stream mode on the publisher side.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The returned callback when the method succeeds.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>



Sample code

```
client.enableDualStream(function() {
  console.log('Enable dual stream success!')
}, function(err) {
  console,log(err)
})
```

**Note:** 

This method does not apply to the following scenarios:

- Audio-only mode \(audio: true, video: false\)
- Safari browsers
- Screen-sharing scenario

#### Set the Low-video Stream Parameter \(setLowStreamParameter\)

```
client.setLowStreamParameter({
  width: 320,
  height: 240,
  framerate: 15,
  bitrate: 200,
})
```

If you enable the dual-stream mode, this method sets the low-video stream profile.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>width</td>
<td>(optional) Width of the low-video stream frame. The <em>width</em> and <em>height</em> parameters are bound together, and take effect only when both are set. Otherwise, the SDK will assign default values.</td>
</tr>
<tr><td>height</td>
<td>(optional) Height of the low-video stream frame. The <em>width</em> and <em>height</em> parameters are bound together, and take effect only when both are set. Otherwise, the SDK will assign default values.</td>
</tr>
<tr><td>framerate</td>
<td>(optional) Frame rate of the low-video stream frame. If this parameter is not set, the SDK will assign a default value.</td>
</tr>
<tr><td>bitrate</td>
<td>(optional) Bitrate of the low-video stream frame. If this parameter is not set, the SDK will assign a default value.</td>
</tr>
</tbody>
</table>



**Note:** 

- As different web browsers have different restrictions on the video profile, the parameters you set may fail to take effect. The Firefox browser has a fixed frame rate of 30 fps, therefore the frame rate settings do not work on the Firefox browser.
- Due to limitations of some devices and browsers, the resolution you set may fail to take effect and get adjusted by the browser. In this case, billings will be calculated based on the actual resolution.
- Call [Join an AgoraRTC Channel \(join\)](../../en/API%20Reference/live_video_web.md) before using this method.
- Screen sharing supports the high-video stream only.

#### Set the Remote Video-stream Type \(setRemoteVideoStreamType\)

```
setRemoteVideoStreamType(<stream>, <streamType>)
```

When a remote user sends dual streams \(a hybrid of a high-video stream and a low-video stream\), this method decides on which stream to receive on the subscriber side. If this method is not used, the subscriber receives the high-video stream.

**Note:** 

As not all web browsers are compatible with dual streams, Agora does not recommend developers setting the resolution of the low-video stream.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>The remote video stream object.</td>
</tr>
<tr><td>streamType</td>
<td>Number</td>
<td><p>Sets the remote video stream type.</p>
<p>The following lists the video-stream types:</p>
<blockquote>
<div><ul>
<li>0: High-video stream</li>
<li>1: Low-video stream</li>
</ul>
</div></blockquote>
</td>
</tr>
</tbody>
</table>

Sample Code

```
switchStream = function (){
  if (highOrLow === 0) {
    highOrLow = 1
    console.log("Set to low");
  }
  else {
    highOrLow = 0
    console.log("Set to high");
  }

  client.setRemoteVideoStreamType(stream, hightOrLow);
}
```

Some web browsers may not be fully compatible with dual streams:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Browser</strong></td>
<td><strong>Possible problem</strong></td>
</tr>
<tr><td>Safari on MacOS</td>
<td>A high-video stream and a low-video stream share the same frame rate and resolution.</td>
</tr>
<tr><td>Firefox</td>
<td>A low-video stream has a fixed frame rate of 30 fps.</td>
</tr>
<tr><td>Safari on iOS</td>
<td>Safari 11 does not support switching between the two video streams.</td>
</tr>
</tbody>
</table>

#### Set Stream Fallback Option \(setStreamFallbackOption\)

Use this method to set stream fallback option on the receiver. Under poor network conditions, the SDK can choose to subscribe to the low-video stream or only the audio stream.

**Note:** 

This method can only be used when the publisher has enabled the dual-stream mode.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>stream</td>
<td>Object</td>
<td>The remote stream</td>
</tr>
<tr><td>fallbackType</td>
<td>Number</td>
<td>The fallback option:
<ul>
<li>0: Disable the fallback.</li>
<li>1: Automatically subscribe to the low-video stream under poor network.</li>
<li>2: Automatically subscribe to the low-video stream or only the audio stream under poor network.</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Disable Dual Streams \(disableDualStream\)

```
disableDualStream(<onSuccess>, <onFailure>)
```

This method disables dual streams.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The method call succeeds.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The method call fails.</td>
</tr>
</tbody>
</table>

Sample Code

```
client.disableDualStream(function() {
  console.log('Disable dual stream success!')
}, function(err) {
  console.log(err)
})
```

#### Enable Volume Indicator (enableAudioVolumeIndicator)

```
enableAudioVolumeIndicator(interval)
```

This method enables the SDK to report the active speaker and his/her volume regularly.

If this method is enabled, the SDK will return the volume indication in Volume Indicator (volume-indicator) every two seconds, regardless of whether there are active speakers.

Note

- Agora recommends enabling DTX in AgoraRTC.createStream at the same time .
- If you have mutiple web pages running the Web SDK, this function might not work.

Sample Code

```javascript
client.enableAudioVolumeIndicator(); // Triggers the "volume-indicator" callback event every two seconds.
client.on("volume-indicator", function(evt){
    evt.attr.forEach(function(volume, index){
            console.log(#{index} UID ${volume.uid} Level ${volume.level});
    });
});
```

#### Configure Push Stream \(configPublisher\)

```
configPublisher(<width>, <height>, <framerate>, <bitrate>, <publisherUrl>)
```

This method configures the push-stream settings for the engine before joining the channel for CDN Live.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>width</td>
<td>Integer</td>
<td>Width of the output data stream set for CDN Live, <em>360</em> is the default value.</td>
</tr>
<tr><td>height</td>
<td>Integer</td>
<td>Height of the output data stream set for CDN Live, <em>640</em> is the default value.</td>
</tr>
<tr><td>framerate</td>
<td>Integer</td>
<td>Frame rate of the output data stream set for CDN Live, <em>15</em> fps is the default value.</td>
</tr>
<tr><td>bitrate</td>
<td>Integer</td>
<td>Bitrate of the output data stream set for CDN Live, <em>500</em> kbps is the default value.</td>
</tr>
<tr><td>publisherUrl</td>
<td>String</td>
<td>The push-stream address for the picture-in-picture layouts, <em>null</em> is the default value.</td>
</tr>
</tbody>
</table>



Sample Code

```
client.configPublisher({
  width: width,
  height: height,
  framerate: framerate,
  bitrate: bitrate,
  publishUrl: "rtmp://xxx/xxx/"
});
```

#### Start a Live Stream \(startLiveStreaming\)

```
startLiveStreaming(<url>, <enableTranscoding>);
```

This method starts a live stream.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>url</td>
<td>String</td>
<td>URL address for the live stream.</td>
</tr>
<tr><td>enableTranscoding</td>
<td>Boolean</td>
<td>(Optional) Set whether to enable live transcoding.</td>
</tr>
</tbody>
</table>



Sample Code

```
client.setLiveTranscoding(<coding>);
//if enableTranscoding is set to true, setLiveTranscoding must be called before startLiveStreaming
client.startLiveStreaming(<url>, true)
```

#### Set Live Transcoding \(setLiveTranscoding\)

```
setLiveTranscoding(<coding>);
```

This method sets live transcoding.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>coding</td>
<td>Object</td>
<td>Transcoding settings</td>
</tr>
</tbody>
</table>



Sample Code

```
var LiveTranscoding = {
  width: 640,
  height: 360,
  videoBitrate: 400,
  videoFramerate: 15,
  lowLatency: false,
  audioSampleRate: AgoraRTC.AUDIO_SAMPLE_RATE_48000,
  audioBitrate: 48,
  audioChannels: 1,
  videoGop: 30,
  videoCodecProfile: AgoraRTC.VIDEO_CODEC_PROFILE_HIGH,
  userCount: 0,
  userConfigExtraInfo: {},
  backgroundColor: 0x000000,
  transcodingUsers: [],
};
```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Attribute</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>width</td>
<td>Integer</td>
<td>Width of the canvas; default value is 640</td>
</tr>
<tr><td>height</td>
<td>Integer</td>
<td>Height of the canvas; default value is 360</td>
</tr>
<tr><td>videoBitrate</td>
<td>Integer</td>
<td>Bitrate of the video stream; default value is 400 kbit/s</td>
</tr>
<tr><td>videoFramerate</td>
<td>Integer</td>
<td>Frame rate of the video stream; default value is 15 fps</td>
</tr>
<tr><td>lowLatency</td>
<td>Boolean</td>
<td>Low latency; default value is false</td>
</tr>
<tr><td>audioSampleRate</td>
<td>Integer</td>
<td><p>Audio sampling rate; default value is 48 kHz</p>
<blockquote>
<div><ul>
<li>AgoraRTC.AUDIO_SAMPLE_RATE_32000 (32 kHz)</li>
<li>AgoraRTC.AUDIO_SAMPLE_RATE_44100 (44.1 kHz)</li>
<li>AgoraRTC.AUDIO_SAMPLE_RATE_48000 (48 kHz)</li>
</ul>
</div></blockquote>
</td>
</tr>
<tr><td>audioBitrate</td>
<td>Integer</td>
<td>Bitrate of the audio; default value is 48</td>
</tr>
<tr><td>audioChannels</td>
<td>Integer</td>
<td>Number of the audio channel; default value is 1</td>
</tr>
<tr><td>videoGop</td>
<td>Integer</td>
<td>Video GOP; default value is 30</td>
</tr>
<tr><td>videoCodecProfile</td>
<td>Integer</td>
<td><p>Codec profile of the video; default value is high profile (100)</p>
<blockquote>
<div><ul>
<li>AgoraRTC.VIDEO_CODEC_PROFILE_BASELINE</li>
<li>AgoraRTC.VIDEO_CODEC_PROFILE_MAIN</li>
<li>AgoraRTC.VIDEO_CODEC_PROFILE_HIGH</li>
</ul>
</div></blockquote>
</td>
</tr>
<tr><td>userCount</td>
<td>Integer</td>
<td>Number of users; default value is 0</td>
</tr>
<tr><td>userConfigExtraInfo</td>
<td>Object</td>
<td>Preset configuration: extra information of the user</td>
</tr>
<tr><td>backgroundColor</td>
<td>Integer</td>
<td>Background color; default is 0x000000</td>
</tr>
<tr><td>transcodingUsers</td>
<td>Array</td>
<td>Transcoding settings related to users. See the following table for details.</td>
</tr>
</tbody>
</table>



**Definition of Transcoding Users**

```
var TranscodingUser =
{ uid: 0, x: 0, y: 0, width: 0, height: 0, zOrder: 0, alpha: 1.0, };

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Attribute</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>Integer</td>
<td>User ID of the CDN host</td>
</tr>
<tr><td>x</td>
<td>Integer</td>
<td>The position of the upper left end of the video on the horizontal axis</td>
</tr>
<tr><td>y</td>
<td>Integer</td>
<td>The position of the upper left end of the video on the vertical axis</td>
</tr>
<tr><td>width</td>
<td>Integer</td>
<td>Width of the video</td>
</tr>
<tr><td>height</td>
<td>Integer</td>
<td>Height of the vide</td>
</tr>
<tr><td>zOrder</td>
<td>Integer</td>
<td>Layer of the video</td>
</tr>
<tr><td>alpha</td>
<td>Double</td>
<td>Transparency of the video. The default value is 1.0.</td>
</tr>
</tbody>
</table>



#### Stop Live Streaming \(stopLiveStreaming\)

```
stopLiveStreaming(<url>);

```

This method stops and deletes the live streaming.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>url</td>
<td>String</td>
<td>URL address of the live streaming</td>
</tr>
</tbody>
</table>



**Note:** 

- To use the functions of Live Broadcast 1.0, use the configPublisher method after createStream.
- To use the functions of Live Broadcast 2.0, use the startLiveStreaming and setLiveTranscoding methods after createStream. For details, see [Advanced: Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_web.md).

#### Deploy the Nginx Server \(setProxyServer\)

```
client.setProxyServer(<domainName>);

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>domainName</td>
<td>String</td>
<td>Your Nginx server domain name</td>
</tr>
</tbody>
</table>



**Note:** 

- Please ensure that you call this API before [Join an AgoraRTC Channel \(join\)](../../en/API%20Reference/live_video_web.md) .
- Proxy services by different service providers may result in slow performance if you are using the Firefox browser. Therefore, Agora recommends using the same service provider for the proxy services. If you use different service providers, Agora recommends not using the Firefox browser.

#### Deploy the TURN Server \(setTurnServer\)

```
client.setTurnServer(<config>);

```

**Note:** 

Please ensure that you call this API before [Join an AgoraRTC Channel \(join\)](../../en/API%20Reference/live_video_web.md).

#### Enable Built-in Encryption \(setEncryptionSecret\)

```
client.setEncryptionSecret(<secret>);

```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>secret</td>
<td>String</td>
<td>The encryption password</td>
</tr>
</tbody>
</table>

**Note:** 

Please ensure that you call this API before [Join an AgoraRTC Channel \(join\)](../../en/API%20Reference/live_video_web.md).

#### Set the Encryption Mode \(setEncryptionMode\)

```
client.setEncryptionMode(<encryptionMode>);

```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>encryptionMode</td>
<td><p>The encryption mode:</p>
<ul>
<li>aes-128-xts: Sets the encrption mode as AES128XTS</li>
<li>aes-256-xts: Sets the encrption mode as AES256XTS</li>
<li>aes-128-ecb: Sets the encrption mode as AES128ECB</li>
</ul>
</td>
</tr>
</tbody>
</table>



**Note:** 

Please ensure that you call this API before [Join an AgoraRTC Channel \(join\)](../../en/API%20Reference/live_video_web.md) .

#### Renew the Token \(renewToken\)

```
renewToken(<token>)

```

This method renews your token.

Once the Token schema is enabled, the token expires after a certain period of time. In case of the onTokenPrivilegeWillExpire or onTokenPrivilegeDidExpire callback events, the application should renew the Token by calling this method. Not doing so will result in SDK disconnecting with the server.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>token</td>
<td>String</td>
<td>Specifies the renewed Token.</td>
</tr>
</tbody>
</table>



#### Renew the Channel Key \(renewChannelKey\)

```
renewChannelKey(channelKey, onSuccess, onFailure)

```

This method renews your channel key.

Once the Channel Key schema is enabled, the key expires after a certain period of time. When the *onFailure* callback reports the error *DYNAMIC\_KEY\_TIMEOUT*, the application should renew the Channel Key by calling this method. Not doing so will result in SDK disconnecting with the server.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>channelKey</td>
<td>String</td>
<td>Specifies the renewed Channel Key.</td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The returned callback when the method succeeds.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>

### Callback Events

#### Microphone/Camera Permission Granted \(accessAllowed/accessDenied\)

This callback notifies the application that the user has given access to the camera and microphone.

Sample Code

```
// The user has granted access to the camera and microphone.
localStream.on("accessAllowed", function(){
   console.log("accessAllowed");
})

// The user has denied access to the camera and microphone.
localStream.on("accessDenied", function(){
   console.log("accessDenied");
})

```

#### Local Stream Published \(stream-published\)

This callback notifies the application that the local stream has been published.

Sample Code

```
client.on('stream-published', function(evt) {
    console.log("local stream published");
    //……
})

```

#### Remote Stream Added \(stream-added\)

This callback notifies the application that the remote stream has been added.

Sample Code

```
client.on('stream-added', function(evt) {
    var stream = evt.stream;
    console.log("new stream added ", stream.getId());
    // Subscribe the stream.
    //……
})

```

#### Remote Stream Removed \(stream-removed\)

This callback notifies the application that the remote stream has been removed; such as, a peer user has called *unpublishstream*.

Sample Code

```
client.on('stream-removed', function(evt) {
    var stream = evt.stream;
    console.log("remote stream was removed", stream.getId());
    //……
});

```

#### Remote Stream Subscribed \(stream-subscribed\)

This callback notifies the application that a user has subscribed to a remote stream.

Sample Code

```
client.on('stream-subscribed', function(evt) {
    var stream = evt.stream;
    console.log("new stream subscribed ", stream.getId());
    // Play the stream.
    //……
})

```

#### Peer User Left the Channel Event \(peer-leave\)

This callback notifies the application that the peer user has left the channel; such as, the peer user has called *client.leave\(\)*.

Sample Code

```
client.on('peer-leave', function(evt) {
    var uid = evt.uid;
    console.log("remote user left ", uid);
    //……
});

```

#### Peer User Enabled Audio Mute \(mute-audio\)

This callback notifies the application that the peer user has muted the audio.

Sample Code

```
client.on('mute-audio', function(evt) {
  var uid = evt.uid;
  console.log("mute audio:" + uid);
  //alert("mute audio:" + uid);
});

```

#### Peer User Enabled Audio Unmute \(unmute-audio\)

This callback notifies the application that the peer user has unmuted the audio.

Sample Code

```
client.on('unmute-audio', function (evt) {
  var uid = evt.uid;
  console.log("unmute audio:" + uid);
});

```

#### PPeer User Turned Video Off \(mute-video\)

This callback notifies the application that the peer user has turned off the video.

Sample Code

```
client.on('mute-video', function (evt) {
  var uid = evt.uid;
  console.log("mute video" + uid);
  //alert("mute video:" + uid);
})

```

#### Peer User Turned Video On \(unmute-video\)

This callback notifies the application that the peer user has turned on the video.

Sample Code

```
client.on('unmute-video', function (evt) {
  var uid = evt.uid;
  console.log("unmute video:" + uid);
})

```

#### Peer User Banned \(client-banned\)

This callback notifies the peer user that he/she has been banned from the channel before joining it. Only the banned user receives this callback.

Sample Code

```
client.on('client-banned', function (evt) {
   var uid = evt.uid;
   var attr = evt.attr;
   console.log(" user banned:" + uid + ", banntype:" + attr);
   alert(" user banned:" + uid + ", banntype:" + attr);
});

```

#### Audio Volume Indication \(active-speaker\)

This callback notifies the application who is the active speaker in the channel.

Sample Code

```
client.on('active-speaker', function(evt) {
     var uid = evt.uid;
     console.log("update active speaker: client " + uid);
  });

```

#### Volume Indicator (volume-indicator)

This callback notifies the application of all the speaking users and their volumes. It is disabled by default.

You can enable this event by calling Enable Volume Indicator (enableAudioVolumeIndicator) . If enabled, it reports the volumes regularly regardless of whether there are users speaking.

The volume is an integer ranging from 0 to 100. Usually a user with volume above five will be counted as a speaking user.

Sample Code

```javascript
client.on("volume-indicator", function(evt){
    evt.attr.forEach(function(volume, index){
    console.log(`#{index} UID ${volume.uid} Level ${volume.level}`);
  });
});
```

#### Live Streaming Started \(liveStreamingStarted\)

```
client.on('liveStreamingStarted', args);

```

This callback notifies the application that the live streaming has started.

#### Live Streaming Failed \(liveStreamingFailed\)

```
client.on('liveStreamingFailed', args);

```

This callback notifies the application that the live streaming has failed.

#### Live Streaming Stopped \(liveStreamingStopped\)

```
client.on('liveStreamingStopped', args);

```

This callback notifies the application that the live streaming has stopped.

#### Live Transcoding Updated \(liveTranscodingUpdated\)

```
client.on('liveTranscodingUpdated', args);

```

This callback notifies the application that the live streaming has updated.

#### Token Will Expire \(onTokenPrivilegeWillExpire\)

This callback notifies the application that the Token will expire in 30 seconds.

You should request a new Token from your server and call the Renew the Token \(renewToken\) method.

```
client.on('onTokenPrivilegeWillExpire', function(){
  //After requesting a new token
  client.renewToken(token);
});

```

#### Token Expired \(onTokenPrivilegeDidExpire\)

This callback notifies the application that the Token has expired.

You should request a new Token from your server and call the Renew the Token \(renewToken\) method.

```
client.on('onTokenPrivilegeDidExpire', function(){
  //After requesting a new token
  client.renewToken(token);
});

```

#### Error Messages Reported \(error\)

This callback notifies the application that an error message is reported and requires error handling.

Sample Code

```
client.on('error', function(err) {
    console.log("Got error msg:", err.reason);
  });

```

For details, see [Error Codes and Warning Codes](../../en/API%20Reference/the_error_web.md) .

## Stream Methods

### Initialize the Stream Object \(init\)

```
init(<onSuccess>, <onFailure>)

```

This method initializes the stream object.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>onSuccess</td>
<td>Function</td>
<td>(Optional) The returned callback when the method succeeds.</td>
</tr>
<tr><td>onFailure</td>
<td>Function</td>
<td>(Optional) The returned callback when the method fails.</td>
</tr>
</tbody>
</table>

If this method fails, the returned callback will show the error message, for example:
`{type: "error",  msg: "MEDIA_OPTION_INVALID"}`

The error messages include：

- MEDIA_OPTION_INVALID: The camera is occupied or the resolution is not supported (on browsers in early versions).
- DEVICES_NOT_FOUND: No device is found.
- NOT_SUPPORTED: The browser does not support using camera and microphone.
- PERMISSION_DENIED: The device is disabled by the browser or the user has denied permission of using the device.
- CONSTRAINT_NOT_SATISFIED: The settings are illegal (on browsers in early versions).
- UNDEFINED: Undefined error.

Sample Code

```
init(function() {
    console.log("local stream initialized");
    // publish the stream
    //……
}, function(err) {
    console.error("local stream init failed ", err);
    //error handling
});

```

### Play the Video/Audio Stream \(play\)

```
play(<elementID>)


```

This method plays the video or audio stream.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>elementID</td>
<td>String</td>
<td>Represents the HTML element ID.</td>
</tr>
</tbody>
</table>

Sample Code

```
stream.play('agora_remote'); // stream will be played in the element with the ID agora_remote

```

### Stop the Video/Audio Stream \(stop\)

```
stop()


```

Call this method to stop playing first to control the players set by play\(\).

### Close the Video/Audio Stream \(close\)

```
close()


```

This method stops capturing the video/audio data.

### Set the Audio Profile（setAudioProfile）

```
setAudioProfile(<profile>)


```

This method sets the video profile. It is optional and works only when called before stream.init\(\). The default value is music\_standard.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>profile</td>
<td>String</td>
<td><p>The audio profile has the following options：</p>
<blockquote>
<div><ul>
<li>speech_standard: Sample rate 32 kHz, mono, encoding rate 24 kbps</li>
<li>music_standard: Sample rate 48 kHz, mono, encoding rate 40 kbps</li>
<li>standard_stereo: Sample rate 48 kHz, stereo, encoding rate 64 kbps</li>
<li>high_quality: Sample rate 48 kHz, stereo, encoding rate 128 kbps</li>
<li>high_quality_stereo: Sample rate 48 kHz, stereo, encoding rate 192 kbps</li>
</ul>
</div></blockquote>
</td>
</tr>
</tbody>
</table>



**Note:** 

Due to the limitations of browsers, some browsers may not be fully compatible with the audio profile you set.

- Firefox does not support setting the audio encoding rate.
- Safari does not support stereo audio.
- The latest version of Google Chrome does not support playing stereo audio, but supports sending a stereo audio stream.

### Set the Video Profile \(setVideoProfile\)

```
setVideoProfile(<profile>)


```

This method sets the video profile. It is optional and works only when called before *stream.init\(\)*. The default value is *480\_1*.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>profile</td>
<td>String</td>
<td>The video profile. See the following table for its definition.</td>
</tr>
</tbody>
</table>



**Video Profile Definition**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Video profile</td>
<td>Resolution</td>
<td>Frame rate (fps)</td>
<td>Bitrate (kbit/s)</td>
</tr>
<tr><td>120P</td>
<td>160 x 120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_1</td>
<td>160 x 120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_3</td>
<td>120 x 120</td>
<td>15</td>
<td>50</td>
</tr>
<tr><td>180P</td>
<td>320 x 180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_1</td>
<td>320 x 180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_3</td>
<td>180 x 180</td>
<td>15</td>
<td>100</td>
</tr>
<tr><td>180P_4</td>
<td>240 x 180</td>
<td>15</td>
<td>120</td>
</tr>
<tr><td>240P</td>
<td>320 x 240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_1</td>
<td>320 x 240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_3</td>
<td>240 x 240</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>240P_4</td>
<td>424 x 240</td>
<td>15</td>
<td>220</td>
</tr>
<tr><td>360P</td>
<td>640 x 360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_1</td>
<td>640 x 360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_3</td>
<td>360 x 360</td>
<td>15</td>
<td>260</td>
</tr>
<tr><td>360P_4</td>
<td>640 x 360</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>360P_6</td>
<td>360 x 360</td>
<td>30</td>
<td>400</td>
</tr>
<tr><td>360P_7</td>
<td>480 x 360</td>
<td>15</td>
<td>320</td>
</tr>
<tr><td>360P_8</td>
<td>480 x 360</td>
<td>30</td>
<td>490</td>
</tr>
<tr><td>360P_9</td>
<td>640 x 360</td>
<td>15</td>
<td>800</td>
</tr>
<tr><td>360P_10</td>
<td>640 x 360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>360P_11</td>
<td>640 x 360</td>
<td>24</td>
<td>1000</td>
</tr>
<tr><td>480P</td>
<td>640 x 480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_1</td>
<td>640 x 480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_2</td>
<td>648 x 480</td>
<td>30</td>
<td>1000</td>
</tr>
<tr><td>480P_3</td>
<td>480 x 480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>640 x 480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>480 x 480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>848 x 480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>848 x 480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>480P_10</td>
<td>640 x 480</td>
<td>10</td>
<td>400</td>
</tr>
<tr><td>720P</td>
<td>1280 x 720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_1</td>
<td>1280 x 720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_2</td>
<td>1280 x 720</td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>720P_3</td>
<td>1280 x 720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>960 x 720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>960 x 720</td>
<td>30</td>
<td>1380</td>
</tr>
<tr><td>1080P</td>
<td>1920 x 1080* <a href="#f5" id="id6">[5]</a></td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_1</td>
<td>1920 x 1080* <a href="#f5" id="id7">[5]</a></td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_2</td>
<td>1920 x 1080* <a href="#f5" id="id8">[5]</a></td>
<td>30</td>
<td>3000</td>
</tr>
<tr><td>1080P_3</td>
<td>1920 x 1080* <a href="#f5" id="id9">[5]</a></td>
<td>30</td>
<td>3150</td>
</tr>
<tr><td>1080P_5</td>
<td>1920 x 1080* <a href="#f5" id="id10">[5]</a></td>
<td>60</td>
<td>4780</td>
</tr>
<tr><td>1440P</td>
<td>2560 x 1440* <a href="#f5" id="id11">[5]</a></td>
<td>30</td>
<td>4850</td>
</tr>
<tr><td>1440P_1</td>
<td>2560 x 1440* <a href="#f5" id="id12">[5]</a></td>
<td>30</td>
<td>4850</td>
</tr>
<tr><td>1440P_2</td>
<td>2560 x 1440* <a href="#f5" id="id13">[5]</a></td>
<td>60</td>
<td>7350</td>
</tr>
<tr><td>4K</td>
<td>3840 x 2160* <a href="#f5" id="id14">[5]</a></td>
<td>30</td>
<td>8910</td>
</tr>
<tr><td>4K_1</td>
<td>3840 x 2160* <a href="#f5" id="id15">[5]</a></td>
<td>30</td>
<td>8910</td>
</tr>
<tr><td>4K_3</td>
<td>3840 x 2160* <a href="#f5" id="id16">[5]</a></td>
<td>60</td>
<td>13500</td>
</tr>
</tbody>
</table>



**Note:** Whether 1080 resolution or above can be supported depends on the device. If the device cannot support 1080p, the frame rate will be lower than the one listed in the table. Agora optimizes the video in lower-end devices. For special requirements, please contact [support@agora.io](mailto:support@agora.io).

**Note:** 

Given the limitations of browsers, some browsers may not be fully compatible with the video profile you set.

*Opera browsers 52 and 53 support only the following video profiles due to their own limitations*

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Video profile</td>
<td>Resolution</td>
<td>Frame rate (fps)</td>
<td>Bitrate (kbit/s)</td>
</tr>
<tr><td>480P</td>
<td>640 x 480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_1</td>
<td>640 x 480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_2</td>
<td>648 x 480</td>
<td>30</td>
<td>1000</td>
</tr>
<tr><td>480P_3</td>
<td>480 x 480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>640 x 480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>480 x 480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>848 x 480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>848 x 480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>480P_10</td>
<td>640 x 480</td>
<td>10</td>
<td>400</td>
</tr>
<tr><td>720P</td>
<td>1280 x 720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_1</td>
<td>1280 x 720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_2</td>
<td>1280 x 720</td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>720P_3</td>
<td>1280 x 720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>960 x 720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>960 x 720</td>
<td>30</td>
<td>1380</td>
</tr>
</tbody>
</table>



*The Web Safari browser supports the following video profiles*

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Video Profile</td>
<td>Resolution (widthxheight)</td>
<td>Bitrate</td>
</tr>
<tr><td>4K</td>
<td>3840 x 2160</td>
<td>8910</td>
</tr>
<tr><td>4K_1</td>
<td>3840 x 2160</td>
<td>8910</td>
</tr>
<tr><td>4K_3</td>
<td>3840 x 2160</td>
<td>13500</td>
</tr>
<tr><td>1440P</td>
<td>2560 x 1440</td>
<td>4850</td>
</tr>
<tr><td>1440P_1</td>
<td>2560 x 1440</td>
<td>4850</td>
</tr>
<tr><td>1440P_2</td>
<td>2560 x 1440</td>
<td>7350</td>
</tr>
<tr><td>1080P</td>
<td>1920 x 1080</td>
<td>2080</td>
</tr>
<tr><td>1080P_1</td>
<td>1920 x 1080</td>
<td>2080</td>
</tr>
<tr><td>1080P_2</td>
<td>1920 x 1080</td>
<td>3000</td>
</tr>
<tr><td>1080P_3</td>
<td>1920 x 1080</td>
<td>3150</td>
</tr>
<tr><td>1080_5</td>
<td>1920 x 1080</td>
<td>4780</td>
</tr>
<tr><td>720P</td>
<td>1280 x 720</td>
<td>1130</td>
</tr>
<tr><td>720P_1</td>
<td>1280 x 720</td>
<td>1130</td>
</tr>
<tr><td>720P_2</td>
<td>1280 x 720</td>
<td>2080</td>
</tr>
<tr><td>720P_3</td>
<td>1280 x 720</td>
<td>1710</td>
</tr>
<tr><td>480P</td>
<td>640 x 480</td>
<td>500</td>
</tr>
<tr><td>480P_1</td>
<td>640 x 480</td>
<td>500</td>
</tr>
<tr><td>480P_4</td>
<td>640 x 480</td>
<td>750</td>
</tr>
</tbody>
</table>



**Note:** 

The video frame rate is 30 fps on the Safari brower, which does not support modifying the video frame rate.

Sample Code

```
setVideoProfile('480p_1');


```

### Set the Screen Profile \(setScreenProfile\)

```
setScreenProfile(<profile>)


```

This method sets the profile of the screen.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>profile</td>
<td>String</td>
<td>Strings of the screen profile. See the following table for details.</td>
</tr>
</tbody>
</table>



**Screen Profile Definition**

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Screen Profile</strong></td>
<td><strong>Resolution</strong></td>
<td><strong>Frame Rate</strong></td>
</tr>
<tr><td>480p_1</td>
<td>640 x 480</td>
<td>5</td>
</tr>
<tr><td>480p_2</td>
<td>640 x 480</td>
<td>30</td>
</tr>
<tr><td>720p_1</td>
<td>1280 x 720</td>
<td>5</td>
</tr>
<tr><td>720p_2</td>
<td>1280 x 720</td>
<td>30</td>
</tr>
<tr><td>1080p_1</td>
<td>1920 x 1080</td>
<td>5</td>
</tr>
<tr><td>1080p_2</td>
<td>1920 x 1080</td>
<td>30</td>
</tr>
</tbody>
</table>

### Retrieve the Current Audio Level \(getAudioLevel\)

```
getAudioLevel();

```

This method retrieves the current audio level. For example:

```
setInterval(function() {
  var audioLevel = stream.getAudioLevel();
  // Use audioLevel to render the UI
}, 100)

```

Call `setTimeout` or `setInterval` to retrieve the local or remote audio change.

This method does not apply to streams that contain no audio data and may result in warnings.

**Note:** 

On Chrome 90+, the first call of this method must be triggered by the users’ clicking action, see [https://goo.gl/7K7WLu](https://goo.gl/7K7WLu).

### Set the Volume (setAudioVolume)

```javascript
setAudioVolume(volume);
```

This method set the volume.

The volume parameter is number type, ranging from 0 to 100.

### Set the Audio Output (setAudioOutput)

```javas
setAudioOutput(deviceId)
```

This method sets the auido output device. You can use it to switch between the microphone and the speakerphone.

deviceId can be retrieved by `AgoraRTC.getDevices` .

Note

Only Chrome 49+ supports this function.

### Retrieve the Video Flag \(hasVideo\)

```
hasVideo()

```

This method retrieves the video flag.

### Retrieve the Audio Flag \(hasAudio\)

```
hasAudio()

```

This method retrieves the audio flag.

### Enable the Video \(enableVideo\)

```
enableVideo()

```

This method enables the video track in the stream and acts like a video restart. It works only when the video flag has been set to *true* in the stream.

### Disable the Video \(disableVideo\)

```
disableVideo()

```

This method disables the video track in the stream and acts like a video pause. It works only when the video flag has been set to *true* in stream.

### Enable the Audio \(enableAudio\)

```
enableAudio()

```

This method enables the audio track in the stream and acts like an audio restart.

### Disable the Audio \(disableAudio\)

```
disableAudio()

```

This method disables the audio track in the stream and acts like an audio pause.

### Retrieve the Stream ID \(getId\)

```
getId()


```

This method retrieves the stream ID.

### Get the Connection Status

```
getStats(function(stats){
  console.log(stats);
});


```

This method gets the connection status of the Agora SDK.

If it is a publishing stream, then the callback of this method contains the following information:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>audioSendBytes</td>
<td>String</td>
<td>Bytes of the sent audio</td>
</tr>
<tr><td>audioSendPackets</td>
<td>String</td>
<td>Packets of the sent audio</td>
</tr>
<tr><td>audioSendPacketsLost</td>
<td>String</td>
<td>Number of lost packets of the sent audio</td>
</tr>
<tr><td>videoSendBytes</td>
<td>String</td>
<td>Bytes of the sent video</td>
</tr>
<tr><td>videoSendPackets</td>
<td>String</td>
<td>Packets of the sent video</td>
</tr>
<tr><td>videoSendPacketsLost</td>
<td>String</td>
<td>Number of lost packets of the sent video</td>
</tr>
<tr><td>videoSendFrameRate</td>
<td>String</td>
<td>Frame rate of the sent video</td>
</tr>
<tr><td>videoSendResolutionWidth</td>
<td>String</td>
<td>Resolution width of the sent video</td>
</tr>
<tr><td>videoSendResolutionHeight</td>
<td>String</td>
<td>Resolution height of the sent video</td>
</tr>
<tr><td>accessDelay</td>
<td>String</td>
<td>Delay in accessing the gateway (ms)</td>
</tr>
</tbody>
</table>

If it is a subscribing stream, then the callback of this method contains the following information:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>audioReceiveBytes</td>
<td>String</td>
<td>Bytes of the received audio</td>
</tr>
<tr><td>audioReceivePackets</td>
<td>String</td>
<td>Packets of the received audio</td>
</tr>
<tr><td>audioReceivePacketsLost</td>
<td>String</td>
<td>Number of lost packets of the received audio</td>
</tr>
<tr><td>videoReceiveBytes</td>
<td>String</td>
<td>Bytes of the received video</td>
</tr>
<tr><td>videoReceivePackets</td>
<td>String</td>
<td>Packets of the received video</td>
</tr>
<tr><td>videoReceivePacketsLost</td>
<td>String</td>
<td>Number of lost packets of the received video</td>
</tr>
<tr><td>videoReceiveFrameRate</td>
<td>String</td>
<td>Frame rate rof the received video</td>
</tr>
<tr><td>videoReceiveDecodeFrameRate</td>
<td>String</td>
<td>Decode frame rate after the video has been received</td>
</tr>
<tr><td>videoReceivedResolutionWidth</td>
<td>String</td>
<td>Resolution width of the received video</td>
</tr>
<tr><td>videoReceivedResolutionHeight</td>
<td>String</td>
<td>Resolution height of the received video</td>
</tr>
<tr><td>accessDelay</td>
<td>String</td>
<td>Delay in accessing the gateway (ms)</td>
</tr>
<tr><td>endToEndDelay</td>
<td>Array</td>
<td>End to end delay (ms) <a href="#f2" id="id3">[2]</a></td>
</tr>
<tr><td>videoReceiveDelay</td>
<td>Array</td>
<td>Delay in receiving the video (ms) <a href="#f2" id="id4">[2]</a></td>
</tr>
<tr><td>audioReceiveDelay</td>
<td>Array</td>
<td>Delay in receiving the audio  (ms) <a href="#f4" id="id5">[4]</a></td>
</tr>
</tbody>
</table>



**Note:** Delay from sending to receiving data

**Note:** Delay from sending to playing the video, only supported by Chrome for now

**Note:** Delay from sending to palying the audio, only supported by Chrome for now

**Note:** 

Firefox and Safari have limitations in receiving the following callback parameters:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Web Browser</strong></td>
<td><strong>Parameters that Cannot be Received</strong></td>
</tr>
<tr><td>Firefox</td>
<td><ul>
<li>audioSendPacketsLost</li>
</ul>
</td>
</tr>
<tr><td>Safari</td>
<td><ul>
<li>audioSendPacketsLost</li>
</ul>
</td>
</tr>
</tbody>
</table>



## Error Codes and Warning Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_web.md).
