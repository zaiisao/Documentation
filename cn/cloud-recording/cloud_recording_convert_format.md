
---
title: 音视频格式转换
description: 解释如何用脚本进行不同音频格式和视频格式的转换
platform: All Platforms
updatedAt: Tue Oct 08 2019 03:33:23 GMT+0800 (CST)
---
# 音视频格式转换
## 功能描述

你可以使用我们的音视频格式转换脚本，对音视频文件进行格式转换。该脚本支持以下文件格式之间的转换：

- 音频：MP3，WAV，AAC
- 视频：MP4，TS

该脚本不支持音频文件和视频文件之间的格式转换。

## 前提条件

### 环境准备

以下物理或虚拟服务器：

- Ubuntu 14.04+ x64
- CentOS 6.5+（推荐 7.0）x64

安装 python 2.7 及以上版本

### 文件准备

确保你已将所有待转码的文件下载到本地。该脚本不支持直接对第三方云存储中的文件进行转码。

## 转码步骤

### 1.获取音视频格式转换脚本

下载 [Agora 音视频格式转换脚本](https://download.agora.io/acrsdk/release/format_convert_1.0.tar.gz) 压缩包并解压。

### 2.输入命令行

输入以下命令行：

```
python format_convert.py <directory> <source_format> <destination_format>
```

其中：

- `directory`：源文件所在目录。
- `source_format`：源文件的格式，即文件后缀名。`source_format` 中的英文字母必须为小写，即：
  - 音频：`mp3`，`wav`，或 `aac`
  - 视频：`mp4` 或 `ts`
- `destination_format`：要转换成的目的格式。`destination_format` 中的英文字母必须为小写，即：
  - 音频：`mp3`，`wav`，或 `aac`
  - 视频：`mp4` 或 `ts`

输入命令行后，脚本会在指定目录下寻找所有符合源文件格式的文件进行转码，转码后的文件主名与源文件主名一致，后缀名为目的格式的后缀。

## 转码示例

如要将 `/home/testname/testfolder` 目录下的所有 MP4 文件转换成 TS 格式，命令如下：

```
python format_convert.py /home/testname/testfolder/ mp4 ts
```
