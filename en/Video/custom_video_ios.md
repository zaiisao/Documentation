
---
title: Customize the Audio/Video Source and Renderer
description: 
platform: iOS
updatedAt: Thu Nov 15 2018 06:24:32 GMT+0000 (UTC)
---
# Customize the Audio/Video Source and Renderer
## Introduction

By default, An App uses the internal audio and video modules for capturing and rendering during real-time communication. If developers hope to use external audio or video source and renderer, this page shows how to use the APIs provided by Agora SDK to customize the audio and video source and renderer.

**Customizing audio and video source and renderer** mainly applies to the following scenarios:

- When the audio or video source captured by the internal modules can not meet the needs of the developers. For example, for the purpose of image enhancement, developers need to process the captured video frame with a preprocessing library.
- When an App contains its own audio or video module and wants a cusmized source for  code reuse.
- When developers want to use non-camera source, such as recorded screen data.
- When developers need flexible device resource allocation to avoid conflicts with other services.

## Implementaions

Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/ios_video.md) for details.

### Customize the Audio Source

Use the Push method to customize the audio source, where the SDK conducts no data processing to the audio frame, such as noise reduction.

```swift
//swift
// Push the video frame in the format of rawData
agoraKit.pushExternalAudioFrameRawData("your rawData", samples: "per push samples", timestamp: 0)

// Push the video frame in the format of CMSampleBuffer
agoraKit.pushExternalAudioFrameSampleBuffer("your CMSampleBuffer")
```

```objective-c
//objective-c
// Push the video frame in the format of rawData
[agoraKit pushExternalAudioFrameRawData: "your rawData" samples: "per push samples", timestamp: 0];

// Push the video frame in the format of CMSampleBuffe
[agoraKit pushExternalAudioFrameSampleBuffer: "your CMSampleBuffer"];
```

### Customize the Video Source

Agora SDK provides two methods to customze the video source:

- The MediaIO method (Recommended).
- The Push method. This method skips processing the video frame, and works best for clients with frame optimization capacity.

#### The MediaIO Method

Use the IVideoSource interface in MediaIO to customize the video source. This method sends the external video frame to the server, and developers need to implement local rendering if local preview is to be enabled.

1. Implement the AgoraVideoSourceProtocal to create a video source class.

	```swift
	//swift
	// variable in the protocal
		 var consumer: AgoraVideoFrameConsumer?
	// use the consumer method to transfer the video data to Agora SDK

		// transfer the video frame in the format of rawData
		 consumer.consumeRawData("your rawData", withTimestamp: CMTimeMake(1, 15), format: "your data format", size: size, rotation: rotation)

		 // transfer the video frame in the format of CVPixelBuffer
		 consumer.consumePixelBuffer("your pixelBuffer", withTimestamp: CMTimeMake(1, 15), rotation: rotation)

	// Implement the protocal
	1. Set the buffer type to capture the video
		func bufferType() -> AgoraVideoBufferType {
				return bufferType
		}

	2.initialize the customi  video source
		func shouldInitialize() -> Bool {
		}

	3. transfer the video data with Consumer when the customized video source starts capturing 
		func shouldStart() {
		}

	4. stops capturing
		func shouldStop() { 
		}

	5. release the video source
		func shouldDispose() {
		}
	```
	
	```objective-c
	//objective-c
	@synthesize consumer;

	- (BOOL)shouldInitialize {
			return YES;
	}

	- (void)shouldStart {
	}

	- (void)shouldStop {
	}

	- (void)shouldDispose {
	}

	- (AgoraVideoBufferType)bufferType {
			return AgoraVideoBufferTypePixelBuffer;
	}
	```
	
2. Pass the VideoSource object to AgoraRtcEngineKit.

	```swift
	//swift
	agoraKit.setVideoSource(videoSource)
	```

	```objective-c
	//objective-c
	[agoraKit setVideoSource: videoSource];
	```
	
#### The Push Method

Compared to the MediaIO method, the Push method uses less codes, but lacks any optimization to the captured video frame. This method requires developers to do the processing.

