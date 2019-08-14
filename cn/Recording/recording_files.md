
---
title: 管理录制文件
description: 
platform: All Platforms
updatedAt: Wed Aug 14 2019 10:43:48 GMT+0800 (CST)
---
# 管理录制文件
### 设置录制文件的目录结构

#### 使用默认的目录结构

通过 `recordFileRootDir` 参数设置录制文件的根目录，设置了此目录后，会自动生成子目录。

以录制生成一个 mp4 文件为例，自动生成的子目录为 `yyyymmdd/ChannelName_HHMMSS_MSUSNS/xxxx.mp4`，其中：

- `yyyymmdd`：加入频道的日期。该目录下包含当日开始录制的所有的文件和目录，时区为 UTC+0。
- `ChannelName_HHMMSS_MSUSNS`：录制文件的上一层目录，即录制文件存储在执行录制操作当天的该目录下。该目录名包含频道名和含有小时、分钟、秒、毫秒、微秒和纳秒的时间戳。时间戳为服务器开始录制的时间，时区为 UTC+0。

>- v2.3.0 之前的版本，录制文件的上一层目录为 `ChannelName_HHMMSS`，以频道名加上时间戳（含有小时、分钟和秒）命名。
>- v2.3.0 之后（包括 v2.3.0）的版本，录制文件的上一层目录为 `ChannelName_HHMMSS_MSUSNS`，以频道名加上时间戳（含有小时、分钟、秒、毫秒、微秒和纳秒）命名。

#### 自定义目录结构

自定义目录结构时，需要创建 JSON 格式的配置文件，通过 `cfgFilePath` 参数指定该配置文件的存放路径。

通过配置文件中的 `Recording_Dir` 参数自定义录制文件的目录结构，例如：`{“Recording_Dir” : “recording path”}`，其中 `Recording_Dir` 是固定的，不能改动。

> 不推荐自定义目录结构，因为若多个录制实例使用相同的配置参数，则会导致不同录制实例的文件存放到相同的目录，无法区分。

### 生成的录制文件

录制相关的单个文件:

<table>

<tbody>

<tr><td><strong>文件</strong></td>

<td><strong>描述</strong></td>

</tr>

<tr><td><code>UID_HHMMSSMS.aac</code></td>

<td>无论在移动端、PC 端还是网页端录制，每个 UID 均会对应生成一个 aac 文件。该文件仅包含该用户相关的音频内容</td>

</tr>

<tr><td><code>UID_HHMMSSMS.mp4</code></td>

<td>如果在移动或 PC 端录制，每个 UID 均会对应生成一个 mp4 文件。如一个用户多次进入退出频道，该用户会有多个视频 mp4 文件，该文件仅包含该用户相关的视频内容</td>

</tr>

<tr><td><code>UID_HHMMSSMS.webm</code></td>

<td>如果在网页端录制，每个 UID 均会对应生成一个 webm 文件。如一个用户多次进入退出频道，该用户会有多个视频 webm 文件，该文件仅包含该用户相关的视频内容</td>

</tr>

<tr><td><code>recording2-done.txt</code></td>

<td>标识本频道录制结束</td>

</tr>

<tr><td><code>UID_HHMMSSMS.txt</code></td>

<td>记录每个音视频文件开始和结束的时间，视频文件的信息，例如宽高，旋转信息等</td>

</tr>

</tbody>

</table>

录制文件仅保存在你的服务器上，Agora 无法访问，所以你可以自行采取保护措施或者咨询安全专家。 处理这些录制文件的方法同在你的服务器上处理一般文件的方法一样。

在保存录制文件的目录下，每个录制文件都有一个 `recording_sys.log` 文件。如果在录制过程中出现故障，可以在这两个文件下查看导致问题出现的原因。
