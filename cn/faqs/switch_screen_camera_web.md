
---
title: 如何切换屏幕共享流和摄像头视频流？
description: 
platform: Web
updatedAt: Mon Dec 09 2019 10:24:41 GMT+0800 (CST)
---
# 如何切换屏幕共享流和摄像头视频流？
本文介绍在使用 Agora Web SDK 实现[屏幕共享](https://docs.agora.io/cn/Interactive%20Broadcast/screensharing_web?platform=Web)的场景中，如何在发布的屏幕共享流和摄像头采集的视频流之间进行切换。

屏幕共享流和摄像头采集的视频流互相切换的方法是一样的，下面的介绍均以从屏幕共享流切换到摄像头视频流为例。

## 方案一：创建两路流

发送端创建两个 Client 对象，对应两路流：屏幕共享流和本地摄像头视频流。具体的实现方法请参考[同时共享屏幕和开启视频](https://docs.agora.io/cn/Interactive%20Broadcast/screensharing_web?platform=Web#a-namebotha同时共享屏幕和开启视频)。

在需要从屏幕共享流切换到摄像头视频流时，接收端选择订阅摄像头视频流即可。

如果你的应用场景允许同时创建两路流，我们推荐使用这种方案。

## 方案二：关闭当前流再新建流发布

1. 依次调用 `Stream.unpublish` 和 `Stream.close` 取消发布并关闭当前屏幕共享流。
2. 调用 `AgoraRTC.createStream` 新建本地视频流。
3. 依次调用 `Stream.init` 和 `Stream.publish` 初始化并发布视频流。

方案二的兼容性和稳定性较好，缺点是无法动态切换，切换时间较长，可能需要一到两秒的时间。

## 方案三：替换当前视频轨道

调用 `Stream.replaceTrack` 方法将本地音视频流中的视频轨道替换为屏幕共享流。

方案三可以实现动态切换且切换速度快，但是 `replaceTrack` 方法限制较多：

- 仅支持 Chrome 65+，Safari 以及最新版 Firefox 浏览器。
- 部分移动设备上可能不生效。
- 在双流模式下无法切换到屏幕共享流。
