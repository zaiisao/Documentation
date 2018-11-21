
---
title: 实现语音通话
description: 
platform: Android_C++
updatedAt: Fri Sep 28 2018 20:04:33 GMT+0800 (CST)
---
# 实现语音通话
# 实现语音通话

在本页你可以了解如何使用 Agora C++ SDK 在 Android 平台上实现语音通话。

## 准备环境

1.  关于具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，参考使用 Agora Native SDK 在 Android 平台实现语音通话的环境准备： [设置开发环境](../../cn/Quickstart%20Guide/android_audio.md) 。

2.  使用 Agora C++ SDK 时，*添加 SDK* 和 *添加 sourceSets* 的步骤和使用 Agora Native SDK 有所不同：

-   添加 SDK:

     -   联系 [sales@agora.io](mailto:sales@agora.io) 获取 Agora C++ SDK 。
     -   在 App Activity 中加载 SDK 动态库，如图：

       ![cpp_sdk_android_add_library.png](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537413322600)

-   添加 sourceSets:

     在 build.graddle 下添加 jar 包依赖地址,如图：
		 
	   ![cpp_sdk_android_sourceset_add_library.png](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537413415253)


3.  在编译配置文件中:

    -   声明共享库模块
    -   为共享库导出头文件
    -   链接动态库文件

		![cpp_sdk_android_makefile_add_library.png](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537413461358)
  
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

1.  创建并加入频道。


```
m_lpAgoraEngine->joinChannel(NULL, "Your Cahnnel Name", NULL, 0);
```

你现在可以开始 1 对 1 单聊或群聊了！单聊和群聊的区别在于加入同一频道的人数不同。


