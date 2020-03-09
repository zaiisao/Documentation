
---
title: 美颜
description: 
platform: Android
updatedAt: Mon Mar 09 2020 05:07:17 GMT+0800 (CST)
---
# 美颜
## 功能描述

在社交娱乐或教育场景中，用户进行视频通话或直播时，常常希望向对方呈现良好的肌肤状态和精神面貌。Agora SDK 提供 API 方法，帮助开发者轻松实现基础美颜功能。用户可以开启美颜开关，调整美白、磨皮、祛痘、红润效果等美颜参数，实现自然的美颜效果。

具体效果可参考下图：

![](https://web-cdn.agora.io/docs-files/1553753110307)

## 实现方法

在实现美颜功能前，请确保已在你的项目中实现基本的实时音视频功能。详见[开始音视频通话](../../cn/Video/start_call_android.md)或[开始互动直播](../../cn/Video/start_live_android.md)。

调用 [`setBeautyEffectOptions`](https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa9327de4fb0c29f840b1e68ca2e83fc6) 方法设置基础美颜功能。

该方法有 2 个参数：

* `enabled` 代表是否开启美颜功能。
* `options` 代表美颜选项，包含 `lighteningContrastLevel`（明暗对比度）、`lightening`（亮度）、`smoothness`（平滑度）、`redness`（红色度）四个参数，可用来实现美白、磨皮、红润等效果。

### 示例代码

```java
mRtcEngine.setBeautyEffectOptions(true, new BeautyOptions(LIGHTENING_CONTRAST_NORMAL, 0.5F, 0.5F, 0.5F));
```

同时，我们在 GitHub 提供已实现美颜功能的开源示例项目。你可以前往 [OpenLive-Android](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Android) 下载体验并参考 [`LiveActivity.java`](https://github.com/AgoraIO/Basic-Video-Broadcasting/blob/master/OpenLive-Android/app/src/main/java/io/agora/openlive/activities/LiveActivity.java) 文件中 `onBeautyClicked` 方法的代码。

## 开发注意事项

- 以上方法有返回值，返回值小于 0 表示方法调用失败。
- 美颜功能的开启会对低端机的性能造成影响，以至于无法达到预期的要求。对于低端机，视频编码设置为 360p 30 fps，720p 15 fps 或更高分辨率时，我们不建议开启美颜。
