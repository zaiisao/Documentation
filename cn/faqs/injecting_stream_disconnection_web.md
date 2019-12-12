
---
title: 在输入在线媒体流过程中，连接断开后如何处理？
description: 
platform: Web
updatedAt: Mon Dec 02 2019 10:35:28 GMT+0800 (CST)
---
# 在输入在线媒体流过程中，连接断开后如何处理？

[输入在线媒体流](https://docs.agora.io/cn/Interactive%20Broadcast/inject_stream_web?platform=Web)是指将频道外的在线媒体流输入到正在进行的直播频道中。

输入在线媒体流时，SDK 会连接到专门用于拉取外部媒体流的 Agora 服务器。当 SDK 与拉流服务器的连接断开时会尝试自动重连。如果重连失败，SDK 会触发 `Client.on("inject-streaming-disconnected")` 回调。监听到该事件时，你可以先调用 [`removeInjectStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#removeinjectstreamurl) 停止拉流，再调用 [`addInjectStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#addinjectstreamurl) 重新开始拉流。
