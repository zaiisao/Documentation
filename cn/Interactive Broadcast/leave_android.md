
---
title: 离开频道
description: android平台离开频道
platform: Android
updatedAt: Thu Nov 01 2018 08:19:06 GMT+0000 (UTC)
---
# 离开频道
# 离开频道
调用 `leaveChannel` 方法离开频道，结束或退出通话或直播。

不论当前是否还在直播中，调用该方法会把直播相关的所有资源释放掉。`leaveChannel` 并不会直接让用户离开频道。调用该方法后 SDK 会触发 `onLeaveChannel` 回调。

```
 private void leaveChannel() {
    mRtcEngine.leaveChannel();
}
```

> 如果在调用 `leaveChannel` 方法后立即使用 `destroy`，则退出频道会被打断，SDK 也不会触发 `onLeaveChannel` 回调。

