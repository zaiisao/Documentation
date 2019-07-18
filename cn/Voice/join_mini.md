
---
title: 加入频道
description: 小程序加入频道
platform: 微信小程序
updatedAt: Thu Jul 18 2019 11:30:51 GMT+0800 (CST)
---
# 加入频道
## 前提条件

在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Voice/miniapp_video.md)。

加入频道时，你需要传入 Token。在 Dashboard 注册项目后，你可以获取一个临时 Token 用于测试。参考如下步骤获取临时 Token。

1. 开启 App Certificate，详见[启用 App 证书](../../cn/Voice/token.md)。
2. 在项目详情处，点击**生成临时 Token**，输入频道名，你就会在 **Token** 页面获取一个临时 Token。

	![](https://web-cdn.agora.io/docs-files/1562926292439)

	![](https://web-cdn.agora.io/docs-files/1562926303571)

## 实现方法
初始化客户端对象后，在成功的回调中调用  加入频道 `join` 方法，并在该方法中填入以下参数值：

-   `tokenOrKey`：能标识用户角色和权限的 Token。测试环境下，你可以使用获取到的临时 Token。生产环境下，我们推荐你使用在自己的服务端生成的正式 Token。
-   `channel`：能标识频道的频道名。
-   `uid`：用户的 ID，整数，需保证唯一性。如果不指定，即用户 ID 设置为 null，回调会返回一个服务器分配的 `uid`。


```
client.join(token, channel, uid, onSuccess, onFailure);
```

如果你的小程序中有切后台的场景需求，还需要调用 `rejoin` 方法做好重连机制。

重连与正常加入频道类似，即将 进入频道 `join` 方法替换为 重新加入频道 `rejoin` 方法，且该方法需要加上当前频道内所有用户的 `uid` 列表作为额外参数。 重连后会重新发布和订阅流，参考 Github 示例代码更新推流/拉流地址即可。该过程可以参考 Github 开源 [示例项目](https://github.com/AgoraIO/Agora-Miniapp-Tutorial) 的 `reinitAgoraClient` 方法。

```
client.rejoin(token, channel, uid, uids, onSuccess, onFailure);
```

示例代码：

```
client.init(APPID, () => {
  // uids 为频道中已有的用户 UID 列表
  client.rejoin(, channel, uid, uids, () => {
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

- [发布和订阅音视频流](../../cn/Voice/publish_mini.md)
