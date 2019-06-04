
---
title: Integrate the SDK
description: 
platform: Windows
updatedAt: Tue Jun 04 2019 09:23:47 GMT+0800 (CST)
---
# Integrate the SDK
## Prerequisites

-   OS: Microsoft Windows 7+

-   Compiler: Microsoft Visual C++ 2013+

-   Development environment: Microsoft Visual Studio 2013 (Recommended)


> If you use Microsoft Visual Studio 2013+, you may encounter compatibility issues.

## Create an Agora Account and Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/) and follow the on-screen instructions to create a project.
2. Click the **Project Management** icon ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation panel.
3. Find the corresponding **App ID** under the created project.


## Add the Agora SDK to Your Project

1.  Download the [Windows SDK](https://docs.agora.io/en/Agora%20Platform/downloads), and unzip it.
2.  Add the `sdk/include` folder to the INCLUDE directory of your project.
3.  Add the `sdk/lib` folder to the LIB directory of your project, and ensure that agora_rtc_sdk.lib is linked to your project.
4.  Copy all *dll* files under `sdk/dll`  to the directory of your executable file.

## Next Steps
You have set up the Windows development environment and can start a call/live broadcast following the steps under **Quickstart Guide**:
- Initialize the SDK
- Join a Channel
- Publish and Subscribe to Streams



