
---
title: 创建实例
description: 安卓平台初始化引擎
platform: Android
updatedAt: Fri Jul 19 2019 08:23:01 GMT+0800 (CST)
---
# 创建实例
## 前提条件

在创建实例前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/android_audio.md)。

初始化过程中，你需要传入一个的 App ID。因此需要现在 Agora Dashboard 注册项目并获取 App ID。

1. 进入 [控制台](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)



## 实现方法

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

> 请确保在调用其他方法前先调用 `create` 方法创建并初始化 RtcEngine。

## 相关文档
完成创建实例后，你可以使用 Agora SDK，依次实现如下功能进行语音通话：
* [加入频道](../../cn/Voice/join_communication_android.md)
* [发布和订阅音频流](../../cn/Voice/publish_android_audio.md)

如果对网络或音质有特殊的需求，你还可以在加入频道前：
* [进行通话前网络质量检测](../../cn/Voice/lastmile_android.md)
* [使用双声道/高音质](../../cn/Voice/audio_profile_android_audio.md)


