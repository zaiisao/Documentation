
---
title: Video Conference of 7+ Participants
description: 
platform: Web
updatedAt: Thu Nov 01 2018 10:20:12 GMT+0000 (UTC)
---
# Video Conference of 7+ Participants
# Video Conference of 7+ Participants

If a video call is participated by too many people, latency or packet loss may occur.

Now, if we set the subscribing stream to the **1-N** Mode, that is, set 1 strem as high and the rest low, then a maximum of 17 people can join a video call or interactive broadcast without experiencing latency.

This page shows you how to enable a video conference for 7+ participants on the website by using the Agora Web SDK, as well as relevant considerations.



## 1. User Role

In a video conference of 7+ participants, each client is set as a host (broadcaster). The video interaction is a process of joining hosts together.

## 2. Settings Before Subscribing to A Remote Stream

If you want to subscribe to the low stream, you can set the stream type before subscribing to the stream by calling `setRemoteVideoStreamType`.

```javascript
setRemoteVideoStreamType(stream, streamType)
```

## 3. Setting Dual Streams

### 3.1 Setting High-stream Profile

A 17-way video call theoretically supports all the video profiles listed in `setVideoProfile` . However, Agora does not recommend using video profiles that exceed either a resolution of 640 x 480 or a frame rate of 15 fps.

| **Resolution** | **Frame Rate** | **Bit Rate** |
| -------------- | -------------- | ------------ |
| 640 x 480      | 15 fps         | 500 kbps     |
| 640 x 360      | 15 fps         | 400 kbps     |
| 640 x 360      | 30 fps         | 600 kbps     |

### 3.2 Default Low-stream Profile

Once you have enabled the dual-stream mode, you can set the high-stream profile in the web application, and the Web SDK will automatically adapt the low-stream profile accordingly.

```javascript
localStream.setVideoProfile('480P_1')
```

**See the table below for the correlation between the high and low stream**

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

| **Profile** | **Resolution** | **Frame Rate** |
| ----------- | -------------- | -------------- |
| 90P_1       | 160x90         | 15             |
| 120P        | 160x120        | 15             |
| 120P_1      | 160x120        | 15             |
| 120P_3      | 120x120        | 15             |
| 120P_4      | 212x120        | 15             |
| 180P        | 320x180        | 15             |
| 180P_1      | 320X180        | 15             |
| 180P_3      | 180x180        | 15             |
| 180P_4      | 424x240        | 15             |
| 240P        | 320x240        | 15             |
| 240P_1      | 320X240        | 15             |
| 240P_3      | 240x240        | 15             |
| 240P_4      | 424x240        | 15             |
| 360P        | 640x360        | 15             |
| 360P_1      | 640X360        | 15             |
| 360P_3      | 360x360        | 15             |
| 360P_4      | 640x360        | 30             |
| 360P_6      | 360x360        | 30             |
| 360P_7      | 480x360        | 15             |
| 360P_8      | 480x360        | 30             |
| 360P_9      | 640x360        | 15             |
| 360P_10     | 640x360        | 24             |
| 360P_11     | 640x360        | 24             |
| 480P        | 640x480        | 15             |
| 480P_1      | 640x480        | 15             |
| 480P_2      | 648x480        | 30             |
| 480P_3      | 480x480        | 15             |
| 480P_4      | 640x480        | 30             |
| 480P_6      | 480x480        | 30             |
| 480P_8      | 848x480        | 15             |
| 480P_9      | 848x480        | 30             |
| 480P_10     | 640x480        | 10             |
| 720P        | 1280x720       | 15             |
| 720P_1      | 1280x720       | 15             |
| 720P_2      | 1280x720       | 15             |
| 720P_3      | 1280x720       | 30             |
| 720P_5      | 960x720        | 15             |
| 720P_6      | 960x720        | 30             |

> Since browsers use an internal algorithm to adjust the stream, the actual low stream may be slightly different from that shown in the table.



### 3.3 Customizing the Low-stream Profile

