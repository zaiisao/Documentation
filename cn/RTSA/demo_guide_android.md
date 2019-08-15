
---
title: Demo 使用指南
description: v1.1.1
platform: Android
updatedAt: Thu Aug 15 2019 03:54:24 GMT+0800 (CST)
---
# Demo 使用指南
本文指导开发者在将 RTSA SDK 集成到项目中之前，编译并运行模拟数据 Demo 进行初步了解。

## 前提条件
请确保开发环境满足以下要求:

* Android 5.0 及以上
* Android Studio 3.0 及以上

请获取以下文件：

* 最新版本的 Agora RTSA SDK for Android。
* 模拟数据 Demo。

## 创建 Agora 账号并获取 App ID
1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/RTSA/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


## 编译运行模拟数据 Demo

步骤如下：

1. 配置 SDK。
	解压 RTSA SDK 包，将 SDK 包中的 agora-rtc-sdk.jar 放置于模拟数据 Demo 的 andorid/app/libs 目录下，将包中的其它文件夹放入 andorid/app/src/main/jniLibs 目录下。

2. 配置 App ID。
	
	1). 进入源码目录：
~~~shell
cd andorid/app/src/main/java/io/agora/rtc/agorartcdatademo/
~~~
	
	2). 将 PLACEHOLDER 重命名为 AppIdAndCert.java：
~~~shell
cp PLACEHOLDER AppIdAndCert.java
~~~
	
	3). 在 AppIdAndCert.java 中填写你获取到的 App ID：
~~~java
static final String APP_ID = "$YOUR_APP_ID";
~~~

3. 在 Android Studio 中打开模拟数据 Demo，连接两台 Android 设备，编译运行。
	
	1). 点击 Sync 按钮，提示 Build: completed successfully 表示编译成功后，点击 Run → Run app 按钮运行 Demo。
	
	2). Demo 中点击右下角按钮，进入频道并自动收发数据。
	
 ![](https://web-cdn.agora.io/docs-files/1565604729166)
	
	3). 再次点击该按钮退出频道，停止收发数据，并打印简略统计信息。
	
	![](https://web-cdn.agora.io/docs-files/1565604782304)
