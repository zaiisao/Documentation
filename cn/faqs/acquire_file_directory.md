
---
title: 如何获取 M3U8 文件地址
description: How to get M3U8 file directory
platform: All Platforms
updatedAt: Tue Nov 19 2019 10:41:06 GMT+0800 (CST)
---
# 如何获取 M3U8 文件地址
完整的 M3U8 文件地址由云存储空间外链域名和 M3U8 文件名组成，一般在你的第三方云存储里可以直接复制。

![](https://web-cdn.agora.io/docs-files/1561621201492)

你可以通过以下方式获得 M3U8 文件名信息：

- RESTful API
  - [`query`](../../cn/cloud-recording/cloud_recording_api_rest.md) 和 [`stop`](../../cn/cloud-recording/cloud_recording_api_rest.md) 的响应中 `fileList` 字段
  - 回调事件 [`cloud_recording_file_infos`](../../cn/cloud-recording/cloud_recording_callback_rest.md) 的 `fileList` 字段
- C++
  - [`OnRecordingUploadingProgress`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `recording_playlist_filename` 参数
  - [`OnRecordingUploaded`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `file_name` 参数
  - [`OnRecordingBackedUp`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `file_name` 参数
- Java
  - [`onRecordingUploadingProgress`](../../cn/cloud-recording/cloud_recording_api_java.md) 中的 `recording_playlist_filename` 参数
  - [`onRecordingUploaded`](../../cn/cloud-recording/cloud_recording_api_java.md) 中的 `file_name` 参数
  - [`onRecordingBackedUp`](../../cn/cloud-recording/cloud_recording_api_java.md) 中的 `file_name` 参数
