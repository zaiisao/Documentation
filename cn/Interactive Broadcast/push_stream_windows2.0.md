
---
title: 推流到 CDN
description: 
platform: Windows
updatedAt: Mon May 20 2019 07:58:26 GMT+0800 (CST)
---
# 推流到 CDN
## 功能描述

将直播流发布到 CDN（Content Delivery Network）的过程称为 CDN 直播推流，用户无需安装 App，可以通过 Web 浏览器观看直播。

在推流到 CDN 过程中，当频道中有多个主播时，通常会涉及到[转码](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#转码)，将多个直播流组合成单个流，并设置这个流的音视频属性和合图布局。



声网提供的 CDN 旁路推流方案主要基于以下 API 进行推流、外部输入视频源、转码和布局设置：

-   <code>addPublishStreamUrl</code>
-   <code>removePublishStreamUrl</code>
-   <code>setLiveTranscoding</code>


声网的 CDN 旁路推流方案具有以下优点：

-   能够随时启动或停止推流

-   增加了控制信息

-   能够在不间断推流的同时增减推流地址

-   通过回调接口掌握推流成功与否

-   较少的接口便于客户快速升级。

## 推流到 CDN

您需要联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。

> 声网今后将在 Dashboard 提供自助服务。

> -   主播需要通过 <code>addPublishStreamUrl</code> 接口指定推流地址。推流地址可以在开始推流后动态增删。

> -   主播需要在 <code>joinChannel</code> 成功后通过 <code>setLiveTranscoding</code> 接口定义转码参数和设置（RTMP 画布大小、多人混流布局等信息）。


推流架构图如下：

<img alt="../_images/live_ios_publishing_stream_cn.png" src="https://web-cdn.agora.io/docs-files/cn/live_ios_publishing_stream_cn.png"/>



### 示例代码：

```
//CDN 转码设置
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
//config.audioBitrate
config.width = 640;
config.height = 720;
config.videoFramerate = 30;
config.userCount = 1;  //如果 userCount > 1，则需要为每个 transcodingUsers 分别设置布局
config.videoCodecProfile = HIGH;

//分配用户视窗的合图布局
config.transcodingUser = new TranscodingUser[config.userCount];
config.transcodingUser[0].uid = 123456; //uid 应和 joinChannel() 输入的 uid 保持一致
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 640;
config.transcodingUser[0].height = 720;

m_rtcEngine->setLiveTranscoding(config);
// 添加推流地址
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }
```

```
// 添加推流地址
m_rtcEngine->addPublishStreamUrl(url, false);
```

```
// 删除推流地址
m_rtcEngine->removePublishStreamUrl(url);
```

## 调整合图布局

频道内有多个主播时，需要通过设置 <code>transcodingUser</code> 调整合图布局。

> 主播需要在<code> joinChannel</code> 后通过 <code>setLiveTranscoding</code> 接口定义转码参数和设置（RTMP 画布大小、多人混流布局等信息）。

### 示例 1: 两人横向平铺

如果你想显示以下布局:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_2host.png" style="width: 500px;"/>


设置参数如下:

```
Canvas:
     width: 640
     height: 360
     backgroundColor: #FFFFFF

User0:
      userId: 123
      x: 0
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0

User1:
      userId: 456
      x: 320
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0
```

### 示例 2: 三人纵向平铺

如果你想显示以下布局:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_3host.png" style="width: 370px;"/>


设置参数如下:

```
Canvas:
      width: 360
      height: 640
      backgroundColor: #FFFFFF

   User0:
      userId: 123
      x: 0
      y: 0
      width: 360
      height: 640
      zOrder: 1
      alpha: 1.0

   User1:
       userId: 456
       x: 0
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0

   User2:
       userId: 789
       x: 180
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0
```

### 示例 3: 1 人全屏 + 多人任意悬浮小窗

如果你想显示以下布局:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/cn/sei_random.png" style="width: 370px;"/>


设置参数如下:

```
Canvas:
   width: 360
   height: 640
   backgroundColor: #FFFFFF

User0:
   userId: 123
   x: 0
   y: 0
   width: 360
   height: 640
   zOrder: 1
   alpha: 1.0

User1:
    userId: 456
    x: 45
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0

User2:
    userId: 789
    x: 185
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0
```



