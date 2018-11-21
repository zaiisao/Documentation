
---
title: 设置开发环境
description: 
platform: Android
updatedAt: Tue Sep 18 2018 21:35:12 GMT+0800 (CST)
---
# 设置开发环境
# 设置开发环境

## 前提条件

请确保满足以下开发环境要求:

-   Android SDK API Level \>= 16

-   Android Studio 2.0 或以上版本

-   Android [视频通话/视频直播 SDK](https://docs.agora.io/cn/2.3.1/download)

-   App 要求 Android 4.1 或以上设备


请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


> 你也可以下载 [语音通话/语音直播 SDK](https://docs.agora.io/cn/2.3.1/download) 并设置开发环境。语音和视频包的唯一区别是视频 SDK 包含语音和视频功能，详见 [设置开发环境](../../cn/Quickstart%20Guide/android_audio.md)。

## 注册账号和 App ID

1.  在 [Agora 官网](https://www.agora.io/cn/) 注册一个账号。注册成功后，页面会跳转到 [Dashboard](https://dashboard.agora.io/)。

2.  在 **项目列表** 页面点击 **添加新项目** 。

3.  填写想要的 **项目名**， 然后点击 **提交** 。

4.  在所创建的项目下复制 App ID。

5.  在你的 Android Studio 项目里，定位到 `app/src/main/res/values/strings.xml` 文件，并添加 App ID，如下图所示:

	<img alt="../_images/video_appid.png" src="https://web-cdn.agora.io/docs-files/cn/video_appid.png" style="width: 530.0px; height: 85.0px;"/>


## 添加 SDK

1.  在你的 Android Studio 项目里，从 **Project Files** 视图的 `app `文件夹下打开 `build.gradle` 文件。请把 `libs` 文件夹路径记在右边的 `fileTree` 里，因为这是你稍后放置 Android SDK 文件的路径。

	<img alt="../_images/video_buildgradle.png" src="https://web-cdn.agora.io/docs-files/cn/video_buildgradle.png" style="width: 884.8px; height: 368.0px;"/>

> -  `libs` 路径与应用程序的 `app` 目录相关。
> -   确保路径名称不包含中文字符。如果路径包含中文字符，则代码无法编译成功且会显示包含随机 ASCII 字符的错误信息。


2.  [下载](https://docs.agora.io/cn/2.3.1/download) Android 视频通话/视频直播 SDK。

3.  将下载的软件包中 `libs` 文件夹下的库复制到上文 `build.gradle` 文件的路径下。下表列出了 `libs` 文件夹内包含的内容:

	<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>文件/文件夹名称</strong></td>
<td><strong>备注</strong></td>
</tr>
<tr><td>agora-rtc-sdk.jar</td>
<td>必要的</td>
</tr>
<tr><td>armeabi-v7a/</td>
<td>文件夹</td>
</tr>
<tr><td>x86/</td>
<td>文件夹</td>
</tr>
<tr><td>arm64-v8a</td>
<td>文件夹</td>
</tr>
<tr><td>include</td>
<td>文件夹</td>
</tr>
</tbody>
</table>



## 添加 sourceSets

在 `build.gradle` 文件里, 将 `sourceSets` 添加到 Android JSON 对象。将 `libs` 的路径设置为 `app` 的 `libs` 目录的相对路径。

```
android {

...

sourceSets{
 main {
   jniLibs.srcDirs = ['../../../libs']
 }
}

}
```

## 同步项目文件

点击 **Sync Project With Gradle Files** ，直到同步完成。

<img alt="../_images/android9.png" src="https://web-cdn.agora.io/docs-files/cn/android9.png" style="width: 508.8px; height: 212.4px;"/>


## 配置 NDK

如果出现以下问题，请下载并安装 [Android NDK](https://developer.android.com/ndk/)：

<img alt="../_images/android6.png" src="https://web-cdn.agora.io/docs-files/cn/android6.png" style="width: 559.2px; height: 157.2px;"/>

1.  点击 Configure 按钮，并选择 **Project Defaults** \> **Project Structure**。 如下图所示:

	<img alt="../_images/project_structure.png" src="https://web-cdn.agora.io/docs-files/cn/project_structure.png" style="width: 886.0px; height: 687.0px;"/>

2.  将 [Android NDK](https://developer.android.com/ndk/) 复制到 Android Studio 项目的 Android NDK 位置。如下图所示：

	<img alt="../_images/android7.png" src="https://web-cdn.agora.io/docs-files/cn/android7.png" style="width: 680.8px; height: 605.6px;"/>

3.  点击 **Sync Project With Gradle Files** , 重新将 Android 项目与 NDK 文件进行同步。

	<img alt="../_images/android9.png" src="https://web-cdn.agora.io/docs-files/cn/android9.png" style="width: 508.8px; height: 212.4px;"/>


## 添加权限

1.  打开 **app \> src \> main** 路径下的 `AndroidManifest.xml `文件，添加必要的设备权限。例如:

	```
	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="io.agora.openvideocall">

	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.RECORD_AUDIO" />
	<uses-permission android:name="android.permission.CAMERA" />
	<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

	...

	</manifest>
	```

2.  点击 **Sync Project With Gradle Files** , 重新将 Android 项目与 `AndroidManifest.xml` 文件进行同步。

	<img alt="../_images/android9.png" src="https://web-cdn.agora.io/docs-files/cn/android9.png" style="width: 508.8px; height: 212.4px;"/>


## 防止混淆代码

在 `proguard-rules.pro` 文件中，为 Agora SDK 添加 -keep 类的配置，这样可以防止混淆 Agora SDK 公共类名称:

```
-keep class io.agora.**{*;}
```

现在你已经设置好了 Android 开发环境, 可以开始使用 Agora 视频通话/视频直播 SDK 了！


