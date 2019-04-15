
---
title: 实现视频通话
description: 
platform: Android
updatedAt: Fri Nov 02 2018 03:59:48 GMT+0800 (CST)
---
# 实现视频通话
在本页你可以了解如何使用 Agora SDK 实现视频通话。

## 准备环境

1.  关于具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/android_video.md) 。

2.  参考 [Android Video Tutorial](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-Android-Tutorial-1to1) 了解如何从头创建一个示例项目。


## 快速开始

### 创建实例

导入以下 Agora API 包:

-   `io.agora.rtc.Constants`
-   `io.agora.rtc.IRtcEngineEventHandler`
-   `io.agora.rtc.RtcEngine`
-   `io.agora.rtc.video.VideoCanvas`


进入视频通话频道之前，调用 `create` 创建一个实例。在该方法中:

-   填入 [设置开发环境](../../cn/Quickstart%20Guide/android_video.md) 里获取的 App ID。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
-   指定一个事件回调。SDK 通过指定的事件通知应用程序 SDK 的运行事件，如: 加入或离开频道，新用户加入频道等。


```
import io.agora.rtc.Constants;
import io.agora.rtc.IRtcEngineEventHandler;
import io.agora.rtc.RtcEngine;
import io.agora.rtc.video.VideoCanvas;

...

private void initializeAgoraEngine() {
    try {
        mRtcEngine = RtcEngine.create(getBaseContext(), getString(R.string.agora_app_id), mRtcEventHandler);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));

        throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```

### 设置频道模式

创建实例后，调用 `setChannelProfile` 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。 
在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。


```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_COMMUNICATION);
```

### 打开视频模式

调用 `enableVideo` 方法打开视频模式。在 Agora SDK 中，音频功能是默认打开的，因此在加入频道前，或通话过程中，你都可以调用该方法开启视频。

-   如果在加入频道前打开，则进入频道后直接加入视频通话。
-   如果在通话过程中打开，则由纯音频通话切换为视频通话。


```
mRtcEngine.enableVideo();
```

### 设置视频编码属性

打开视频模式后，调用 `setVideoEncoderConfiguration` 方法设置视频的编码属性。

在该方法中，指定你想要的视频编码的分辨率、帧率、码率以及视频编码的方向模式。详细的视频编码参数定义，参考 设置视频编码属性 `setVideoEncoderConfiguration` 中的描述。

> -   该方法设置的参数为理想情况下的最大值。当视频引擎因网络等原因无法达到设置的分辨率、帧率或码率值时，会取最接近设置值的最大值。
> -   如果设备的摄像头无法支持定义的视频属性，SDK 会为摄像头自动选择一个合理的分辨率。该行为对视频编码没有影响，编码时 SDK 仍沿用该方法中定义的分辨率。


```
VideoEncoderConfiguration.ORIENTATION_MODE
orientationMode =
VideoEncoderConfiguration.ORIENTATION_MODE.ORIENTATION_MODE_FIXED_PORTRAIT;

    VideoEncoderConfiguration.VideoDimensions dimensions = new VideoEncoderConfiguration.VideoDimensions(360, 640);

    VideoEncoderConfiguration videoEncoderConfiguration = new VideoEncoderConfiguration(dimensions, frameRate, bitrate, orientationMode);

mRtcEngine.setVideoEncoderConfiguration(videoEncoderConfiguration);
```

### 设置本地视频视图

本地视频视图，是指用户在本地设备上看到的本地视频流的视图。

在加入频道前，调用 `setupLocalVideo` 方法使应用程序绑定本地视频流的显示视窗，并设置本地看到的本地视频视图。`setupLocalVideo` 为视频流创建 *SurfaceView* 对象，并初始化:

-   将 `setZOrderMediaOverlay` 设置为 true，用以覆盖父视图;
-   将 `SurfaceView `添加到 local\_video\_view\_container 布局中;

调用 `setupLocalVideo` 将新的 VideoCanvas 对象传递给引擎，该引擎绑定本地视频流的视频窗口 SurfaceView 并配置视频的显示设置。


