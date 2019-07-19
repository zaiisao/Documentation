
---
title: 集成客户端
description: 
platform: Windows
updatedAt: Fri Jun 14 2019 06:10:02 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora SDK for Windows 进行通话/直播前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

## 前提条件

请确保满足以下开发环境要求:

-   操作系统： Microsoft Windows 7+
-   编译器：Microsoft Visual C++ 2013+
-   开发环境：Microsoft Visual Studio 2013 （推荐）


> 使用版本号高于 Microsoft Visual Studio 2013 的开发环境时，可能会有兼容性问题。

## 创建项目并获取 App ID

1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/Audio%20Broadcast/sign_in_and_sign_up.md)。
2. 点击 **项目列表** 处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)



## 添加 SDK

1.  下载 [Windows Voice SDK](https://docs.agora.io/cn/Agora%20Platform/downloads)，解压并打开。
2.  将 `sdk/include` 目录添加到项目的 INCLUDE 目录下。
3.  将 `sdk/lib` 目录放入项目的 LIB 目录下，并确认 agora_rtc_sdk.lib 与项目有连接。 
4.  将 `sdk/dll` 下的 dll 文件复制到你的可执行文件所在的目录下。

现在你已经设置好了 Windows 开发环境，可以开始使用 Agora SDK 了！

## 相关文档
完成了客户端集成后，你可以使用 Agora SDK，依次实现左侧《快速开始》菜单栏下的步骤，进行通话/直播：

- 初始化
- 加入频道
- 发布和订阅流



