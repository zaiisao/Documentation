
---
title: 你们提到的 qps 是针对单个 SDK 的还是针对单个实例？
description: 
platform: All Platforms
updatedAt: Thu Feb 13 2020 15:23:23 GMT+0800 (CST)
---
# 你们提到的 qps 是针对单个 SDK 的还是针对单个实例？
qps 是 queries per second 的缩写。所有的 qps 都是针对单个 RtmClient 实例而言，而非针对单个 Agora RTM SDK。

- 在 Native 平台，你可以通过创建多实例提高 API 的调用频率限制；
- 在 Web 平台，我们*暂不建议*通过创建多实例提高 API 的调用频率
