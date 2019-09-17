
---
title: 在调用 Stream.init 时设备报错
description: 
platform: Web
updatedAt: Tue Sep 17 2019 10:27:43 GMT+0800 (CST)
---
# 在调用 Stream.init 时设备报错
以下为初始化音视频流时常见的设备错误：

- `NotAllowedError`：用户拒绝授予对应的摄像头或麦克风权限。
- `NotFoundError`：找不到指定的媒体流。检查你的麦克风和摄像头是否正常工作。
- `SecurityError`：没有使用 HTTPS 协议。如果需要打开摄像头，请务必使用 HTTPS 协议或者 localhost。

更多的错误请参考 [`Stream.init`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init) 中的描述。
