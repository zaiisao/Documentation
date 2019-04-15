
---
title: 加入频道
description: 小程序加入频道
platform: 微信小程序
updatedAt: Thu Dec 13 2018 08:59:29 GMT+0800 (CST)
---
# 加入频道
在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Audio%20Broadcast/miniapp_video.md)。

## 实现方法
初始化客户端对象后，在成功的回调中调用  加入频道 `join` 方法，并在该方法中填入以下参数值：

-   `tokenOrKey`：在用户服务器端生成的动态密钥（Channel Key 或 Token）。详细生成办法见 [密钥说明](../../cn/Audio%20Broadcast/token.md) 。如果没有开启动态密钥，设置为 null。

-   `channel`：能标识频道的频道名。

-   `uid`：用户的 ID，整数，需保证唯一性。如果不指定，即用户 ID 设置为 null，回调会返回一个服务器分配的 `uid`。


```
client.join(tokenOrKey, channel, uid, onSuccess, onFailure);
```

如果你的小程序中有切后台的场景需求，还需要调用 `rejoin` 方法做好重连机制。

重连与正常加入频道类似，即将 进入频道 `join` 方法替换为 重新加入频道 `rejoin` 方法，且该方法需要加上当前频道内所有用户的 `uid` 列表作为额外参数。 重连后会重新发布和订阅流，参考 Github 示例代码更新推流/拉流地址即可。该过程可以参考 Github 开源 [示例项目](https://github.com/AgoraIO/Agora-Miniapp-Tutorial) 的 `reinitAgoraClient` 方法。

```
client.rejoin(channelKey, channel, uid, uids, onSuccess, onFailure);
```

示例代码：

```
client.init(APPID, () => {
  // uids 为频道中已有的用户 UID 列表
  client.rejoin(undefined, channel, uid, uids, () => {
    Utils.log(`client join channel success`);
    // 获取本地推流 Url 地址
    client.publish(url => {
      Utils.log(`client publish success`);
      resolve(url);
    }, e => {
      Utils.log(`client publish failed: ${e.code} ${e.reason}`);
      reject(e)
    });
  }, e => {
    Utils.log(`client join channel failed: ${e.code} ${e.reason}`);
    reject(e)
  })
}, e => {
  Utils.log(`client init failed: ${e} ${e.code} ${e.reason}`);
  reject(e);
});
```

## 相关文档
成功加入频道后，你可以使用 Agora SDK，实现如下功能进行通话/直播：

- [发布和订阅音视频流](../../cn/Audio%20Broadcast/publish_mini.md)
