
---
title: 集成录制 SDK
description: How to integrate recording SDK
platform: Linux Java
updatedAt: Tue Aug 11 2020 06:19:04 GMT+0800 (CST)
---
# 集成录制 SDK
本页介绍如何设置环境以及集成 Agora 本地服务端录制 SDK。

你需要将 Agora 本地服务端录制 SDK 集成在你的 Linux 服务器上而不是你的 App 上。

>如果你不想自行部署 Linux 服务器，可尝试 [Agora 云端录制](../../cn/cloud-recording/product_cloud_recording.md)。

<img alt="../_images/recording_linux_cn.png" src="https://web-cdn.agora.io/docs-files/cn/recording_linux_cn.png" style="width: 640.0px;"/>

录制某频道内的音视频信息相当于将一个特殊的观众加入该频道。该观众获取频道内的音视频信息，将获取到的信息转码并储存在 Linux 服务器上。 因此，你必须：

- 将本地服务端录制 SDK 集成在你的 Linux 服务器上；
- 在本地服务端录制 SDK 中和进行音视频通话的 Agora SDK 使用同一个 App ID 。

## 前提条件

下表列出了安装 Agora 本地服务端录制 SDK 的基本要求：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td width="100"><strong>硬件和网络</strong></td>
<td><strong>要求</strong></td>
</tr>
<tr><td>服务器</td>
<td><p>物理或虚拟</p>
<ul>
<li>Ubuntu 14.04+ x64</li>
<li>CentOS 6.5+（推荐 7.0）x64</li>
</ul>
</td>
</tr>
<tr><td>网络</td>
<td>这台 Linux 服务器要接入公网，有公网 IP</td>
</tr>
<tr><td>带宽</td>
<td>根据需要同时录制的频道数量和频道内情况确定所需带宽。以下数据可供参考：录制一个分辨率为 640*480 的画面需要的带宽约为 500kbps；录制一个有两个人的频道则需 1Mbps；同时录制 100 个这样的频道，需要带宽为 100Mbps。关于分辨率和带宽的关系，详见 <a href="https://docs.agora.io/cn/Recording/API%20Reference/recording_java/index.html"><span>录制 API</span></a>。</td>
</tr>
<tr><td>域名解析</td>
<td>服务器允许访问 qos.agoralab.co，否则 SDK 无法上报必要的统计数据。</td>
</tr>
</tbody>
</table>

### 参考配置

我们测试了以下云主机配置下的录制并发性能：

- AWS：Intel(R) Xeon(R) Platinum 8124M CPU @ 3.00 GHz
- 16 虚拟核 CPU，32 GB 内存
- 磁盘 I/O: 412 MB/s

测试条件：

- 每个频道内有两个人，视频分辨率设为 320 × 240 ，帧率设为 15 fps 。
- 对于合流录制文件，视频分辨率设为 640 × 480，视频帧率设为 15 fps，视频码率设为 500 Kbps ；音频码率设为 48 Kbps。

不同频道模式和录制模式下，录制并发性能如下：

<table>
  <tr>
    <th>频道模式</th>
    <th>录制模式</th>
    <th>测试结果</th>
  </tr>
  <tr>
    <td rowspan="3">直播模式</td>
    <td>视频单流录制</td>
    <td>215 个频道并发时，CPU 占用为 75% 左右<br>建议并发 200 个频道</td>
  </tr>
  <tr>
    <td>视频合流录制</td>
    <td>70 个频道并发时，CPU 占用 75% 左右<br>建议并发 60 个频道</td>
  </tr>
  <tr>
    <td>纯音频合流录制</td>
    <td>300 个频道并发时，CPU 占用为 75% 左右</td>
  </tr>
  <tr>
    <td rowspan="2">通信模式</td>
    <td>视频单流录制</td>
    <td>210 个频道并发时，CPU 占用为 75% 左右<br>建议并发 200 个频道</td>
  </tr>
  <tr>
    <td>视频合流录制</td>
    <td>60 个频道并发时，CPU 占用为 75% 左右</td>
  </tr>
</table>

你可参考上述云主机配置和对应的录制性能，根据自己的录制需要选择和配置云主机，详见[使用云容器部署录制 SDK](../../cn/Recording/recording_docker.md)。

## 准备环境

在你的 Linux 服务器上进行以下操作：

