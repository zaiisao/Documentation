
---
title: 录制相关
description: 
platform: 录制相关
updatedAt: Thu Nov 29 2018 08:13:57 GMT+0000 (UTC)
---
# 录制相关
## 录制 SDK

### 开始录制后，为什么没有录制文件？

请检查 `appId` 设置的是否与客户端使用 Agora Native SDK 时设置的一样。
请检查 `channelProfile` 的模式是否与客户端使用 Agora Native SDK 设置的一样。例如，录制的设为直播模式，Agora Native SDK 设置通信模式，直播和通信模式的视频无法互通，因此不会有录制文件。

### 录制结束后，为什么检查录制的视频没有声音？

录制的音视频文件为独立的，视频为 mp4 格式文件，语音为 aac 格式文件。视频文件本身不含语音，你需要手动转码将多个音视频文件合并成一个文件。若检查发现已是转码合成后的最终视频文件，联系技术支持。

### 录制结束后，为什么无法播放录制的 MP4 文件？

请参考[播放器列表](../../cn/API%20Reference/recording_java.md)，查看是否使用了不支持的播放器。

### 启用加密模式后，为什么无法播放录制的视频文件，声音也不正常？

启用加密模式后，如果密码输入不正确或者没有输入密码，录制的文件无法播放。

由于语音被加密了，导致声音不正常。

### 当我将加密密码设置为跟客户端一样时，为什么录制仍然不成功？

请确保加密模式与客户端设置的一样，小写的 aes-128-xts 或 aes-256-xts 。

### 如何使用 Channel Key?

详见[Channel Key 密钥说明](../../cn/Agora%20Platform/channel_key.md)。

### 当启用合流模式之后，为什么只有一个 uid 为 0 的录制文件？

这是默认的设计，当启用合流模式后，多个用户加入频道，所有人的语音会被录制在同一个文件里，uid 为 0。

### 如何对录制文件进行转码?

关于如何手动对录制文件进行转码，详见[录制音视频](../../cn/Recording/recording_voice_video.md)。

如果出现问题，请联系客户支持分析和定位问题。

### 我可以自己设置录制文件的路径和文件名？

你可以自己设置录制文件的根目录，但是自己无法命名。详见[录制音视频](../../cn/Recording/recording_voice_video.md)。

### 如何保证同时录制的频道数没有超过服务器容量(CPU/内存)?

请参考文档[设置开发环境](../../cn/Recording/recording_env.md)。

### 为什么客户端可以加入频道，而录制端无法加入频道（录制端可以发包，但是却收不到来自 Agora 服务器的包）？

请检查防火墙配置，详见[设置开发环境](../../cn/Recording/recording_env.md)。

### 可以指定只录某个用户的音视频么?

暂时不支持。

### 如何知道程序是正常退出频道，还是异常退出频道?

如果是正常退出频道，首先 SDK 会触发 `onLeaveChanne`，并会返回错误码 ERR_INTERNAL_FAILED = 3。
如果是异常退出频道，SDK 不会触发 `onLeaveChannel`，而是触发 `onError` 并返回错误码 ERR_INTERNAL_FAILED = 3 。

### 录制文件转码后的格式是什么?

详见[录制音视频](../../cn/Recording/recording_voice_video.md)。

### 使用命令行时，只输入默认的 must 参数，为何启动不了录制程序？

请检查参数配置，例如在输入 `--appliteDir` 参数时，只需指到 **video_recorder** 文件夹，不要指到 **video_recorder** 这个文件。

### 为什么录制并转码完成后的视频，在播放的时候，视频前面会黑一小段？

可能的原因如下：

* 网络不好；
* 视频包 I 帧收到之后，才会创建视频录制，在此之前收到其他 B,P 帧会丢掉；
* 视频包的每一帧都比音频的大，所以音频包通常都会比视频包先收到并开启录制。

### 能录制旋转的视频吗？

录制 SDK 仅保存从 Client 端传来的视频。在生成 mp4 文件时，SDK 会根据 `uid_xxx.txt` 文件中的信息对视频旋转一次。因此无论视频在录制过程中旋转几次，录制 SDK 只会按 `uid_xxx.txt` 文件中的第一个旋转信息进行旋转。你可以根据 `uid_xxx.txt` 文件的旋转信息自己修改转码脚本，然后得到旋转后的视频。目前计划在 2.3 版本中加入使用转码脚本进行视频旋转的功能。

