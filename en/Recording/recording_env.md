
---
title: Set Up Your Environment
description: 
platform: Linux
updatedAt: Thu Dec 20 2018 06:18:12 GMT+0000 (UTC)
---
# Set Up Your Environment
In this topic, you will learn how to set up environment to integrate Agora Recording SDK.

## Hardware Requirements

The following table lists the hardware requirements:

<table>
<thead>
<tr><th>Hardware</th>
<th>Requirements</th>
</tr>
</thead>
<tbody>
<tr><td>Server</td>
<td>Physical or virtual.</td>
</tr>
<tr><td>System</td>
<td>Ubuntu Linux 14.04+ LTS 64-bit or CentOS 7+ x64</td>
</tr>
<tr><td>Network</td>
<td>The Linux server needs Internet access</td>
</tr>
<tr><td>Internet Bandwidth</td>
<td><p>Decide the Internet bandwidth based on the number of channels being recorded simultaneously. Refer to the following data:</p>
<div><ul>
<li>When the resolution of the recorded scene is 640*360, the bandwidth is 500kbps;</li>
<li>To record a channel with two users, you need a bandwidth of 1 Mbps;</li>
<li>For 100 channels, you need a bandwidth of 100Mbps.</li>
</ul>
</div>
<p>For detailed bandwidth data, refer to <a href="../../en/API%20Reference/recording_cpp.md"><span>Recording API</span></a> .</p>
</td>
</tr>
</tbody>
</table>



Agora recommends the following hardware configurattions:

<table>
<thead>
<tr><th>Product</th>
<th>Description</th>
<th>Number</th>
</tr>
</thead>
<tbody>
<tr><td>SUPERMICRO SYS-6017R-TDF</td>
<td>1U rack-mounted SYS-6017R-TDF
Intel® Xeon® E5-2600 Series Processor</td>
<td>1</td>
</tr>
<tr><td>Case</td>
<td>1U Rackmountable
(440-W high-efficiency redundant power supply w/ PMBus)</td>
<td>1</td>
</tr>
<tr><td>Processor</td>
<td>Intel Xeon E5-2620V2 2.1 G, L3:15M, 6C (P4X-DPE52620V2-SR1AN)</td>
<td>2</td>
</tr>
<tr><td>Memory</td>
<td>MEM-DR380L-HL06-ER16 (8-GB DDR3-1600 2Rx8 1.35-V ECC REG RoHS)</td>
<td>1</td>
</tr>
<tr><td>Hard Disk</td>
<td>250-G 3.5 SATA Enterprise (HDD-T0250-WD2503ABYZ)</td>
<td>2</td>
</tr>
</tbody>
</table>



Assuming two users are in a channel in a video call \(communication mode\), with the resolution of 640 * 360, frame rate of 15 fps and bitrate of one video stream of 500 Kbps, using single-stream recording:

The CPU is fully loaded and 100 channels are recorded simultaneously:

-   Each channel writes to the disk at a speed of 60 kB/s. The total write-in speed is 6.0 MB/s, which is much lower than the maximum write-in speed of the disk;

-   Each channel uses 25 MB of memory. Thus, 2.5 GB of memory, which is 31% of the total memory, is taken;

-   The downstream Internet flow for each channel is 500 kbps \* 2 = 1 Mbps. The total downstream flow is 100 Mbps. The upstream flow is neglected.


## Compatibility with the Agora SDKs

The recording SDK supports:

-   recording the communication that uses the native SDK;

-   recording the communication that uses the web SDK;

-   recording the communication that uses both the native SDK and the web SDK;


The recording SDK is compatible with the following Agora SDKs:

-   Agora Native SDK v1.7.0+ for all platforms;

-   Agora Web SDK v1.12.0+ .

> If any user in the channel uses Agora SDK which is not compatible with Agora recording SDK, recording fails for the whole channel.

## Setting up the environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Agora Recording SDK for Linux package. The package structure is listed as follows:

    <img alt="../_images/linux_structure.png" src="https://web-cdn.agora.io/docs-files/en/linux_structure.png" style="width: 500.0px;"/>

<table>
<thead>
<tr><th>Folder</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>bin</td>
<td>The directory where AgoraCoreService is located</td>
</tr>
<tr><td>include</td>
<td><ul>
<li>base: Required header files for developing the recording application</li>
<li>IAgoraLinuxSdkCommon.h: Public structure and enumeration</li>
<li>IAgoraRecordingEngine.h: Interface of the recording engine and its config information</li>
</ul>
</td>
</tr>
<tr><td>libs</td>
<td>Required libraries for developing the recording application</td>
</tr>
<tr><td>samples</td>
<td><p>Sample code</p>
<ul>
<li>agorasdk: Demo that implements the C++ interface and callbacks</li>
<li>base: Public sample code</li>
<li>cpp: C++ sample code<ul>
<li>release/bin/recorder: Parent process that can be run</li>
</ul>
</li>
<li>java: Java sample code<ul>
<li>native: Native code</li>
<li>native/jni: JNI delegate</li>
<li>src: java code</li>
<li>src/io/agora/recording/RecordingEventHandler.java: Callback interface class</li>
<li>src/io/agora/recording/RecordingSDK.java: Recording interface class</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td>Tools</td>
<td>Transcoding tools</td>
</tr>
</tbody>
</table>



2.  Open the TCP ports 1080 and 8000.

3.  Open the UDP ports:

    -   Duplex ports: 1080, 4000-4030, 8000, 9700 and 25000;

    -   Simplex downstream ports used by the recording processes.


> -   Use the command line `iptables -L` to check the UDP port.
> -   To record the content in channels, you need one recording process for each of the channels. One recording process requires four simplex downstream ports. There must be no port conflict among the processes, including the system processes and all the recording processes.
>  -   Agora recommends that you specify the range of ports used by the recording processes. Configure a large range for all recording processes \(Agora recommends 40000 ~ 41000 or larger\). If so, the Recording SDK assigns ports to each recording process within the specified range and avoids port conflicts automatically. To set the port range, you need to configure the parameters `lowUdpPort` and `highUdpPort`;
>  -   If the parameters, `lowUdpPort` and `highUdpPort`, are not specified, the ports used by the recording processes are at random, which may cause port conflicts.


4.  Set whitelist domains:

    -   agora.io

    -   vocs.agora.io

    -   qoslbs.agora.io

    -   qos.agora.io

5.  Ensure that your compiler is gcc 4.4+.

6.  For the purpose of debug, Agora recommends you to enable core dump on your Linux system. Possible crash information will be recorded. 
7.  If you wish to use java delegate to enable recording, you must install JDK.



