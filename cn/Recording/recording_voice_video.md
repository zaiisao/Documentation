
---
title: 录制音视频
description: 
platform: All Platforms
updatedAt: Fri Mar 01 2019 08:34:58 GMT+0000 (UTC)
---
# 录制音视频
本文介绍如何使用 Agora 录制 SDK 来实现不同的录制模式、各模式下生成何种文件以及录制后如何调用转码脚本将文件进行转换。

无论你选择何种录制模式，如果：

- 音频录制过程中，推流用户退出频道，超过 15 秒再进入频道，会认为是一个新的 session，录制文件会切片。
- 视频录制过程中，退出频道后再进入、修改分辨率、视频静音或取消静音，均会导致视频文件切片。

> 使用录制 SDK 必须与 Agora Native/Web SDK 设置相同的频道模式，否则可能导致问题。

## 录制文件

录制支持两种模式：

- 单流模式：分别录制每个 UID 的音频流和视频流。
- 合流模式：对同一个频道的多个用户可以进行混音和合图录制。

<a name ="individualrecording"></a>

### 单流模式录制

设置参数 isMixingEnabled = 0，即启动单流模式录制。 单流模式下分别录制每个 UID 的音频和视频。

你可以根据需要生成的文件类型，设置其他参数以选择不同的录制模式：

<table>

<colgroup>

<col/>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>参数设置</strong></td>

<td><strong>转码前生成文件</strong></td>

</tr>

<tr><td>仅录制音频</td>

<td><code>isAudioOnly = 1</code>,<code> isVideoOnly = 0</code></td>

<td>每个 UID 生成一个音频文件</td>

</tr>

<tr><td>仅录制视频</td>

<td><code>isAudioOnly = 0</code>, <code>isVideoOnly = 1</code></td>

<td><ul>

<li>native 端每个 UID 生成一个 mp4 文件</li>

<li>web 端每个 UID 生成一个 webm 文件</li>

</ul>

</td>

</tr>

<tr><td>同时录制音频 + 视频</td>

<td><code>isAudioOnly = 0</code>, <code>isVideoOnly = 0</code></td>

<td>每个 UID 生成一个音频文件、一个 mp4 或 webm 文件，且音视频分开</td>

</tr>

</tbody>

</table>



如需合成每个 UID 的音视频，则需要通过 video_convert.py 脚本进行转码。详见 使用转码脚本 。

### 合流 + 不实时转码模式录制

设置参数 <code>isMixingEnabled</code> = 1，<code>mixedVideoAudio</code>= 0，即启动合流 + 不实时转码录制。 该模式可实现音频混音录制和视频合图录制，且音视频文件分开。 你还可以调用 setVideoMixingLayout 接口来设置视频合图布局。

你可以根据需要生成的文件类型，设置其他参数以选择不同的录制模式：

<table>

<colgroup>

<col/>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>参数设置</strong></td>

<td><strong>转码前生成文件</strong></td>

</tr>

<tr><td>仅录制音频</td>

<td><code>isAudioOnly = 1</code>, <code>isVideoOnly = 0</code></td>

<td>一个 aac 混音文件</td>

</tr>

<tr><td>仅录制视频</td>

<td><code>isAudioOnly = 0</code>, <code>isVideoOnly = 1</code></td>

<td>一个 mp4 合图文件</td>

</tr>

<tr><td>同时录制音频 + 视频</td>

<td><code>isAudioOnly = 0</code>, <code>isVideoOnly = 0</code></td>

<td>一个 aac 混音文件，一个 mp4 合图文件，且音频和视频文件分开</td>

</tr>

</tbody>

</table>



如需合成音视频，则需要通过 video\_convert.py脚本进行转码。详见 使用转码脚本 。

### 合流 + 实时转码录制模式录制

设置参数 isMixingEnabled = 1，mixedVideoAudio= 1，即启动合流 + 实时转码录制。 该模式无需转码即可实时合成音视频合流录制文件。

你可以根据需要生成的文件类型，设置其他参数以选择不同的录制模式：

<table>

<colgroup>

<col/>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>参数设置</strong></td>

<td><strong>生成文件</strong></td>

</tr>

<tr><td>仅录制音频</td>

<td><code>isAudioOnly = 1</code>, <code>isVideoOnly = 0</code></td>

<td>一个 aac 混音文件</td>

</tr>

<tr><td>仅录制视频</td>

<td><code>isAudioOnly = 0</code>, <code>isVideoOnly = 1</code></td>

<td>一个 mp4 合图文件</td>

</tr>

<tr><td>同时录制音频 + 视频</td>

<td><code>isAudioOnly = 0</code>, <code>isVideoOnly = 0</code> </td>

<td>一个既有音频也有视频的 mp4 文件</td>

</tr>

</tbody>

</table>


## 原始音视频数据

Agora 录制 SDK 目前支持单流模式下的原始音视频数据，以及合流模式下音频混音后的原始音频数据。

### 单流模式

你可以根据需要生成的文件类型，选择不同的录制模式：

<table>

<colgroup>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>生成文件</strong></td>