## 录制文件异常

### 录制文件夹下没有生成录制文件

* 确认录制进程是否成功加入到频道。检查录制的 appid 、频道号是否有效，如果开启了 app certificate，是否携带了 `channelKey` 或 Token，以及`channelKey` 或 Token 是否有效。你可以通过通过检查录制日志 `recording_sys.log` 里的 `-appID`，`–channel`，`–channelKey` 参数进行判断。

* 确认频道内至少有一个 Native/Web 用户。频道内需要至少有一个 Native/Web 用户才能进行录制。如果只有录制客户端，则无法录到文件。如果频道内确认有用户，则还需要确认用户是否有发流，如果没有发流录制也是不会生成文件的。

### 录制出来的文件时长小于通话时长

通过水晶球检查客户端和录制端在频道内的时间段是否一致。如果一致，联系技术支持。

### 录制结束之后只有音频文件，没有视频文件。

* 检查客户端和录制端的频道模式是否一致。
* 如果频道模式一致，检查录制的参数 `isAudioOnly` 是否设置为 true，设置为 true 只录制音频，不录制视频。

### 录制出来的视频，打开播放黑屏，但是声音正常

可能是由于使用了不支持的播放器，请参考[播放器列表](../../cn/API%20Reference/recording_java.md)。

### 录制出来的视频倒置

请升级至官网最新版本，如还有问题，联系技术支持。

### 录制文件出现切片

* 检查 Native/Web 和录制的频道模式是否一致。
* 频道内同时有 Native/Web 端时，确保 Native 端开启互通 `enableWebSdkInteroperability` 。

### 录制出来的文件，音画不同步怎么办

请升级至官网最新版本，如还有问题，联系技术支持。

## 录制状态异常

### 录制退出报错

如出现 Error: 3, with stat_code:16 报错时，录制属于正常退出。通过 leave_path code 判断录制退出的原因。

![](https://web-cdn.agora.io/docs-files/1540452150871)

* LEAVE_CODE_INIT(0)：初始化失败
* LEAVE_CODE_SIG(1<<1)：由信号触发的退出
* LEAVE_CODE_NO_USERS(1<<2)：频道内除录制外，没有其他用户
* LEAVE_CODE_TIMER_CATCH(1<<3)：捕获到信号错误
* LEAVE_CODE_CLIENT_LEAVE(1<<4)：wrapper 层主动退出

> 日志里的 code 为上面括号内 2 进制移位后的 10 进制数。比如，(1<<1)=2; (1<<2)=4。所以上面的 leave channel with code:12是由 (1<<2)=4 + (1<<3) =8 得来的。

一般都是频道内没有用户，录制正常退出了。检查 `recording_sys.log`，是否有 "No users in channel" 的关键字即可确认。

### 如何判断录制是否崩溃

录制崩溃可能导致以下情况：

* 视频文件无法播放。
* `uid_xxx.txt` 文件最后没有 mp4 文件的 close 信息。


### 录制崩溃之后怎么办

请升级至官网最新版本，如果无法解决：

2.2.3 及之后的版本请检查在 AgoraCoreService 同一目录下有没有生成 `crash.log`。

2.2.3 之前的版本请检查在 AgoraCoreService 同一目录下有没有生成 core 文件。
 
1. 如果有生成 core 文件，那么按照下面流程：
   a. 把 bin/AgoraCoreService 放在和 core 文件一起，然后命令行执行 gdb -c core_xxxx   AgoraCoreService。
   b. 将 core 文件、a 中的结果、`recording_sys.log` 提供给技术支持。
2. 如果没有找到 core 文件：
   a. 如果没有专门设置 core 文件的目录，那么 core 文件一般是在录制的 AgoraCoreService 文件所在的目录。
   b. 如果还是没有找到，Linux 上执行 ulimit -c， 输出如果为0，则说明 coredump 没有打开，需要通过执行 ulimit -c unlimited 打开。
   c. 打开后，之后再出现 crash 就可以生成 core 文件了。
   d. 针对这次没有 core 文件生成的场景，提供 `recording_sys.log` 给技术支持。
