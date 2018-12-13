
---
title: 初始化客户端对象
description: 微信小程序初始化客户端
platform: 微信小程序
updatedAt: Thu Dec 13 2018 08:53:52 GMT+0000 (UTC)
---
# 初始化客户端对象
将项目的 App ID 填入 初始化客户端对象 `init` 方法，即可初始化客户端对象。

```
client.init(appId, onSuccess, onFailure);
```

> - 由于小程序 SDK 支持与 Native SDK 和 Web SDK 互通，请确保各 SDK 使用相同的 App ID。
> - 如果你的 App ID 启用了 App Certificate，则在 [加入频道](../../cn/Video/join_live_mini.md) 时必须使用 Channel Key 或 Token。


