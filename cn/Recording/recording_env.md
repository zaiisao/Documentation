
---
title: 设置开发环境
description: 
platform: Linux
updatedAt: Wed Sep 26 2018 04:11:00 GMT+0800 (CST)
---
# 设置开发环境
# 设置开发环境

本页介绍如何设置环境以集成 Agora Recording SDK for Linux 。

## 硬件和网络要求

下表列出了安装录制 SDK 的基本要求：

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
<li>Ubuntu Linux 14.04+ LTS x64</li>
<li>CentOS 7+ x64</li>
</ul>
</td>
</tr>
<tr><td>网络</td>
<td>这台 Linux 服务器要接入公网，有公网 IP</td>
</tr>
<tr><td>带宽</td>
<td>根据需要同时录制的频道数量和频道内情况确定所需带宽。以下数据可供参考：录制一个分辨率为 640*480 的画面需要的带宽约为 500kbps；录制一个有两个人的频道则需 1Mbps；同时录制 100 个这样的频道，需要带宽为 100Mbps。关于分辨率和带宽的关系，详见 <a href="../../cn/API%20Reference/recording_cpp.md"><span>录制 API</span></a>。</td>
</tr>
<tr><td>域名解析</td>
<td>服务器允许访问 qos.agoralab.co，否则 SDK 无法上报必要的统计数据。</td>
</tr>
</tbody>
</table>


参考硬件配置：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>产品</strong></td>
<td><strong>描述</strong></td>
<td><strong>数量</strong></td>
</tr>
<tr><td>SUPERMICRO SYS-6017R-TDF</td>
<td>1U rack-mounted SYS-6017R-TDF, 双路 Intel® Xeon® E5-2600 系列处理器</td>
<td>1</td>
</tr>
<tr><td>机箱</td>
<td>1U Rackmountable(440W high-efficiency redundant power supply w/ PMBus)</td>
<td>1</td>
</tr>
<tr><td>处理器</td>
<td>Intel Xeon E5-2620V2 2.1G, L3:15M, 6C((P4X-DPE52620V2-SR1AN)</td>
<td>2</td>
</tr>
<tr><td>内存</td>
<td>MEM-DR380L-HL06-ER16(8GB DDR3-1600 2Rx8 1.35v ECC REG RoHS)</td>
<td>1</td>
</tr>
<tr><td>硬盘</td>
<td>250G 3.5 SATA Enterprise (HDD-T0250-WD2503ABYZ)</td>
<td>2</td>
</tr>
</tbody>
</table>



假设每个频道内有两个人进行视频通话（通信模式），分辨率是 640\*480 ，帧率为 15fps ，单流码率为 500kbps ：

实测在参考硬件配置下， 12 核 24 线程的 CPU 满载并发 100 个频道，此时：

-   每个频道占用约 25M 内存；总共占用约 2.5G 内存。内存占用率约为 31%；

-   每个频道写入磁盘的速度约为 60kB/s ；总写入速度约为 6.0MB/s ，远低于磁盘的最大写入速度；

-   每个频道下行网络流量约为 500kbps \* 2 = 1Mbps ，总下行流量约为 100 Mbps ，上行流量可以忽略不计。


## SDK 兼容性

录制 SDK 支持：

-   纯Native 端录制；

-   纯 Web 端录制

-   Web 与 Native 互通时录制。


录制 SDK 与以下 SDK 兼容:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>SDK</strong></td>
<td><strong>版本</strong></td>
</tr>
<tr><td>Agora Native SDK</td>
<td>录制 SDK 与全平台 Agora Native SDK 1.7.0+ 兼容，如果频道内有任何人使用了 1.6 版本的 Agora Native SDK， 则整个频道无法录制</td>
</tr>
<tr><td>Agora Web SDK</td>
<td>录制 SDK 与 Agora Web SDK 1.12.0+ 兼容</td>
</tr>
</tbody>
</table>



## 准备环境

1.  [下载](https://docs.agora.io/cn/2.2/download)

    <img alt="../_images/linux_structure.png" src="https://web-cdn.agora.io/docs-files/cn/linux_structure.png" style="width: 412.0px; height: 712.0px;"/>



其中:

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



2.  打开 TCP 端口：1080、8000。

3.  打开 UDP 端口：双向 1080、4000-4030、8000、9700、25000 和 所有的录制进程所使用的单向下行端口。



> -   录制一个频道的内容需要开启一个对应的录制进程；单个录制进程需要使用 4 个单向下行端口。进程（包括各个录制进程和系统进程）之间不得有端口冲突。

> - Agora 建议您指定录制进程使用端口的范围。您可以为多个录制进程统一配置较大的端口范围（Agora 建议 40000 ~ 41000 或更大）。此时，录制 SDK 会在指定范围内为每个录制进程分配端口，并避免端口的冲突。要设置端口范围，您需要配置参数 `lowUdpPort` 和`highUdpPort`。

> -  如果不指定参数 `lowUdpPort` 和 `highUdpPort` ，录制进程所使用的端口为随机端口，会有端口冲突的风险。
 
> -   查看 UDP 端口时可以使用`iptables -L`命令。


4.  将域名 .agora.io、vocs.agora.io、qoslbs.agora.io、qos.agora.io 设为白名单。

5.  安装编译器: gcc 4.4+ 。

6.  为调试方便，Agora 建议您打开系统的 core dump 功能以记录可能产生的程序崩溃信息。可以参考以下脚本 `create_core.sh`。

7.  如果你希望用 java 编写录制相关的程序，你必须先配置好 jdk 环境，并确保包含 jni.h 。



