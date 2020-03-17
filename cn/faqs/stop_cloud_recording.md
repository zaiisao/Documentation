
---
title: 如何停止云端录制
description: 描述如何停止云端录制
platform: All Platforms
updatedAt: Tue Nov 19 2019 10:41:23 GMT+0800 (CST)
---
# 如何停止云端录制
你可以调用  [`stop`](../../cn/faqs/cloud_recording_api_rest.md) 方法离开频道，停止录制。

当频道空闲（无用户）超过预设的时间（默认 30 秒）后，云端录制也会自动退出频道停止录制。你可以在开始录制的时候设置 `maxIdleTime` 参数来控制超时退出的时间。