</tr>

<tr><td>仅录制音频</td>

<td><ul>

<li>AAC 文件：AUDIO_FORMAT_AAC_FRAME_TYPE = 1</li>

<li>PCM 文件：AUDIO_FORMAT_PCM_FRAME_TYPE = 2</li>

</ul>

</td>

</tr>

<tr><td>仅录制视频</td>

<td><ul>

<li>H264 文件：VIDEO_FORMAT_H264_FRAME_TYPE = 1</li>

<li>YUV 文件：VIDEO_FORMAT_YUV_FRAME_TYPE = 2</li>

</ul>

</td>

</tr>

<tr><td>同时录制音频 + 视频</td>

<td><ul>

<li>H264 + AAC 文件</li>

<li>H264 + PCM 文件</li>

<li>YUV + AAC 文件</li>

<li>YUV + PCM 文件</li>

</ul>

</td>

</tr>

</tbody>

</table>



Web 端不支持 H264 的原始音视频数据，支持 YUV 的原始音视频数据。

### 合流模式

Agora 录制 SDK 目前仅支持多个音频流混音后的原始音频数据，并生成 PCM 文件。

<table>

<colgroup>

<col/>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>参数设置</strong></td>

<td><strong>生成文件</strong></td>

</tr>

<tr><td>仅录制音频</td>

<td><code>isAudioOnly = 1</code>, <code>isVideoOnly = 0</code></td>

<td>PCM 文件：AUDIO_FORMAT_MIXED_PCM_FRAM_TYPE = 3</td>

</tr>

</tbody>

</table>

原始数据录制不支持多个视频流合图，请勿设置 isMixingEnabled 参数。

## 截屏

Agora 录制 SDK 目前仅支持单流模式下截屏，无需转码。

<table>

<colgroup>

<col/>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>参数设置</strong></td>

<td><strong>生成文件</strong></td>

</tr>

<tr><td>仅录制视频</td>

<td><code>isVideoOnly = 1</code>，<code>decodeVideo = 3/4</code>，<code>captureInterval = 1</code> <sup>[1]</sup></td>

<td><ul>

<li>jpg 文件</li>

<li>jpg 缓存</li>

</ul>

</td>

</tr>

</tbody>

</table>



[1] captureInterval 参数可以设置截屏的时间间隔，最小值为 1 秒，默认值为 5 秒，需要配合加入频道时的 decodeVideo = 3/4 一起使用。

## 边录制边截屏

Agora 录制 SDK 目前仅支持单流的录制文件 + 单流的截屏，截屏无需转码，录制文件转码与 单流模式录制 相同。

<table>

<colgroup>

<col/>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>录制模式</strong></td>

<td><strong>参数设置</strong></td>

<td><strong>生成文件</strong></td>

</tr>

<tr><td>边录制边截屏</td>

<td><code>decodeVideo = 5</code>，<code>captureInterval = 1</code></td>

<td><ul>

<li>native 端每个 UID 生成一个 mp4 文件（转码前）</li>

<li>web 端每个 UID 生成一个 webm 文件（转码前）</li>

<li>jpg 文件</li>

</ul>

</td>

</tr>

</tbody>

</table>

<a name = "Managing_the_Recorded_Files"></a>

## 管理录制文件

config.json <sup>[2]</sup> 文件中的 recordFileRootDir指定了顶级录制目录路径。录制子目录结构如下:

- yyyy\mm\dd (日期)：每天都会创建一个新的日期目录（如果当天执行了录制操作）。该目录下包含所有当天的文件和目录。
- ChannelName_HHMMSS：录制文件存储在执行录制操作当天的该目录下。录制文件带有频道名称和含有小时，分钟，以及秒的时间戳。时间戳为服务器开始录制的时间。
- ChannelName_HHMMSS目录下包含以下录制相关的单个文件:

<table>

<colgroup>

<col/>

<col/>

</colgroup>

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



[2] 配置文件config.json 的内容为录制文件存放的绝对路径，该文件由用户自己生成。录制时，需要通过 cfgFilePath 参数指定该配置文件中的存放路径。

<a name="using-transcoding-script"></a>

## 使用转码脚本

录制完成后，需要转码工具 video_convert.py 和 ffmpeg 将录制的文件进行合成 (转码工具可通过命令行 tar -xvf 解压)：

- 如果录制生成的是多个纯音频文件，转码后将合并生成 m4a 文件，文件名为 UID_HHMMSSMS.m4a
- 如果录制生成的是多个音视频文件，转码后将合并生成 mp4 文件，文件名为UID_HHMMSSMS_av.mp4，如果希望合并每个 session 的音频文件和视频文件，则:
  - 当同一个 UID 退出后再进入频道的时间间隔少于 15 秒，则认为是同一个 session，不会产生新的音频文件，但会产生新的视频文件，并且会和之前同一个 session 内的音视频文件合并入同一个 UID_HHMMSSMS_av.mp4 文件。
  - 当同一个 UID 退出后再进入频道的时间间隔大于 15 秒，则认为是一个新的 session，会产生新的音频文件和视频文件，并且这段音视频会被计入新 session 的 UID_HHMMSSMS_av.mp4文件里。
