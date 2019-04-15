
---
title: 切换用户角色
description: 小程序平台设置或切换用户角色
platform: 微信小程序
updatedAt: Fri Nov 02 2018 04:18:44 GMT+0800 (CST)
---
# 切换用户角色
使用 `setRole` 方法设置用户角色。

```
client.setRole(role);
```

> 该方法必须在 [加入频道](../../cn/Audio%20Broadcast/join_live_mini.md) 前调用才能生效。如果用户已经在频道中，则需要先退出频道，使用该方法设置好用户角色后，再次加入频道。

