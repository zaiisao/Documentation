
---
title: 离开频道
description: 
platform: Web
updatedAt: Thu Nov 01 2018 08:20:04 GMT+0000 (UTC)
---
# 离开频道
# 离开频道
使用 `client.leave` 方法让用户离开当前通话或直播（频道）。

```javascript
client.leave(function () {
  console.log("Leavel channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```
