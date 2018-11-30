
---
title: 录制快速开始
description: 
platform: Java
updatedAt: Fri Nov 30 2018 07:02:11 GMT+0000 (UTC)
---
# 录制快速开始
本页介绍如何使用 Agora Recording SDK for Linux 录制语音或视频通话。

Agora Recording SDK for Linux（简称录制 SDK）支持：

-   录制通信或直播模式

-   录制一个频道内所有参与者的语音和视频内容

-   同时录制多个频道内所有参与者的语音和视频内容

-   同时录制一个或多个频道内所有参与者的纯语音内容

-   支持加密频道 \(使用了 Agora SDK 内置的加密方案\) 的录制


你需要将 Agora Recording SDK for Linux 集成在你的 Linux 服务器上而不是你的 App 上。

<img alt="../_images/recording_linux_cn.png" src="https://web-cdn.agora.io/docs-files/cn/recording_linux_cn.png" style="width: 640.0px;"/>


录制某频道内的音视频信息相当于将一个特殊的观众加入该频道。该观众获取频道内的音视频信息，将获取到的信息转码并储存在 Linux 服务器上。 因此，你必须：

-   将录制 SDK 集成在你的 Linux 服务器上；

-   在录制 SDK 中和进行音视频通话的其他声网 SDK 中使用同一个 APP ID 。关于 APP ID 的具体信息，详见 [获取 App ID](../../cn/Agora%20Platform/token.md)；

-   指定希望录制的频道。

> 使用录制 SDK 必须与 Agora Native/Web SDK 设置相同的频道模式，否则可能导致问题。
## 安装要求

参看 [设置开发环境](../../cn/Quickstart%20Guide/recording_env.md) 以完成环境配置。

## 快速开始

### 加载 Java 库

加载标准库：

```
import io.agora.recording.common.*;
import io.agora.recording.common.Common.*;
import java.lang.InterruptedException;
import java.io.FileWriter;
import java.io.IOException;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Vector;
import java.util.HashMap;
import java.util.Map;
import java.nio.ByteBuffer;
import java.nio.channels.WritableByteChannel;
import java.nio.channels.Channels;
```

### 定义 AgoraJavaRecording 类

```
class AgoraJavaRecording{
...
}
```

### 定义 main 方法

```
public static void main(String[] args){
...
}
```

### 定义变量

#### 定义用于 Agora Engine 的变量

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>变量</th>
<th>用途</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>uid</span></code></td>
<td>用户 ID</td>
</tr>
<tr><td><code><span>appId</span></code></td>
<td>App ID</td>
</tr>
<tr><td><code><span>channelKey</span></code></td>
<td>频道密钥</td>
</tr>
<tr><td><code><span>name</span></code></td>
<td>频道名</td>
</tr>
<tr><td><code><span>channelProfile</span></code></td>
<td>频道设置</td>
</tr>
</tbody>
</table>



```
int uid = 0;
String appId = "";
String channelKey = "";
String name = "";
int channelProfile = 0;
```

#### 定义用于录制设置的变量

定义加密模式和密钥。

```
String decryptionMode = "";
String secret = "";
```

定义视频合图的分辨率和空闲时间（单位：秒）。

```
String mixResolution = "360,640,15,500";
int idleLimitSec=5*60;//300s
```

定义路径。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>变量</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>applitePath</span></code></td>
<td>AgoraCoreService 所在目录</td>
</tr>
<tr><td><code><span>recordFileRootDir</span></code></td>
<td>录制文件所在目录</td>
</tr>
<tr><td><code><span>cfgFilePath</span></code></td>
<td>配置文件所在目录</td>
</tr>
</tbody>
</table>



```
String applitePath = "";
String recordFileRootDir = "";
String cfgFilePath = "";
```

定义最低和最高的 UDP 端口.

```
int lowUdpPort = 0;//40000;
int highUdpPort = 0;//40004;
```

定义音视频合图变量。

```
boolean isAudioOnly=false;
boolean isVideoOnly=false;
boolean isMixingEnabled=false;
boolean mixedVideoAudio=false;
```

定义音视频流格式。

```
int getAudioFrame = AUDIO_FORMAT_TYPE.AUDIO_FORMAT_DEFAULT_TYPE.ordinal();
int getVideoFrame = VIDEO_FORMAT_TYPE.VIDEO_FORMAT_DEFAULT_TYPE.ordinal();
int streamType = REMOTE_VIDEO_STREAM_TYPE.REMOTE_VIDEO_STREAM_HIGH.ordinal();
```

定义截图时间间隔和截图模式。

```
int captureInterval = 5;
int triggerMode = 0;
```

定义视频参数。

```
int width = 0;
int height = 0;
int fps = 0;
int kbps = 0;
int count = 0;
```

### 检查并读取命令行参数

```
if(args.length % 2 !=0){
  System.out.println("command line parameters error, should be '--key value' format!");
  return;
}
String key = "";
String value = "";
Map<String,String> map = new HashMap<String,String>();
if(0 < args.length ){
  for(int i = 0; i<args.length-1; i+=2){
    key = args[i];
    value = args[i+1];
    map.put(key, value);
  }
}
Object Appid = map.get("--appId");
Object Uid = map.get("--uid");
Object Channel = map.get("--channel");
Object AppliteDir = map.get("--appliteDir");
Object ChannelKey = map.get("--channelKey");
Object ChannelProfile = map.get("--channelProfile");
Object IsAudioOnly = map.get("--isAudioOnly");
Object IsVideoOnly = map.get("--isVideoOnly");
Object IsMixingEnabled = map.get("--isMixingEnabled");
Object MixResolution = map.get("--mixResolution");
Object MixedVideoAudio = map.get("--mixedVideoAudio");
Object DecryptionMode = map.get("--decryptionMode");
Object Secret = map.get("--secret");
Object Idle = map.get("--idle");
Object RecordFileRootDir = map.get("--recordFileRootDir");
Object LowUdpPort = map.get("--lowUdpPort");
Object HighUdpPort = map.get("--highUdpPort");
Object GetAudioFrame = map.get("--getAudioFrame");
Object GetVideoFrame = map.get("--getVideoFrame");
Object CaptureInterval = map.get("--captureInterval");
Object CfgFilePath = map.get("--cfgFilePath");
Object StreamType = map.get("--streamType");
Object TriggerMode = map.get("--triggerMode");
Object ProxyServer = map.get("--proxyServer");
```

