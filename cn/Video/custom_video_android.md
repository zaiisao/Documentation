
---
title: 客户端自定义采集和渲染
description: 
platform: Android
updatedAt: Tue Nov 13 2018 03:48:58 GMT+0000 (UTC)
---
# 客户端自定义采集和渲染
## 功能介绍

实时通信过程中，Agora SDK 通常会启动默认的音视频模块进行采集和渲染。如果想要在客户端实现自定义音视频采集和渲染，则可以使用自定义的音视频源或渲染器，来进行实现。

自定义采集和渲染主要适用于以下场景：

* 当 SDK 内置的音视频源不能满足开发者需求时，比如需要使用自定义的美颜库或前处理库
* 开发者的 App 中已有自己的音频或视频模块，为了复用代码，也可以自定义音视频源
* 开发者希望使用非 Camera 采集的视频源，如录屏数据
* 有些系统独占的视频采集设备，为避免与其他业务产生冲突，需要灵活的设备管理策略


## 实现方法

在开始自定义采集和渲染前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Video/android_video.md)。

### 自定义音频源

目前音频源只有 Push一种方式，同视频源类似，SDK 不会对采用 Push 方法传入的音频数据做消噪等处理。

```java
//java
	// 首先开启外部音频源模式
	rtcEngine.setExternalAudioSource(
		true,      // 开启外部音频源
		44100,     // 采样率，可以有8k，16k，32k，44.1k和48kHz等模式
		1          // 外部音源的通道数，最多2个
	);

	// 持续的输出音频数据
	rtcEngine.pushExternalAudioFrame(
		data,             // byte[] 类型的音频数据
		timestamp         // 时间戳
	);
```

### 自定义视频源

Agora SDK 目前提供两种自定义视频源的方法：

* 通过 Video Source 方法。Agora 推荐使用该方法自定义视频源。
* 通过 External Frame 方法。该方法为旧的接口使用形式，略过了 SDK 对帧的优化处理，适合客户端有能力对帧进行优化的用户。

#### 使用 Video Source 自定义视频源

Video Source 是新 MediaIO 系列接口的视频输出源部分。需要注意的是，这个 API 只负责将视频帧数据传输到服务器，如需本地预览则需要应用开发者自己处理本地渲染逻辑。示例代码如下：

```java
//java
	IVideoFrameConsumer mConsumer;
	boolean mHasStarted;

	// 先创建一个实现VideoSource接口的实例
	VideoSource source = new VideoSource() {
		@Override
		public int getBufferType() {
			// 返回当前帧数据的类型，每种数据类型在SDK内部会经过不同的处理，
			// 所以必须与帧数据的类型保持一致。
			// 若切换VideoSource的类型，必须重新创建另一个实例
			// 有三种类型
			return BufferType.BYTE_ARRAY;
			// return BufferType.TEXTURE
			// return BufferType.BYTE_BUFFER;
		}

		@Override
 		public boolean onInitialize(IVideoFrameConsumer consumer) {
			// consumer是由SDK创建的，在video source生命
			// 周期中注意保存它的引用。
			mConsumer = consumer;
		}

		@Override
 		public boolean onStart() {
			mHasStarted = true;
		}

		@Override
  		public void onStop() {
			mHasStarted = false;
		}

		@Override
 		public void onDispose() {
			// 释放对consumer的引用
			mConsumer = null;
		}
	};

	// 将输出流切换到刚创建的VideoSource实例
	rtcEngine.setVideoSource(source);

	// 在得到视频帧数据之后，可以调用consumer类的方法传送数据
	// 必须根据帧数据的类型来选择用不同的方法。
	// 假设从视频源中得到的视频为data, 从Android相机中获取的帧类型
	// 可能是NV21和TEXTURE_OES，假设当前类型为byte array，即NV21
	if (mHasStarted && mConsumer != null) {
		mConsumer.consumeByteArrayFrame(data, AgoraVideoFrame.NV21, width, height, rotation, timestamp);
	}
```

#### 使用 External Frame 方法自定义视频源
External Frame 使用的是外部视频源 Push 接口。相对于 MedioIO 接口，Push 接口代码较少，但缺少 SDK 对帧的优化过程，需要用户对自己采集到的视频数据进行处理。

```java
//java
	// 首先通知SDK现在开始使用外部视频源
	rtcEngine.setExternalVideoSource(
		true，      // 是否使用外部视频源
		false,      // 是否使用texture作为输出
		true        // true为使用推送模式；false为拉取模式，但目前不支持
	);

	// 在获得视频数据的时候调用push方法将数据传送出去
	rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(
		// 在构造方法传入帧数据的参数，比如格式，宽高等
		// 具体的请查看AgoraVideoFrame类的说明
	));
```

> 开发者也可以选择自己管理视频设备的生命周期，只是根据 Media Engine 的回调来打开和关闭视频帧的输入开关。对于开发者 App 之前已有自己的采集模块，需要集成 Agora SDK 以获得实时通信能力的使用场景下，这种方式更简单。详见 [使用 Agora SDK 提供的组件自定义视频源](../../cn/Video/custom_advanced_android.md) 中的描述。

### 自定义渲染器

你可以使用 MediaIO 接口实现 Video Sink 来自定义渲染器。示例代码如下：

```java
//java
	IVideoSink sink = new IVideoSink() {
		@Override
		public boolean onInitialize () {
			return true;
		}

		@Override
		public boolean onStart() {
			return true;
		}
 
		@Override
		public void onStop() {

		}
 
		@Override
		public void onDispose() {

		}
 
		@Override
		public long getEGLContextHandle() {
			// 构造你的egl context
			// 返回0代表渲染器中并没有创建egl context
			return 0;
		}
 
		@Override
		public int getBufferType() {
			return BufferType.BYTE_ARRAY;
		}
 
		@Override
		public int getPixelFormat() {
			return PixelFormat.NV21;
		}
	}

	rtcEngine.setLocalVideoRenderer(sink);
```


> 为了方便开发者集成和创建自定义的视频渲染器，Agora 也提供了一些辅助类和示例 demo；开发者也可以直接使用这些组件，或者利用这些组件构建自定义的渲染器，详见下文的 [使用 Agora SDK 提供的组件自定义渲染器](../../cn/Video/custom_advanced_android.md) 。

Agora 目前提供自定义视频源及渲染器的示例程序，请前往 Github 下载 [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-Android) 并体验。


## 开发注意事项

客户端自定义采集和渲染属于较复杂的功能，开发者自身需要具备音视频相关知识，能够自己独立开发完成采集与渲染。

