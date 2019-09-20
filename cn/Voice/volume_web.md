
---
title: 调整通话音量
description: How to adjust volume on web
platform: Web
updatedAt: Fri Sep 20 2019 03:30:43 GMT+0800 (CST)
---
# 调整通话音量
## 功能描述
 在使用我们 SDK 时，开发者可以对 SDK 采集到的声音及 SDK 播放到声卡的声音音量进行调整，以满足产品在声音上的个性化需求。比如进行双人通话时，想实现静音操作，可以通过调整播放音量的接口将音量设置为 0。


## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Voice/web_prepare.md)。

### 调节别人的音量

```javascript
client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // 将订阅流的音量设置为 50
  stream.setAudioVolume(50);
});
```

### 静音

```javascript
client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // 将订阅流静音
  stream.setAudioVolume(0);
});
```

## 开发注意事项

- 音量设置太大在某些设备上可能出现爆音现象
