
---
title: 水晶球更新历史
description: 
platform: All Platforms
updatedAt: Fri Sep 25 2020 07:13:49 GMT+0800 (CST)
---
# 水晶球更新历史
## 简介
水晶球（Agora Analytics）是 Agora 为开发者提供的全周期通话质量监测、回溯和分析的解决方案，致力于帮助你及时发现问题、定位原因，并最终解决问题以提升用户体验。详见[水晶球概览](../../cn/Agora%20Platform/aa_guide.md)。

## 2020.09

**新增特性**

#### 1. 大频道 RESTful API (Beta)

该版本新增大频道 RESTful API，你可以获取以下数据：

- 大频道中体验异常的用户数。体验异常包含视频卡顿、加入大频道慢等。
- 尝试加入大频道的用户数。
- 大频道中用户对通话的评分。
- 大频道的在线用户数。

<div class="alert note">使用大频道 RESTful API 前，你需要联系 <a href="mailto:sales@agora.io">sales@agora.io</a > 开通大频道功能。详见<a href="../../cn/Agora%20Platform/aa_big_channel.md">大频道</a >和 <a href="https://docs.agora.io/cn/Agora%20Platform/aa_api?platform=All%20Platforms#big_channel">大频道 RESTful API</a >。</div>

**改进**

#### 1. 通话调查

- **通话体验质量页面**的事件轴移至**端到端详情页面**的**音视频上行和网络丢包**、**音视频下行和端到端丢包**面板。

- **端到端详情页面**新增以下指标：

   - 视频大流发送码率
   - 视频小流发送码率
   - 视频采集帧率
   - 视频发送分辨率

- 支持在**通话概览页面**一键创建工单。

- 优化 UI 样式。

#### 2. 其他改进

除 RESTful API 外，水晶球各功能默认使用本地时间。

## 2020.06

本次发布新增水晶球内嵌功能（Beta），帮助开发者快速在自己的系统中嵌入水晶球功能，减少对接成本并提升问题调查效率。详见[水晶球内嵌](../../cn/Agora%20Platform/aa_embeded.md)。

## 2020.05

本次发布新增实时预警功能（Beta），实现实时异常识别、根因分析，帮助客户及时定位异常源。详见[实时预警](../../cn/Agora%20Platform/aa_realtime_alarm.md)。

## 2020.01

本次发布新增大频道功能（Beta），实现频道级监控、异常识别，能极大提高用户运营大频道的效率。详见[大频道](../../cn/Agora%20Platform/aa_big_channel.md)。

## 2019.10

本次发布新增实时数据功能（Beta），帮助用户实时掌握项目动态，及时识别异常通话体验和异常根因。详见[实时数据](../../cn/Agora%20Platform/aa_live_data.md)。

## 2019.09

本次发布新增数据洞察功能（Beta），提供整体、地域、版本的质量监控，用户可以历时分析通话规模和质量。详见[数据洞察](../../cn/Agora%20Platform/aa_data_insight.md)。

## 2019.08

本次发布新增关键事件功能，提升问题调查效率。

## 2018.08

本次发布提供通话调查功能，帮助用户查找通话并分析质量问题。详见[通话调查](../../cn/Agora%20Platform/aa_call_search.md)。
