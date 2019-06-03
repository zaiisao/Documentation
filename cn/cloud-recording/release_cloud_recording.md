
---
title: 云端录制发版说明
description: 
platform: Linux
updatedAt: Mon Jun 03 2019 02:05:35 GMT+0800 (CST)
---
# 云端录制发版说明
## 简介

Agora 云端录制，可为 Agora 实时音视频提供录制服务。同 Agora 本地服务端录制相比，Agora 云端录制无需部署 Linux 服务器，减轻了研发和运维的压力，更轻量便捷。点击[云端录制产品概述](../../cn/cloud-recording/product_cloud_recording.md)了解关键特性。

### 兼容性

Agora 云端录制与以下 Agora SDK 兼容:

| SDK              | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| Agora Native SDK | 云端录制与全平台 Agora Native SDK 1.7.0 或更高版本兼容，如果频道内有任何人使用了 1.6 版本的 Agora Native SDK， 则整个频道无法录制。 |
| Agora Web SDK    | 云端录制 与 Agora Web SDK 1.12.0 或更高版本兼容。            |

##  1.0.0 版

该版本于 2019 年 4 月 29 日发布。本次发版为云端录制的第一次发版，主要包括以下功能:

- Agora Native SDK 和 Agora Web SDK 的高清音视频通话的录制
- 频道内所有用户的音视频合流录制
- 支持 3 种合流布局样式：Float（默认布局）、BestFit（自适应布局）、Vertical（垂直布局）。具体可参考 [TranscodingConfig](../../cn/cloud-recording/cloud_recording_api.md) 表格中的描述。
- 第三方云存储，目前支持七牛云、阿里云和 Amazon S3
- 提供 C++ 和 Java SDK 包
