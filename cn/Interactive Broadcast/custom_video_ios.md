
---
title: 客户端自定义采集和渲染
description: 
platform: iOS
updatedAt: Mon Nov 12 2018 03:09:48 GMT+0000 (UTC)
---
# 客户端自定义采集和渲染
## 功能介绍

实时通信过程中，Agora SDK 通常会启动默认的音视频模块进行采集和渲染。如果想要在客户端实现自定义音视频采集和渲染，则可以使用自定义的音视频源或渲染器，来进行实现。

自定义采集和渲染主要适用于以下场景：

- 当 SDK 内置的音视频源不能满足开发者需求时，比如需要使用自定义的美颜库或前处理库
- 开发者的 App 中已有自己的音频或视频模块，为了复用代码，也可以自定义音视频源
- 开发者希望使用非 Camera 采集的视频源，如录屏数据
- 有些系统独占的视频采集设备，为避免与其他业务产生冲突，需要灵活的设备管理策略

## 实现方法

在查看下文内容前，请确保你已参考 [集成客户端](../../cn/Interactive%20Broadcast/ios_video.md) 完成 SDK 集成。

### 视频自采集

视频自采集通过 `pushExternalVideoFrame` 接口实现。 

```swift
//swift
// 推入数据类型为 CVPixelBufferRef
let videoFrame = AgoraVideoFrame()
videoFrame.format = 12
videoFrame.time = CMTimeMake(1, 15)
videoFrame.textureBuf = "Your CVPixelBufferRef"
videoFrame.ratation = 0

// 推入数据类型为 rawData
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

### 音频自采集

音频自采集通过 `pushExternalAudioFrame` 接口实现。

```swift
//swift
// 推入数据类型为 rawData
agoraKit.pushExternalAudioFrameRawData("your rawData", samples: "per push samples", timestamp: 0)

// 推入数据类型为 CMSampleBuffer
agoraKit.pushExternalAudioFrameSampleBuffer("your CMSampleBuffer")
```

### 使用 MediaIO 接口自定义采集和渲染

#### 自定义视频源

1. 遵守 AgoraVideoSourceProtocol 协议， 并实现接口，构建自定义的 Video Source 类。

	```swift
	//swift
	// 协议中的变量
		 var consumer: AgoraVideoFrameConsumer?
	// 调用 consumer 的方法，将视频数据推入Agora SDK:

		// 推入rawData类型
		 consumer.consumeRawData("your rawData", withTimestamp: CMTimeMake(1, 15), format: "your data format", size: size, rotation: rotation)

		 // 推入CVPixelBuffer
		 consumer.consumePixelBuffer("your pixelBuffer", withTimestamp: CMTimeMake(1, 15), rotation: rotation)

	// 协议中的方法
	1. 视频采集使用的 Buffer 类型
		func bufferType() -> AgoraVideoBufferType {
				return bufferType
		}

	2. 在初始化视频源 (shouldInitialize) 中, 初始化自定义的 Video Source
		func shouldInitialize() -> Bool {
		}

	3. 自定义视频源开始采集视频数据，并通过 consumer 推入视频数据
		func shouldStart() {
		}

	4. 自定义视频源停止采集视频数据
		func shouldStop() { 
		}

	5. 在释放自定义视频源
		func shouldDispose() {
		}
	```

2. 将遵守了 AgoraVideoSourceProtocol 协议的自定义 VideoSource 对象设给 AgoraRtcEngineKit。

	```swift
	//swift
	agoraKit.setVideoSource(videoSource)
	```

#### 自定义渲染源

1. 遵守 AgoraVideoSinkProtocol 协议， 并实现接口，构建自定义的 Video Renderer 类。

	```swift
	//swift
	// 协议中的方法
	1. 希望 Agora SDK 抛出的视频 Buffer 类型
		func bufferType() -> AgoraVideoBufferType {
				return bufferType
		}

		希望 Agora SDK 抛出的视频数据格式
		func pixelFormat() -> AgoraVideoPixelFormat {
				return pixelFormat
		}

	2. 初始化自定义的 Video Renderer
		func shouldInitialize() -> Bool {
				return true
		}

	3. 	启动自定义的 Video Renderer   
		func shouldStart() {

		}

	4. Agora SDK 停止抛出视频数据
		func shouldStop() {

		}

	5. 自定义的 Video Renderer 可以被释放   
		func shouldDispose() {

		}

	6. Agora SDK 通过该接口抛出 CVPixelBuffer 类型的视频数据, 自定义 Video Renderer 可以获取数据进行渲染
		func renderPixelBuffer(_ pixelBuffer: CVPixelBuffer, rotation: AgoraVideoRotation) {
		}

		Agora SDK 通过该接口抛出 rawData 类型的视频数据, 自定义 Video Renderer 可以获取数据进行渲染
		func renderRawData(_ rawData: UnsafeMutableRawPointer, size: CGSize, rotation: AgoraVideoRotation) {
		}
	}
	```

2. 将遵守了  AgoraVideoSourceProtocol 协议的自定义 VideoRenderer 对象设给 AgoraRtcEngineKit。

	```swift
	//swift
	agoraKit.setLocalVideoRenderer(videoRenderer)
	agoraKit.setRemoteVideoRenderer(videoRenderer, forUserId: uid)
	```

Agora 目前提供自定义视频源和渲染器的示例程序，请前往 Github 下载 [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-iOS) 并体验。

## 开发注意事项
客户端自定义采集和渲染属于较复杂的功能，开发者自身需要具备音视频相关知识，能够自己独立开发完成采集与渲染。
