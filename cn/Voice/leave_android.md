
---
title: 离开频道
description: android平台离开频道
platform: Android
updatedAt: Tue Oct 29 2019 04:00:29 GMT+0800 (CST)
---
# 离开频道
通话或直播结束时，你可以使用 Agora SDK 离开频道。

## 实现方法
调用 `leaveChannel` 方法离开频道，结束或退出通话或直播。

不论当前是否还在直播中，调用该方法会把直播相关的所有资源释放掉。`leaveChannel` 并不会直接让用户离开频道。调用该方法后 SDK 会触发 `onLeaveChannel` 回调。

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> 如果在调用 `leaveChannel` 方法后立即使用 `destroy`，则退出频道会被打断，SDK 也不会触发 `onLeaveChannel` 回调。


## 相关文档
你已在 App 中集成了基本的通话或直播功能。现在可以参考《常用功能》中的步骤实现更高级、复杂的功能。

如果在集成或使用中遇到问题，可以参考如下文档进行排查或解决，或登录 [控制台](https://console.agora.io) 提交工单，Agora 技术支持会与你联系。

- [常见问题回答](../../cn/Agora%20Platform/general_questions.md)
- [集成与使用](../../cn/Agora%20Platform/general_questions.md)
- [故障排除](../../cn/Agora%20Platform/general_questions.md)
