
---
title: 设置音频属性
description: How to set audio profile for Web
platform: Web
updatedAt: Wed Sep 25 2019 10:08:12 GMT+0800 (CST)
---
# 设置音频属性
## 功能描述
 在一些比较专业的场景里，用户对声音的效果尤为敏感，比如语音电台，此时就需要对双声道和高音质的支持。
 所谓的高音质指的是我们提供采样率为 48 kHz、码率 192 Kbps 的能力，帮助用户实现高逼真的音乐场景，这种能力在语音电台、唱歌比赛类直播场景中应用较多。
## 实现方法
在设置音频属性前，请确保已在你的项目中实现基本的实时音视频功能。详见[实现音视频通话](../../cn/Video/start_call_web.md)或[实现互动直播](../../cn/Video/start_live_web.md)。

Agora Web SDK 提供 `setAudioProfile` 方法给开发者根据场景需求灵活配置适合的音质属性。这个方法的参数 `profile` 代表不同的音频参数配置，比如采样率、码率和编码模式等。

### API 时序图

下图展示使用设置音频属性的 API 调用时序：

![](https://web-cdn.agora.io/docs-files/1569380046096)

### 示例代码

```javascript
  // 设置音频属性为 48 kHz 采样率，双声道，编码码率约 192 Kbps
  localStream.setAudioProfile("high_quality_stereo");
  localStream.init(function(){
   // init successful
  });
```

### API 参考

- [setAudioProfile](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#setaudioprofile)

## 开发注意事项

- 该方法需要在 `Stream.init` 之前调用
