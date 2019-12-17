
---
title: 发版说明
description: 
platform: All Platforms
updatedAt: Tue Dec 17 2019 08:26:07 GMT+0800 (CST)
---
# 发版说明
## 简介
实时码流加速（Real-Time Streaming Acceleration, RTSA）提供 API，帮助自研音视频编码的用户实现高联通性、高实时性、高稳定性的码流传输。

## 1.2.1 版
该版本于 2019 年 8 月 15日发布。改进如下。

**改进**
- 增强 String UID 的注册刷新/失效/一致性检测。
- 完善异常数据包过滤。

## 1.2.0 版
该版本于 2019 年 8 月 9 日发布。新增功能和修复问题如下。

**新增功能**
#### 支持 String 型用户名
v1.2.0 新增 `agora_rtc_init_with_name` 方法，支持用户使用 String 型用户名作为用户标识进行初始化。

**修复问题**
- P2P 链路可通但链路状态拥塞时，消除反复 P2P 连接尝试造成带来的数据传输损伤。
