
---
title: 云端录制更新历史
description: 
platform: Linux
updatedAt: Mon Jun 08 2020 07:13:42 GMT+0800 (CST)
---
# 云端录制更新历史
## 简介

Agora 云端录制，可为 Agora 实时音视频提供录制服务。同 Agora 本地服务端录制相比，Agora 云端录制无需部署 Linux 服务器，减轻了研发和运维的压力，更轻量便捷。点击[云端录制产品概述](../../cn/cloud-recording/product_cloud_recording.md)了解关键特性。

### 兼容性

Agora 云端录制与以下 Agora SDK 兼容:

<style>
table th:first-of-type {
width: 150px;
}
</style>
| SDK              | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| Agora Native SDK | 云端录制与全平台 Agora Native SDK 1.7.0 或更高版本兼容，如果频道内有任何人使用了 1.6 版本的 Agora Native SDK， 则整个频道无法录制。 |
| Agora Web SDK    | 云端录制与 Agora Web SDK 1.12.0 或更高版本兼容。            |

## 2020.05.09

本次发布新增了对金山云存储的支持。

## 2020.04.17

本次发布新增了截图功能，详见[视频截图](../../cn/cloud-recording/cloud_recording_screen_capture.md)。

## 2019.12.16
本次发布提高了云端录制服务的可用性。当出现服务器断网、进程被杀时，云端录制会自动切换到新的服务器，在短时间内恢复录制服务。详情见[云端录制服务器断网、进程被杀的处理](../../cn/faq/high-availability.md)。

**API 变更**

[`acquire`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/acquire) 方法中新增了 `resourceExpiredHour` 参数，用于设置云端录制 RESTful API 的调用时效。

RESTful API 回调通知中新增事件 [`session_exit`](../../cn/cloud-recording/cloud_recording_callback_rest.md)，提供云端录制服务的退出状态。


## 2019.11.15

调用 `start` 方法后，你可以通过 `query` 请求立刻获得录制文件名，而不再需要等待 15 秒。

##  2019.10.24

本次发布新增了对腾讯云存储的支持。


##  2019.10.08

本次发布的新增特性、改进及修复问题如下：

**新增特性**

#### 1. 单流录制

RESTful API 新增单流录制模式，支持分开录制每个 UID 的音频流和视频流。使用方法详见[单流录制](../../cn/cloud-recording/cloud_recording_individual_mode.md)。同时，Agora 提供了音视频合并脚本，用于合并单流录制模式下生成的音频和视频文件。使用方法详见[音视频文件合并](../../cn/cloud-recording/cloud_recording_merge_files.md)。

#### 2. 录制指定 UID 的音视频流

RESTful API 新增 `subscribeAudioUids` 和 `subscribeVideoUids` 参数，支持录制指定 UID 的音视频流。

#### 3. 自定义录制文件存储路径

RESTful API 新增 `fileNamePrefix` 参数，支持指定录制文件在第三方云存储的存储位置。

#### 4. 频道状态变化的时间戳

RESTful API 回调通知中新增事件 `recorder_audio_stream_state_changed` 和 `recorder_video_stream_state_changed`，提供音频流和视频流状态变化的时间，和该路流对应的 UID。

#### 5. 音视频格式转换脚本

Agora 提供了音视频格式转换脚本，用于 TS、MP3、MP4 等文件格式之间的转换。使用方法详见[音视频格式转换](../../cn/cloud-recording/cloud_recording_convert_format.md)。

#### 6. 同步回放

通过 RESTful API 回调或解析 M3U8 文件，你可以获取录制开始时的时间戳，从而将录制的音视频和其他数据流文件（白板、课件、课堂聊天记录等）同步回放。详情见[同步回放](../../cn/cloud-recording/cloud_recording_playback.md)。

**改进**

在出现错误时，你会在 HTTP 响应包体内收到相应的错误信息，而不只是错误码。错误码详情见[云端录制 RESTful API](../../cn/cloud-recording/cloud_recording_api_rest.md)。

**问题修复**

如果因为第三方云存储的 `bucket` 及 `key` 参数值错误导致上传失败，云端录制服务将会直接报错，而不再将切片文件上传至 Agora 云备份。

##  2019.07.19

本次发布的新增特性、改进及修复问题如下：

**新增特性**

#### 自定义合流布局

RESTful API 新增自定义的合流布局。使用方法详见[自定义合流布局](../../cn/cloud-recording/cloud_recording_layout.md)。

你可以在开始录制时将 `mixedVideoLayout` 设为 3，并在 `layoutConfig` 中设置每个用户的画面大小和位置。你也可以在录制过程中调用 `updateLayout` 方法随时更新合流布局。

#### 自定义背景色

RESTful API 新增 `backgroundColor` 参数，支持自定义合流的画布背景色。无论使用何种合流布局，都可以通过该参数设置画布的背景色。

#### 录制的时间戳

为方便开发者获得精准的录制开始时间，RESTful API 提供录制开始第一片切片的 Unix 时间戳 `sliceStartTime`，可以通过 `query` 方法查询获得。同时在 RESTful API 的回调通知中新增事件 `recorder_slice_start`，提供第一片录制切片开始的时间和上一次录制失败的时间。

**改进**

RESTful API 优化了对 `resourceId` 和 `cname` 以及 `uid` 是否对应的检查。

**修复问题**

修复了默认的合流布局中存在的一些小问题。

##  2019.07.02

- 将合流布局默认背景色修改为黑色。
- 改善了在弱网环境下录制的视频卡顿问题。

##  2019.06.13

本次发布主要新增了对 RESTful API 的支持，无需集成 SDK，直接通过网络请求来控制云端录制。

你可以参考下面的文档使用 RESTful API：

- [RESTful API 录制](../../cn/cloud-recording/cloud_recording_rest.md)：使用 RESTful API 进行录制
- [云端录制 RESTful API](../../cn/cloud-recording/cloud_recording_api_rest.md)：RESTful API 参考
- [云端录制 RESTful API 回调服务](../../cn/cloud-recording/cloud_recording_callback_rest.md)：开通回调服务接收云端录制事件通知

##  2019.04.29

本次发布主要包括以下功能:

- Agora Native SDK 和 Agora Web SDK 的高清音视频通话的录制
- 频道内所有用户的音视频合流录制
- 支持 3 种合流布局样式：Float（默认布局）、BestFit（自适应布局）、Vertical（垂直布局）。
- 第三方云存储，目前支持七牛云、阿里云和 Amazon S3
- 提供 C++ 和 Java SDK 包
