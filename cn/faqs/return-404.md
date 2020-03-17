
---
title: 为什么成功开启云端录制后调用 query 方法返回 404？
description: 
platform: All Platforms
updatedAt: Mon Dec 16 2019 09:16:15 GMT+0800 (CST)
---
# 为什么成功开启云端录制后调用 query 方法返回 404？
通过 `start` 方法成功开启云端录制后，调用 `query` 方法返回 404 状态码，可能原因如下：

- 云端录制启动后，录制服务会进行参数检查，如果检查出现问题，则有可能导致录制停止。请检查 `transcoding` 等参数设置是否正确。你可以参考[如何设置录制视频的分辨率](https://docs.agora.io/cn/faq/recording_video_profile)设置 `transcoding`。
- 第三方云存储信息有误，如 `accessKey` 或 `secretKey` 错误，导致录制文件上传失败。
- `clientRequest`  中的 `token` 与待录制的频道使用的动态秘钥不一致，导致云端录制无法加入频道。
- 云端录制启动后，频道内没有用户发流，当超过 `maxIdleTime` 即最长空闲频道时间后，云录制会自动退出。
- 云端录制服务器断网或进程被杀。此时，调用 `query`、`updateLayout`、或 `stop` 均会返回 404。云端录制的故障处理中心会在 90 秒内判断故障原因，并采取相应的应对措施。你可以在一段时间后再次调用 `query` 方法，查询录制服务是否已恢复。详情见[云端录制服务器断网、进程被杀的处理](../../cn/faq/high-availability.md)。
