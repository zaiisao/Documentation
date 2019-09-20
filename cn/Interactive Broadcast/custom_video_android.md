
---
title: 自定义视频采集和渲染
description: 
platform: Android
updatedAt: Fri Sep 20 2019 09:26:21 GMT+0800 (CST)
---
# 自定义视频采集和渲染
## 功能介绍

实时通信过程中，Agora SDK 通常会启动默认的音视频模块进行采集和渲染。如果想要在客户端实现自定义音视频采集和渲染，则可以使用自定义的音视频源或渲染器，来进行实现。

**自定义采集和渲染**主要适用于以下场景：

* 当 SDK 内置的音视频源不能满足开发者需求时，比如需要使用自定义的美颜库或前处理库
* 开发者的 App 中已有自己的音频或视频模块，为了复用代码，也可以自定义音视频源
* 开发者希望使用非 Camera 采集的视频源，如录屏数据
* 有些系统独占的视频采集设备，为避免与其他业务产生冲突，需要灵活的设备管理策略


## 实现方法

在开始自定义采集和渲染前，请确保你已完成环境准备、安装包获取等步骤，详见 [集成客户端](../../cn/Interactive%20Broadcast/android_video.md)。

### 自定义音频源

你可以使用 Push 方法自定义音频源。该方法下，SDK 默认不会对采集传入的音频数据做消噪等处理。如有音频消噪需求，需要开发者自行实现。

```java
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

#### API 参考
* [`pushExternalAudioFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9e219a679d066cfc2544b5e8f9d4d69f)

### 自定义视频源

Agora SDK 目前提供两种自定义视频源的方法：

* 通过 MediaIO 中的 IVideoSource 接口实现。该类接口可以搭配自定义渲染器使用，实现更为丰富的场景
* 通过 Push 方法实现。该方法使用简单，功能也较为单一

#### 使用 MediaIO 接口自定义视频源

你可以使用 MediaIO 中的 IVideoSource 接口实现自定义视频源。该方法将视频帧数据传输到 SDK，如需本地预览，还需要应用开发者自己处理本地渲染逻辑。示例代码如下：

```java
IVideoFrameConsumer mConsumer;
boolean mHasStarted;

// 先创建一个实现 VideoSource 接口的实例
VideoSource source = new VideoSource() {
	@Override
	public int getBufferType() {
		// 返回当前帧数据的 Buffer 类型，每种数据类型在 SDK 内部会经过不同的处理，所以必须与帧数据的类型保持一致
		// 若切换 VideoSource 的类型，必须重新创建另一个实例
		// 有三种类型：BYTE_BUFFER(1)；BYTE_ARRAY(2)；TEXTURE(3)
		return BufferType.BYTE_ARRAY;
	}

	@Override
 	public boolean onInitialize(IVideoFrameConsumer consumer) {
		// Consumer 是由 SDK 创建的，在 VideoSource 生命周期中注意保存它的引用
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
		// 释放对 Consumer 的引用
		mConsumer = null;
	}
};

// 将输出流切换到刚创建的 VideoSource 实例
rtcEngine.setVideoSource(source);

// 在得到视频帧数据之后，可以调用 Consumer 类的方法传送数据
// 必须根据帧数据的类型来选择用不同的方法
// 假设从视频源中得到的视频为 Data, 从 Android 相机中获取的帧类型可能是 NV21 和 TEXTURE_OES，假设当前类型为 byte array，即 NV21
if (mHasStarted && mConsumer != null) {
	mConsumer.consumeByteArrayFrame(data, AgoraVideoFrame.NV21, width, height, rotation, timestamp);
}
```

##### API 参考

* [`setVideoSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa240e991d12b5240fc5fd362cbc0d521)
* [`IVideoSource`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1mediaio_1_1_i_video_source.html)

#### 使用 Push 方法自定义视频源

相对于 MedioIO 接口，Push 方法代码较少，但缺少 SDK 对帧的优化过程，需要用户对自己采集到的视频数据进行处理。

```java
// java
// 首先通知SDK现在开始使用外部视频源
rtcEngine.setExternalVideoSource(
	true，      // 是否使用外部视频源
	false,       // 是否使用 Texture作为输出
	true         // true 为使用推送模式，false 为拉取模式，但目前不支持
);

// 在获得视频数据的时候调用 Push 方法将数据传送出去
rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(
	// 在构造方法传入帧数据的参数，比如格式，宽高等
	// 具体的请查看 AgoraVideoFrame 类的说明
));
```

##### API 参考
* [`pushExternalVideoFrame`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a6e7327f4449800a2c2ddc200eb2c0386)

开发者也可以选择自己管理视频设备的生命周期，只是根据 Media Engine 的回调来打开和关闭视频帧的输入开关。对于开发者 App 之前已有自己的采集模块，需要集成 Agora SDK 以获得实时通信能力的使用场景下，这种方式更简单。详见 [使用 Agora SDK 提供的组件自定义视频源](../../cn/Interactive%20Broadcast/custom_advanced_android.md) 中的描述。

### 自定义渲染器

你可以使用 MediaIO 中的 IVideoSink 接口来自定义渲染器。示例代码如下：

```java
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
		// 构造你的 Egl context
		// 返回 0 代表渲染器中并没有创建 Egl context
		return 0;
	}
 
    // 返回当前渲染器需要的数据 Buffer 类型
	// 若切换 VideoSink 的类型，必须重新创建另一个实例
	// 有三种类型：BYTE_BUFFER(1)；BYTE_ARRAY(2)；TEXTURE(3)
	@Override
	public int getBufferType() {
		return BufferType.BYTE_ARRAY;
	}
 
    // 返回当前渲染器需要的 Pixel 格式
	@Override
	public int getPixelFormat() {
		return PixelFormat.NV21;
	}
	
	// SDK 调用该方法将获取到的视频帧传给渲染器
	@Override
	public void consumeByteArrayFrame(byte[] data, int format, int width, int height, int rotation, long timestamp) {
  
	// 渲染器在此进行渲染
	}

}

rtcEngine.setLocalVideoRenderer(sink);
```

####  API 参考
* [`setLocalVideoRenderer`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab10fd6d8dd89a5bca09b115ecd9e3416)
* [`setRemoteVideoRenderer`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0da32c040cb9d987df2950b83459ba56)
* [`IVideoSink`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1mediaio_1_1_i_video_sink.html)


为了方便开发者集成和创建自定义的视频渲染器，Agora 也提供了一些辅助类和示例代码；开发者也可以直接使用这些组件，或者利用这些组件构建自定义的渲染器，详见下文的 [使用 Agora SDK 提供的组件自定义渲染器](../../cn/Interactive%20Broadcast/custom_advanced_android.md) 。

Agora 目前提供自定义视频源及渲染器的示例程序，请前往 Github 下载 [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-Android) 并体验。


## 开发注意事项

客户端自定义采集和渲染属于较复杂的功能，开发者自身需要具备音视频相关知识，能够自己独立开发完成采集与渲染。

