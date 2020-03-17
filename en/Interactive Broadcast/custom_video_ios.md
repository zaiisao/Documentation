
---
title: Custom Video Source and Renderer
description: 
platform: iOS
updatedAt: Fri Sep 20 2019 09:36:41 GMT+0800 (CST)
---
# Custom Video Source and Renderer
## Introduction

By default, an app uses the internal audio and video modules for capturing and rendering during real-time communication. You can use an external audio or video source and renderer. This page shows how to use the methods provided by Agora SDK to customize the audio and video source and renderer.

**Customizing the audio and video source and renderer** mainly applies to the following scenarios:

* When the audio or video source captured by the internal modules do not meet your needs. For example, you need to process the captured video frame with a preprocessing library for image enhancement.
* When an app has its own audio or video module and uses a customized source for code reuse.
* When you want to use a non-camera source, such as recorded screen data.
* When you need flexible device resource allocation to avoid conflicts with other services.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md).

### Customize the Audio Source

Use the push method to customize the audio source, where by default the SDK conducts no data processing to the audio frame, such as noise reduction. Implement noise reduction on your own if you have such requirements.

```swift
// swift
// Push the video frame in the rawData format.
agoraKit.pushExternalAudioFrameRawData("your rawData", samples: "per push samples", timestamp: 0)

// Push the video frame in the CMSampleBuffer format.
agoraKit.pushExternalAudioFrameSampleBuffer("your CMSampleBuffer")
```

```objective-c
// objective-c
// Push the video frame in the rawData format.
[agoraKit pushExternalAudioFrameRawData: "your rawData" samples: "per push samples", timestamp: 0];

// Push the video frame in the CMSampleBuffer format.
[agoraKit pushExternalAudioFrameSampleBuffer: "your CMSampleBuffer"];
```

#### API Reference
* [`pushExternalAudioFrameRawData:samples:timestamp:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalAudioFrameRawData:samples:timestamp:)
* [`pushExternalAudioFrameSampleBuffer:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalAudioFrameSampleBuffer:)


### Customize the Video Source

Agora SDK provides two methods to customize the video source:

- The AgoraVideoSourceProtocol of MediaIO. You can use this protocol with AgoraVideoSinkProtocol for more varied scenarios. 
- Push method. This method is easy to use and works best for clients with frame optimization capacity.

#### MediaIO Method

Use the AgoraVideoSourceProtocol in MediaIO to customize the video source. This method sends the external video frame to the server, and you need to implement local rendering if the local preview is enabled.

1. Implement the AgoraVideoSourceProtocol to create a video source class.

	```swift
	//swift
	// variable in the protocal
		 var consumer: AgoraVideoFrameConsumer?
	// Use the consumer method to transfer the video data to the Agora SDK.

		 // Transfer the video frame in the rawData format.
		 consumer.consumeRawData("your rawData", withTimestamp: CMTimeMake(1, 15), format: "your data format", size: size, rotation: rotation)

		 // Transfer the video frame in the CVPixelBuffer format.
		 consumer.consumePixelBuffer("your pixelBuffer", withTimestamp: CMTimeMake(1, 15), rotation: rotation)

	// Implement the protocol.
	1. Set the buffer type to capture the video.
		func bufferType() -> AgoraVideoBufferType {
				return bufferType
		}

	2. Initialize the custom video source.
		func shouldInitialize() -> Bool {
		}

	3. Transfer the video data with Consumer when the customized video source starts capturing.
		func shouldStart() {
		}

	4. Stop capturing.
		func shouldStop() { 
		}

	5. Release the video source.
		func shouldDispose() {
		}
	```
	
	```objective-c
	// objective-c
	// Variable in the protocol.
	@synthesize consumer;
	// Use the consumer method to transfer the video data to the Agora SDK:

		// Transfer the video frame in the rawData format.
		[consumer consumeRawData: "your rawData" withTimestamp: CMTimeMake(1, 15) format: "your data format" size: size rotation: rotation];

		// Transfer the video frame in the CVPixelBuffer format.
		[consumer consumePixelBuffer: "your pixelBuffer" withTimestamp: CMTimeMake(1, 15) rotation: rotation];

	// Implement the protocol.
	1. Set the buffer type to capture the video.
		- (AgoraVideoBufferType)bufferType {
				return AgoraVideoBufferTypePixelBuffer;
		}

	2. Initialize the custom video source.
		- (BOOL)shouldInitialize {
				return YES;
		}

	3. Transfer the video data with Consumer when the customized video source starts capturing.
		- (void)shouldStart {
		}

	4. Stop capturing.
		- (void)shouldStop {
		}

	5. Release the video source.
		- (void)shouldDispose {
		}
	```
	
2. Pass the VideoSource object to AgoraRtcEngineKit.

	```swift
	// swift
	agoraKit.setVideoSource(videoSource)
	```

	```objective-c
	// objective-c
	[agoraKit setVideoSource: videoSource];
	```
	
##### API Reference

