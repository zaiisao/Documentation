
---
title: 客户端自定义采集和渲染
description: 
platform: Android
updatedAt: Mon Nov 12 2018 09:42:26 GMT+0000 (UTC)
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

在查看下文内容前，请确保你已参考 [集成客户端](../../cn/Video/android_video.md) 完成 SDK 集成。

### 使用 MediaIO 接口自定义采集和渲染

#### 自定义视频源

步骤 1：实现 `IVideoSource` 接口，构建自定义的 Video Source 类

-  确定视频源使用的 Buffer 类型并在 获取 Buffer 类型 `getBufferType` 的实现中指定

	```
	int getBufferType();
	```

-  在 初始化视频源 `onInitialize` 中保存传入的 `IVideoFrameConsumer` 对象

	```
	boolean onInitialize(IVideoFrameConsumer consumer);
	```

-  在 启动视频源 `onStart` 后使用 `IVideoFrameConsumer` 输入视频帧

	```
	boolean onStart();
	```

- 在 停止视频源 `onStop` 中停止使用 `IVideoFrameConsumer` 对象输入视频帧

	```
	void onStop();
	```

-  在 释放视频源 `onDispose` 中释放 `IVideoFrameConsumer` 对象

	```
	void onDispose();
	```

步骤 2：实例 `IVideoSource` 接口，创建自定义的视频源对象

步骤 3：通过 Media Engine 的 设置视频源 `setVideoSource` 方法把自定义的视频源对象设置给 Media Engine

```
public abstract int setVideoSource(IVideoSource source);
```

步骤 4：Media Engine 会在适当的实际调用自定义视频源中实现的 `IVideoSource` 的方法


