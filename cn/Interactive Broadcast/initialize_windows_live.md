
---
title: 创建 AgoraRtcEngine 实例并初始化
description: windows平台初始化
platform: Windows
updatedAt: Thu Mar 07 2019 07:47:24 GMT+0000 (UTC)
---
# 创建 AgoraRtcEngine 实例并初始化
在创建 AgoraRtcEngine 实例并初始化前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Interactive%20Broadcast/windows_video.md)。

## 实现方法
进入频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

## 相关文档
完成创建实例后，你可以使用 Agora SDK，依次实现如下功能进行互动直播：
* [加入频道](../../cn/Interactive%20Broadcast/join_live_windows.md)
* [切换用户角色](../../cn/Interactive%20Broadcast/role_windows.md)
* [发布和订阅音频流](../../cn/Interactive%20Broadcast/publish_windows_live.md)

如果对网络或音质有特殊的需求，你还可以在加入频道前：
* [进行通话前网络质量监测](../../cn/Interactive%20Broadcast/lastmile_windows.md)
* [使用双声道/高音质](../../cn/Interactive%20Broadcast/audio_profile_windows.md)

## 常见问题

Media Foundation 和 dshow 是微软提供的两种视频采集方式。有些摄像头两个都支持；有些摄像头仅支持其中一个视频采集方式，造成摄像头无采集视频。声网允许客户通过如下私有接口设置 SDK 的视频采集方式。

### 私有接口

`agora::rtc::AParameter::AParameter::setInt(const char* key, int value);
`

### 参数说明

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>赋值</strong></td>
</tr>
<tr><td><code>key</code></td>
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>account</code></td>
<td>"che.video.videoCaptureType"</td>
</tr>
<tr><td><code>value</code></td>
<ul>
<li>0: DirectShow</li>
<li>1: VFW (Video for Windows)</li>
<li>2: Media Foundation</li>
<li>3: DirectShow</li>
</ul>
</td>
</tr>
</tbody>
</table>


### 示例代码

`AParameter apm(*m_lpAgoraEngine);
 int nRet = apm->setInt("che.video.videoCaptureType", nType);`



