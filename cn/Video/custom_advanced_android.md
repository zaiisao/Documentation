
---
title: 使用组件自定义视频源和渲染器
description: 
platform: Android
updatedAt: Fri Jul 03 2020 09:29:22 GMT+0800 (CST)
---
# 使用组件自定义视频源和渲染器
<a name = "custom_video_source"></a>
## 使用 Agora SDK 提供的组件自定义视频源

### 1. AgoraBufferedCamera2 的用法

`AgoraBufferedCamera2.java` 这个类展示了实现 ByteBuffer 和 ByteArray 类型的自定义视频源。

`AgoraBufferedCamera2` 的构造函数需要传入一个 Context；还有一个可选的定义了 Camera 参数以及 Buffer 类型的 CaptureParamters。默认使用 640x480 分辨率，YUV420P 的图像格式，ByteBuffer 的数据类型。

```
AgoraBufferedCamera2 source = new AgoraBufferedCamera2(this);
source.useFrontCamera(true);
rtcEngine.setVideoSource(source);
```

当使用 ByteArray 或 ByteBuffer 类型的视频源的时候，SDK 可以接收 Pixel format 为 YUV420p, NV21 或 RGBA 格式的视频帧数据。

### 2. AgoraTextureCamera 的用法

AgoraTextureCamera.java 这个类展示了实现 Texture 类型的自定义视频源，开发者可以直接使用 AgoraTextureCamera 作为视频源。

```
IVideoSource source = new AgoraTextureCamera(this, 640, 480);
rtcEngine.setVideoSource(source);
```

### 3. 辅助类和组件

#### SurfaceTextureHelper 类

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

#### TextureSource 类

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

### 4. 示例：利用 TextureSource 实现自定义屏幕录制视频源

以自定义的屏幕录制视频源为例，全部代码请参考 GitHub。

#### 步骤 1：实现设备回调方法

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

#### 步骤 2：获取 SurfaceTexture 并使用创建 Virtual Display，用于捕获屏幕数据

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

#### 步骤 3：重载获取视频数据的回调方法

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

#### 步骤 4：不再使用视频源时，需要在适当的地方释放之前申请的资源

```
public void sourceRelease() {
   releaseProjection();
   release();
}
```

<a name = "custom_video_renderer"></a>
## 使用 Agora SDK 提供的组件自定义渲染器

Agora SDK 中提供了默认的渲染器的实现，用来显示本地视频图像和对端视频图像。大部分应用场景下，使用默认的渲染器就可以满足 App 开发者的需求。但考虑到复杂业务场景，我们也开放了自定义渲染器的接口。

-   开发者希望拿到本地或者对端的视频帧数据进行处理，而不是直接渲染到 View 上。
-   开发者希望使用通用的 SurfaceView 对象或者自定义的 view 组件。
-   开发者希望往特定区域渲染图像，比如在游戏框架中使用


### AgoraSurfaceView 的用法

`AgoraSurfaceView` 继承了 SurfaceView 同时实现了 `IVideoSink` 接口，可以渲染 YUV420P、RGB、NV21 和 texture 类型（2D/OES）的视频帧。

```
AgoraSurfaceView render = new AgoraSurfaceView(this);
render.init(MediaIO.BufferType.BYTE_ARRAY, I420, null);
render.setZOrderOnTop(true);
rtcEngine.setLocalVideoRenderer(render);
```

### AgoraTextureView 的用法

`AgoraTextureView` 继承了 TextureView 并实现了 `IVideoSink` 接口,可以渲染 YUV420P、RGB 和 texture类型（2D/OES）的视频帧。 下面代码展示了 `AgoraTextureView` 与自定义视频源配合使用，利用 TextureSource 所创建的 GL 环境：

```
AgoraTextureCamera source = new AgoraTextureCamera(this, 640, 480);
AgoraTextureView render = (AgoraTextureView) findViewById(R.id.agora_texture_view);
render.init(source.getEglContext());
render.setBufferType(MediaIO.BufferType.TEXTURE);
render.setPixelFormat(MediaIO.PixelFormat.TEXTURE_OES);
rtcEngine().setVideoSource(source);
rtcEngine().setLocalVideoRenderer(render);
```

### 辅助类和组件

#### BaseVideoRenderer 类

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

