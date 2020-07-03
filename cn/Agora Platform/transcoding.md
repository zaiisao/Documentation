
---
title: 转码 (transcoding)
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 10:36:08 GMT+0800 (CST)
---
# 转码 (transcoding)
转码指将音视频解码并重新编码，从而实现格式、属性等的转换的过程。转码通常涉及到编码格式（如 H.264、AAC）、编码属性（如采样率、码率、I 帧间隔）、媒体封装格式（如 MP4、TS）等的转换。

云端录制和本地服务端录制涉及多个转码的场景。例如，合流录制模式下，录制服务会对音频和视频流进行实时转码，将多路流合并成一路流。单流录制模式下，开发者可以通过转码对生成的录制文件作进一步处理，如将每个 UID 的音频和视频文件合并成一个音视频文件，或将切片文件合并成 MP4 文件及其他文件格式。

推流到 CDN 的过程中，当涉及到多人连麦直播，主播发布多路音视频流时，Agora 服务器会通过转码将多路流合并为一路流。

<div class="alert info">相关链接：<li><a href="https://docs.agora.io/cn/cloud-recording/cloud_recording_composite_mode?platform=All20%Platforms">合流录制（云端录制）
</a></li><li><a href="https://docs.agora.io/cn/Recording/recording_composite_mode?platform=Linux">合流录制（本地服务端录制）</a></li><li><a href="https://docs.agora.io/cn/Interactive%20Broadcast/cdn_streaming_android?platform=Android">推流到 CDN</a></li>
</div>
<a href="../../cn/Agora%20Platform/terms.md"><button>返回术语库</button></a>
