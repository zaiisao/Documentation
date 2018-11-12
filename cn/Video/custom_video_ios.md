
---
title: 客户端自定义采集和渲染
description: 
platform: iOS
updatedAt: Mon Nov 12 2018 02:54:09 GMT+0000 (UTC)
---
# 客户端自定义采集和渲染
## 场景描述

实时通讯过程中，Agora SDK 通常会启动默认的内置的摄像头进行视频推流，以及默认的视频渲染器进行视频渲染。如果需要自定义视频源及视频渲染器，则可以通过参考本文中的步骤调用 API 实现。一般来讲，本章内容适合以下场景：

- SDK 内置的 Camera 视频源功能不能满足开发者需求时，比如需要使用自定义的美颜库或者前处理库
- 开发者 App 中已经有自己的图像视频模块，为了复用代码可以自定义视频源
- 开发者希望使用非 Camera 的视频源，比如录屏数据
- 有些系统独占的视频采集设备，为了避免与其他业务冲突，需要灵活得设备管理策略

## 集成 SDK

详见 [集成客户端](../../cn/Video/ios_video.md) 。

## 实现自定义视频源

步骤 1：实现 `AgoraVideoSourceProtocol `接口，构建自定义的 Video Source 类

1. 在 获取 Buffer 类型 \(`bufferType`\) 的实现中指定视频采集使用的 Buffer 类型

	```c++
	- (AgoraVideoBufferType)bufferType;
	```

2. 在 初始化视频源 \(`shouldInitialize`\) 中准备好系统环境，进行初始化参数等设置

	```c++
	- (BOOL)shouldInitialize;
	```

3. 在 启动视频源 \(`shouldStart`\) 中开始采集视频数据

	```c++
	- (void)shouldStart;
	```

4. 将采集到的数据通过 `AgoraVideoFrameConsumer` 接口传给 Media Engine
5. 在 停止视频源 \(`shouldStop`\) 中停止采集视频数据

	```c++
	- (void)shouldStop;
	```

6. 在 释放视频源 \(`shouldDispose`\) 中释放硬件等系统资源

	```c++
	- (void)shouldDispose;
	```

步骤 2：创建自定义的视频源对象

步骤 3：通过 Media Engine 的 设置视频源 \(`setVideoSource`\) 方法把自定义的视频源对象设置给 Media Engine

```c++
- (void)setVideoSource:(id<AgoraVideoSourceProtocol>_Nullable)videoSource;
```

步骤 4：Media Engine 会在适当的实际调用自定义视频源中实现的 `AgoraVideoSourceProtocol` 的方法

## 实现自定义渲染器

步骤 1：根据实际需要的数据类型和格式，实现 获取 Buffer 类型 \(`bufferType`\) 和 获取像素格式 \(`pixelFormat`\) ，设置数据帧的类型和格式

步骤 2：依次实现 初始化渲染器 \(`shouldInitialize`\)、 启动渲染器 \(`shouldStart`\)、 停止渲染器 \(`shouldStop`\) 和 释放渲染器 \(`shouldDispose`\) ，实现对自定义渲染器的状态控制

步骤 3：根据步骤 1 中 Buffer Type 定义的类型，选择实现对应的 Render 方法，用于获取视频帧数据

步骤 4：创建自定义的渲染对象

步骤 5：通过 Media Engine 的 设置本地视频渲染器 \(`setLocalVideoRenderer`\) 和 设置远端视频渲染器 \(`setRemoteVideoRenderer`\) 方法，设置用于本地渲染或者对方图像渲染

步骤 6：Media Engine 会再根据内部状态调用 `AgoraVideoSinkProtocol` 中定义的方法。

## 示例程序

声网 Agora 目前提供自定义视频源及渲染器的示例程序，请前往 Github 下载 [Agora Custom Media Device Sample App for iOS](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-iOS) 并体验。
