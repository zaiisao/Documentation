
---
title: 游戏相关
description: 
platform: 游戏相关
updatedAt: Thu Nov 01 2018 08:14:52 GMT+0000 (UTC)
---
# 游戏相关
# 游戏相关

### 我在使用 Unity3D 5.4 编译代码示例时，为什么报错?

由于 Agora 提供的代码示例使用的是 Unity3D 5.5，所以当你使用 Unity3D 5.4 时，可能会出现以下问题：

* CheckConsistency: GameObject does not reference component MonoBehaviour. Fixing. 当报这个错误时，删除掉项目当中的 HelloUnity3D 这个场景 (scene)，新建一个同名的场景，然后把 HelloUnity3D.cs 添加到该场景上 
* 导出 Google Android Project (ADT) 编译通过，但是运行的时候崩溃。当出现这个问题时， 请确保权限是否都正常，将子模块 AgoraAudioKit.plugin 的 AndroidManifest.xml 拷贝到自己的项目当中

AMG SDK 使用的是 Unity Plugins 机制集成，更多问题，建议认真阅读 相关文档 <https://docs.unity3d.com/Manual/PluginInspector.html>。

### 在 iOS 平台上编译代码示例时，为什么会出现签名出错?

如果报下图左边的错误时，请按照下图右边的方式解决该问题：

![](https://web-cdn.agora.io/docs-files/1539311534693)

### 在 iOS 平台上编译代码示例时，为什么会出现 bitcode 出错?

如果在编译过程出现以下 bitcode 报错信息，请关闭 bitcode：

![](https://web-cdn.agora.io/docs-files/1539311586378)

关闭方式如下：
1. 选中当前 Target 。
2. 选择 Build Settings。
3. 选择 Enable Bitcode，并将其设置为 No。

![](https://web-cdn.agora.io/docs-files/1539311614257)

### 在 iOS 平台上编译代码示例时，为什么会出现 core-telephony 出错?

当出现下图左边的错误时，请添加下图右边的 framework：

![](https://web-cdn.agora.io/docs-files/1539311666449)

在 iOS 平台上编译代码示例时，为什么会出现 libresolv 出错

当出现下图左边的错误时，请添加下图右边的所需库：

![](https://web-cdn.agora.io/docs-files/1539311709226)

### 在编译 Unity 代码示例时，我该如何导出 gradle 项目?

完成 [实现游戏语音功能](../../cn/Quickstart%20Guide/game_unity_android.md) 的编译步骤后，请根据实际需要使用如下命令进行编译(须根据自己环境修改命令)：

```
/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/bin/java -classpath 
"/Applications/Unity/PlaybackEngines/AndroidPlayer/Tools/gradle/lib/gradle-launcher-2.14.jar" 
org.gradle.launcher.GradleMain "clean" "assembleDebug"
```

### Unity 发生 DllNotFoundException？

通常是以下几点可能的原因：

* 使用编辑器调试，或者 Windows 调试的 ， 我们 默认不提供 Windows SDK ，所以不支持 mac 或者 Windows 直接调试。
*  SDK 不完整，通常直接建议客户把官网下载的 Demo 的 Plugin 直接添加到工程中。

