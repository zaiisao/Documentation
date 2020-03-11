
---
title: 媒体播放器组件
description: 
platform: Windows
updatedAt: Mon Mar 09 2020 10:49:33 GMT+0800 (CST)
---
# 媒体播放器组件
## 功能描述

当你使用 Agora Native SDK 实现音视频互动直播时，如果你想将本地媒体文件或在线媒体流经过  SD-RTN 分发给远端用户观看，那么你需要使用媒体播放器组件播放这个文件。主播可以通过媒体播放器组件实时调节播放情况。

<a name="format"></a>
### 支持格式

- 本地：AVI、MP4、MKV 和 FLV 格式的文件。
- 在线：RTMP 流和 RTSP 流。

<div class="alert note">目前只支持播放采样率为 32、44.1 或 48 kHz 的单或双声道媒体文件。</div>

### 使用指南

使用媒体播放器组件时，你可以开始/暂停播放，调节播放进度，调节播放音量，选择是否将播放的媒体文件发布给远端用户观看：

- 你可以将媒体播放器组件播放的媒体文件分发给远端用户。在这种场景下，你可能需要使用双进程。一个进程里采集并发送你的直播视频，另一个进程里采集并发送播放器播放的文件。

- 你也可以将媒体播放器组件作为你的本地播放器使用，本文对这种情况不多加讨论。

## 快速使用媒体播放器组件

### 前提条件

- 操作系统 Windows 7 及以上。
- 开发工具 Microsoft Visual Studio 2015 及以上。

### 使用 MediaPlayerKitQuickstart

