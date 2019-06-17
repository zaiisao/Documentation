
---
title: 云端录制 RESTful API 快速开始
description: Quick start for rest api
platform: All Platforms
updatedAt: Mon Jun 17 2019 06:55:22 GMT+0800 (CST)
---
# 云端录制 RESTful API 快速开始
Agora 云端录制服务提供 RESTful API，无需集成 SDK，直接通过网络请求开启和控制云录制，在自己的网页或应用中灵活使用。

> 你需要通过自己的 app server 发送网络请求。

![img](https://web-cdn.agora.io/docs-files/1559295245761)

通过 RESTful API，你可以发送网络请求控制云端录制：

- 开始/结束云端录制
- 查询当前的录制状态

同时云端录制 RESTful API 还提供回调服务，开通后你可以收到云端录制的事件通知，下表列出回调服务和使用 RESTful API 查询录制状态的区别，你可以根据自己的需要选择是否开通回调服务。

| 查询状态                                                     | 回调服务                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| 调用 [`query`](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法即可查询 | 需配置用于接收回调的 HTTP 服务器，并单独开通服务 |
| 需主动查询，无法保证及时性                                   | 事件发生时及时通知                               |
| 仅提供 M3U8 文件名和录制的状态                               | 提供云端录制所有的事件通知和具体信息             |

如需使用回调服务，请参考 [RESTful API 回调](../../cn/cloud-recording/cloud_recording_callback_rest.md)。

## 前提条件

请确保满足以下要求：

- 联系 [sales@agora.io](mailto:sales@agora.io) 开通云端录制服务。
- 开通第三方云存储服务，目前支持七牛云、阿里云（推荐）和 Amazon S3。

## 认证

云端录制 RESTful API 仅支持 HTTPS 协议。发送请求时，你需要提供 `api_key:api_secret` 通过 Basic HTTP 认证：

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

你可以在 Dashboard 的 [RESTful API](https://dashboard.agora.io/restful) 页面找到你的 Customer ID 和 Customer Certificate。

## 数据格式

所有的请求都发送给域名：`api.agora.io`。

- 请求：请求的格式详见 [云端录制 RESTful API](../../cn/cloud-recording/cloud_recording_api_rest.md) 中的示例
- 响应：响应内容的格式为 JSON

## 调用步骤

一般进行云端录制的步骤如下：

1. 通过 [`acquire`](../../cn/cloud-recording/cloud_recording_api_rest.md) 请求获取一个 resource ID 用于开启云端录制。
2. 获取 resource ID 后在 5 分钟内调用 [`start`](../../cn/cloud-recording/cloud_recording_api_rest.md) 开始云端录制。
3. 录制完成后调用 [`stop`](../../cn/cloud-recording/cloud_recording_api_rest.md) 停止录制。

在整个过程中可以通过 [`query`](../../cn/cloud-recording/cloud_recording_api_rest.md) 请求查询云端录制的状态。

![](https://web-cdn.agora.io/docs-files/1559640254333)

具体的请求和响应内容请参考 [云端录制 RESTful API](https://docs.agora.io/cloud_recording_api_rest?platform=All%20Platforms)。

## 上传和管理录制文件

开始录制后，Agora 服务器会将录制内容切片为多个 TS 文件并不断上传至预先设定的第三方云存储，直至录制结束。

### 录制 ID

录制 ID 是录制的唯一标识，每一次云端录制对应一个录制 ID。

通过 [`start`](../../cn/cloud-recording/cloud_recording_api_rest.md) 请求成功开始录制后，你可以在响应中获取录制 ID（`sid`），所有的回调中也都包含录制 ID。

### 录制文件索引

每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。

M3U8 文件名由录制 ID 和频道名组成，如 `sid_cname.M3U8`。

### 上传录制文件

录制文件的上传由 Agora 服务器自动完成，你需要关注下面的回调。

- 录制过程中
  - [`uploading_progress`](../../cn/cloud-recording/cloud_recording_callback_rest.md)：录制开始后约每分钟触发一次，该回调中的 `progress` 参数表示已上传文件占已录制文件的百分比。
- 停止录制后，根据上传情况，SDK 会触发以下回调之一：
  - [`uploaded`](../../cn/cloud-recording/cloud_recording_callback_rest.md)：如果录制文件全部成功上传至预先设定的云存储，最后一个录制切片文件上传完成时触发该回调，通知上传完成。
  - [`backuped`](../../cn/cloud-recording/cloud_recording_callback_rest.md)：如果有录制文件未能成功上传至第三方云存储，Agora 服务器会将这部分文件自动上传至 Agora 云备份，当录制结束后会触发该回调。Agora 云备份会继续尝试将这部分文件上传至设定的第三方云存储。如果等待五分钟后仍然不能正常[播放录制文件](../../cn/cloud-recording/cloud_recording_onlineplay.md)，请联系 Agora 技术支持。
