
---
title: 发版说明
description: 
platform: Web
updatedAt: Sun Apr 28 2019 06:02:48 GMT+0800 (CST)
---
# 发版说明
## 简介

作为声网信令 SDK 的升级换代产品，Agora RTM SDK 提供了更为简洁的逻辑实现和更为稳定的消息传送机制，帮助开发者快速搭建实时消息应用场景。

### 优势

- 支持发送点对点消息和频道消息。 
- 支持多实例，允许在一个线程中调用多个 RTM 服务。 
- 对错误码进行分类方便快速定位问题。

更多产品功能及性能介绍，详见： [产品概述](../../cn/Real-time-Messaging/RTM_product.md) 。

## 0.9.0 版

该版本于 2019 年 2 月 4 日发布。

首次发布。

关键功能

- 发送或接收点对点消息。
- 加入或离开频道。
- 发送或接收频道消息。

> 本版本默认上传日志到服务端。如需禁用服务器上传，请在调用 `createInstance` 方法时将 `enableLogUpload` 设为 `false` 。
> ```JavaScript
> AgoraRTM.createInstance('demoAppId', { enableLogUpload: false });
> ```