- 如果录制生成的是多个音视频文件，并且希望把多个不同 session 的 mp4 文件以 merge 方式合并，转码后将合并生成 mp4 文件，文件名为 UID_0_merge_av.mp4、UID_1_merge_av.mp4、UID_2_merge_av.mp4……
  - automatically mode下使用-m参数，会把同一个 uid 的所有音视频文件合并，并生成唯一的一个UID_0_merge_av.mp4 文件。
  - manually mode 下，由于是根据 startService和 stopService 来划分文件管理的，每一个 start/stop 为一次 service。因此如果有多次 start 和 stop，就会产生多个 service，因此使用-m参数就会生成多个 UID_XX_merge_av.mp4文件。
  
> Agora 在 [录制 SDK](https://docs-preview.agoralab.co/cn/Recording/downloads) 的 tools 文件夹下提供转码工具 ffmpeg 和 video_convert.py，解压 ffmpeg，并确保和 video_convert.py 在同一目录下。

执行 python video_convert.py，即可看到相关用法：

    Usage: video_convert.py [options]
    
    Options:
      -h, --help
      -f FOLDER, --folder=FOLDER
                            Convert folder
      -m MODE, --mode=MODE  Convert merge mode, [0: txt merge A/V(Default); 1: uid
                            merge A/V; 2: uid merge audio; 3: uid merge video]
      -p PFS,  --fps=FPS    Convert fps, default 15
      -s --saving           Convert Do not time sync
      -r RESOLUTION, --resolution=RESOLUTION
                            Specific resolution to convert '-r width height'
                            Eg: '-r 640 360'

<table>

<colgroup>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>选择</strong></td>

<td><strong>描述</strong></td>

</tr>

<tr><td><code>-f</code></td>

<td>待转码文件所在的文件夹路径</td>

</tr>

<tr><td><code>-m</code></td>

<td><p>转码模式，其中：</p>

<ul>

<li>0：按 session 转码，即把同一个 session 的音频和视频合成一个文件</li>

<li>1：把同一个用户的音频和视频按时间顺序合成一个文件</li>

<li>2：把同一个用户的音频按时间顺序合成一个文件</li>

<li>3：把同一个用户的视频按时间顺序合成一个文件</li>

</ul>

</td>

</tr>

<tr><td><code>-p</code></td>

<td>帧率设置参数，支持合图模式和单流模式，默认为 15 fps <sup>[3]</sup></td>

</tr>

<tr><td><code>-s</code></td>

<td>保存模式，表示转码是否需要严格时间同步，即用户离开频道的时间是否占用录制文件时长，需与 <code>-m</code> = 1/2/3 一起使用，否则无效。默认为 all time recording <sup>[4]</sup></td>

</tr>

<tr><td><code>-r</code></td>

<td>设置转码的分辨率，格式为“宽 高”。</td>

</tr>

</tbody>

</table>



[3] 帧率设置参数可以设置帧率，支持 5fps ~ 120fps。如果设置低于 5fps，则按 5fps 执行。

[4] all time recording 模式下，如果用户录制中途退出频道后重新加入，用户退出的时间会以黑色的画面呈现在录制文件里。例如，用户在频道里 2 分钟后，退出频道 30 分钟，后再加入频道 2 分钟，录制文件长度最终为 34 分钟，其中 30 分钟为黑屏。

你也可以参考下面的图示，来理解转码脚本的选择：

![](https://web-cdn.agora.io/docs-files/1544430828726)

转码完成后的 mp4 文件几乎支持所有主流播放器，例如:

<table>

<colgroup>

<col/>

<col/>

</colgroup>

<tbody>

<tr><td><strong>操作系统</strong></td>

<td><strong>播放器</strong></td>

</tr>

<tr><td>Windows</td>

<td>Windows Media Player, KM Player, VLC Player</td>

</tr>

<tr><td>Mac</td>

<td>Mac Quick Time Player, Movist, MPlayerX, KMPlayer</td>

</tr>

<tr><td>iOS</td>

<td>iOS 默认播放器， VLC， KMPlayer</td>

</tr>

<tr><td>Android</td>

<td>Android 默认播放器，MXPlayer，VLC for Android, KMPlayer</td>

</tr>

</tbody>

</table>



只有当录制文件夹内有 recording2-done.txt文件时，才可以进行转码。转码完成后，会生成一个convert-done.txt，此时无需再次进行转码。只要使用转码脚本，转码完成后会生成一个 convert.log, 和音视频文件在同一个路径。

## 保护录制文件

录制文件仅保存在您的服务器上，Agora 无法访问，所以如何保护录制文件取决于部署录制服务的您自己采取保护措施或者咨询安全专家。 处理这些录制文件的方法跟在您的服务器上处理一般文件的方法一样。

在 ChannelName_HHMMSS目录下，每个录制文件都有一个 recording.log和 recording_sys.log 文件。如果在录制过程中出现故障，可以在这两个文件下查看导致问题出现的原因，

