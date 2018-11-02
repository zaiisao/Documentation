
---
title: 创建实例
description: 安卓平台初始化引擎
platform: Android
updatedAt: Thu Nov 01 2018 08:01:11 GMT+0000 (UTC)
---
# 创建实例
# 创建实例
导入以下 Agora API 包:

- `io.agora.rtc.Constants`
- `io.agora.rtc.IRtcEngineEventHandler`
- `io.agora.rtc.RtcEngine`
- `io.agora.rtc.video.VideoCanvas`

进入频道之前，调用 `create` 创建一个实例。在该方法中:

- 填入获取到的 App ID。只有 App ID 相同的应用程序才能进入同一个频道进行互通。
- 指定一个事件回调。SDK 通过指定的事件通知应用程序 SDK 的运行事件，如: 加入或离开频道，新用户加入频道等。

```java
import io.agora.rtc.Constants;
import io.agora.rtc.IRtcEngineEventHandler;
import io.agora.rtc.RtcEngine;
import io.agora.rtc.video.VideoCanvas;

...

private void initializeAgoraEngine() {
    try {
        mRtcEngine = RtcEngine.create(getBaseContext(), getString(R.string.agora_app_id), mRtcEventHandler);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));

        throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```


