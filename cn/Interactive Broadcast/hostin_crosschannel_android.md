
---
title: 实现跨直播间连麦
description: 
platform: Android
updatedAt: Fri Sep 28 2018 19:50:49 GMT+0800 (CST)
---
# 实现跨直播间连麦
# 实现跨直播间连麦

一般的直播场景里，同一直播频道 \(即直播间\) 的主播之间可以连麦互动，但是不同频道的主播之间无法连麦互动。本文提供了跨直播间主播连麦方案，如下所示:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方案</strong></td>
<td><strong>正面</strong></td>
<td><strong>负面</strong></td>
</tr>
<tr><td><a href="#server_hostin">服务器端跨直播间连麦</a></td>
<td>可进行跨直播间连麦互动，增加玩法</td>
<td>需要增加接口调用，保证接口调用逻辑和调用顺序正确</td>
</tr>
<tr><td><a href="#client_hostin">客户端跨直播间连麦</a></td>
<td>可进行跨直播间连麦互动，增加玩法</td>
<td>需要加入信令机制进行管理，逻辑较复杂，且主播间断开连麦时，需要恢复到连麦前状态</td>
</tr>
</tbody>
</table>


<a id = "server_hostin"></a>
## 服务器端跨直播间连麦

以 A、B、C 三个主播为例，该方案为三个主播加入同一个 Agora 直播频道，不同主播的观众端拉到的 RTMP 流不同。 A 主播的观众与 A 主播同在一个 App 直播间，这个逻辑是由你在 App 自己实现的。 我们说的跨直播间连麦指的是跨 App 直播间连麦，不是指不同的 Agora 频道间连麦。

在这个 App 直播间里，A 主播通过调用 API 设定该直播间的主播布局，例如，下图显示 A 观众看到的布局为 A 主播大图，其他连麦的主播为小图:

<img alt="../_images/server_hostin_cn.png" src="https://web-cdn.agora.io/docs-files/cn/server_hostin_cn.png" style="width: 588.0px; height: 728.0px;"/>


### 准备工作

1.  你已根据 [进阶：推流到 CDN](../../cn/Quickstart%20Guide/push_stream_android.md) 文档里的 **服务器端推流** 章节开通了 RTMP 流查看功能。

2.  你已搭建好 web 服务器。

3.  你已创建好一个内嵌了播放器的 HTML 文件。


### 主播端

1.  A、B、C 三个主播分别调用 `PublisherConfiguration` 方法设置旁路直播相关配置:


例如:

*主播 A:*

```
PublisherConfiguration:
  owner: true
  size: 360,640
  framerate: 15
  bitrate: 500
  defaultLayout: 1
  lifecycle: STREAM_LIFE_CYCLE_BIND2OWNER
  publisherUrl: rtmpPushUrlA
```

*主播 B:*

```
PublisherConfiguration:
    owner: true
    size: 360,640
    framerate: 15
    bitrate: 500
    defaultLayout: 1
  lifecycle: STREAM_LIFE_CYCLE_BIND2OWNER
    publisherUrl: rtmpPushUrlB
```

*主播 C:*

```
PublisherConfiguration:
    owner: true
    size: 360,640
    framerate: 15
    bitrate: 500
    defaultLayout: 1
  lifecycle: STREAM_LIFE_CYCLE_BIND2OWNER
    publisherUrl: rtmpPushUrlC
```

2.  A、B、C 三个主播分别调用 `joinChannel` 方法加入同一个 Agora 频道。


### 观众端

观众无需调用 `joinChannel` 加入主播所在的频道，只需要访问直播频道 URL 观看旁路直播即可，且能一键分享到各大社交平台。

主播们加入频道后，观众打开直播频道 URL \(各自拉到的 RTMP 流不同\)，看到的布局方式为:

-   默认看到主播上文调用 `PublisherConfiguration` 的 *defaultLayout* 设置的布局

-   如果用户使用 [进阶：推流到 CDN](../../cn/Quickstart%20Guide/push_stream_android.md) 设置了合图布局，则会看到对应的合图布局信息

<a id = "client_hostin"></a>
## 客户端跨直播间连麦

客户端跨直播间连麦，需要依赖于信令层的机制，流程示例如下:

<img alt="../_images/native_hostin.png" src="https://web-cdn.agora.io/docs-files/cn/native_hostin.png" style="width: 624.6px; height: 417.6px;"/>


### 步骤

1.  主播 A 通过信令请求主播 B 进行连麦；

2.  主播 B 通过信令同意连麦；

3.  主播 B 通过信令通知主播 B 的所有观众；

4.  主播 B 的所有观众退出 B 频道，加入 A 频道；

5.  主播 B 退出 B 频道，加入 A 频道;


此时，主播 A 与主播 B 可以进行连麦互动。两人或多人主播进行连麦时，流程与上述流程一致。 当连麦结束时，频道 B 的主播和观众需要恢复到连麦前的状态。