* [`setVideoSource:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoSource:)
* [`AgoraVideoSourceProtocal`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraVideoSourceProtocol.html)
	
#### Push Method

Compared to the MediaIO method, the push method uses less code but lacks any optimization of the captured video frame. This method requires you to do the processing.

```swift
// swift
// Push the video frame in the VPixelBufferRef format.
let videoFrame = AgoraVideoFrame()
videoFrame.format = 12
// [NSDate date].timeIntervalSince1970 indicates the current timestamp, and 1000 indicates 1000 frames per second.
videoFrame.time = CMTimeMakeWithSeconds([NSDate date].timeIntervalSince1970, 1000)
videoFrame.textureBuf = "Your CVPixelBufferRef"
videoFrame.ratation = 0

// Push the video frame in the rawData format.
let videoFrame = AgoraVideoFrame()
videoFrame.format = "your data fromat"
// [NSDate date].timeIntervalSince1970 indicates the current timestamp, and 1000 indicates 1000 frames per second.
videoFrame.time = CMTimeMakeWithSeconds([NSDate date].timeIntervalSince1970, 1000)
videoFrame.data = "your rawData"
videoFrame.strideInPixels = "your stride"
videoFrame.height = "your height"
videoFrame.dataBuf = "your rawData"
videoFrame.ratation = 0

agoraKit.pushExternalVideoFrame(videoFrame)
```

```objective-c
// objective-c
// Push the video frame in the CVPixelBufferRef format.
AgoraVideoFrame *videoFrame = [[AgoraVideoFrame alloc] init];
videoFrame.format = 12;
// [NSDate date].timeIntervalSince1970 indicates the current timestamp, and 1000 indicates 1000 frames per second.
videoFrame.time = CMTimeMakeWithSeconds([NSDate date].timeIntervalSince1970, 1000);
videoFrame.textureBuf = "Your CVPixelBufferRef";
videoFrame.ratation = 0;

// Push the video frame in the rawData format.
AgoraVideoFrame *videoFrame = [[AgoraVideoFrame alloc] init];
videoFrame.format = "your data fromat";
// [NSDate date].timeIntervalSince1970 indicates the current timestamp, and 1000 indicates 1000 frames per second.
videoFrame.time = CMTimeMakeWithSeconds([NSDate date].timeIntervalSince1970, 1000);
videoFrame.data = "your rawData";
videoFrame.strideInPixels = "your stride";
videoFrame.height = "your height";
videoFrame.dataBuf = "your rawData";
videoFrame.ratation = 0;

[agoraKit pushExternalVideoFrame: videoFrame];
```

##### API Reference

* [`pushExternalVideoFrame:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pushExternalVideoFrame:)

### Customize the Video Renderer

Use the IVideoSink Interface of MediaIO to customize the video renderer.

1. Implement the AgoraVideoSinkProtocal and create the customized video renderer class.

	```swift
	// swift
	// Implement AgoraVideoSinkProtocal.
	1. Set the buffer type that the Agora SDK sends.
		func bufferType() -> AgoraVideoBufferType {
				return bufferType
		}

		Set the video data format that the Agora SDK sends.
		func pixelFormat() -> AgoraVideoPixelFormat {
				return pixelFormat
		}

	2. Initialize the custom Video Renderer.
		func shouldInitialize() -> Bool {
				return true
		}

	3. Start the Video Renderer.  
		func shouldStart() {

		}

	4. The Agora SDK stops sending video data.
		func shouldStop() {

		}

	5. Release the custom Video Renderer.
		func shouldDispose() {

		}

	6. The Agora SDK sends the video frame in the CVPixelBuffer format, and the customized Video Renderer gets the data for rendering.
		func renderPixelBuffer(_ pixelBuffer: CVPixelBuffer, rotation: AgoraVideoRotation) {
		}

		The Agora SDK sends the video frame in the rawData format, and the customized Video Renderer gets the data for rendering.
		func renderRawData(_ rawData: UnsafeMutableRawPointer, size: CGSize, rotation: AgoraVideoRotation) {
		}
	}
	```

	```objective-c
	// objective-c
	// Implement AgoraVideoSinkProtocal.
	1. Set the buffer type that the Agora SDK sends.
		- (AgoraVideoBufferType)bufferType {
				return AgoraVideoBufferTypePixelBuffer;
		}

		Set the video data format that the Agora SDK sends.
		- (AgoraVideoPixelFormat)pixelFormat {
			 return AgoraVideoPixelFormatI420;
		}

	2. Initialize the custom Video Renderer.
		- (BOOL)shouldInitialize {
			return YES;
		}

	3. Start the Video Renderer.
		- (void)shouldStart {
		}

	4. The Agora SDK stops sending the video data.
		- (void)shouldStop {
		}

	5. Release the custom Video Renderer.
		- (void)shouldDispose {
		}

	6. The Agora SDK sends the video frame in the CVPixelBuffer format, and the customized Video Renderer gets the data for rendering.
		- (void)renderPixelBuffer:(CVPixelBufferRef _Nonnull)pixelBuffer rotation:(AgoraVideoRotation)rotation {
		}

		The Agora SDK sends the video frame in the rawData format, and the customized Video Renderer gets the data for rendering.
		- (void)renderRawData:(void * _Nonnull)rawData size:(CGSize)size rotation:(AgoraVideoRotation)rotation {
		}
	```
	
2. Pass the VideoRenderer object to AgoraRtcEngineKit.

	```swift
	// swift
	agoraKit.setLocalVideoRenderer(videoRenderer)
	agoraKit.setRemoteVideoRenderer(videoRenderer, forUserId: uid)
	```
	
	```objective-c
	// objective-c
	[agoraKit setLocalVideoRenderer: videoRenderer];
	[agoraKit setRemoteVideoRenderer: videoRenderer, uid];
	```

#### API Reference

* [`setLocalVideoRenderer:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVideoRenderer:)
* [`setRemoteVideoRenderer:forUserId:`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVideoRenderer:forUserId:)
* [`AgoraVideoSinkProtocal`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Protocols/AgoraVideoSinkProtocol.html)

Agora provides a sample app for customizing the video source and video sink. See [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-iOS).

## Considerations
Customizing the audio/video source and renderer is an advanced feature provided by Agora SDK. Ensure that you are experienced in audio and video application development.


	

