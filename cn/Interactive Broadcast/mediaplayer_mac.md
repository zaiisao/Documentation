
---
title: 媒体播放器组件
description: 
platform: macOS
updatedAt: Tue Dec 31 2019 09:48:42 GMT+0800 (CST)
---
# 媒体播放器组件
## 功能描述

当你使用 Agora Native SDK 实现音视频互动直播时，如果你想将本地视频文件或在线视频流经过 SD-RTN 分发给远端用户观看，那么你需要使用媒体播放器组件播放这个视频。主播可以通过媒体播放器组件实时调节视频的播放情况。

<a name="format"></a>
### 支持格式

媒体播放器组件支持播放以下视频：
- 本地视频：文件格式为 AVI、MP4、MKV 和 FLV。
- 在线视频：RTMP 流和 RTSP 流。

<div class="alert note">目前只支持播放采样率为 32 kHz、44100 Hz 或 48 kHz 的单/双声道视频。</div>

### 使用指南

使用媒体播放器组件时，你可以开始/暂停播放视频，调节播放进度，调节播放音量，选择是否将播放的视频发布给远端用户观看：

- 你可以将媒体播放器组件播放的视频分发给远端用户。在这种场景下，你可能需要使用双进程。一个进程里采集并发送你的直播视频，另一个进程里采集并发送播放器播放的视频。如果你只以一个进程加入频道，那么远端用户只能看到与这个进程对应的一个视频窗口。

- 你也可以将媒体播放器组件作为你的本地播放器使用，本文对这种情况不多加讨论。

## 快速使用媒体播放器组件

### 前提条件

- Xcode 10.12 及以上。
- 系统 macOS 10.12 及以上的真机。
- 请确保你的项目已设置有效的开发者签名。


### 快速体验媒体播放器组件

