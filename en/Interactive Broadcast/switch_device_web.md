
---
title: Test or Select a Media Device
description: 
platform: Web
updatedAt: Thu Feb 21 2019 09:04:57 GMT+0800 (CST)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection feature, allowing you to check if a camera or an audio device (a headset, microphone, or speaker) works.

You can use the media device test and selection feature in the following scenarios:

- A host self-checks before starting a live broadcast.
- An online user checks if the media device works.

## Implementation

### Microphone/Camera Test

```javascript
// Find all audio devices.
AgoraRTC.getDevices(function(devices){
	var audioDevices = devices.filter(function(device){
		return device.kind === "audioinput";
	});
	var videoDevices = devices.filter(function(device){
		return device.kind === "videoinput";
	});
	
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	// Initialize the client.
	var uid = Math.floor(Math.random()*10000);
	var selectedMicrophoneId = ...;
	var selectedCameraId = ...;
	var stream = AgoraRTC.createStream({
		streamID: uid,
		// Set audio to true if testing the microphone.
		audio: true,
		microphoneId: selectedMicrophoneId,
		// Set video to true if testing the camera.
		video: false,
		cameraId: selectedCameraId,
		screen: false
	});
	
	// Initialize the stream.
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

### External Playback Device Test

```javascript
// Find all audio devices.
AgoraRTC.getDevices(function(devices){
	var audioDevices = devices.filter(function(device){
		return device.kind === "audiooutput";
	});
	
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	// Initialize the client.
	var selectedOutputDevice = ...;
	client.on("stream-subscribed", function(evt){
		var stream = evt.stream;
		// Set the output device.
		stream.setAudioOutput(selectedOutputDevice);
	});
});
```

## Considerations

- If the input device fails to initialize, you can check the error message in [Developer Center](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init).
- When using localStorage to store the local device ID, note that the device ID is variable, and the device ID caching mechanism is different for different web browsers.
