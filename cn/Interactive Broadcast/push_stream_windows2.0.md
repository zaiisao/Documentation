
---
title: 推流到 CDN
description: 
platform: Windows
updatedAt: Tue Sep 24 2019 05:44:51 GMT+0800 (CST)
---
# 推流到 CDN
## 功能描述

将直播流发布到 CDN（Content Delivery Network）的过程称为 CDN 直播推流，用户无需安装 App，可以通过 Web 浏览器观看直播。

在推流到 CDN 过程中，当频道中有多个主播时，通常会涉及到[转码](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#转码)，将多个直播流组合成单个流，并设置这个流的音视频属性和合图布局。



推流实现原理如下：

<img alt="../_images/live_ios_publishing_stream_cn.png" src="https://web-cdn.agora.io/docs-files/cn/live_ios_publishing_stream_cn.png"/>

## 前提条件

请确保已开通 CDN 旁路推流功能，步骤如下：
1. 登录 [Agora Dashboard](https://dashboard.agora.io)，点击左侧导航栏 ![img](https://web-cdn.agora.io/docs-files/1551250582235) 按钮进入**产品用量**页面。
2. 在页面左上角展开下拉列表选择需要开通 CDN 旁路推流的项目，然后点击旁路推流下的**分钟数**。
![](https://web-cdn.agora.io/docs-files/1569297956098)
3. 点击**开启旁路推流服务**。
4. 点击**应用** 即可开通旁路推流服务，并得到 500 个最大并发频道数。

<div class="alert note"> 并发频道数 N，指用户可以同时使用 N 路流进行推流转码。</div>

开通成功后，你可以在用量页面看到旁路推流的使用情况。

## 实现方法

声网提供的 CDN 旁路推流方案主要基于以下 API 进行推流、转码和布局设置：

-   [`setLiveTranscoding`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)：设置直播转码参数
-   [`addPublishStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)：添加推流地址
-   [`removePublishStreamUrl`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)：删除推流地址

其中：

- 主播在 `joinChannel` 成功后通过 `setLiveTranscoding` 接口定义转码参数和设置（RTMP 画布大小等信息）。CDN 音频推流时仍需设置一个 16 &times; 16 的最小视窗。
- 主播通过 `addPublishStreamUrl`接口指定推流地址。推流地址可以在开始推流后动态增删。


### 示例代码：

```C++
// CDN 转码设置
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
config.audioBitrate = 48;
config.width = 640;
config.height = 720;
config.videoFramerate = 30;
config.userCount = 1;  //如果 userCount > 1，则需要为每个 transcodingUsers 分别设置布局
config.videoCodecProfile = HIGH;

// 分配用户视窗的合图布局
config.transcodingUser = new TranscodingUser[config.userCount];
config.transcodingUser[0].uid = 123456; //uid 应和 joinChannel() 输入的 uid 保持一致
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 640;
config.transcodingUser[0].height = 720;

m_rtcEngine->setLiveTranscoding(config);
```

```C++
// 添加推流地址
// transcodingEnabled 设置为 true，表示开启转码。如开启，则必须通过 setLiveTranscoding 接口配置 LiveTranscoding 类。单主播模式下，我们不建议使用转码。
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }
```

```C++
// 删除推流地址
m_rtcEngine->removePublishStreamUrl(url);
```

### 调整合图布局

频道内有多个主播时，需要通过设置 <code>transcodingUser</code> 调整合图布局。

> 主播需要在<code> joinChannel</code> 后通过 <code>setLiveTranscoding</code> 接口定义转码参数和设置（RTMP 画布大小、多人混流布局等信息）。

**示例 1: 两人横向平铺**

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

**示例 2: 三人纵向平铺**

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

**示例 3: 1 人全屏 + 多人任意悬浮小窗**

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

## 开发注意事项

使用该功能需要联系 sales@agora.io 开通旁路推流。
