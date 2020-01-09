
---
title: Implement Voice for Gaming
description: 
platform: Android
updatedAt: Tue Jan 07 2020 10:12:43 GMT+0800 (CST)
---
# Implement Voice for Gaming
In this quickstart, you will learn how to use the Interactive Gaming Voice C++ SDK to make a voice call on Android platform.

## Prerequisites

1.  See [Set up the development environment](https://docs.agora.io/en/Audio%20Broadcast/start_live_android?platform=Android#set-up-the-development-environment) for the prerequisites, environment setup and SDK integration.

2.  When you use the SDK, the steps to *add SDK* and *add sourceSets* are different from using Agora Native SDK:

-   Add SDK:

     -   Download the [Interactive Gaming Audio Only SDK](https://docs.agora.io/en/Agora%20Platform/downloads)
     -   Load the SDK dynamic library in App Activity as shown:

			![cpp_sdk_android_add_library.png](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537413589164)
  
-   Add sourceSets:

    Add the jar package dependency directory under build.gradle as shown:

	![cpp_sdk_android_sourceset_add_library.png](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537413625140)

3.  In the build configuration file:

    -   Declare shared library module
    -   Export header files for shared libraries
    -   Link dynamic library files

		![cpp_sdk_android_makefile_add_library.png](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537413655851)

  
## Quickstart

1.  Create an IRtcEngine object and fill in your App ID.

	```
	m_lpAgoraEngine = (IRtcEngine *)createAgoraRtcEngine();
	RtcEngineContext ctx;

	// inherit the event handler m_EngineEventHandler from IRtcEngineEventHandler
	ctx.eventHandler = &m_EngineEventHandler;

	// Fill in your App ID
	ctx.appId = APP_ID;

	m_lpAgoraEngine->initialize(ctx);
	```

2.  Create and join a channel.

	```
	m_lpAgoraEngine->joinChannel(NULL, "Your Cahnnel Name", NULL, 0);
	```

You can now start 1 on 1 chat or group chat! The only difference is that the number of people joining the same channel.


