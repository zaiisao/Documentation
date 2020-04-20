
---
title: 美颜
description: 
platform: Windows
updatedAt: Mon Apr 20 2020 02:21:43 GMT+0800 (CST)
---
# 美颜

## 功能描述

在社交娱乐或教育场景中，用户进行视频通话或直播时，常常希望向对方呈现良好的肌肤状态和精神面貌。Agora SDK 提供 API 方法，帮助开发者轻松实现基础美颜功能。用户可以开启美颜开关，调整美白、磨皮、祛痘、红润效果等美颜参数，实现自然的美颜效果。

具体效果可参考下图：

![](https://web-cdn.agora.io/docs-files/1553753110307)

## 实现方法

在实现美颜功能前，请确保已在你的项目中实现基本的实时音视频功能。详见[开始音视频通话](../../cn/Interactive%20Broadcast/start_call_windows.md)或[开始互动直播](../../cn/Interactive%20Broadcast/start_live_windows.md)。

调用 [`setBeautyEffectOptions`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5899cc462e5250028c9afada4df98d48) 方法设置基础美颜功能。

该方法有 2 个参数：

- `enabled` 代表是否开启美颜功能。
- `options` 代表美颜选项，包含 `lighteningContrastLevel`（明暗对比度）、`lighteningLevel`（明亮度）、`smoothnessLevel`（平滑度）、`rednessLevel`（红润度）四个参数，可用来实现美白、磨皮、红润等效果。

### 示例代码

```c++
//开启美颜功能并以默认值设置美颜选项
bool enabled = true;
agora::rtc::BeautyOptions options;
options.lighteningContrastLevel = BeautyOptions::LIGHTENING_CONTRAST_NORMAL;
options.lighteningLevel = 0.7;
options.smoothnessLevel = 0.5;
options.rednessLevel = 0.1;
  
m_lpAgoraEngine->setBeautyEffectOptions(enabled, options);
```

同时，我们在 GitHub 提供已实现美颜功能的开源示例项目。你可以前往 [OpenLive-Windows](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-Windows) 或 [OpenVideoCall-Windows](https://github.com/AgoraIO/Basic-Video-Call/tree/dev/3.0.0/Group-Video/OpenVideoCall-Windows) 下载体验。
