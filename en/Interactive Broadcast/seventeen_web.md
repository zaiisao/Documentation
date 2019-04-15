
---
title: Video Conference of 7+ Users
description: 
platform: Web
updatedAt: Tue Dec 11 2018 18:18:31 GMT+0800 (CST)
---
# Video Conference of 7+ Users
A video conference with too many hosts may cause network latency and packet loss. 

If we set the subscribing stream to the **1-N** Mode (set one stream as high and the rest as low), then a maximum of 17 users can join as hosts in an interactive broadcast without any network latency.

This page describes the scenarios and considerations for implementing a video conference of 7+ participants on a website using the Agora Web SDK.

## 1. User Role

In a video conference of 7+ participants, each client is set as a host (broadcaster). The video interaction is a process of joining the hosts together.

## 2. Settings Before Subscribing to a Remote Stream

If you want to subscribe to the low stream, you can set the stream type before subscribing to the stream by calling the `setRemoteVideoStreamType` method.

```javascript
setRemoteVideoStreamType(stream, streamType)
```

## 3. Setting Dual Streams

### 3.1 Setting the High-stream Profile

A 17-way video call supports all video profiles listed in the `setVideoProfile` method. However, Agora does not recommend using video profiles that exceed either a resolution of 640 x 480 or a frame rate of 15 fps.

| **Resolution** | **Frame Rate** | **Bitrate** |
| -------------- | -------------- | ------------ |
| 640 x 480      | 15 fps         | 500 Kbps     |
| 640 x 360      | 15 fps         | 400 Kbps     |
| 640 x 360      | 30 fps         | 600 Kbps     |

### 3.2 Default Low-stream Profile

Once you have enabled the dual-stream mode, you can set the high-stream profile in the web application, and the Web SDK will automatically adapt the low-stream profile accordingly.

```javascript
localStream.setVideoProfile('480P_1')
```

**See the following table for the correlation between the high stream and low stream**

| **High-stream Profile** | **Low-stream Profile** |
| ----------------------- | ---------------------- |
| 720P_5                  | 120P_1                 |
| 720P_6                  | 120P_1                 |
| 480P                    | 120P_1                 |
| 480P_1                  | 120P_1                 |
| 480P_2                  | 120P_1                 |
| 480P_4                  | 120P_1                 |
| 480P_10                 | 120P_1                 |
| 360P_7                  | 120P_1                 |
| 360P_8                  | 120P_1                 |
| 240P                    | 120P_1                 |
| 240P_1                  | 120P_1                 |
| 180P_4                  | 120P_1                 |
| 120P_3                  | 120P_1                 |
| 120P                    | 120P_1                 |
| 120P_1                  | 120P_1                 |
| 480P_3                  | 120P_3                 |
| 480P_6                  | 120P_3                 |
| 360P_3                  | 120P_3                 |
| 360P_6                  | 120P_3                 |
| 240P_3                  | 120P_3                 |
| 180P_3                  | 120P_3                 |
| 480P_8                  | 120P_4                 |
| 480P_9                  | 120P_4                 |
| 240P_4                  | 120P_4                 |
| 720P                    | 90P_1                  |
| 720P_1                  | 90P_1                  |
| 720P_2                  | 90P_1                  |
| 720P_3                  | 90P_1                  |
| 360P                    | 90P_1                  |
| 360P_1                  | 90P_1                  |
| 360P_4                  | 90P_1                  |
| 360P_9                  | 90P_1                  |
| 360P_10                 | 90P_1                  |
| 360P_11                 | 90P_1                  |
| 180P                    | 90P_1                  |
| 180P_1                  | 90P_1                  |

**Video Profile Definition**

| **Profile** | **Resolution**   | **Frame Rate** |
| ----------- | ---------------- | -------------- |
| 90P_1       | 160 x 90         | 15             |
| 120P        | 160 x 120        | 15             |
| 120P_1      | 160 x 120        | 15             |
| 120P_3      | 120 x 120        | 15             |
| 120P_4      | 212 x 120        | 15             |
| 180P        | 320 x 180        | 15             |
| 180P_1      | 320 x 180        | 15             |
| 180P_3      | 180 x 180        | 15             |
| 180P_4      | 424 x 240        | 15             |
| 240P        | 320 x 240        | 15             |
| 240P_1      | 320 x 240        | 15             |
| 240P_3      | 240 x 240        | 15             |
| 240P_4      | 424 x 240        | 15             |
| 360P        | 640 x 360        | 15             |
| 360P_1      | 640 x 360        | 15             |
| 360P_3      | 360 x 360        | 15             |
| 360P_4      | 640 x 360        | 30             |
| 360P_6      | 360 x 360        | 30             |
| 360P_7      | 480 x 360        | 15             |
| 360P_8      | 480 x 360        | 30             |
| 360P_9      | 640 x 360        | 15             |
| 360P_10     | 640 x 360        | 24             |
| 360P_11     | 640 x 360        | 24             |
| 480P        | 640 x 480        | 15             |
| 480P_1      | 640 x 480        | 15             |
| 480P_2      | 648 x 480        | 30             |
| 480P_3      | 480 x 480        | 15             |
| 480P_4      | 640 x 480        | 30             |
| 480P_6      | 480 x 480        | 30             |
| 480P_8      | 848 x 480        | 15             |
| 480P_9      | 848 x 480        | 30             |
| 480P_10     | 640 x 480        | 10             |
| 720P        | 1280 x 720       | 15             |
| 720P_1      | 1280 x 720       | 15             |
| 720P_2      | 1280 x 720       | 15             |
| 720P_3      | 1280 x 720       | 30             |
| 720P_5      | 960 x 720        | 15             |
| 720P_6      | 960 x 720        | 30             |

