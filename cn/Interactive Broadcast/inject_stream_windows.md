
---
title: 外部输入直播视频源
description: 
platform: Windows
updatedAt: Fri Sep 28 2018 19:50:02 GMT+0800 (CST)
---
# 外部输入直播视频源
# 外部输入直播视频源

## 简介

直播场景下，如果可以将采集到的视频，添加到正在进行的直播中，直播室里的主播和观众可以一起边看电影、比赛或演出，边进行点评、互动等功能，会让现有的直播话题更广、体验更好。

针对该需求，Agora 开发了 **外部输入直播视频源** 功能。通过该功能，

-   可以指定输入源（视频或纯音频），比如常用在音视频网站上的内容，作为直播源，替代 Camera 输入直播给频道内所有观众

-   可以对输入源的 video profile 进行设置

-   如果启动并设置了旁路直播，也可以将输入的视频源直播给所有旁路观众


主要涉及如下场景：

-   无人机或网络摄像头直接把采集到的视频推流出去，作为视频源导入直播

-   直播中直接拉入一路或多路 RTMP 或者 HLS 流，实现多人看视频互动的功能

-   支持赛事直播，最多同时支持 6 人连麦直播


> 外部视频源的启动/停止，只能由主播使用。 主播退出频道后，无需再调用 <code>removeInjectStreamUrl</code> 接口。 观众需要订阅主播才能观看外部视频源。 外部音视频源可以为纯音频源。

## 实现方法

本页演示如何通过调用 API 来实现输入外部视频源的功能。你也可以按照实际需要，自由组合 API，实现更多功能。

<img alt="../_images/inject_live_windows.png" src="https://web-cdn.agora.io/docs-files/cn/inject_live_windows.png" style="width: 595.0px; height: 568.0px;"/>


1.  创建 RtcEngine 对象 (`create`）。


```
IRtcEngine*createAgoraRtcEngine(agora::IRtcEngineEventHandler* pEventHandler)
```

2.  设置频道属性 (`setChannelProfile`)。


```
int setChannelProfile (CHANNEL_PROFILE_TYPE profile)
```

3.  设置本地视频显示属性 (`setupLocalVideo`)。


```
int setupLocalVideo(const VideoCanvas& canvas)
```

4.  设置远端视频显示属性 (`setupRemoteVideo`)。


```
int setupRemoteVideo(const VideoCanvas& canvas)
```

5.  打开视频模式 (`enableVideo`)。


```
int enableVideo()
```

6.  设置本地视频属性 (`setVideoProfile`)。


```
int setVideoProfile(VIDEO_PROFILE_TYPE profile)
```

7.  开始视频预览 (`startPreview`)。


```
int startPreview()
```

8.  加入频道 (`joinChannel`)。


```
int joinChannel(const char* token, const char* channel, const char* info, uid_t uid)
```

9.  输入外部视频源 (`addInjectStreamUrl`)。


```
virtual int addInjectStreamUrl(const char* url, const InjectStreamConfig& config) = 0
```

10.  删除外部视频源 (`removeInjectStreamUrl`)。


```
virtual int removeInjectStreamUrl(const char* url) = 0;
```

11.  离开频道 (`leaveChannel`)。


```
int leaveChannel ()
```

12.  停止视频预览 (`stopPreview`)。


```
int stopPreview()
```