If you have enabled dual-stream mode, you can also customize the low-stream video profile by using the following method:

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
| width         | (optional) The width of the low-stream video frame. Parameters `width` and `height` are bound together, and therefore take effect only when both are set. Otherwise, the SDK will allocate a default value for the parameter. |
| height        | (optional) The height of the low-stream video frame. Parameters `width` and `height` are bound together, and therefore take effect only when both are set. Otherwise, the SDK will allocate a default value for the parameter. |
| framerate     | (optional) The framerate of the low-stream video frame. If you do not set this parameter, the SDK will allocate a default value for it. |
| bitrate       | (optional) The birtate of the low-stream video frame. If you do not set this parameter, the SDK will allocate a default value for it. |

> - Since browsers have different restrictions on the video profile, the parameters you set may fail to take effect. Up till now, we have found that the Firefox browser has a fixed framerate of 30 fps and therefore framerate settings does not work
> - You need to call `client.join` before using this method.
> - Screen sharing supports the high stream only.

### 3.4 Resolution Ratio of the High and Low Stream

The resolution ratio of the low-stream video profile is identical to that of the high-stream profile. For example:

- If a high-stream video profile (on a PC) is set to a resolution of 640 x 480, then the low-stream video profile must be set to a resolution with the same length-width ratio, which is 4:3.
- If a high-stream video profile (on a PC) is set to a resolution of 640 x 360, then the low-stream video profile must be set to a resolution with the same length-width ratio, which is 16:9.

### 3.5 Enabling Dual-stream Mode

Whether to enable dual-stream mode or not depends on the actual situation. If the video call is participated by more than 7 people, or your App uses dual windows, one big and one small, Agora recommends enabling dual-stream mode. Use the following code snippet to enable dual-stream mode:

```javascript
enableDualStream();
```

### 3.6 User Interface Layout

Agora recommends using a layout with one big window and multiple small windows.

- Use a high-stream layout for the big window.
- Use a low-stream layout for small windows.

## 4. Releasing Memory Resources

When a user is offline, the website releases the memory resources of the video view in the format of H5.

## 5. Environment Requirements

1. Ensure that your device and browser meets the following requirements:

   - Window: Windows 7, Windows 8.x, Windows 10
     - Chrome Browser, later than Chrome 58 (for HTTPS only)
     - Firefox Browser, later than Firefox 56 (for HTTPS only). Currently the Firefox supports only 8 windows on the video call screen.

   - Mac: later than Safari 11
     - Chrome Browser, later than Chrome 58 (for HTTPS only)
     - Firefox Browser, later than Firefox 56 (for HTTPS only)

   > Currently the Web SDK suppots 17-way video talk on PCs only, not on mobile devices.

2. Ensure that your Web SDK version is later than 2.1.

## 6. Enabling a 17-way Video Call with the Sample Code

You can enable a 17-way video call on the website by using the following methods:
![](https://web-cdn.agora.io/docs-files/1539758012237)

1. Check the browser compatibility.

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

1. Join the AgoraRTC channel.

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

	After you have created a local client and stream object, use the `client.enableDualStream` method to enable the dual-stream mode to switch between the high and low streams:

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

	In the `stream-added` method of the Client Interface, every newly added stream is automatically switched to the high stream, while the previous high stream is switched back to low:

	```javascript
	function switchStream (prev, next) {
		// set prev stream to low
			 precStream && client.setRemoteVideoStreamType(prevStream, 1)
			 // set next stream to high
				 nextStream && client.setRemoteVideoStreamType(nextStream, 0)
	}

	client.on('stream-added', function (evt) {
		let nextStream = evt.stream
		switchStream(prevStream, nextStream)
		// do sth else such as enlarging stream's containder and resize
	})
	```

	You can also switch between the high and low streams manually according to your actual needs:

	```javascript
	$('#id').dblclick(function (e) {
		var stream
		// stream = getStreamById(e.target.id)
		//switch to high
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

1. (optional) Disable the dual-stream mode.

	```javascript
	disableDualStream(onSuccess, onFailure)
	```

1. Leave the AgoraRTC channel (leave).

	```javascript
	leave(onSuccess, onFailure)
	```



## 7. Known Issues and Restrictions

- Users on the Windows Firefox can subscribe to a maximum of 8 streams.
- The Firefox browser on Mac will freeze when leaving the 17-way channel.
