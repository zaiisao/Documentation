
---
title: 切换用户角色
description: 小程序平台设置或切换用户角色
platform: 微信小程序
updatedAt: Thu Nov 01 2018 08:12:54 GMT+0000 (UTC)
---
# 切换用户角色
# 切换用户角色
使用 `setRole` 方法设置用户角色。

```
client.setRole(role);
```

> 该方法必须在进入频道 [join](https://docs.agora.io/cn/Interactive%20Broadcast/cn/Quickstart%20Guide/join_live_mini) 前调用才能生效。如果用户已经在频道中，则需要先退出频道，使用该方法设置好用户角色后，再次加入频道。

