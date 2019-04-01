
---
title: 美颜
description: 
platform: iOS
updatedAt: Mon Apr 01 2019 09:36:11 GMT+0000 (UTC)
---
# 美颜
## 功能描述
在社交娱乐或教育场景中，用户进行视频通话或直播时，常常希望向对方呈现良好的肌肤状态和精神面貌。Agora SDK 提供 API 方法，帮助 App 轻松实现基础美颜功能。用户可以开启美颜开关，调整美白、磨皮、祛痘、红润效果等美颜参数，实现自然的美颜效果。

具体效果可参考下图：

![](https://web-cdn.agora.io/docs-files/1553753458997)

## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Video/ios_video.md)。

Agora SDK 提供 [setBeautyEffectOptions](https://docs.agora.io/cn/Video/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:) 方法帮助开发者根据场景需求灵活配置基础美颜功能。

该方法有 2 个参数：

`enabled` 代表是否开启美颜功能。
`options` 代表美颜选项，包含 `lighteningContrastLevel`（明暗对比度）、`lightening`（亮度）、`smoothness`（平滑度）、`redness`（红色度）四个参数，可用来实现美白、磨皮、红润等效果。

```swift
//swift
let options = AgoraBeautyOptions()
options.lighteningContrastLevel = .normal
options.rednessLevel = 0
options.smoothnessLevel = 0
options.lighteningLevel = 0

agoraKit.setBeautyEffectOptions(true, options: options)
```

```oc 
//objective-c
AgoraBeautyOptions *options = [[AgoraBeautyOptions alloc] init];
options.lighteningContrastLevel = AgoraLighteningContrastNormal;
options.rednessLevel = 0;
options.smoothnessLevel = 0;
options.lighteningContrastLevel = 0;

[self.agoraKit setBeautyEffectOptions:YES options:options];
```

## 开发注意事项
以上方法有返回值，返回值小于 0 表示方法调用失败。

