
---
title: 离开频道
description: 
platform: 微信小程序
updatedAt: Tue Oct 29 2019 03:00:41 GMT+0800 (CST)
---
# 离开频道
通话或直播结束时，你可以使用 Agora SDK 离开频道。

## 实现方法

使用 退出频道 `leave` 方法让用户离开当前通话或直播（频道）。

```
client.leave(onSuccess, onFailure);
```


> 由于微信小程序的部分内容是黑盒且不太稳定，我们不建议复用 Client。我们的最佳实践是在每次加入频道的时候重新建立一个新的 Client对象。调用 `leave` 或 `destory` 都会销毁 Client 对象的内部属性。

## 相关文档
你已在小程序中集成了基本的通话或直播功能。

如果在集成或使用中遇到问题，可以参考如下文档进行排查或解决，或登录 [控制台](https://dashboard.agora.io) 提交工单，Agora 技术支持会与你联系。

- [常见问题回答](../../cn/Agora%20Platform/general_questions.md)
- [集成与使用](../../cn/Agora%20Platform/general_questions.md)
- [故障排除](../../cn/Agora%20Platform/general_questions.md)