### 传入命令行参数

```
appId = String.valueOf(Appid);
uid = Integer.parseInt(String.valueOf(Uid));
appId = String.valueOf(Appid);
name = String.valueOf(Channel);
applitePath = String.valueOf(AppliteDir);

if(ChannelKey != null) channelKey = String.valueOf(ChannelKey);
if(ChannelProfile != null) channelProfile = Integer.parseInt(String.valueOf(ChannelProfile));
if(DecryptionMode != null) decryptionMode = String.valueOf(DecryptionMode);
if(Secret != null) secret = String.valueOf(Secret);
if(MixResolution != null) mixResolution = String.valueOf(MixResolution);
if(Idle != null) idleLimitSec = Integer.parseInt(String.valueOf(Idle));
if(RecordFileRootDir != null) recordFileRootDir = String.valueOf(RecordFileRootDir);
if(CfgFilePath != null) cfgFilePath = String.valueOf(CfgFilePath);
if(LowUdpPort != null) lowUdpPort = Integer.parseInt(String.valueOf(LowUdpPort));
if(HighUdpPort != null) highUdpPort = Integer.parseInt(String.valueOf(HighUdpPort));
if(IsAudioOnly != null &&(Integer.parseInt(String.valueOf(IsAudioOnly)) == 1)) isAudioOnly = true;
if(IsVideoOnly != null &&(Integer.parseInt(String.valueOf(IsVideoOnly)) == 1)) isVideoOnly = true;
if(IsMixingEnabled != null &&(Integer.parseInt(String.valueOf(IsMixingEnabled))==1)) isMixingEnabled = true;
if(MixedVideoAudio != null &&(Integer.parseInt(String.valueOf(MixedVideoAudio)) == 1)) mixedVideoAudio = true;
if(GetAudioFrame != null) getAudioFrame = Integer.parseInt(String.valueOf(GetAudioFrame));
if(GetVideoFrame != null) getVideoFrame = Integer.parseInt(String.valueOf(GetVideoFrame));
if(StreamType != null) streamType = Integer.parseInt(String.valueOf(StreamType));
if(CaptureInterval != null) captureInterval = Integer.parseInt(String.valueOf(CaptureInterval));
if(TriggerMode != null) triggerMode = Integer.parseInt(String.valueOf(TriggerMode));
if(ProxyServer != null) proxyServer = String.valueOf(ProxyServer);
```

### 实例化录制类和参数类

```
AgoraJavaRecording ars = new AgoraJavaRecording();
RecordingConfig config= new RecordingConfig();
config.channelProfile = CHANNEL_PROFILE_TYPE.values()[channelProfile];
config.idleLimitSec = idleLimitSec;
config.isVideoOnly = isVideoOnly;
config.isAudioOnly = isAudioOnly;
config.isMixingEnabled = isMixingEnabled;
config.mixResolution = mixResolution;
config.mixedVideoAudio = mixedVideoAudio;
config.appliteDir = applitePath;
config.recordFileRootDir = recordFileRootDir;
config.cfgFilePath = cfgFilePath;
config.secret = secret;
config.decryptionMode = decryptionMode;
config.lowUdpPort = lowUdpPort;
config.highUdpPort = highUdpPort;
config.captureInterval = captureInterval;
config.decodeAudio = AUDIO_FORMAT_TYPE.values()[getAudioFrame];
config.decodeVideo = VIDEO_FORMAT_TYPE.values()[getVideoFrame];
config.streamType = REMOTE_VIDEO_STREAM_TYPE.values()[streamType];
config.triggerMode = triggerMode;
config.proxyServer = proxyServer;
```

### 传入合图参数

```
ars.isMixMode = isMixingEnabled;
ars.profile_type = CHANNEL_PROFILE_TYPE.values()[channelProfile];
if(isMixingEnabled && !isAudioOnly) {
  String[] sourceStrArray=mixResolution.split(",");
  if(sourceStrArray.length != 4) {
       System.out.println("Illegal resolution:"+mixResolution);
        return;
    }
    ars.width = Integer.valueOf(sourceStrArray[0]).intValue();
    ars.height = Integer.valueOf(sourceStrArray[1]).intValue();
    ars.fps = Integer.valueOf(sourceStrArray[2]).intValue();
    ars.kbps = Integer.valueOf(sourceStrArray[3]).intValue();
}
```

### 创建并加入频道

```
ars.createChannel(appId, channelKey,name,uid,config);
```

### 开始录制

你现在可以开始录制了。

## 参考文档

关于录制的更多功能，参考 [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md) 。

关于录制 SDK 的 API 的详细信息，参考 [录制 API Beta](../../cn/API%20Reference/recording_java.md) 。

录制的示例代码已包含在录制 SDK 包中。Java 的示例代码在位于 ./samples/java 中。


