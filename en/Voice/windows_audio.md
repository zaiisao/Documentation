
---
title: Integrate the SDK
description: 
platform: Windows
updatedAt: Fri Jun 14 2019 05:35:43 GMT+0800 (CST)
---
# Integrate the SDK
## Prerequisites

-   OS: Microsoft Windows 7+
-   Compiler: Microsoft Visual C++ 2013+
-   Development environment: Microsoft Visual Studio 2013 (Recommended)

> If you use Microsoft Visual Studio 2013+, you may encounter compatibility issues.

## Create an Agora Project and Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Voice/sign_in_and_sign_up.md).

2. Click **Get Started** under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523371446)

3. Input your project name in the pop-up window and click **Create**. Follow the on-screen instructions to get to know the basic steps to start a video call. Once the project is created, you can find it under **Projects**.

	![](https://web-cdn.agora.io/docs-files/1563523478084)
	
4. Click the **Edit** button behind the new project, or the **Project Management** button ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to go to the **Project Management** page.

 ![](https://web-cdn.agora.io/docs-files/1563523678240)

5. On the **Project Management** panel, find the **App ID** of your project.

 ![](https://web-cdn.agora.io/docs-files/1563523737158)


## Add the Agora SDK to Your Project

1.  Download the [Windows Voice SDK](https://docs.agora.io/en/Agora%20Platform/downloads), and unzip it.
2.  Add the `sdk/include` folder to the INCLUDE directory of your project.
3.  Add the `sdk/lib` folder to the LIB directory of your project, and ensure that agora_rtc_sdk.lib is linked to your project.
4.  Copy all *dll* files under `sdk/dll`  to the directory of your executable file.

## Next Steps
You have set up the Windows development environment and can start a call/live broadcast following the steps under **Quickstart Guide**:
- Initialize the SDK
- Join a Channel
- Publish and Subscribe to Streams



