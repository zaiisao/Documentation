
---
title: 集成客户端
description: 
platform: Android
updatedAt: Mon Jul 08 2019 03:18:02 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora SDK for Android 进行通话/直播前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

## 前提条件

请确保满足以下开发环境要求：

- Android SDK API Level ≥ 16

- Android Studio 3.0 或以上版本

- App 要求 Android 4.1 或以上设备

- 在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。

- 如果你的应用以 Android 9 为目标平台，请关注 [Android 隐私权变更](https://developer.android.com/about/versions/pie/android-9.0-changes-28?hl=zh-CN#privacy-changes-p)。

需要下载的文件：

Android [语音通话/语音直播 SDK](https://docs.agora.io/cn/Agora%20Platform/downloads)

下载的文件包括 libs 文件和 sample 文件，其中 libs 文件包括：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>文件/文件夹名称</strong></td>
<td><strong>文件类型</strong></td>
</tr>
<tr><td>agora-rtc-sdk.jar</td>
<td>Java JAR 文件</td>
</tr>
<tr><td>arm64-v8a</td>
<td>文件夹</td>
</tr>
<tr><td>armeabi-v7a</td>
<td>文件夹</td>
</tr>
<tr><td>include</td>
<td>文件夹</td>
</tr>
<tr><td>x86</td>
<td>文件夹</td>
</tr>
</tbody>
</table>


> 在正式使用 SDK 前，你可以尝试先在 sample 文件上集成 SDK。



## 创建项目并获取 App ID

1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。
2. 点击 **项目列表** 处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)

	 
## 添加 SDK

1. 设置 libs 存放路径。使用 Android Studio 打开你想要运行的项目（本文以 sample 文件为例），选择 *app/src/main/build.gradle* 文件，将预设的 libs 路径添加到 fileTree 代码行中。

  ![](https://web-cdn.agora.io/docs-files/1549865429141)

> 确保路径名称不包含中文字符。如果路径包含中文字符，则代码无法编译成功且会显示包含随机 ASCII 字符的错误信息。

2. 添加 libs 文件包。根据步骤 1 中预设的路径添加 libs 文件包。

3. 添加 sourceSets。在 `build.gradle` 文件里, 设置 `sourceSets` 的存放路径，该路径必须与 libs 路径一致。

   ```
   android {
    ...
    sourceSets {
           main {
               jniLibs.srcDirs = ['../../../libs']
           }
       }
   }
   ```

4. 添加 App ID。选择 *app/src/main/res/values/strings.xml* 文件，在下列代码中填写 App ID。

   ```
   <resources>
       <string name="app_name">Agora-Android-Voice-Tutorial</string>
       ...
       <string name="agora_app_id">YOUR APP ID</string>
   </resources>
   ```

5. 同步项目文件。点击 **Sync Project With Gradle Files** 按钮，直到同步完成。


## 配置 NDK

如需调用 `libs` 包中 include 文件夹里的插件时，需要配置 NDK 环境：

1. 点击 **Configure** 按钮，并选择 **Project Defaults** \> **Project Structure**，点击下载 Android NDK。

	 ![](https://web-cdn.agora.io/docs-files/1543307382646)
	
2. 下载完成后点击 **Finish**，Android Studio 会自动添加 NDK 路径。

	![](https://web-cdn.agora.io/docs-files/1543307802875)
	
	若 NDK 路径没有自动添加，则点击 select 手动添加即可，并在 local.properties 配置文件中检查配置路径是否正确。
	
	![](https://web-cdn.agora.io/docs-files/1543308080068)
	
3. 点击 **Sync Project With Gradle Files** 按钮，重新同步 Android 项目文件。
   

## 添加权限

1. 打开 *app/src/main/AndroidManifest.xml* 文件，添加必要的设备权限。例如：

	```
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="io.agora.tutorials1v1acall">
      
  <uses-permission android:name="android.permission.READ_PHONE_STATE" />	
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.RECORD_AUDIO" />
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <!-- The Agora SDK requires Bluetooth permissions in case users are using Bluetooth devices.-->
  <uses-permission android:name="android.permission.BLUETOOTH" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
  
  ...
  </manifest>
	```

2. 点击 **Sync Project With Gradle Files** , 重新同步 Android 项目文件。

## 防止混淆代码

在 `proguard-rules.pro` 文件中，为 Agora SDK 添加 `-keep` 类的配置，这样可以防止混淆 Agora SDK 公共类名称:

```
-keep class io.agora.**{*;}
```

## 相关文档
完成了客户端集成后，你可以使用 Agora SDK，依次实现左侧《快速开始》菜单栏下的步骤，进行通话/直播：

- 初始化
- 加入频道
- 发布和订阅流