> 开发者也可以选择自己管理视频设备的生命周期，只是根据 Media Engine 的回调来打开和关闭视频帧的输入开关。对于开发者 App 之前已有自己的采集模块，需要集成 Agora SDK 以获得实时通信能力的使用场景下，这种方式更简单。详见下文 [使用 Agora SDK 提供的组件自定义视频源](#custom_video_source) 中的描述。

#### 自定义渲染器

步骤 1：根据实际需要的数据类型和格式，实现 获取 Buffer 类型 `getBufferType` 和 获取像素格式 `getPixelFormat` ，设置数据帧的类型和格式

步骤 2：依次实现 初始化渲染器 `onInitialize`、启动渲染器 `onStart`、停止渲染器 `onStop` 和 释放渲染器 `onDispose` ，实现对自定义渲染器的状态控制

步骤 3：根据步骤 1 中定义的类型，选择实现 `IVideoFrameConsumer` 中对应格式的 Consumer 方法，用于获取视频帧数据

步骤 4：创建自定义的渲染对象

步骤 5：通过 Media Engine 的 设置本地视频渲染器 `setLocalVideoRenderer` 和 设置远端视频渲染器 `setRemoteVideoRenderer` 方法，设置用于本地渲染或者对方图像渲染

步骤 6：Media Engine 会再根据内部状态调用 `IVideoSink` 接口 中定义的方法。

> 为了方便开发者集成和创建自定义的视频渲染器，Agora 也提供了一些辅助类和示例 demo；开发者也可以直接使用这些组件，或者利用这些组件构建自定义的渲染器，详见下文的 [使用 Agora SDK 提供的组件自定义渲染器](#custom_video_renderer) 。

Agora 目前提供自定义视频源及渲染器的示例程序，请前往 Github 下载 [Agora Custom Media Device](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-Android) 并体验。

<a name = "custom_video_source"></a>
### 使用 Agora SDK 提供的组件自定义视频源

#### 1. AgoraBufferedCamera2 的用法

`AgoraBufferedCamera2.java` 这个类展示了实现 ByteBuffer 和 ByteArray 类型的自定义视频源。

`AgoraBufferedCamera2` 的构造函数需要传入一个 Context；还有一个可选的定义了 Camera 参数以及 Buffer 类型的 CaptureParamters。默认使用 640x480 分辨率，YUV420P 的图像格式，ByteBuffer 的数据类型。

```
AgoraBufferedCamera2 source = new AgoraBufferedCamera2(this);
source.useFrontCamera(true);
rtcEngine.setVideoSource(source);
```

当使用 ByteArray 或 ByteBuffer 类型的视频源的时候，SDK 可以接收 Pixel format 为 YUV420p, NV21 或 RGBA 格式的视频帧数据。

#### 2. AgoraTextureCamera 的用法

AgoraTextureCamera.java 这个类展示了实现 Texture 类型的自定义视频源，开发者可以直接使用 AgoraTextureCamera 作为视频源。

```
IVideoSource source = new AgoraTextureCamera(this, 640, 480);
rtcEngine.setVideoSource(source);
```

#### 3. 辅助类和组件

##### SurfaceTextureHelper 类

`SurfaceTextureHelper` 类 SDK 提供的辅助类，用于方便开发者使用 SurfaceTexture，而无需关心构建 GL 环境，创建 texture，以及线程之间的交互。 SurfaceTextureHelper 的主要功能：

1.  创建纹理对象，并利用纹理对象生成 SurfaceTexture

2.  当 SurfaceTexture 捕获到视频帧时，通知开发者 texture 内容已经更新


**创建 SurfaceTextureHelper**

```
public static SurfaceTextureHelper create(final String threadName, final EglBase.Context sharedContext);
```

利用 `create` 方法可以创建 SurfaceTextureHelper。这时会新创建一个 GL 线程，并在这个 GL 环境中创建好 texture 和 SurfaceTexture。

**获取 SurfaceTexture**

```
public EglBase.Context getEglContext();
public Handler getHandler();
public SurfaceTexture getSurfaceTexture();
```

该方法获取创建出来的 texture；如果需要访问 GL 环境或者 GL 线程，也可以通过 `getEglContext` 和 `getHandler` 拿到。

**监听 SurfaceTexture**

```
public interface OnTextureFrameAvailableListener {
    abstract void onTextureFrameAvailable(int oesTextureId, float[] transformMatrix, long timestampNs);
}
public void startListening(final OnTextureFrameAvailableListener listener);
public void stopListening();
```

该方法设置 Listener 来监听SurfaceTexture是否有新的视频帧，通过 `startListening` 和 `stopListening` 来启动或停止监听。

**释放视频资源**

```
void dispose();
```

如果不再需要使用 SurfaceTexture，使用该方法释放相关资源。

##### TextureSource 类

SDK 提供了 TextureSource 类，来方便开发者构建 Texture 类型的视频源；它封装了 SurfaceTexture 的创建和与其关联的 Texture 内容的更新，以及对 `IVideoFrameConsumer` 的操作；只需要关心三点：

-   与视频源设备的交互或适配

-   对 TextureSource 中获取到的 SurfaceTexture 捕获视频帧；

-   在 Texture 内容更新后把 Texture 送入 RtcEngine


具体实现步骤如下：

步骤 1: 实现 TextureSource 的 4 个回调方法，在方法完成对视频设备的操作或者功能适配

```
abstract protected boolean onCapturerOpened();
abstract protected boolean onCapturerStarted();
abstract protected void onCapturerStopped();
abstract protected void onCapturerClosed();
```

步骤 2: 获取 SurfaceTexture，并利用它捕获视频帧

```
public SurfaceTexture getSurfaceTexture();
```

捕获视频帧的几种方法详见上一篇中对 SurfaceTexture 组件的介绍。

步骤 3: 当捕获到视频帧并更新到 Texture 内容后，会调用回调函数 `onTextureFrameAvailable`，重载这个方法的实现，然后在这个方法中把视频帧传输给 RtcEngine。

```
public void onTextureFrameAvailable(int oesTextureId, float[] transformMatrix, long timestampNs);
```

步骤 4: 当不再需要视频源时，释放相关的资源

```
public void release();
```

#### 4. 示例：利用 TextureSource 实现自定义屏幕录制视频源

以自定义的屏幕录制视频源为例，全部代码请参考 Github。

##### 步骤 1：实现设备回调方法

```
public class ScreenRecordSource extends TextureSource {
    private Context mContext;
    private boolean mIsStart;
    private VirtualDisplay mVirtualDisplay;
    private MediaProjection mMediaProjection;

    public ScreenRecordSource(Context context, int width, int height, int dpi, MediaProjection mediaProjection) {
        super(null, width, height);
        mContext = context;
        mMediaProjection = mediaProjection;
    }

    @Override
    protected boolean onCapturerOpened() {
        createVirtualDisplay();
        return true;
    }
    @Override
    protected boolean onCapturerStarted() {
        return mIsStart = true;
    }
    @Override
    protected void onCapturerStopped() {
        mIsStart = false;
    }
    @Override
    protected void onCapturerClosed() {
        releaseVirtualDisplay();
    }
 }
```

##### 步骤 2：获取 SurfaceTexture 并使用创建 Virtual Display，用于捕获屏幕数据

```
private void createVirtualDisplay() {
    Surface inputSurface = new Surface(getSurfaceTexture);
    if (mVirtualDisplay == null) {
        mVirtualDisplay = mediaProjection.createVirtualDisplay("MainScreen", mWidth, mHeight, mDpi,
                    DisplayManager.VIRTUAL_DISPLAY_FLAG_AUTO_MIRROR, inputSurface, null, null);
    }
}

private void virtualDisplay() {
    if (virtualDisplay != null) {
        virtualDisplay.release();
    }
    virtualDisplay = null;
}
```

##### 步骤 3：重载获取视频数据的回调方法

```
@Override
public void onTextureFrameAvailable(int oesTextureId, float[] transformMatrix, long timeStampNs) {
    super.onTextureFrameAvailable(oesTextureId, transformMatrix, timeStampNs);
    if (mIsStart && mConsumer != null && mConsumer.get() != null) {
        mConsumer.get().consumeTextureFrame(oesTextureId, TEXTURE_OES.intValue(), mWidth, mHeight,
                                            0, System.currentTimeMillis(), transformMatrix);
    }
}
```

> 确认调用了父类方法：*super.onTextureFrameAvailable\(oesTextureId, transformMatrix, timeStampNs\);*

##### 步骤 4：不再使用视频源时，需要在适当的地方释放之前申请的资源

```
public void sourceRelease() {
   releaseProjection();
   release();
}
```

<a name = "custom_video_renderer"></a>
### 使用 Agora SDK 提供的组件自定义渲染器

Agora SDK 中提供了默认的渲染器的实现，用来显示本地视频图像和对端视频图像。大部分应用场景下，使用默认的渲染器就可以满足 App 开发者的需求。但考虑到复杂业务场景，我们也开放了自定义渲染器的接口。

-   开发者希望拿到本地或者对端的视频帧数据进行处理，而不是直接渲染到 View 上。
-   开发者希望使用通用的 SurfaceView 对象或者自定义的 view 组件。
-   开发者希望往特定区域渲染图像，比如在游戏框架中使用


#### AgoraSurfaceView 的用法

`AgoraSurfaceView` 继承了 SurfaceView 同时实现了 `IVideoSink` 接口，可以渲染 YUV、RGB 和 texture 类型（2D/OES）的视频帧。

```
AgoraSurfaceView render = new AgoraSurfaceView(this);
render.init(MediaIO.BufferType.BYTE_ARRAY, I420, null);
render.setZOrderOnTop(true);
rtcEngine.setLocalVideoRenderer(render);
```

#### AgoraTextureView 的用法

`AgoraTextureView` 继承了 TextureView 并实现了 `IVideoSink` 接口,可以渲染 YUV、RGB 和 texture类型（2D/OES）的视频帧。 下面代码展示了 `AgoraTextureView` 与自定义视频源配合使用，利用 TextureSource 所创建的 GL 环境：

```
AgoraTextureCamera source = new AgoraTextureCamera(this, 640, 480);
AgoraTextureView render = (AgoraTextureView) findViewById(R.id.agora_texture_view);
render.init(source.getEglContext());
render.setBufferType(MediaIO.BufferType.TEXTURE);
render.setPixelFormat(MediaIO.PixelFormat.TEXTURE_OES);
rtcEngine().setVideoSource(source);
rtcEngine().setLocalVideoRenderer(render);
```

#### 辅助类和组件

##### BaseVideoRenderer 类

Agora SDK 提供了 `BaseVideoRenderer` 类，辅助开发者自定义渲染器。其主要功能：

1.  可以渲染多种图像格式：I420，RGBA，TEXTURE\_2D/OES

2.  支持多种渲染目标：SurfaceView，TextureView，Surface 以及 SurfaceTexture


常用的使用步骤：

1.  创建自定义的渲染器类，实现 `IVideoSink` 接口，内嵌 `BaseVideoRenderer` 对象。

2.  指定数据帧的类型和格式，通过调用内嵌的 `BaseVideoRenderer` 对象的 `setBufferType` 和 `setPixelFormat` 方法。

3.  由于 `BaseVideoRenderer` 使用 OpenGL 渲染，也创建了 EGLContext，可以共享 EGLContext 的 Handle 给 Media Engine。

4.  设置渲染目标，通过调用内嵌的 `BaseVideoRenderer` 对象的 `setRenderView` 或 `setRenderSurface`。

5.  实现渲染器状态控制的方法：`onInitialize`、`onStart`、`onStop` 和 `onDispose`。

6.  实现 `IVideoFrameConsumer` 的 `consumer` 方法，调用 `BaseVideoRenderer` 对象的对应格式的 `consumer` 方法可以将接收到的视频帧渲染到渲染目标上。

## 开发注意事项

客户端自定义采集和渲染属于较复杂的功能，开发者自身需要具备音视频相关知识，能够自己独立开发完成采集与渲染。

