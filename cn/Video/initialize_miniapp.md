
---
title: 初始化客户端对象
description: 微信小程序初始化客户端
platform: 微信小程序
updatedAt: Thu Dec 13 2018 08:54:04 GMT+0800 (CST)
---
# 初始化客户端对象
在初始化客户端对象前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Video/miniapp_video.md)。

## 实现方法
将项目的 App ID 填入 初始化客户端对象 `init` 方法，即可初始化客户端对象。

```
client.init(appId, onSuccess, onFailure);
```

> - 由于小程序 SDK 支持与 Native SDK 和 Web SDK 互通，请确保各 SDK 使用相同的 App ID。
> - 如果你的 App ID 启用了 App Certificate，则在 [加入频道](../../cn/Video/join_live_mini.md) 时必须使用 Channel Key 或 Token。


## 相关文档

完成初始化客户端对象后，你可以使用 Agora Miniapp SDK for WeChat，依次实现如下功能进行通话/直播：

- [加入频道](../../cn/Video/join_mini.md)
- [发布和订阅流](../../cn/Video/publish_mini.md)

如有需要，请在加入频道前[切换用户角色](../../cn/Video/role_mini.md)，以加入直播频道。
