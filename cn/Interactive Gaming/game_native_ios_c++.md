
---
title: 实现游戏语音功能
description: 
platform: iOS
updatedAt: Tue Jan 07 2020 14:42:26 GMT+0800 (CST)
---
# 实现游戏语音功能
在本页你可以了解如何使用游戏语音版 C++ SDK 在 iOS 平台上实现语音通话。

## 准备环境

1.  关于具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，参考使用 Agora Native SDK 在 iOS 平台实现语音通话的环境准备：[准备开发环境](https://docs.agora.io/cn/Audio%20Broadcast/start_live_ios?platform=iOS#%E5%87%86%E5%A4%87%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)。

2.  使用游戏语音版 C++ SDK 时，需要 *添加 SDK* 和 *添加 Framework Search Paths* ：

-   添加 SDK: 

   - 下载 [游戏语音版 C++ SDK](https://docs.agora.io/cn/Agora%20Platform/downloads)。
   - 将 AgoraAudioKit.framework 文件添加至你的工程项目内。
   - 添加声网的 framework 库。声网的 framework 库依赖以下系统库:

     * `AgoraAudioKit.framework`
     * `Accelerate.framework`
     * `SystemConfiguration.framework`
     * `libc++.tbd`
     * `libredolv.tbd`
     * `CoreMedia.framework`
     * `AudioToolbox.framework`
     * `CoreTelephony.framework`
     * `AVFoundation.framework`
     * `CFNetwork.framework`
     
-   添加声网的 Framework Search Paths。
   
	 ![](https://web-cdn.agora.io/docs-files/1539335950857)

3.  在你的工程项目中导入 iOS C++ SDK 的头文件：

	![](https://web-cdn.agora.io/docs-files/1539335976784)
	
## 快速开始

1.  创建 IRtcEngine 对象,并填入 App ID。

  ```
  // m_lpAgoraEngine 是该类的数据成员
  m_lpAgoraEngine = (IRtcEngine *)createAgoraRtcEngine();
  RtcEngineContext ctx;

  // 从 IRtcEngineEventHandler 继承 event handler m_EngineEventHandler
  ctx.eventHandler = &m_EngineEventHandler;

  // 填写你的 App ID
  ctx.appId = APP_ID;

  m_lpAgoraEngine->initialize(ctx);
  ```

2.  创建并加入频道。

  ```
  m_lpAgoraEngine->joinChannel(NULL, "Your Cahnnel Name", NULL, 0);
  ```

你现在可以开始 1 对 1 单聊或群聊了！单聊和群聊的区别在于加入同一频道的人数不同。


