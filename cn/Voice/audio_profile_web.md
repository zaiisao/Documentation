
---
title: 设置音频属性
description: How to set audio profile for Web
platform: Web
updatedAt: Wed Sep 25 2019 02:54:30 GMT+0800 (CST)
---
# 设置音频属性
## 功能描述
 在一些比较专业的场景里，用户对声音的效果尤为敏感，比如语音电台，此时就需要对双声道和高音质的支持。
 所谓的高音质指的是我们提供采样率为 48 Khz、码率 192 Kbps 的能力，帮助用户实现高逼真的音乐场景，这种能力在语音电台、唱歌比赛类直播场景中应用较多。
## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Voice/web_prepare.md)。

Agora Web SDK 提供 [setAudioProfile](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile) 方法给开发者根据场景需求灵活配置适合的音质属性。这个方法的参数 `profile` 代表不同的音频参数配置，比如采样率、码率和编码模式等。

```javascript
  // 设置音频属性为 48 kHz 采样率，双声道，编码码率约 192 Kbps
  localStream.setAudioProfile("high_quality_stereo");
  localStream.init(function(){
   // init successful
  });
```

> 其他的参数选项，请参考 [setAudioProfile](https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile)。

## 开发注意事项

- 该方法必须在 `Stream.init` 之前调用