1. 下载并解压 [MediaPlayerKitQuickstart](https://github.com/AgoraIO/Advanced-Video/tree/dev/backup/MediaPlayer/Mediaplayer-Windows) 文件。
2. 使用 Microsoft Visual Studio 打开 `MediaPlayerKitQuickstart.sln` 文件。
3. 点击**生成** > **生成解决方案**。
4. 编译成功后，点击**调试** > **开始调试**。
5. 调试成功后，你可以看到一个播放着视频的窗口。

你可以在 “`.cpp`” 文件如下代码里修改视频路径，让媒体播放器组件播放你想要的视频：

``` C++
//视频路径
std::string path = "F://1080.mp4";
  char *videopath = (char *)path.data();
  mediaPlayerKit->load(videopath, true);
```

## 实现媒体播放器组件

### 前提条件

- 操作系统 Windows 7 及以上。
- 开发工具 Microsoft Visual Studio 2015 及以上。

### 集成媒体播放器组件

**1. 准备工作**

- 下载并解压 Agora Native SDK（详见[下载](https://docs.agora.io/cn/Agora%20Platform/downloads)专区的**视频通话/视频互动直播 SDK**）
- 请确保你已完成 Agora Native SDK 的集成工作，详见[集成客户端](../../cn/Audio%20Broadcast/start_live_windows.md)。
- 下载并解压 MediaPlayerKit 文件。

**2. 创建项目**

<div class="alert warning"> 在此步，你应该基于已有的集成了 Agora Native SDK 的项目继续集成 MediaPlayerKit 的项目，而不是从头创建一个新项目。</div>

- 新建一个基于 **Visual C++** 的 **Windows 桌面应用程序**，点击**确定**。
- 在这个空项目中点击**生成** > **生成解决方案**。
- 编译成功后，点击**调试** > **开始调试**。
- 调试成功后，开始下一步。

**3. 导入文件**
- 将 `MediaPlayerKit` 和 Agora Native SDK 的 `sdk` 文件夹拷贝到你的项目文件中。
- 点击**源文件**，右键**添加** > **现有项目**，导入 `MediaPlayerKit/videokit` 文件。

**4. 导入路径**
点击你的项目文件，右键点击**属性**。
- 点开 **C/C++** > **常规**，在**附加包含目录** 中填入以下路径：
	- `./sdk/include;`
	- `./MediaPlayerKit/include;`
	- `./MediaPlayerKit/videokit;`
- 点开**链接器** > **常规**，在**附加库目录**中填入以下路径：
	- `./MediaPlayerKit/lib;`
	- `./sdk/lib;`
- 点开**链接器** > **输入**，在**附加依赖项**中填入以下路径：
	- `./MediaPlayerKit/lib/re_sampler.lib;`
	- `./MediaPlayerKit/lib/MediaPlayerKit.lib;`
	- `./sdk/lib/agora_rtc_sdk.lib;`

**5. 编译项目**
- 点击你的项目文件，右键点击**属性**，点开 **C/C++** > **预编译头**，在**预编译头**中选择**不使用预编译头**。
- 点击**生成** > **生成解决方案**

编译成功后，你可以参考[调用媒体播放器组件接口](#1)或 [API 文档](#2)完成你的项目。

<a name="1"></a>
### 调用媒体播放器组件接口


**API 时序图**

![](https://web-cdn.agora.io/docs-files/1567498507910)

请确保你已使用 Agora Native SDK 在你的项目中完成基本的实时音视频功能，详见[实现互动直播](../../cn/Audio%20Broadcast/start_live_windows.md)。

**准备工作**

1. 调用 `createMediaPlayerKitWithRtcEngine` 方法创建引擎。

2. 调用 `setEventHandler` 和 `setMediaInfoCallback` 方法设置事件回调和媒体信息回调。

3. 调用 `setVideoView` 方法设置本地视图。

4. 调用 `load` 方法将视频载入内存。
> 你可以按照自己的需求，从本地路径或 URL 路径载入视频。只要它符合视频格式，详见[支持格式](#format)。

```C++
mediaPlayerKit = createMediaPlayerKitWithRtcEngine(rtcEngine,sampleRate);
mediaPlayerKit->setEventHandler(handler);
mediaPlayerKit->setMediaInfoCallback(infoCallback);
mediaPlayerKit->setVideoView(hwnd);
mediaPlayerKit->load(url.toUtf8().data(),true);
```

**本地播放**

调用 `play`、`adjustPlaybackSignalVolume`、`seekTo`、`getCurrentPosition`、`pause` 和 `resume` 等方法实时调节播放器。

```C++
mediaPlayerKit->play();
mediaPlayerKit->adjustPlaybackSignalVolume(volume);
mediaPlayerKit->seekTo(msec);
mediaPlayerKit->getCurrentPosition();
mediaPlayerKit->pause();
mediaPlayerKit->resume();
...
```

**分享到远端**

1. 调用 `publishVideo` 或 `publishAudio` 方法将媒体文件的视频流或音频流分享给 Agora 频道内的远端用户。

2. 调用 `adjustPublishSignalVolume` 方法调节远端用户接收到的音量。

```C++
mediaPlayerKit->publishVideo();
mediaPlayerKit->publishAudio();
mediaPlayerKit->adjustPublishSignalVolume(volume);
```

**取消分享**

调用 `unpublishVideo` 或 `unpublishAudio` 方法即可停止把视频或音频流发布给远端用户。

```C++
mediaPlayerKit->unpublishVideo();
mediaPlayerKit->unpublishAudio();
```

**结束播放**

1. 调用 `stop` 方法停止播放视频。

2. 调用 `unload` 方法从释放加载到内存的视频。

3. 调用 `destroy` 方法销毁一个 MediaPlayerKit 实例并退出媒体播放器组件。

```C++
mediaPlayerKit->stop();
mediaPlayerKit->unload();
mediaPlayerKit->destroy();
```


<a name="2"></a>
## API 文档
详见 [API 文档](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/mediaplayer_cpp/index.html)

## 注意事项

如果你用媒体播放器组件播放 URL 路径的视频，遇到因为网络问题而导致的媒体播放器组件播放错误后，你需要在网络情况好转后，调用 `load` 方法重新载入视频。
