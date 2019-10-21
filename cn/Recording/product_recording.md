
---
title: 产品概述
description: 
platform: Linux
updatedAt: Mon Oct 21 2019 10:38:10 GMT+0800 (CST)
---
# 产品概述
<div class="alert note">提示：<br>Agora 本地服务端录制 SDK 需要部署在 Linux 服务器上，并需要你自行运维。如果你不想部署 Linux 服务器，而是想要通过 RESTful API 以更加便捷的方式实现录制功能，推荐使用 <a href="https://docs.agora.io/cn/cloud-recording/product_cloud_recording?platform=Linux">Agora 云端录制</a></li>。  </div> 

Agora 本地服务端录制 SDK，是 Agora 针对音视频通话、直播研发的录制插件，与 Agora Native SDK （1.7.0 或更高版本） 及 Agora Web SDK \(1.12.0 或更高版本\) 兼容，通过简单的操作方法，帮助开发者快速、灵活地部署录制服务，来实现一对一、一对多的音视频通话或直播的录制。

有了录制功能，你可以将语音聊天、视频聊天以及直播的内容储存下来，提供给更多的人在方便的时间观看。举个例子，某个用户报名参加了某线上课程，除了在规定的时间段上线听课外，他还可以选择在其他时间段观看课程录像，方便复习或补课。这个功能就是课程提供者在服务端部署录制服务来实现的。

## 功能描述

Agora 本地服务端录制 SDK 支持录制 [Agora RTC SDK](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk) 的高清音视频通话，具体如下：

| 功能               | 描述                                                         | 
| :----------------- | :----------------------------------------------------------- |
| 选择录制内容       | 可选择：<li>仅录制音频<li>仅录制视频<li>同时录制音视频                   | -                                                            |
| 选择录制模式       | 可选择：<li>[单流录制](../../cn/Recording/individual_recording.md)模式：分开录制频道内每个 UID 的音频流和视频流。每个 UID 均有其对应的音频文件和视频文件。<li>[合流录制](../../cn/Recording/composite_recording.md)模式：频道内所有 UID 的音频混合录制为一个纯音频文件，所有 UID 的视频混合录制为一个纯视频文件。可指定合流音频属性和视频属性。 | 
| [设置合流布局](../../cn/Recording/recording_layout_guide.md)       | 合流录制模式下，支持设置合流布局，指定发流用户画面的大小及其在视频画布上的位置，设置用户和画面的背景图。 | 
| [获取原始音视频数据](../../cn/Recording/recording_raw_data.md) | 支持获取：<Li>AAC 和 PCM 格式的原始音频数据<li>H.264 和 YUV 格式的原始视频数据 | 
| [视频截图](../../cn/Recording/recording_screen_capture.md)           | 单流录制模式下，支持：<li>仅截图，获取 JPG 图片。<li>边录制边截图，获取多个视频文件和 JPG 图片。<br>合流录制模式下，支持边录制边对各单流截图，获取一个视频文件和多个 JPG 图片。 | 
| 支持代理     | 支持配置代理服务器或[使用云代理服务](../../cn/Recording/cloudproxy_recording.md)，实现内网访问 Agora 服务，进行录制。                          | 
| 录制大流或小流     | Agora RTC SDK 开启[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala%E5%8F%8C%E6%B5%81%E6%A8%A1%E5%BC%8F)时，可选择：<li>仅录制大流<li>仅录制小流 | 

## 适用场景

Agora 本地服务端录制 SDK 应用广泛，主要可以在以下场景中发挥重要作用：

| 行业     | 适用场景                                                     |
| -------- | ------------------------------------------------------------ |
| 在线教育 | 在 1v1 、1v多的小班线上课堂中，提供高质量的音视频录制：<br/><li>方便用户在课程结束后，反复观看、收听录制下来的课堂视频或音频，来巩固及复习学习成果；<li>因时间冲突错过上课的用户也可以观看课堂视频或音频进行学习。 |
| 社交直播 | <li>精彩瞬间录制<li>直播回放<li>截图鉴黄                     |
| 金融行业 | 在开展在线理财、开户、面签等业务时，应国家监管要求，必须提供录音录像服务，形成交易记录的视频，存档备查。 |
| 客服中心 | <li>方便后期用户调研<li>获取相关用户信息<li>客服质量评估     |
| 远程医疗 | 对远程问诊、在线咨询过程进行在线录制，帮助病人足不出户获取医疗资源，并方便后期诊疗参考等。 |

## 产品特性

Agora 本地服务端录制 SDK 主要有以下特性：

| 特性     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| 高可靠性 | 支持集群部署，动态扩容，提供高可用性服务。                   |
| 高安全性 | 提供视频通话、数据传输、数据存储等端到端安全保障机制，详情可参考[信息安全说明](../../cn/Agora%20Platform/security.md)。 |
| 兼容性   | 支持 CentOS 6.5+ x64 和 Ubuntu 14.04+ x64 的操作系统。         |
| 稳定易用 | 操作方法简单易学，能帮助开发者快速上手，灵活地部署录制服务，在移动端和网页端轻松地实现录制功能。 |
| 灵活组合 | 通过灵活组合各个功能，可以无缝应用于所需的多种场景，实现更完善的服务。 |
	
## SDK 兼容性

Agora 本地服务端录制 SDK 支持：

- 纯 Native 端录制；
- 纯 Web 端录制
- Web 与 Native 互通时录制。

Agora 本地服务端录制 SDK 与以下 Agora SDK 兼容:

| Agora SDK        | 兼容版本 |
| ---------------- | -------- |
| Agora Native SDK | 1.7.0 及以上   |
| Agora Web SDK    | 1.12.0 及以上  |

> 如果频道内有任何用户使用了不兼容版本的 SDK，则整个频道无法录制。

## 相关文档和示例代码

- [集成本地服务端录制 SDK](../../cn/Quickstart%20Guide/recording_integrate_cpp.md) 和[开始录制](../../cn/Quickstart%20Guide/recording_cmd_cpp.md)展示了如何从零开始完成 Agora 本地服务端录制 SDK 的部署及使用，包括环境搭建、集成方法、编译代码、演示录制等内容。
- [录制 API](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/index.html) 展示了使用 Agora 本地服务端录制 SDK 过程中你可以调用的各 API，以及调用这些 API 能实现的功能、以及会收到的回调等内容。
- [服务端录制示例代码](https://github.com/AgoraIO/Basic-Recording/)演示了如何录制音视频, 例如: 纯音频录制，混音，合图，加解密，设置 UDP 端口等。
