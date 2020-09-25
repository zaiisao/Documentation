
---
title: 单流录制
description: 解释如何实现在云端录制中实现单流录制
platform: All Platforms
updatedAt: Fri Sep 25 2020 04:15:52 GMT+0800 (CST)
---
# 单流录制
## 功能描述

云端录制支持两种录制模式：

- 单流录制模式：分别录制频道中每个 UID 的音频流和视频流，每个 UID 均有其对应的音频文件和视频文件。
- 合流录制模式：频道内所有或指定 UID 的音视频混合录制为一个音视频文件。

本文介绍如何通过设置 RESTful API 参数在单流模式下进行录制。

阅读本文前，请确保你已了解如何使用 RESTful API 进行云端录制，详见[云端录制 RESTful API 快速开始](../../cn/cloud-recording/cloud_recording_rest.md)。单流或合流模式的设置必须在开始录制的时候完成，不支持在录制开始后切换模式。请参阅[单流录制模式和合流录制模式的区别](https://docs.agora.io/cn/faq/recording_mode)来决定应使用哪种模式。

> 为方便起见，本文假设频道内每个用户都发送音频和视频。如果有用户没有发送音频或视频（例如直播频道中的观众），一般不会生成该用户的音频或视频录制文件（Web 端例外）。

## 实现方法

在调用 [`start`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/start) 方法时，将 `mode` 参数设置为 `individual`，即可启用单流模式。

该模式下录制文件的音视频编码配置如下：

-  音频编码配置：采样率固定为 48 kHz，声道数和码率与原始音频流一致。
-  视频编码配置：与原始视频流一致。

根据录制内容的不同，录制生成的文件如下表所示：

| 录制内容     | 参数设置      | 录制生成文件          |
| :--- | :--- | :------- |
| 仅录制音频     | `streamTypes` 设为 `0`    | 每个 UID 生成一个 M3U8 文件和多个 TS/WebM 文件，TS/WebM 文件内仅存储该 UID 的音频数据。|
| 仅录制视频     | `streamTypes` 设为 `1`   | 每个 UID 生成一个 M3U8 文件和多个 TS/WebM 文件，TS/WebM 文件内仅存储该 UID 的视频数据。 |
| 同时录制音视频     | （默认设置） `streamTypes` 设为 `2`| 每个 UID 生成两个 M3U8 文件和多个 TS/WebM 文件，TS/WebM 文件分开存储 UID 的音频数据及视频数据。 |

> 单流模式下，Web 端选择 VP8 编码时，云端录制生成的切片文件为 WebM 文件。其他情况下生成的切片文件均为 TS 文件。

单流模式下，当频道中有两个用户，且同时录制音视频时，录制生成的文件如下图所示：

![](https://web-cdn.agora.io/docs-files/1576157340343)

录制后共生成四个 M3U8 文件（每个 UID 对应两个 M3U8 文件）和多个 TS/WebM 文件，分开存储音视频数据。通过[音视频合并转码脚本](../../cn/cloud-recording/cloud_recording_merge_files.md)完成文件合并后，你会得到两个 MP4 文件。

录制文件的命名规则详见[管理录制文件](../../cn/cloud-recording/cloud_recording_manage_files.md)。

## start 请求示例

- 在单流模式下进行录制的请求 URL 为：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/individual/start
```

- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：

```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "channelType": 0, 
            "videoStreamType": 1, 
            "subscribeVideoUids": ["123","456"], 
            "subscribeAudioUids": ["123","456"],
            "subscribeUidGroup": 0
        }, 
        "storageConfig": {
            "accessKey": "xxxxxxf",
            "region": 3,
            "bucket": "xxxxx",
            "secretKey": "xxxxx",
            "vendor": 2,
            "fileNamePrefix": ["directory1","directory2"]
       }
   }
}
```

## 开发注意事项

- 单流模式下每个 UID 的音频和视频均分开录制，如需合成每个 UID 的音视频，需要通过 `convert.py` 脚本进行转码。详见[音视频文件合并](../../cn/cloud-recording/cloud_recording_merge_files.md)。
- 如果录制内容选择**仅录制视频**或**同时录制音视频**，但是 Web 端用户没有发送视频，也会生成视频录制文件，录制黑屏。