1. 下载并解压 [MediaPlayerKitQuickstart](https://github.com/AgoraIO/Advanced-Video/tree/master/MediaPlayer/Mediaplayer-Mac) 文件。
2. 使用 Xcode 打开 `MediaPlayerKitQuickstart.xcodeproj` 文件。
3. 点击编译。
4. 编译成功后，你可以看到弹出一个新界面。
> 如果编译出错，请检查是否已设置有效的开发者签名。
5. 点击界面上的 **Settings**，设置远端用户看到的视频画质，然后点击 **Confirm**。
6. 填入 **Channel Name**，选择 **Join as Broadcaster**，随后你可以看到媒体播放器组件的播放界面。
![](https://web-cdn.agora.io/docs-files/1567481228573)

7. 在界面的下方你可以从左到右看到以下图标：开始/暂停播放，进度条，音量栏，**PUBLISH STREAMING** 和设置。
8. 点击此界面的文字框（位于下载图标下），你可以选择本地路径导入视频文件，并将视频发布到远端（默认）。

### 注意事项
MediaPlayerKitQuickstart 只支持从本地导入视频，不能通过 URL 地址导入在线视频。

## 实现媒体播放器组件

### 前提条件

- Xcode 10.12 及以上。
- 系统 macOS 10.12 及以上的真机。
- 请确保你的项目已设置有效的开发者签名。

### 集成媒体播放器组件  

**1. 准备工作**

- 下载并解压 Agora Native SDK（详见[下载](https://docs.agora.io/cn/Agora%20Platform/downloads)专区的**视频通话/视频互动直播 SDK**）
- 请确保你已经完成 Agora Native SDK 的集成工作，详见[集成客户端](../../cn/Interactive%20Broadcast/mac_video.md)。
- 下载并解压 MediaPlayerKit 文件。

**2. 创建项目**
<div class="alert warning">在此步，你应该基于已有的集成了 Agora Native SDK 的项目继续集成 MediaPlayerKit 的项目，而不是从头创建一个新项目。此处仅是展现一个新建项目的步骤，如果你不需要参考，可以略过此步。</div>

- 打开 Xcode，**Create a new Xcode project**。
- 在**Choose a template for your new project** 页面上选择 **macOS** 和 Application 下的 **cocoa App**，点击 **Next**。
- 填入你的 **Product Name**，比如 “MediaPlayer”，选择 **Language** 为 Objective-C，点击 **Next**。
- 选择存放项目文件的本地路径，比如 `/Users/xxx/Desktop`，点击 **Create**，随后你可以看到你的项目页面。

<a name="import"></a>
**3. 导入文件**
- 在 “MediaPlayer” **项目文件**处右键，点击 **Add Files to "MediaPlayer"**。
	- 导入 `AgoraRtcEngineKit.framework` 文件：
		- 本地路径选择此文件；
		- **Destination** 选择 **Copy items if needed**；
		- **Added folder** 选择 **Create groups**；
		- 点击 **Add**。
	- 导入 `MediaPlayerKit.framework` 文件：
       - 导入方法同上。
- 在 “MediaPlayer” **文件**处右键，点击 **Add Files to "MediaPlayer"**。
	- 导入 `Agora_MediaPlayer_Kit` 文件：
     	- 导入方法同上。

**4. 导入库**
- 点击 **Build Phases**，展开 **Link Binary With Libraries**，可以看到导入文件后已经填入的库：
	- `MediaPlayerKit.framework`
	- `liblibyuv.a`
	- `AgoraRtcEngineKit.framework`

> 请确保这三个库已经添加，否则你需回到[步骤 3](#import)。

- 点击 “**+**” 号添加以下 21 个库：
	- `Carbon.framework`
	- `ForceFeedback.framework`
	- `IOKit.framework`
	- `libbz2.tbd`
	- `libiconv.2.4.0.tbd`
	- `QuartzCore.framework`
	- `SecurityFoundation.framework`
	- `CoreImage.framework`
	- `OpenGL.framework`
	- `OpenCL.framework`
	- `CoreVideo.framework`
	- `SystemConfiguration.framework`
	- `libresolv.tbd`
	- `libc++.tbd`
	- `libz.tbd`
	- `CoreAudio.framework`
	- `CoreWLAN.framework`
	- `CoreMedia.framework`
	- `AudioToolbox.framework`
	- `VideoToolbox.framework`
	- `AVFoundation.framework`

**5. 配置库路径**
- 找到 **Header Search Paths**：
	- 点击 **Build Settings**；
	- 选择 **All** 和 **Levels**；
	- 在搜索栏中填入 **Header Search Paths**。

- 点开 **Header Search Paths** 并添加 `libyuv.h` 文件的路径:
	- 在项目页面左侧展开并找到 “MediaPlayer” **文件**下的 **Agora_MediaPlayer_Kit** > **third_party** > **libyuv** > **include** > **libyuv.h** 文件；
	- 在此处右键选择 **Show in Finder**，你可以看到 `libyuv.h` 文件本地绝对路径为 /Users/xxx/Desktop/MediaPlayer/MediaPlayer/Agora_MediaPlayer_Kit/third_party_libyuv/include；
	> `/Users/xxx/Desktop/MediaPlayer` 即为你的项目路径`$(PROJECT_DIR)`。

	- 填入`libyuv.h` 文件本地相对路径： `$(PROJECT_DIR)/MediaPlayer/Agora_MediaPlayer_Kit/third_party_libyuv/include`。
	

**6. 编译项目** 
- 点击编译。
- 编译成功后，弹出一个窗口。

你可以参考[调用媒体播放器组件接口](#1)或 [API 文档](#2)完成你的项目。

<a name="1"></a>
### 调用媒体播放器组件接口

**API 时序图**
![](https://web-cdn.agora.io/docs-files/1567498466302)

> - 图中仅展示最基本的接口，如果你有更复杂的需求，也可调用 Agora Native SDK 里更多的接口，比如[`enableDualStreamMode`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)。
> - 因为 MediaPlayer Kit 接口中的 `setVideoView` 可以设置本地视频窗口，所以你无需事先调用 Agora Native SDK 接口中的 [`setupLocalVideo`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a744003a9c0230684e985e42d14361f28)。

1. 调用 Agora Native SDK 接口完成初始化和基本的视频设置。
	- 调用 `shareEngineWithAppId` 方法初创建并始化一个 AgoraRtcEngineKit 单实例。详见[实现方法](https://docs.agora.io/cn/Interactive%20Broadcast/initialize_mac_live?platform=macOS#实现方法)。
	- 调用 `setChannelProfile` 方法设置频道模式为直播模式。详见[设置频道模式实现方法](https://docs.agora.io/cn/Interactive%20Broadcast/join_live_mac?platform=macOS#设置频道模式为直播)。
	- 调用 `setClientRole` 方法，根据需要将用户设置为主播。
	```Objective-c
	[self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
	```
	- 调用 `enableVideo` 方法打开视频模式。详见[打开视频模式](https://docs.agora.io/cn/Interactive%20Broadcast/publish_mac_live?platform=macOS#打开视频模式)。

2. 调用 MediaPlayer Kit 接口完成初始化和其他准备工作。

	- 调用 `shareInstance` 和 `createMediaPlayerKitWithRtcEngine()`方法初始化一个 MediaPlayer Kit 单实例。
	```Objective-c
	 [[MediaPlayerKit shareInstance] createMediaPlayerKitWithRtcEngine:agoraKit withSampleRate:44100];
	```

	- 调用 `MediaPlayerKitDelegate` 方法设置代理方法。详见 [API 文档](#2)。
	```Objective-c
	 [MediaPlayerKit shareInstance].delegate = self;
	```
	- 调用 `setVideoView` 方法设置本地视图。
	```Objective-c
	 [[MediaPlayerKit shareInstance] setVideoView:self.view];
	```
	
3. 调用 Agora Native SDK 接口完成频道管理。
	- 调用 `joinChannelByToken` 方法加入频道。详见[加入主播频道](https://docs.agora.io/cn/Interactive%20Broadcast/join_live_mac?platform=macOS#加入直播频道)。
> 你需要在 `setVideoView` 后调用本方法。

	- 调用 [`setupRemoteVideo`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupRemoteVideo:) 方法设置远端用户视图。

4. 调用 MediaPlayer Kit 接口载入视频。
	- 调用 `load` 方法将视频载入内存。
> 你可以按照自己的需求，从本地路径或 URL 路径载入视频。只要它符合视频格式，详见[支持格式](#format)。
> 
	```Objective-c
	[[MediaPlayerKit shareInstance] load:url isAutoPlay:true/false];
	```

<a name="3"></a>
5. 调用 MediaPlayerKit 接口完成媒体播放器功能。
	- 调用 `play`、`adjustPlaybackSignalVolume`、`seekTo`、`getCurrentPosition`、`pause` 和 `resume` 等方法实时调节播放器。
> 你可以参考 [API 文档](#2)了解这些方法。
>
```Objective-c
 [[MediaPlayerKit shareInstance] play];
 [[MediaPlayerKit shareInstance] adjustPublishSignalVolume:volume];
 [[MediaPlayerKit shareInstance] seekTo:msec];
 [[MediaPlayerKit shareInstance] getCurrentPosition];
...
```

6. 调用 MediaPlayer Kit 接口将音视频发布给远端用户观看。
	- 如果你只想将**音频**发布给远端用户：
		- 调用 `publishAudio` 方法发布音频。
	```Objective-c
	 [[MediaPlayerKit shareInstance] publishAudio];
	```
		- 使用[步骤5](#3) 中的接口实时调节播放器。

		- 调用 `adjustPublishSignalVolume` 方法调节远端用户接收到的音量。
	```Objective-c
	 [[MediaPlayerKit shareInstance] adjustPublishSignalVolume:<#(int)#>];
	```

		- 调用 `unpublishAudio` 方法即可停止把音频发布给远端用户。
	```Objective-c
	[[MediaPlayerKit shareInstance] unpublishAudio];
	```

	- 如果你想将**视频**发布给远端用户：
		- 调用 `publishVideo` 方法发布视频。
	```Objective-c
	 [[MediaPlayerKit shareInstance] publishVideo];
	```

		- 使用[步骤5](#3) 中的接口实时调节播放器。

		- 调用 `adjustPublishSignalVolume` 方法调节远端用户接收到的音量。
	```Objective-c
	 [[MediaPlayerKit shareInstance] adjustPublishSignalVolume:<#(int)#>];
	```

		- 调用 `unpublishVideo` 方法即可停止把视频发布给远端用户。
	```Objective-c
	 [[MediaPlayerKit shareInstance] unPublishVideo];
	```

7. 调用 MediaPlayer Kit 接口停止播放并退出媒体播放器。
	- 调用 `stop` 方法停止播放视频。
	```Objective-c
	[[MediaPlayerKit shareInstance] stop];
	```

	- 调用 `unload` 方法从释放加载到内存的视频。
	```Objective-c
	 [[MediaPlayerKit shareInstance] unload];
	```

	- 调用 `destroy` 方法销毁一个 MediaPlayerKit 实例并退出媒体播放器组件。
> 调用 `destroy` 方法后， 取消绑定本地视频流的显示视图。
>
```Objective-c
 [[MediaPlayerKit shareInstance] destroy];
```


<a name="2"></a>
## API 文档
详见 [API 文档](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/mediaplayer_oc/docs/headers/MediaPlayer-Kit-Objective-C-API-Overview.html)

## 注意事项

如果你用媒体播放器组件播放 URL 路径的视频，遇到因为网络问题而导致的媒体播放器组件播放错误后，你需要在网络情况好转后，调用 `load` 方法重新载入视频。
