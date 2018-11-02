
---
title: 其他常见问题
description: 
platform: 其他常见问题
updatedAt: Fri Nov 02 2018 04:05:49 GMT+0000 (UTC)
---
# 其他常见问题
### 无法加入频道

查看是否有错误码，以及查看是哪种错误码引起的加入频道失败。

如果错误码提示是因为没有退出上次通话引起的，先调用 `leaveChannel `，然后再次加入频道。

### 无法通过 Dynamic Key/Token 验证

检查：
* Dynamic Key/Token 生成的算法是否正确。
* App ID 和 App Certificate 是否填写正确，注意区分大小写。
* expireTime 是否晚于当前时间。

### 当用户加入频道后切换到后台模式一段时间后(例如 30 分钟)，发现 iOS App 崩溃了

该问题出现的原因是当集成 Agora SDK 时，没有在 Background Modes 里选择 **Audio**, **AirPlay**, and **Picture in Picture** 。

![](https://web-cdn.agora.io/docs-files/1539316495983)

你仅需要在 Xcode 勾选该选项即可。

### 呼叫成功，但是没有声音

初始化信令时，有初始化媒体（使用 `getInstanceWithMediaKey`）。

呼叫成功后，必须加入频道才能听见声音。

### 呼叫失败

1. 请确保您已登录。只有在登录状态才能发起呼叫。
2. 检查对方是否为在线状态，如果对方为离线，则无法呼叫成功。
3. 当 App 在 iOS 上进入后台模式后，通话中断。在您的App上：
    * 选择 project>Capabilities
    * 打开 Background Modes， 选择 Audio, AirPlay and Picture in Picture 和 Voice over ip 选项。

### 日志打印中出现 Failed to decode frame xxx, return error code is -1

这个是解码失败，最常见的情形是网络不好时丢包导致收不到完整帧，引起解码出错，如果日志打印中出现这段信息，检查一下当前的网络环境。

### XP 系统初始化失败

Agora 的 SDK 从 1.1 版本开始，编译器默认采用 VC2013，依旧会支持 XP 系统，但由于操作系统未部署相关运行时库，需从微软下载，地址如下(该地址可能由于微软官方变更而失效): https://www.microsoft.com/en-us/download/details.aspx?id=40784

请勿从其他第三方转载地址下载，很多下载站所提供版本并非最新版，即使安装也无法正常运行。

### 启动 app 时出现 Exception 信息

当集成 Agora SDK 以后，启动 app 出现下面的 Exception 信息：

```
dalvik.system.PathClassLoader[DexPathList[[zip file xxxxx.apk],nativeLibraryDirectories=[/data/app/xxxxx/lib/arm,/vendor/lib/,system/lib]]]
couldn’t find "libHDACEngine.so" java.lang.Runtime.loadLibrary(Runtime.java:366)…
```

这种情况是由于集成时没有将 .so 文件放置在正确的平台路径下, 导致 App 启动后找不到 .so 文件导致的。打包 APK 时请注意平台和路径。

