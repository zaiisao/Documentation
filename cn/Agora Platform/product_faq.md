
---
title: 产品
description: 
platform: All Platforms
updatedAt: Mon Jun 10 2019 08:08:23 GMT+0800 (CST)
---
# 产品
本页包含 Agora 产品的相关问题。

### 请问你们提供的解决方案包含什么内容？

我们提供的是 PAAS 层的解决方案。简单来说，我们提供 SDK，里面含有整套 API，你可以将此 SDK 集成到任何 App 或者网页上，实现音视频通话和直播。我们的 SDK 支持 iOS，Android，Windows，macOS，Linux，Web 和微信小程序。

### 声网  SDK 与哪些平台和版本兼容？

声网提供全平台 SDK，包括 iOS，Android，Windows，macOS，Linux，Web，微信小程序等。具体支持版本如下：
* Android：4.1+
* iOS：8.0+
* Windows：XP SP3+
* macOS：10.0+
* 微信小程序：最新版本
* Web：Chrome 58+；Firefox 56+；Safari 11+；Opera 45+；QQ 10+

### 我能通过使用声网 Agora 的实时云服务在我的程序中实现哪些通讯功能？

* App 之间的音视频通话；
* App 和网页 Web 之间的音视频通话；
* 网页 Web 之间的音视频通话；
* App 与手机落地电话通信（通过第三方合作）

### 声网 Agora 提供点播服务吗？

不提供。声网目前只提供音视频通信和全互动直播和连麦服务。

### 声网提供人脸识别、美颜和滤镜贴纸服务吗？

提供的。可以通过 400 632 6626 或 sales@agora.io 联系我们。

### 声网 Agora 提供鉴黄服务吗？

提供的。可以通过 400 632 6626 或 sales@agora.io 联系我们。

### 声网 Agora 支持自动重连么？

是的，支持。

### 声网 Agora 的通话与音频处理有什么特点？

我们在通话和音频处理上有很多的独特算法，希望用户通话时能真的有”声临其境”的体验。这些特点主要包括主动混音、自动增益控制、话音检测、舒适背景噪声和抗啸叫等。

想亲耳试听，了解我们在声音上的独特优势，可参考 https://www.agora.io/cn/voicecall/ 。

### Agora Cloud 与一般的 CDN RTMP 有何不同?

CDN+RTMP 的直播技术使得用户在网页端安装 flash player 就能观看直播，极大降低了观众的门槛。
有别于市面上最常见的 CDN+RTMP 直播技术，Agora.io 提供的直播方案为在 Agora Cloud、主播、以及高级观众(嘉宾) 端之间达到专线级别的实时通讯质量而使用了：

* 私有音视频编码
* 私有传输协议
* 私有节点部署
* 私有传输算法

详见：

<table>
  <tr>
    <th>条目</th>
    <th>常见的 CDN+RTMP 技术</th>
    <th>Agora Cloud</th>
  </tr>
  <tr>
    <td>视频编解码</td>
    <td>H.264</td>
    <td>私有</td>
  </tr>
  <tr>
    <td>语音编解码</td>
    <td>AAC</td>
    <td>私有</td>
  </tr>
  <tr>
    <td>传输协议</td>
    <td>基于 RTMP 的 TCP</td>
    <td>基于 RTMP 的 TCP 基于 UDP 的私有协议</td>
  </tr>
  <tr>
    <td>传输算法</td>
    <td>TCP</td>
    <td>Agora 私有丢包对抗、带宽自适应</td>
  </tr>
  <tr>
    <td>合图布局</td>
    <td>固定</td>
    <td>可动态调整</td>
  </tr>
</table>

为了照顾很多有需求的厂商，声网 Agora 与多家 CDN 进行了对接，且提供了旁路直播功能（可以进行社交分享)。


