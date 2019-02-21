
---
title: Test or Select a Media Device
description: 
platform: Web
updatedAt: Thu Feb 21 2019 09:06:49 GMT+0000 (UTC)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection feature, allowing you to check if an audio device (a headset, microphone, or speaker) works or connects properly to the SD-RTN. For example, in an audio device test, if a user's recorded voice is replayed in 10 seconds, then the system's audio device works and is connected properly to the SD-RTN.

You can use the media device test and selection feature in the following scenarios:

- A host self-checks before starting a live broadcast.
- An online user checks if the media device works.

## Implementation

### Microphone Test

```javascript
// Find all audio devices.
AgoraRTC.getDevices(function(devices){
	var audioDevices = devices.filter(function(device){
		return device.kind === "audioinput";
	});
	
	var client = AgoraRTC.createClient({ mode: 'live', codec: 'h264' });
	// Initialize the client.
	var uid = Math.floor(Math.random()*10000);
	var selectedMicrophoneId = ...;
	var stream = AgoraRTC.createStream({
		streamID: uid,
		// Set audio to true if testing the microphone.
		audio: true,
		microphoneId: selectedMicrophoneId,
	});
	
	// Initialize the stream.
	stream.init(function(){
		// Not needed if preview is not needed.
		stream.play("mic-test")
		// The "volume-indicator" callback is triggered once every two seconds.
		client.enableAudioVolumeIndicator();
		
		// Indicate the volume if needed for the local stream.
		client.on("volume-indicator", function(evt){
			evt.attr.forEach(function(volume, index){
				console.log(#{index} UID ${volume.uid} Level ${volume.level});
			});
		});
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
