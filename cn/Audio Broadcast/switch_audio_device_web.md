
---
title: 音频设备测试与切换
description: 
platform: Web
updatedAt: Wed Feb 20 2019 08:04:22 GMT+0000 (UTC)
---
# 音频设备测试与切换
## 功能描述

很多开发者在 App 上线后会收到用户反馈听不到对方说话。这些问题部分是因为客户的本地麦克风或者喇叭不可用。声网提供的音频测试功能可以帮助开发者进行一些设备测试，检测音频设备是否可以正常录音及播放。在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。

你可以在以下情况使用该功能：
* 直播场景下，在开播前请主播自测。
* 线上用户进行自我排查纠错。

## 实现方法

### 麦克风测试

```javascript
// find all audio devices
AgoraRTC.getDevices(function(devices){
	var audioDevices = devices.filter(function(device){
		return device.kind === "audioinput";
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
	});
	
	// init stream
	stream.init(function(){
		// not needed if preview is not needed
		stream.play("mic-test")
		// the "volume-indicator" callback will be triggered per 2 seconds
		client.enableAudioVolumeIndicator();
		
		// indicate volume if needed for local stream
		client.on("volume-indicator", function(evt){
			evt.attr.forEach(function(volume, index){
				console.log(#{index} UID ${volume.uid} Level ${volume.level});
			});
		});
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

## 开发注意事项

- 在初始化输入设备时可能失败，对应的错误信息请在[开发者中心](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init)查询。
- Device ID是可变的，且不同浏览器的 Device ID 缓存机制不同，在使用 localStorage 存储本地设备 Device ID 时需要注意这样的情况。