> Since web browsers use an internal algorithm to adjust the stream, the actual low stream may be slightly different from that shown in the table.



### 3.3 Customizing the Low-stream Profile

If you enable dual-stream mode, you can customize the low-stream video profile by using the following method:

```javascript
client.setLowStreamParamter({
  width: 120,
  height: 120,
  framerate: 15,
  bitrate: 120,
})
```

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| width         | (Optional) The width of the low-stream video frame. The `width` and `height` parameters are bound together, and therefore take effect only when both are set. Otherwise, the SDK will assign a default value. |
| height        | (Optional) The height of the low-stream video frame. The `width` and `height` parameters are bound together, and therefore take effect only when both are set. Otherwise, the SDK will assign a default value. |
| framerate     | (Optional) The frame rate of the low-stream video frame. If you do not set this parameter, the SDK will assign a default value. |
| bitrate       | (Optional) The bit rate of the low-stream video frame. If you do not set this parameter, the SDK will assign a default value. |

> - Since web browsers have different restrictions on the video profile, the parameters you set may fail to take effect. The Firefox browser has a fixed frame rate of 30 fps.
> - You need to call `client.join` before using this method.
> - Screen sharing supports the high-stream video only.

### 3.4 Aspect Ratio of the High Stream and Low Stream

The aspect ratio of the low-stream video profile is identical to that of the high-stream profile. For example:

- If a high-stream video profile \(on a PC\) is set to a resolution of 640 x 480, then the low-stream video profile must be set to a resolution with the same aspect ratio (4:3).
- If a high-stream video profile \(on a PC\) is set to a resolution of 640 x 360, then the low-stream video profile must be set to a resolution with the same aspect ratio (16:9).


### 3.5 Enabling Dual-stream Mode

Whether or not to enable dual-stream mode depends on the actual situation. If the video call has 7+ users, or your app uses dual windows (one big and one small), then Agora recommends enabling dual-stream mode. Use the following code snippet to enable dual-stream mode:

```javascript
enableDualStream();
```

### 3.6 User Interface Layout

Agora recommends using a layout with one big window and multiple small windows.

-   Use a high-stream layout for the big window.
-   Use a low-stream layout for small windows.

## 4. Releasing Memory Resources

When a user is offline, the website releases all memory resources of the video view in the format of H5.

## 5. Environment Requirements

1. Ensure that your device and web browser meet the following requirements:

   - Windows: Windows 7+
     - Google Chrome 58+ (for HTTPS only)
     - Firefox 56+ (for HTTPS only). Firefox supports only eight windows on a video call screen.

   - Mac: Safari 11+
     - Google Chrome 58+ (for HTTPS only)
     - Firefox 56+ (for HTTPS only)

   > The Web SDK supports a 17-way video call on PCs only, not on mobile devices.

2. Ensure that your Web SDK is v2.1+.

## 6. Enabling a 17-way Video Call with the Sample Code

You can enable a 17-way video call on the website by the following steps:
![](https://web-cdn.agora.io/docs-files/1543559372644)

1. Check the web browser compatibility.

	```javascript
	checkSystemRequirements()
	```
1. Create an audio or video client object.

	```javascript
	createClient()
	```

1. Initialize the client object.

	```javascript
	init(appId, onSuccess, onFailure)
	```

1. Join an AgoraRTC channel.

	```javascript
	join(channelKey, channel, uid, onSuccess, onFailure)
	```

1. Create an audio or video stream.

	```javascript
	createStream(spec)
	```

1. Enable the dual-stream mode.

	```javascript
	enableDualStream(onSuccess, onFailure)
	```

	After you have created a local client and stream object, use the `client.enableDualStream` method to enable the dual-stream mode to switch between the high stream and low stream:

	```javascript
	client.init(key, function () {
		client.join(key, channel, _uid, function (uid) {
			stream = AgoraRTC.createStream(YourConfig)    stream.init(function () {
				client.enableDualStream()
			})
		})
	})
	```

1. Set the remote video stream type.

	```javascript
	setRemoteVideoStreamType(stream, streamType)
		```

	In the `stream-added` method of the client interface, each newly added stream is automatically switched to the high stream, while the previous high stream is switched back to the low stream:

	```javascript
	function switchStream (prev, next) {
		// Set the previous stream to low.
			 precStream && client.setRemoteVideoStreamType(prevStream, 1)
			 // Set the next stream to high.
				 nextStream && client.setRemoteVideoStreamType(nextStream, 0)
	}

	client.on('stream-added', function (evt) {
		let nextStream = evt.stream
		switchStream(prevStream, nextStream)
		// Do something else, such as enlarging the stream's containder and resize.
	})
	```

	You can also switch between the high stream and low stream manually according to your actual needs:

	```javascript
	$('#id').dblclick(function (e) {
		var stream
		// stream = getStreamById(e.target.id)
		// Switch to the high stream.
		client.setRemoteVideoStreamtype(stream, 0)
	})
	```

1. Publish the local stream.

	```javascript
	publish(stream, onFailure)
	```

1. Subscribe to the remote stream.

	```javascript
	subscribe(stream, onFailure)
	```

1. (Optional) Disable the dual-stream mode.

	```javascript
	disableDualStream(onSuccess, onFailure)
	```

1. Leave the AgoraRTC channel (leave).

	```javascript
	leave(onSuccess, onFailure)
	```



## 7. Known Issues and Restrictions

- Windows Firefox users can subscribe to a maximum of eight streams.
- Firefox on Mac freezes when leaving a 17-way channel.
