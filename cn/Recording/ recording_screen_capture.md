
---
title: 截屏
description: 
platform: All Platforms
updatedAt: Wed Aug 14 2019 10:43:34 GMT+0800 (CST)
---
# 截屏

Agora 本地服务端录制 SDK 目前仅支持单流模式下截屏，无需转码。

录制模式与录制内容的设置，详见[使用录制模式](../../cn/Recording/recording_mode.md)。

| **录制内容** | **参数设置**                              | **生成文件**                                                 |
| ------------ | ----------------------------------------- | ------------------------------------------------------------ |
| 仅录制视频   | decodeVideo = 3 或 4，captureInterval = 1 | <li>jpg 文件<li>jpg 缓存                                     |
| 边录制边截屏 | decodeVideo = 5，captureInterval = 1      | <li>Native 端每个 UID 生成一个 mp4 文件（转码前）<li>Web 端每个 UID 生成一个 webm 文件（转码前）<li>jpg 文件 |

`captureInterval` 参数可以设置截屏的时间间隔，最小值为 1 秒，默认值为 5 秒。
