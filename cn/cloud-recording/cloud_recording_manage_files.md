
---
title: 管理录制文件
description: 介绍录制文件的命名规则和如何解析 M3U8 文件
platform: All Platforms
updatedAt: Tue Oct 08 2019 03:34:21 GMT+0800 (CST)
---
# 管理录制文件
## 功能描述

单流模式下，云端录制生成的录制文件包括 M3U8 索引文件和 TS/WebM 切片文件。如果要对录制文件进行进一步处理，如音视频文件合并，或与其他数据流文件同步回放，需要了解录制文件的命名规则、文件格式以及切片规则。


## 录制文件命名规则

单流模式下，云端录制生成的录制文件的命名规则如下：

M3U8 文件名：`<sid>_<cname>__uid_s_<uid>__uid_e_<type>.m3u8`

TS 文件名：`<sid>_<cname>__uid_s_<uid>__uid_e_<type>_utc.ts`

WebM 文件名：`<sid>_<cname>__uid_s_<uid>__uid_e_<type>_utc.webm`

上述文件名中各字段含义如下：

- `<sid>`：录制 ID
- `<cname>`：频道名
- `<uid>`：用户 ID
- `<type>`: 文件类型，`audio` 或 `video`
- `<utc>`：该切片文件开始录制时的 UTC 时间，时区为 UTC+0，由年、月、日、小时、分钟、秒和毫秒组成。例如，`utc` 等于 `20190611073246073`，表示该切片文件的开始时间为 UTC 2019 年 6 月 11 日 7 点 32 分 46 秒 73 毫秒。

示例文件名 `sid713476478245_cnameagora__uid_s_123__uid_e_video_20190920125142485.ts` 中，`sid713476478245` 为录制 ID，`cnameagora` 为频道名，`123` 为用户 ID，录制时间为 2019 年 9 月 20 日 12 点 51 分 42 秒 485 毫秒。

### 转存录制文件命名规则

如果录制文件上传到你指定的云存储失败，云端录制会将文件通过备份云转存至你指定的云存储。为确保不覆盖最新版本，M3U8 文件名会附上后缀，具体命名规则为：`<sid>_<cname>__uid_s_<uid>__uid_e_<type>_<tick>_<index>.m3u8`。例如，文件名 `sid713476478245_cnameagora__uid_s_123__uid_e_video_22194679897_3.m3u8` 中，`22194679897` 是系统启动后到该索引文件生成时的 tick 数，`3` 是该 M3U8 文件的索引数，表示该文件为本次录制中第三次更新的 M3U8 文件快照。通过备份云转存的其他类型文件的命名规则不变。

## M3U8 文件

M3U8 文件包含多个切片文件的文件名及其描述符号。Agora 云端录制产生的 M3U8 文件中用到的描述符号有三种：

- `#EXT-X-AGORA-TRACK-EVENT:EVENT=<event>,TRACK_TYPE=<type>,TIME=<utc>`：音视频流开始或者中断后重新开始的第一个切片文件会附带这一描述符号，描述流状态的变化。
  - `EVENT`: 事件名称，目前只能为 `START`，表示音视频开始或中断后重新开始
  - `TRACK_TYPE`：切片文件内容，`AUDIO` 或 `VIDEO`
  - `TIME`: 流状态变化的时间。UTC 时间，时区为 UTC+0

- `#EXT-X-AGORA-ROTATE:WIDTH=<width>,HEIGHT=<height>,ROTATE=<rotate>,TIME=<utc>`：视频旋转后第一个切片文件会附带这一描述符号，描述视频旋转的信息。一个切片文件可能附带多个视频旋转的描述符。
  - `WIDTH`：视频宽度
  - `HEIGHT`：视频高度
  - `ROTATE`：视频逆时针旋转的角度，只能为 `0`、`90`、`180` 或 `270`
  - `TIME`：视频发生旋转的时间。UTC 时间，时区为 UTC+0。
- `#EXTINF:<length>`：描述切片文件的时长，单位为秒。

