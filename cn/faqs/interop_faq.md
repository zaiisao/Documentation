
---
title: Agora Web SDK 如何与 Agora Native SDK 互通
description: 
platform: Android,iOS,macOS,Web,Windows
updatedAt: Tue Dec 31 2019 16:42:15 GMT+0800 (CST)
---
# Agora Web SDK 如何与 Agora Native SDK 互通
- 在通信模式下，Agora Web SDK 和 Agora Native SDK 默认互通，无需额外设置。
- 在直播模式下，PC 或移动端用户（使用 Agora Native SDK 的用户）必须调用 `enableWebSdkInteroperability` 打开互通功能，详见[移动、桌面、Web 端互通](https://docs.agora.io/cn/Interactive%20Broadcast/interop_web?platform=Web)。
