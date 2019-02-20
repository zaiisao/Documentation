
---
title: 加入频道
description: web平台加入频道
platform: Web
updatedAt: Thu Dec 13 2018 07:58:39 GMT+0000 (UTC)
---
# 加入频道
在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Interactive%20Broadcast/web_prepare.md)。

## 实现方法

初始化 Client 对象完成后， 在成功的回调中调用 `client.join` 方法。

在  `client.join` 方法中填入以下参数值：

- `tokenOrKey`：如果安全要求不高，将参数值设为 null；如果安全要求高，传入从你的服务端获得的 token 或 Channel Key 值。详情参考 [校验用户权限](../../cn/Audio%20Broadcast/token.md)。
- `channel`：频道名称。
- `uid`：用户的 ID， **整数，需保证唯一性**。 如果不指定，即用户 ID 设置为 null，回调会返回一个服务器分配的 uid。

```javascript
client.join(<TOKEN_OR_KEY>, <CHANNEL_NAME>, <UID>, function(uid) {
  console.log("User " + uid + " join channel successfully");

}, function(err) {
  console.log("Join channel failed", err);
});
```

> 使用不同 App ID 的用户即使加入同一个频道也无法互相通话。

## 相关文档
成功加入频道后，你可以使用 Agora SDK，实现如下功能进行互动直播：

- [切换用户角色](../../cn/Interactive%20Broadcast/role_web.md)
- [发布和订阅音视频流](../../cn/Interactive%20Broadcast/publish_web_live.md)

在直播过程中，如果对音量、音效、视频分辨率等有特殊需求，你还可以：

- [调整通话音量](../../cn/Interactive%20Broadcast/volume_web.md)
- [播放音效/音乐混音](../../cn/Interactive%20Broadcast/effect_mixing_web.md)
- [设置视频属性](../../cn/Interactive%20Broadcast/videoProfile_web.md)
