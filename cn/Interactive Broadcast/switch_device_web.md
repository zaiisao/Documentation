
---
title: 音视频设备测试与切换
description: 
platform: Web
updatedAt: Tue Sep 24 2019 05:40:10 GMT+0800 (CST)
---
# 音视频设备测试与切换
## 功能描述

声网提供的音视频测试与切换功能可以帮助开发者进行一些设备测试，检测摄像头是否能正常工作，检测音频设备是否可以正常录音及播放。音频测试检查系统的音频设备（耳麦、扬声器等）和网络连接是否正常。

你可以在以下情况使用该功能：
    1、直播场景下，在开播前请主播自测。
    2、线上用户进行自我排查纠错。

## 实现方法

### 麦克风／摄像头测试

```javascript
// find all audio devices
AgoraRTC.getDevices(function(devices){
	var audioDevices = devices.filter(function(device){
		return device.kind === "audioinput";
	});
	var videoDevices = devices.filter(function(device){
		return device.kind === "videoinput";
	});
	
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	// .. init client and join
	var uid = Math.floor(Math.random()*10000);
	var selectedMicrophoneId = ...;
	var selectedCameraId = ...;
	var stream = AgoraRTC.createStream({
		streamID: uid,
		// set audio to true if testing microphone
		audio: true,
		microphoneId: selectedMicrophoneId,
		// set video to true if testing camera
		video: false,
		cameraId: selectedCameraId,
		screen: false
	});
	
	// init stream
	stream.init(function(){
		// not needed if preview is not needed
		stream.play("mic-test");
		setInterval(function(){
		    // should be greater than 0
		    console.log(`Local Stream Audio Level ${stream.getAudioLevel()}`);
		}, 1000);
	})
});
```

### 外放测试

```javascript
// find all audio devices
AgoraRTC.getDevices(function(devices){
	var audioDevices = devices.filter(function(device){
		return device.kind === "audiooutput";
	});
	
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	// .. init client and join
	var selectedOutputDevice = ...;
	client.on("stream-subscribed", function(evt){
		var stream = evt.stream;
		// set output device
		stream.setAudioOutput(selectedOutputDevice);
	});
});
```

## 注意事项

- 在初始化输入设备时可能失败，对应的错误信息请在[开发者中心](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init)查询。
- Device ID是可变的，且不同浏览器的 Device ID 缓存机制不同，在使用 localStorage 存储本地设备 Device ID 时需要注意这样的情况。