```
 private void setupLocalVideo() {
    FrameLayout container = (FrameLayout) findViewById(R.id.local_video_view_container);
    SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
    surfaceView.setZOrderMediaOverlay(true);
    container.addView(surfaceView);
    mRtcEngine.setupLocalVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_ADAPTIVE, 0));
}
```

### 加入频道

现在你可以加入视频通话频道了。调用 `joinChannel` 方法加入频道。同一个频道内的用户可以互相通话；多个用户加入同一个频道，可以群聊。

-   传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 null。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [密钥说明](../../cn/Agora%20Platform/token.md)。
-   使用不同 App ID 的应用程序是不能互通的。
-   频道内每个用户的 UID 必须是唯一的。如果将 UID 设为 0，系统将自动分配一个 UID。

> 如果已在通话中，用户必须调用 `leaveChannel` 方法退出当前通话，才能进入下一个频道。

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

### 设置远端视频视图

远端视频视图，是指用户在本地设备上看到的远端视频流的视图。

调用 `setupRemoteVideo` 方法设置本地看到的远端用户的视频视图。该方法：

-   在布局里面创建并添加 *View* \(视图\)对象;
-   创建 *VideoCanvas* 并与视图进行绑定;
-   将 View 添加到 local\_video\_view\_container 布局中;
-   调用 `setupLocalVideo` 将新的 VideoCanvas 对象传递给引擎，该引擎绑定本地视频流的视频窗口\(视图\) 并配置视频的显示设置;
-   使用频道名对 View 进行标记;

用户需要在该方法中指定想要看到的远端视图的用户 UID。 如果不知道想要指定的远端用户 UID，也可以在收到其他用户加入频道的回调事件 `onUserJoined` 中获取该 UID。

```
private void setupRemoteVideo(int uid) {
   FrameLayout container = (FrameLayout) findViewById(R.id.remote_video_view_container);

   if (container.getChildCount() >= 1) {
       return;
   }

   SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
   container.addView(surfaceView);
   mRtcEngine.setupRemoteVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_ADAPTIVE, uid));

   surfaceView.setTag(uid);
   View tipMsg = findViewById(R.id.quick_tips_when_use_agora_sdk);
   tipMsg.setVisibility(View.GONE);
  }
```

### 结束通话

调用 `leaveChannel` 方法离开频道，挂掉或结束通话。

不论当前是否还在通话中，调用该方法会把会话相关的所有资源释放掉。`leaveChanne` 并不会直接让用户离开频道。调用该方法后 SDK 会触发 `onLeaveChannel` 回调。

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

如果在调用 `leaveChannel` 方法后立即使用 `destroy`，则退出频道会被打断，SDK 也不会触发 `onLeaveChannel` 回调。

### 结论

现在你可以开始一个一对一通话或多人群聊了，两者唯一的区别在于频道内人数的差别。


SDK 默认打开摄像头，你在加入频道后可以调用以下 API:

 -   `startPreview`：开启视频预览，打开本地预览功能;
 -   `disableVideo`：关闭视频模式，切换到纯语音模式。


## 参考

在上述视频通话过程中，我们使用了 Agora SDK 提供的部分 API 功能：

-  创建 RtcEngine 对象：`create`
-  创建渲染视图：`createRendererView`
-  打开视频模式： `enableVideo`
-  设置本地视频显示属性：`setupLocalVideo`
-  设置远端视频显示属性：`setupRemoteVideo`
-  设置视频属性：`setVideoProfile`
-   加入频道：`joinChannel`


在实现视频通话的过程中，你还可能需要使用以下功能:

-   [视频采集旋转](../../cn/Quickstart%20Guide/rotation_guide_android.md)
-   [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)
-   [选择加密方案](../../cn/Quickstart%20Guide/encryption_android_agora.md)
-   [修改裸数据](../../cn/Quickstart%20Guide/rawdata_android.md)


更多功能实现，请参考 [视频通话 API](https://docs.agora.io/cn/Video/API%20Reference/java/index.html) 中各 API 的功能及描述。


