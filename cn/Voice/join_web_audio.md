
---
title: 加入频道
description: web平台加入频道
platform: Web
updatedAt: Thu Dec 13 2018 07:24:56 GMT+0800 (CST)
---
# 加入频道
在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Voice/web_prepare.md)。

## 实现方法

初始化 Client 对象完成后， 在成功的回调中调用 `client.join` 方法。

在  `client.join` 方法中填入以下参数值：

- `tokenOrKey`：传入能标识用户角色和权限的 Token。Token 需要在你的服务器端生成，详细生成办法见[密钥说明](../../cn/Voice/token.md)。

> 在 [Dashboard](https://dashboard.agora.io/) 注册项目后，你可以获取一个临时 Token 用于测试。生产环境下，我们推荐你使用在自己的服务端生成的正式 Token。
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
成功加入频道后，你可以使用 Agora SDK，实现如下功能进行语音通话：
- [发布和订阅音频流](../../cn/Voice/publish_web_audio.md)

如果在通话过程中，对音量、音效、音调等有特殊需求，你还可以：
- [调整通话音量](../../cn/Voice/volume_web.md)
- [播放音效/音乐混音](../../cn/Voice/effect_mixing_web.md)
