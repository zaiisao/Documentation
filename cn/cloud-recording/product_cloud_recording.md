
---
title: 云端录制
description: 
platform: All Platforms
updatedAt: Fri Jun 12 2020 04:40:04 GMT+0800 (CST)
---
# 云端录制
Agora 云端录制，是 Agora 针对音视频通话、直播研发的录制组件，与 Agora Native SDK （1.7.0 或更高版本） 及 Agora Web SDK (1.12.0 或更高版本) 兼容，通过简单的操作方法，帮助开发者快速、灵活地实现录制服务，实现一对一、一对多的音视频通话或直播的录制。同 Agora 本地服务端录制相比，Agora 云端录制无需部署 Linux 服务器，减轻了研发和运维的压力，更轻量便捷。

有了录制功能，你可以将语音聊天、视频聊天以及直播的内容储存下来，提供给更多的人在方便的时间观看。举个例子，某个用户报名参加了某线上课程，除了在规定的时间段上线听课外，他还可以选择在其他时间段观看课程录像，方便复习或补课。该功能可以通过在客户端配置 Agora 云端录制实现。

## 功能概述

下表列出了 Agora 云端录制的主要功能。你可以访问链接，查看各功能详情。

| <span style="white-space:nowrap;">&emsp;&emsp;&emsp;功能&emsp;&emsp;&emsp;</span>               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| 录制模式           | 支持两种录制模式：<ul><li>[合流录制模式](https://docs.agora.io/cn/cloud-recording/cloud_recording_composite_mode?platform=All%20PlatformsPlatforms)：频道内所有 UID 的音视频混合录制为一个音视频文件。</li><li>[单流录制模式](https://docs.agora.io/cn/cloud-recording/cloud_recording_individual_mode?platform=All%20Platforms)：分开录制频道内每个 UID 的音频流和视频流，每个 UID 均有其对应的音频文件和视频文件。</li></ul> |
| 视频截图     | 在单流模式下，支持[视频截图](../../cn/cloud-recording/cloud_recording_screen_capture.md)。                                  |
| 订阅指定的 UID     | 支持设置订阅白名单或黑名单，以及在录制过程中更新订阅名单。详见[设置订阅名单](https://docs.agora.io/cn/cloud-recording/cloud_recording_subscription)。                                  |
| 订阅指定的媒体类型 | 支持订阅指定的的媒体类型：<ul><li>仅订阅音频</li><li>仅订阅视频</li><li>同时订阅音频和视频</li></ul> |
| 设置音视频属性     | 在合流模式下，支持设置音视频属性，如码率和分辨率。           |
| 设置合流布局       | 在合流模式下，支持[自定义合流布局](https://docs.agora.io/cn/cloud-recording/cloud_recording_layout?platform=Linux#a-namecustoma%E8%87%AA%E5%AE%9A%E4%B9%89%E5%90%88%E6%B5%81%E5%B8%83%E5%B1%80)或[使用预设的布局](https://docs.agora.io/cn/cloud-recording/cloud_recording_layout?platform=Linux#a-namepredefineda%E9%80%89%E6%8B%A9%E9%A2%84%E8%AE%BE%E7%9A%84%E5%90%88%E6%B5%81%E5%B8%83%E5%B1%80%E6%A0%B7%E5%BC%8F)，以及设置屏幕（画布）的背景颜色。支持在录制过程中更新合流布局或背景颜色。 |
| 第三方云存储       | 支持将录制文件存储在以下第三方云存储中：<ul><li>Amazon S3</li><li>阿里云</li><li>腾讯云</li><li>七牛云</li><li>金山云</li></ul>  你可以自定义录制文件在云存储中的存放路径。|
| 录制双流           | 如果 [Agora RTC SDK](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk) 启用了[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#dual-stream)，你可以选择录制大流或小流。 |
| 录制加密频道       | 支持录制采用以下加密方式的频道：<ul><li>AES128XTS</li><li>AES128ECB</li><li>AES256XTS</li></ul> |
| 转码           | Agora 提供转码脚本，用于[合并音视频文件](https://docs.agora.io/cn/cloud-recording/cloud_recording_merge_files?platform=All%20Platforms)以及[转换文件格式](https://docs.agora.io/cn/cloud-recording/cloud_recording_convert_format?platform=All%20Platforms)。 |
| 消息通知服务           | Agora 提供[消息通知服务](../../cn/cloud-recording/cloud_recording_callback_rest.md)。开通该服务后，你会收到云端录制的事件通知，例如：<ul><li>录制文件的文件名</li><li>第一个切片文件的开始时间</li><li>流状态改变时的时间戳</li></ul> |

## 适用场景

Agora 云端录制应用广泛，主要可以在以下场景中发挥重要作用：

| <span style="white-space:nowrap;">&emsp;&emsp;行业&emsp;&emsp;</span>      | 适用场景                                                     |
| -------- | ------------------------------------------------------------ |
| 在线教育 | 在 1v1 、1v多 的小班线上课堂中，提供高质量的音视频录制：<li>方便用户在课程结束后，反复观看、收听录制下来的课堂视频或音频，来巩固及复习学习成果；<li>因时间冲突错过上课的用户也可以观看课堂视频或音频进行学习。 |
| 社交直播 | <li>精彩瞬间录制<li>直播回放<li>截图鉴黄                                 |
| 金融行业 | 在开展在线理财、开户、面签等业务时，应国家监管要求，必须提供录音录像服务，形成交易记录的视频，存档备查。 |
| 客服中心 | <li>方便后期用户调研<li>获取相关用户信息<li>客服质量评估                 |
| 远程医疗 | 对远程问诊、在线咨询过程进行在线录制，帮助病人足不出户获取医疗资源，并方便后期诊疗参考等。 |

## 产品特性

Agora 云端录制主要有以下特性：

| <span style="white-space:nowrap;">&emsp;&emsp;特性&emsp;&emsp;</span>      | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| 高可靠性 | <li>全球分布式集群部署，提供高可用性服务。</li><li>当第三方云存储故障，提供自动存储灾备和延迟上传功能。</li>                   |
| 高安全性 | 提供视频通话、数据传输、数据存储等端到端安全保障机制，详情可参考[信息安全说明](https://docs.agora.io/cn/Agora%20Platform/security)。 |
| 兼容性   | 支持第三方云存储：[七牛云](https://www.qiniu.com/products/kodo)、[阿里云](https://www.aliyun.com/product/oss)、[腾讯云](https://cloud.tencent.com/product/cos) 和 [Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1)。  |
| 稳定易用 | 4 个 RESTful API 调用就可以开始、结束、查询录制，简单易学，能帮助开发者快速集成上线录制服务。 |


## 计费

使用云端录制服务加入频道，订阅需要录制的音视频流，并实时上传到你指定的第三方云存储。录制的音视频流的[集合分辨率](https://docs.agora.io/cn/faq/video_billing#the-Recording-Aggregate-Resolution)分为 Audio、HD、HD+ 三档，根据每档对应的单价，按分钟数计费。具体价格请咨询 sales@agora.io 或 400 6326626。
	
每个录制任务单独计费。例如，基于同一个频道创建两个录制任务，则会分别计费。

> 每月通话（包含录制）总时长不超过一万分钟时，Agora 不收取任何费用，详见[前一万分钟免费说明](https://docs.agora.io/cn/faq/billing_free)。

## 相关文档和示例代码

- [云端录制 RESTful API 快速开始](../../cn/cloud-recording/cloud_recording_rest.md)展示了如何通过 RESTful API 进行录制。
- [云端录制 RESTful API](https://docs.agora.io/cn/cloud-recording/restfulapi) 展示了使用云端录制过程中你可以调用的各 RESTful API 及其功能。
- [云端录制 RESTful API 回调服务](https://docs.agora.io/cn/cloud-recording/cloud_recording_callback_rest)展示了使用云端录制过程中你可能收到的回调及其详细解释。



 

 