1. [下载](https://docs.agora.io/cn/Agora%20Platform/downloads)最新的 Agora 本地服务端录制 SDK 软件包。软件包内容如下:

   <table>
   <colgroup>
   <col/>
   <col/>
   </colgroup>
   <tbody>
   <tr><td><strong>文件夹</strong></td>
   <td><strong>描述</strong></td>
   </tr>
   <tr><td>bin</td>
   <td>AgoraCoreService 所在的目录</td>
   </tr>
   <tr><td>include</td>
   <td><ul>
   <li>base：libs 所依赖的一些基础的头文件</li>
   <li>IAgoraLinuxSdkCommon.h：公共的基础结构体和枚举值</li>
   <li>IAgoraRecordingEngine.h：录制引擎的接口类和配置信息</li>
   </ul>
   </td>
   </tr>
   <tr><td>libs</td>
   <td>录制的依赖库</td>
   </tr>
   <tr><td>samples</td>
   <td><p>代码示例</p>
   <ul>
   <li>agorasdk：对录制 C++ 接口的实现以及回调的处理示例</li>
   <li>base：公共的示例代码</li>
   <li>cpp：C++ 示例代码<ul>
   <li>release/bin/recorder：可运行的父程序</li>
   </ul>
   </li>
   <li>java：java 示例代码<ul>
   <li>native：native code</li>
   <li>native/jni：jni 代理</li>
   <li>src: java 层源代码</li>
   <li>src/io/agora/recording/RecordingEventHandler.java: 回调接口类</li>
   <li>src/io/agora/recording/RecordingSDK.java: 录制接口类</li>
   </ul>
   </li>
   </ul>
   </td>
   </tr>
   <tr><td>tools</td>
   <td>转码工具</td>
   </tr>
   </tbody>
   </table>

2. 安装编译器: gcc 4.4+ 。
3. 配置好 jdk 环境，并确保包含 jni.h。
4. 配置好 Java 的 `CLASSPATH` 和 Linux 的 `LD_LIBRARY_PATH` 环境变量。
5. 如果你的网络环境设置了防火墙限制外网访问，将域名 .agora.io 和 .agoralab.co 及下列目标端口添加到防火墙白名单：
    | 目标端口 | 协议 | 操作 |
| --------------- | -------------- | ----------- |
| 80；1080；5888；8000；9700；25000；30000 | TCP | 允许 |
| 1080；4000-4030；7000；8000；8913；9700；25000 | UDP | 允许 |
6. 打开所有的录制进程所使用的 UDP 端口，端口为在 `RecordingConfig` 中指定的 `lowUdpPort` 和 `highUdpPort` 范围之间的端口。

   > - 录制一个频道的内容需要开启一个对应的录制进程；单个录制进程需要使用 4 个 UDP 端口。进程（包括各个录制进程和系统进程）之间不得有端口冲突。
   > - Agora 建议指定录制进程使用端口的范围。你可以为多个录制进程统一配置较大的端口范围（Agora 建议 40000 ~ 41000 或更大）。此时，本地服务端录制 SDK 会在指定范围内为每个录制进程分配端口，并避免端口的冲突。要设置端口范围，你需要配置参数 `lowUdpPort` 和`highUdpPort`。
   > - 如果不指定参数 `lowUdpPort` 和 `highUdpPort` ，录制进程所使用的端口为随机端口，会有端口冲突的风险。
   > - 使用`iptables -L`命令查看 UDP 端口。


7. 为调试方便，Agora 建议你打开系统的 core dump 功能以记录可能产生的程序崩溃信息。


## 编译代码示例
1. 打开命令行工具，在 **samples/java** 路径下执行如下命令进行环境预设置。其中 `jni_path` 请填入 `jni.h` 文件绝对路径，例如 `/usr/java8u161/jdk1.8.0_161/include/`：

   ```
   source build.sh pre_set jni_path
   ```

2. 在 **samples/java** 下执行编译脚本：

   ```
   ./build.sh build
   ```

编译完成，在本目录下生成一个 **bin** 文件夹，其中的子目录 **io/agora/recording** 下会包含一个 `librecording.so` 文件，如图所示。

![](https://web-cdn.agora.io/docs-files/1544522310646)

完成编译后你就可以使用 demo 在命令行中进行录制了，详见[命令行录制](../../cn/Recording/recording_cmd_java.md)。如果你需要在自己的项目中调用 API 进行录制，还需要将 SDK 相关文件[导入项目](#import)。

## <a name="import"></a>导入项目

将以下文件复制到你的项目：

- 编译生成的  `librecording.so` 文件
- SDK 包中的 `AgoraCoreService` 文件（位于 `bin` 文件夹）
- SDK 包中的 Java 文件（位于 `samples/java/src/io/agora/recording` 目录下）
	- `RecordingEventHandler.java`
	- `RecordingSDK.java`
	- `/common/Common.java`
	- `/common/RecordingConfig.java`
	- `/common/RecordingEngineProperties.java`

举例来说，假设项目顶层目录为 `ROOT_DIR`，将  `librecording.so` 文件复制到 `ROOT_DIR/lib` 目录下，将 `AgoraCoreService` 文件复制到 `ROOT_DIR/bin` 目录下，将上述 Java 文件复制到 `ROOT_DIR/src/io/agora/recording` 目录下。

运行 Java 程序时，设置 `-Djava.library.path=ROOT_DIR/lib` 参数来指定动态库的目录地址。此外你需要在开始录制时设置 `RecordingConfig` 中的 `appliteDir` 参数为 `ROOT_DIR/bin`。

你已经集成了本地服务端录制 SDK，可以调用 API 进行录制了。详见 [Agora Recording Java API Reference](https://docs.agora.io/cn/Recording/API%20Reference/recording_java/index.html)。
