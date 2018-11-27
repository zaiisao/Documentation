
---
title: 集成客户端
description: 
platform: Windows
updatedAt: Wed Nov 14 2018 03:33:48 GMT+0000 (UTC)
---
# 集成客户端
## 前提条件

请确保满足以下开发环境要求:

-   操作系统： Microsoft Windows 7+

-   编译器：Microsoft Visual C++ 2013+

-   开发环境：Microsoft Visual Studio 2013 （推荐）


> 使用版本号高于 Microsoft Visual Studio 2013 的开发环境时，可能会有兼容性问题。

## 创建 Agora 账号并获取 App ID

1. 进入 [https://dashboard.agora.io/](https://dashboard.agora.io/) ，按照屏幕提示创建一个开发者账号。
2. 登录 Dashboard 页面，点击 **添加新项目**。

	<img alt="../_images/appid_1.jpg" src="https://web-cdn.agora.io/docs-files/cn/appid_1.jpg" />

1. 填写 **项目名**，然后点击 **提交**。
2. 在你创建的项目下，查看并获取该项目对应的 **App ID**。

	<img alt="../_images/appid_2.jpg" src="https://web-cdn.agora.io/docs-files/cn/appid_2.jpg" />



## 添加 SDK

1.  下载 [Windows SDK](https://docs.agora.io/cn/Agora%20Platform/downloads)，解压并打开。
2.  将 `sdk/include` 目录添加到项目的 INCLUDE 目录下。
3.  将 `sdk/lib` 目录放入项目的 LIB 目录下，并确认 agora_rtc_sdk.lib 与项目有连接。 
4.  将 `sdk/dll` 下的 dll 文件复制到你的可执行文件所在的目录下。

现在你已经设置好了 Windows 开发环境，可以开始使用 Agora SDK 了！



