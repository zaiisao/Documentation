
---
title: 设置视频属性
description: 
platform: Web
updatedAt: Tue Nov 27 2018 05:45:57 GMT+0000 (UTC)
---
# 设置视频属性
## 功能简介
在视频通话或互动直播中设置视频属性，可以根据用户喜好，调整视频画面的清晰度和流畅度，获得较高的用户体验。

视频属性一般包含视频分辨率、帧率、码率等数据。

## 实现方法

在设置视频属性前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Video/web_prepare.md)。

Agora Web SDK 通过 `setVideProfile` 方法来设置视频相关的属性，比如分辨率、码率、帧率等。参数均为理想情况下的最大值。当视频引擎因网络环境等原因无法达到设置的分辨率、帧率或码率的最大值时，会取最接近最大值的那个值。

```javascript
// javascript
// set video profile before initializing local stream
localStream.setVideoProfile("480p_3");
localStream.init(function(){
	// init successful
});
```

### API 参考
* [`setVideoProfiloe`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#setvideoprofile)

## 开发注意事项
- 不同的浏览器对分辨率的支持可能不同，详细的视频分辨率表可以在[这里](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#setvideoprofile)查看。
- 由于设备和浏览器的限制，部分浏览器对设置的 Video Profile 不一定能全部适配。这种情况下浏览器会自动调整分辨率，计费也将按照实际分辨率计算。
- 视频能否达到 1080P 以上的分辨率取决于设备的性能，在性能配备较低的设备上有可能无法实现。如果采用720P分辨率而设备性能跟不上，则有可能出现帧率过低的情况。
- Safari 浏览器视频帧率为 30 fps，不支持自定义视频帧率。

## 用户常见问题
### 能否推荐一些常用的视频分辨率、帧率、码率？

通常来讲，视频参数的选择要根据产品实际情况来确定，比如，如果是1对1 ，老师和学生的窗口比较大，要求分辨率会高一点，随之帧率和码率也要高一点；如果是1对4， 老师和学生的窗口都比较小，分辨率可以低一点，对应的码率帧率也会低一点，以减少编解码的资源消耗和缓解下行带宽压力。一般可按下列场景中的推荐值进行设置。

- 2 人视频通话场景：
  - 240p（分辨率 320*240、帧率 15 fps、码率 200 Kbps） 
  - 360p（分辨率 640*360、帧率 15 fps、码率 400 Kbps）
- 多人视频通话场景：
  - 120p（分辨率 160*120、帧率 15 fps、码率 65 Kbps）
  - 180p（分辨率 320*180、帧率 15 fps、码率 140 Kbps）
  - 240p（分辨率 320*240、帧率 15fps、码率 200 Kbps）