例如：

```m3u8
#EXT-X-AGORA-ROTATE:WIDTH=640,HEIGHT=480,ROTATE=90,TIME=20190920125142485
#EXT-X-AGORA-TRACK-EVENT:EVENT=START,TRACK_TYPE=VIDEO,TIME=20190920125142485
#EXTINF:6.332000
sid713476478245_cnameagora__uid_s_123__uid_e_video_20190920125142485.ts
```

上述 M3U8 文件中包含一个 TS 切片文件的文件名以及三个描述符号，表示该切片文件是视频流开始或中断后重新开始后的第一片切片文件，相对前一片 TS 文件逆时针旋转了 90 度，总时长为 6.332 秒。

## 切片逻辑

### 视频文件切片

当满足以下任一条件时，云端录制即会对视频文件进行切片：

- 切片时长达到 15 秒，并遇到视频关键帧
- 编码器发生变化
- 视频宽高发生变化
- 视频流发生中断
- 当使用 H.264 编码，单片视频时长超过 5.5 分钟，或单个文件大小超过 50M，云端录制会强制切片。强制切片后，新切片的首帧可能非 I 帧，导致该切片文件不能直接解码播放。例如在通信模式下，数小时内可能仅出现一个 I 帧，容易出现新切片的首帧非 I 帧。

当在 Web 端使用 VP8 编码时，云端录制不会强制切片。

### 音频文件切片

当切片时长达到 15 秒，并遇到音频关键帧时，云端录制即会对音频文件进行切片。

## 音频 M3U8 文件示例

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-ALLOW-CACHE:YES
#EXT-X-TARGETDURATION:18
#EXT-X-DISCONTINUITY
#EXT-X-AGORA-TRACK-EVENT:EVENT=START,TRACK_TYPE=AUDIO,TIME=20190920125142289
#EXTINF:15.019000
sid713476478245_cnameagora__uid_s_123__uid_e_audio_20190920125142289.ts
#EXTINF:15.019000
sid713476478245_cnameagora__uid_s_123__uid_e_audio_20190920125157307.ts
#EXTINF:15.019000
sid713476478245_cnameagora__uid_s_123__uid_e_audio_20190920125212326.ts
#EXTINF:15.019000
sid713476478245_cnameagora__uid_s_123__uid_e_audio_20190920125227345.ts
#EXTINF:12.523000
sid713476478245_cnameagora__uid_s_123__uid_e_audio_20190920125242363.ts
#EXT-X-ENDLIST
```

## 视频 M3U8 文件示例

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-ALLOW-CACHE:YES
#EXT-X-TARGETDURATION:18
#EXT-X-DISCONTINUITY
#EXT-X-AGORA-ROTATE:WIDTH=640,HEIGHT=480,ROTATE=0,TIME=20190920125142485
#EXT-X-AGORA-TRACK-EVENT:EVENT=START,TRACK_TYPE=VIDEO,TIME=20190920125142485
#EXTINF:6.332000
sid713476478245_cnameagora__uid_s_123__uid_e_video_20190920125142485.ts
#EXT-X-AGORA-ROTATE:WIDTH=1280,HEIGHT=720,ROTATE=0,TIME=20190920125149174
#EXT-X-DISCONTINUITY
#EXTINF:17.442000
sid713476478245_cnameagora__uid_s_123__uid_e_video_20190920125149174.ts
#EXT-X-DISCONTINUITY
#EXT-X-AGORA-ROTATE:WIDTH=640,HEIGHT=480,ROTATE=0,TIME=20190920125206616
#EXTINF:33.326000
sid713476478245_cnameagora__uid_s_123__uid_e_video_20190920125206616.ts
#EXT-X-DISCONTINUITY
#EXT-X-AGORA-ROTATE:WIDTH=1280,HEIGHT=720,ROTATE=0,TIME=20190920125239942
#EXTINF:14.815000
sid713476478245_cnameagora__uid_s_123__uid_e_video_20190920125239942.ts
#EXT-X-ENDLIST
```
