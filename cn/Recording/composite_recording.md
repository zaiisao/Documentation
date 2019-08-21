
---
title: 合流录制
description: 
platform: Linux
updatedAt: Wed Aug 14 2019 10:46:55 GMT+0800 (CST)
---
# 合流录制
## 功能描述

本地服务端录制支持两种模式：

- 单流录制：分别录制每个 UID 的音频流和视频流。一个 UID 对应一个音频文件和一个视频文件。
- 合流录制：频道内所有用户的音频混合录制为一个纯音频文件，所有用户的视频混合录制为一个纯视频文件。

本文介绍如何通过命令行的方式进行**合流录制**。

所有命令行参数的设置都是在开始录制的时候完成，请确保你已经完成录制 SDK 的环境准备和集成工作并且了解如何使用命令行开始录制，详见[集成客户端](https://docs-preview.agoralab.co/cn/Recording/recording_integrate_cpp)和[命令行录制](https://docs-preview.agoralab.co/cn/Recording/recording_cmd_cpp)。

> 为方便起见，本文我们假设频道内每个用户都发送音频和视频。如果某个用户没有发送音频或视频，例如直播频道中的观众，一般不会生成该用户的音频或视频录制文件（Web 端例外）。

## 实现方法

在开始录制时将参数 `isMixingEnabled` 设为 1 即可使用合流模式录制。合流模式下搭配不同的参数可以设置多种录制选项。

### 选择录制内容

| 录制内容       | 参数设置                                  | 录制生成文件        |
| :------------- | :---------------------------------------- | :------------------ |
| 仅录制音频     | `--isAudioOnly 1 --isMixingEabled 1`      | 一个 AAC 音频文件   |
| 仅录制视频     | `--isVideoOnly 1 --isMixingEnabled 1`     | 一个 MP4 视频文件   |
| 同时录制音视频 | `--isMixingEnabled 1 --mixedVideoAudio 1` | 一个 MP4 音视频文件 |

> 在仅录制音频或仅录制视频时请不要将 `mixedVideoAudio` 设为 1。

### 设置音视频编码配置

合流模式下你可以自己设置录制文件的音视频编码配置。

#### 音频编码配置

设置 `audioProfile` 参数，设置音频编码模式、采样率、声道数和码率。

- 0：（默认）音频默认设置，采样率 48 KHz，单声道，编码码率为 48 Kbps。
- 1：高音质，采样率 48 KHz，单声道，编码码率 128 Kbps。
- 2：高音质立体声，采样率 48 KHz，双声道，编码码率 192 Kbps。

#### 视频编码配置

设置 `mixResolution` 参数，格式为：width，high，fps，Kbps，从左至右分别对应合流视频的宽、高、帧率和码率。默认的视频编码配置为 360 x 640, 15 fps, 500 Kbps。推荐的视频编码配置请参考 [分辨率、帧率和码率对照表](https://docs.agora.io/cn/faq/recording_video_profile)。

### 设置合流布局

在合流模式录制多人视频时，你需要[设置合流布局](../../cn/Recording/recording_layout_guide.md)。
