
---
title: 离开频道
description: 
platform: Web
updatedAt: Tue Oct 29 2019 03:00:01 GMT+0800 (CST)
---
# 离开频道
通话或直播结束时，你可以使用 Agora SDK 离开频道。

## 实现方法

使用 `client.leave` 方法让用户离开当前通话或直播（频道）。

```javascript
client.leave(function () {
  console.log("Leave channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```

## 相关文档
你已在 App 中集成了基本的通话或直播功能。现在可以参考《常用功能》中的步骤实现更高级、复杂的功能。

如果在集成或使用中遇到问题，可以参考如下文档进行排查或解决，或登录 [控制台](https://dashboard.agora.io) 提交工单，Agora 技术支持会与你联系。

- [常见问题回答](../../cn/Agora%20Platform/general_questions.md)
- [集成与使用](../../cn/Agora%20Platform/general_questions.md)
- [故障排除](../../cn/Agora%20Platform/general_questions.md)
