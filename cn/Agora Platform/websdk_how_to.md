
---
title: Web SDK 相关
description: 
platform: Web SDK 相关
updatedAt: Thu Nov 29 2018 02:43:42 GMT+0000 (UTC)
---
# Web SDK 相关
### 视频过程中，如何更换音视频输入设备？
通过枚举系统设备接口 `getDevices` 可以获取用户的音视频输入输出设备。很多用户想要不选择设备，直接用默认识别到的第一个，可以直接在 `createstream` 接口中把 `cameraId`、`microphoneId` 填为""。用户也可以自己创建对象数组来管理本地设备（通过 kind 中的类型分类）。请注意 `cameraId` 和 `microphoneId` 不是唯一确定的而是随机生成的，部分情况下这个 ID 可能改变。所以目前音视频中途更换设备，需要关掉流之后，重新 `getDevice`，来建立新流。

### 如何开启双流模式？
开启双流模式 接口 `enableDualStream`，发送端调用这个接口开启双流，接收端订阅了流之后用 `setRemoteVideoStreamType()` 随时切换大小流。小流参数可用 `setLowStreamParameter()` 来设置。
