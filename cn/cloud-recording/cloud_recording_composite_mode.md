
---
title: 合流录制
description: 描述如何设置参数以进行合流录制
platform: All Platforms
updatedAt: Fri Sep 25 2020 04:16:14 GMT+0800 (CST)
---
# 合流录制
## 功能描述

云端录制支持两种录制模式：

- 单流录制模式：分别录制频道中每个 UID 的音频流和视频流，每个 UID 均有其对应的音频文件和视频文件。
- 合流录制模式：频道内所有或指定 UID 的音视频混合录制为一个音视频文件。

本文介绍如何通过设置 RESTful API 参数在合流模式下进行录制。

阅读本文前，请确保你已了解如何使用 RESTful API 进行云端录制，详见[云端录制 RESTful API 快速开始](../../cn/cloud-recording/cloud_recording_rest.md)。单流或合流模式的设置必须在开始录制的时候完成，不支持在录制开始后切换模式。请参阅[单流录制模式和合流录制模式的区别](https://docs.agora.io/cn/faq/recording_mode)来决定应使用哪种模式。

> 为方便起见，本文假设频道内每个用户都发送音频和视频。如果有用户没有发送音频或视频（例如直播频道中的观众），一般不会生成该用户的音频或视频录制文件（Web 端例外）。

## 实现方法

在调用 [start](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/start) 方法时，将 `mode` 参数设置为 `mix`，即可启用合流模式。

根据录制内容的不同，录制生成的文件如下表所示：

| 录制内容       | 参数设置                           | 录制生成文件                                                 |
| :------------- | :--------------------------------- | :----------------------------------------------------------- |
| 仅录制音频     | `streamTypes` 设为 `0`               | 生成一个 M3U8 文件和多个 TS 文件，TS 文件内仅存储该频道的音频数据。 |
| 仅录制视频     | `streamTypes` 设为 `1`               | 生成一个 M3U8 文件和多个 TS 文件，TS 文件内仅存储该频道的视频数据。 |
| 同时录制音视频 | （默认设置）`streamTypes` 设为 `2` | 生成一个 M3U8 文件和多个 TS 文件，TS 文件内存储该频道的音视频数据。 |

合流模式下，当频道中有两个用户，且同时录制音视频时，录制生成的文件如下图所示：

![](https://web-cdn.agora.io/docs-files/1576159881401)

录制后共生成一个 M3U8 文件和多个 TS / WebM 文件，包含所有用户的音视频数据。使用[音视频格式转换脚本](../../cn/cloud-recording/cloud_recording_convert_format.md)进行格式转换后，你会得到一个 MP4 文件。

录制文件的命名规则详见[管理录制文件](../../cn/cloud-recording/cloud_recording_manage_files.md)。

## start 请求示例

- 在合流模式下进行录制的请求 URL 为：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/mix/start
```

- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：

```
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "audioProfile": 1,
            "channelType": 0, 
            "videoStreamType": 1, 
            "transcodingConfig": {
                "height": 640, 
                "width": 360,
                "bitrate": 500, 
                "fps": 15, 
                "mixedVideoLayout": 1,
                "backgroundColor": "#FF0000"
						},
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

- 对于 Web 端用户，如果用户从未发布过本地视频（`publish`），或中途取消发布本地视频（`unpublish`），则该用户画面显示为背景色；如果用户禁用视频轨道（`muteVideo`），则该用户画面为黑屏。
- 对于 Native 端用户，如果某用户中途停止发流，该用户画面会停留在最后一帧；退出频道后，该用户画面会显示为背景色。
