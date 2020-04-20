
---
title: Image Enhancement
description: 
platform: Unity
updatedAt: Mon Apr 20 2020 09:40:18 GMT+0800 (CST)
---
# Image Enhancement
## Introduction

Agora provides an image enhancement API for users in social and entertainment scenarios to improve their appearance in video calls or live broadcasts. With this API, users can adjust settings such as the image contrast, brightness, sharpness, and red saturation, as shown in the following figure:

![](https://web-cdn.agora.io/docs-files/1553753660177)

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Video/start_call_unity.md) or [Start a Live Broadcast](../../en/Video/start_live_unity.md) for details.

Call the [`SetBeautyEffectOptions`](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#ad9c5e1a032d8c81c8e2a416a83ca0904) method to flexibly add image enhancement features.

This method has two parameters: 

- `enabled`: Sets whether or not to enable image enhancement.
- `beautyOptions`: Sets the image enhancement options, including `lighteningContrastLevel` for adjusting the contrast level, `lighteningLevel` for adjusting the brightness level, `smoothnessLevel` for adjusting the sharpness level, and `rednessLevel` for adjusting the red saturation level.

### Sample code

```c#
bool enabled = true;
BeautyOptions beautyOptions = new BeautyOptions();
beautyOptions.lighteningContrastLevel = BeautyOptions.LIGHTENING_CONTRAST_LEVEL.LIGHTENING_CONTRAST_HIGH;
beautyOptions.lighteningLevel = 0.7f;
beautyOptions.smoothnessLevel = 0.5f;
beautyOptions.rednessLevel = 0.1f;
// Enables image enhancement and sets the options.
mRtcEngine.SetBeautyEffectOptions(enabled, beautyOptions);
```
