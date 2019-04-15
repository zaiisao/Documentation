
---
title: 切换用户角色
description: web平台上设置或切换用户角色
platform: Web
updatedAt: Tue Feb 19 2019 10:55:32 GMT+0800 (CST)
---
# 切换用户角色

直播频道分主播和观众两种用户角色。两者的区别在于：
- 主播：可以收听和发布音视频消息。根据应用程序的实现，还可以与观众互动、指定观众连麦。同一直播频道内，主播只能听到和看到自己以及连麦主播的音视频。
- 观众：只能收听主播的音视频消息。根据应用程序的实现，还可以发布实时文字消息，与主播互动。同一直播频道内，所有观众都能看到主播以及连麦主播的音视频。

Web v2.5.1 新增 `setClientRole` 接口，可实现用户角色切换。
> 请确保在直播模式（[mode](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.clientconfig.html#mode) 设置为 `live`）下调用。通信模式（[mode](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.clientconfig.html#mode) 设置为 `rtc`）默认所有用户都是主播角色，无法使用本方法。

## 实现方法

在切换用户角色前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Interactive%20Broadcast/web_prepare.md)。


```
//设置 role（用户角色）。role 分为 “host”（主播）和 “audience”（观众）。
client.setClientRole("host", function() {
  console.log("setHost success");
}, function(e) {
  console.log("setHost failed", e);
})

...

//角色变化的回调
client.on("client-role-changed", function(evt) {
  console.log("client-role-changed", evt.role);
});

```

该方法在加入频道前后都可以调用：
- 加入直播频道前，调用该方法将用户设置为主播或观众。
- 直播过程中，调用该方法将用户角色由观众切换为主播（上麦），或由主播切换为观众。

切换用户角色后，会收到 `client-role-changed` 事件回调。

### 开发注意事项

- 如果观众用户调用 [`client.publish`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#publish) ，`role` 会自动切换为 `host`； 
- 如果主播用户调用 [`client.unpublish`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#unpublish) ，`role` 会自动切换为 `audience` 。


## 相关文档
直播频道中的用户角色切换为主播后，可以使用 Agora SDK，实现如下功能进行互动直播：

- [发布和订阅音视频流](../../cn/Interactive%20Broadcast/publish_web_live.md)

如果在直播过程中，对音量、音效、视频分辨率等有特殊需求，你还可以：

- [调整通话音量](../../cn/Interactive%20Broadcast/volume_web.md)
- [播放音效/音乐混音](../../cn/Interactive%20Broadcast/effect_mixing_web.md)
- [设置视频属性](../../cn/Interactive%20Broadcast/videoProfile_web.md)