```swift
//swift
// push the video frame in the format of CVPixelBufferRef
let videoFrame = AgoraVideoFrame()
videoFrame.format = 12
videoFrame.time = CMTimeMake(1, 15)
videoFrame.textureBuf = "Your CVPixelBufferRef"
videoFrame.ratation = 0

// push the video frame in the format of rawData
let videoFrame = AgoraVideoFrame()
videoFrame.format = "your data fromat"
videoFrame.time = CMTimeMake(1, 15)
videoFrame.data = "your CVPixelBufferRef"
videoFrame.strideInPixels = "your stride"
videoFrame.height = "your height"
videoFrame.dataBuf = "your rawData"
videoFrame.ratation = 0

agoraKit.pushExternalVideoFrame(videoFrame)
```

```objective-c
//objective-c
// push the video frame in the format of CVPixelBufferRef
AgoraVideoFrame *videoFrame = [[AgoraVideoFrame alloc] init];
videoFrame.format = 12;
videoFrame.time = CMTimeMake(1, 15);
videoFrame.textureBuf = "Your CVPixelBufferRef";
videoFrame.ratation = 0;

// push the video frame in the format of rawData
AgoraVideoFrame *videoFrame = [[AgoraVideoFrame alloc] init];
videoFrame.format = "your data fromat";
videoFrame.time = CMTimeMake(1, 15);
videoFrame.data = "your CVPixelBufferRef";
videoFrame.strideInPixels = "your stride";
videoFrame.height = "your height";
videoFrame.dataBuf = "your rawData";
videoFrame.ratation = 0;

[agoraKit pushExternalVideoFrame: videoFrame];
```

### Customize the Video Renderer

Use the IVideoSink Interface of MediaIO to customize the video renderer.

1. Implement the AgoraVideoSinkProtocal and create the customized video renderer class.

	```swift
	//swift
	// AgoraVideoSinkProtocal
	1. Set the buffer type that Agora SDK sends
		func bufferType() -> AgoraVideoBufferType {
				return bufferType
		}

		Set the video data format that Agora SDK send
		func pixelFormat() -> AgoraVideoPixelFormat {
				return pixelFormat
		}

	2. Initialize the customized Video Renderer
		func shouldInitialize() -> Bool {
				return true
		}

	3. 	Start the Video Renderer   
		func shouldStart() {

		}

	4. Agora SDKstops sending out video data
		func shouldStop() {

		}

	5. Release the customize Video Rendere
		func shouldDispose() {

		}

	6. Agora SDK sends out the video frame in the format of CVPixelBuffer, and the customized Video Renderer gets the data for rendering
		func renderPixelBuffer(_ pixelBuffer: CVPixelBuffer, rotation: AgoraVideoRotation) {
		}

		Agora SDK sends out the video frame in the format of rawData, and the customized Video Renderer gets the data for rendering
		func renderRawData(_ rawData: UnsafeMutableRawPointer, size: CGSize, rotation: AgoraVideoRotation) {
		}
	}
	```

	```objective-c
	//objective-c
	- (BOOL)shouldInitialize {
		return YES;
	}

	- (void)shouldStart {
	}

	- (void)shouldStop {
	}

	- (void)shouldDispose {
	}

	- (AgoraVideoBufferType)bufferType {
		 return AgoraVideoBufferTypePixelBuffer;
	}

	- (AgoraVideoPixelFormat)pixelFormat {
		 return AgoraVideoPixelFormatI420;
	}

	- (void)renderPixelBuffer:(CVPixelBufferRef _Nonnull)pixelBuffer rotation:(AgoraVideoRotation)rotation {
	}

	- (void)renderRawData:(void * _Nonnull)rawData size:(CGSize)size rotation:(AgoraVideoRotation)rotation {
	}
	```
	
2. Pass the VideoREnderer object to AgoraRtcEngineKit.

	```swift
	//swift
	agoraKit.setLocalVideoRenderer(videoRenderer)
	agoraKit.setRemoteVideoRenderer(videoRenderer, forUserId: uid)
	```
	
	```objective-c
	//objective-c
	[agoraKit setLocalVideoRenderer: videoRenderer];
	[agoraKit setRemoteVideoRenderer: videoRenderer];
	```

Agora also provides a sample app for customizing the video source and video sink. For details, see [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-iOS).

## Considerations
Customizing audio/video source and renderer is an advanced feature provided by Agora SDK. To develop this function, we believe it necessary that you have adequate knowledge and experience in audio and video application development.


	

