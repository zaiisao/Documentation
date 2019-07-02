
---
title: 录制相关
description: 
platform: All Platforms
updatedAt: Tue Jun 11 2019 02:49:39 GMT+0800 (CST)
---
# 录制相关
### 怎么检测录音权限？

* iOS/macOS 平台上：首次运行系统会提示打开录音权限。如被手工关闭后开发者可以查询系统接口 `AVAudioSession.sharedInstance().recordPermission()[iOS8.0+]` 来检测录音权限。 
* Android 平台上：比较复杂，因为各个厂家的实现不一，并没有统一的手段可以检测到所有手机的录音权限。Agora Native SDK 会尝试检测录音失败并且上报 `ERR_ADM_RECORD_AUDIO_FAILED (1018)` 错误码，但未必能覆盖所有机型。

你的 App 可以通过此上报错误码来提示用户检测录音权限。

### 音视频通话时我可以录音吗？

我们提供录音功能，可以通过 API 将录音文件直接存储到开发者指定的服务器上。

### 我到底该用一种录制方案, 录制 SDK, 录制服务，还是 CDN 直播录制?

推荐所有的新用户使用录制 SDK 版，其他方案均已被弃用。

### 录制的触发是由 Linux 服务器控制的吗？
是的。

### 由于服务器自己触发录制，那它怎么知道什么时候加入频道，频道号等？
通过你自己的信令。

当录制 SDK 加入频道时，API 的行为与客户端使用 Agora Native SDK 调用 API 的行为类似。

### 录制完成动作如何捕获

自动模式录制：如果频道内没有人，当idle时间到，则停止录制，然后 `leaveChannel` 离开频道。应用侧监控 `leaveChannel` 则表示录制完成，可以转入下一步处理逻辑：例如录制完成后把录制文件上传到其他服务器上等。

### 是否可以针对频道内的某个用户进行录制
不行。会录下所有有音频或者视频的用户。

### 录制的频道模式必须和 Native SDK 或 Web SDK 设置的频道模式保持一致吗？
对，如果不一致，可能会引发问题。

### 当使用命令行进行录制时，如何停止录制?

当使用命令行进行录制时，会在以下情况下停止录制：

* 在命令行运行时，在键盘上同时按下 Ctrl 和 C 键结束进程。
* 当你设置参数 idle (设置空闲频道超时退出时间) 的值后，如果频道内无用户的状态超过设定的时间，录制程序自动退出。默认值为 300 秒。

### 在使用命令行工具集成录制 SDK 时，报错 java.land.UnsatisfiedLinkError: no recording in java.library.path，如何解决？

报错原因：系统环境找不到 `librecording.so` 库文件。

解决方法：确认 Java demo 是否成功编译并生成了库文件，查看并配置库文件的位置。

例如，若库文件位置为 `/home/user/Desktop/tool/Agora_Recording/samples/java/bin/io/agora/recording/librecording.so`，则在 Linux 系统下，在 `/etc/profile` 或者 `~/.bash_profile`、`~/.bashrc` 下配置 `LD_LIBRARY_PATH` ：
```
LD_LIBRARY_PATH=/home/user/Desktop/tool/Agora_Recording/samples/java/bin/io/agora/recording/librecording.so
```

不同系统与程序配置 `java.library.path` 的方法请参考[修改java.library.path的位置](https://blog.csdn.net/quqibing001/article/details/51201768)。
