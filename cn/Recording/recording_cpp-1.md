
---
title: 录制快速开始
description: 
platform: CPP
updatedAt: Fri Nov 30 2018 06:55:57 GMT+0000 (UTC)
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

### 加载 C++ 库

加载标准库：

```
#include <csignal>
#include <cstdint>
#include <iostream>
#include <sstream>
#include <string>
#include <vector>
#include <algorithm>
```

加载 Agora SDK 库：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>库</th>
<th>用途</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>IAgoraLinuxSdkCommon.h</span></code></td>
<td>包含 Agora 定义的变量和类</td>
</tr>
<tr><td><code><span>IAgoraRecordingEngine.h</span></code></td>
<td>定义了 Agora 录制引擎类</td>
</tr>
<tr><td><code><span>base/atomic.h</span></code></td>
<td>定义了 Agora 命名空间</td>
</tr>
<tr><td><code><span>base/log.h</span></code></td>
<td>定义了 Agora log 类</td>
</tr>
<tr><td><code><span>base/opt_parser.h</span></code></td>
<td>语法解析</td>
</tr>
<tr><td><code><span>agorasdk/AgoraSdk.h</span></code></td>
<td>Agora Recording SDK</td>
</tr>
</tbody>
</table>



```
#include "IAgoraLinuxSdkCommon.h"
#include "IAgoraRecordingEngine.h"

#include "base/atomic.h"
#include "base/log.h"
#include "base/opt_parser.h"
#include "agorasdk/AgoraSdk.h"
```

### 定义命名空间

定义命名空间：

```
using std::string;
using std::cout;
using std::cerr;
using std::endl;

using agora::base::opt_parser;
using agora::linuxsdk::VideoFrame;
using agora::linuxsdk::AudioFrame;
```

### 定义全局变量

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
<tr><td><code><span>g_bSignalStop</span></code></td>
<td>用于标志是否收到停止信号</td>
</tr>
<tr><td><code><span>g_bSignalStartService</span></code></td>
<td>用于标志录制服务是否开始</td>
</tr>
<tr><td><code><span>g_bSignalStopService</span></code></td>
<td>用于标志录制服务是否停止</td>
</tr>
</tbody>
</table>



```
atomic_bool_t g_bSignalStop;
atomic_bool_t g_bSignalStartService;
atomic_bool_t g_bSignalStopService;
```

### 编写开始和结束录制的方法

调用 `start_service` 方法和 `stop_service` 方法会改变 `g_bSignalStartService` 和 `g_bSignalStopService` 的值。

```
void start_service(int signo) {
    (void)signo;
    g_bSignalStartService = true;
}

void stop_service(int signo) {
    (void)signo;
    g_bSignalStopService = true;
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
uint32_t uid = 0;
string appId;
string channelKey;
string name;
uint32_t channelProfile = 0;
```

#### 定义用于录制设置的变量

定义加密模式和密钥。

```
string decryptionMode;
string secret;
```

定义视频合图的分辨率和空闲时间（单位：秒）。

```
string mixResolution("360,640,15,500");
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
string applitePath;
string recordFileRootDir = "";
string cfgFilePath = "";
string proxyServer;
```

定义最低和最高的 UDP 端口.

```
int lowUdpPort = 0;//40000;
int highUdpPort = 0;//40004;
```

定义音视频合图变量。

```
bool isAudioOnly=0;
bool isVideoOnly=0;
bool isMixingEnabled=0;
bool mixedVideoAudio=0;
```

定义音视频流格式。

```
uint32_t getAudioFrame = agora::linuxsdk::AUDIO_FORMAT_DEFAULT_TYPE;
uint32_t getVideoFrame = agora::linuxsdk::VIDEO_FORMAT_DEFAULT_TYPE;
uint32_t streamType = agora::linuxsdk::REMOTE_VIDEO_STREAM_HIGH;
```

定义截图时间间隔和截图模式。

```
int captureInterval = 5;
int triggerMode = agora::linuxsdk::AUTOMATICALLY_MODE;
```

定义视频参数。

```
int width = 0;
int height = 0;
int fps = 0;
int kbps = 0;
```

初始化服务状态标志。

```
g_bSignalStop = false;
g_bSignalStartService = false;
g_bSignalStopService = false;
```

### 定义事件监听器

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>事件</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>SIGQUIT</span></code></td>
<td>中断</td>
</tr>
<tr><td><code><span>SIGABRT</span></code></td>
<td>终止</td>
</tr>
<tr><td><code><span>SIGINT</span></code></td>
<td>打断</td>
</tr>
<tr><td><code><span>SIGPIPE</span></code></td>
<td>通道信号中断</td>
</tr>
</tbody>
</table>



```
signal(SIGQUIT, signal_handler);
signal(SIGABRT, signal_handler);
signal(SIGINT, signal_handler);
signal(SIGPIPE, SIG_IGN);
```

### 设置 Parser 对象

解析以下的 `rtcEngine` 参数：

-   app ID

-   user ID

-   channel name

-   application path

-   channel key

-   channel profile


```
opt_parser parser;

parser.add_long_opt("appId", &appId, "App Id/must", agora::base::opt_parser::require_argu);
parser.add_long_opt("uid", &uid, "User Id default is 0/must", agora::base::opt_parser::require_argu);

parser.add_long_opt("channel", &name, "Channel Id/must", agora::base::opt_parser::require_argu);
parser.add_long_opt("appliteDir", &applitePath, "directory of app lite 'AgoraCoreService', Must pointer to 'Agora_Recording_SDK_for_Linux_FULL/bin/' folder/must",
        agora::base::opt_parser::require_argu);

parser.add_long_opt("channelKey", &channelKey, "channelKey/option");
parser.add_long_opt("channelProfile", &channelProfile, "channel_profile:(0:COMMUNICATION),(1:broadcast) default is 0/option");
```

解析视频，音频和合图参数。

```
parser.add_long_opt("isAudioOnly", &isAudioOnly, "Default 0:A/V, 1:AudioOnly (0:1)/option");
parser.add_long_opt("isVideoOnly", &isVideoOnly, "Default 0:A/V, 1:VideoOnly (0:1)/option");
parser.add_long_opt("isMixingEnabled", &isMixingEnabled, "Mixing Enable? (0:1)/option");
parser.add_long_opt("mixResolution", &mixResolution, "change default resolution for vdieo mix mode/option");
parser.add_long_opt("mixedVideoAudio", &mixedVideoAudio, "mixVideoAudio:(0:seperated Audio,Video) (1:mixed Audio & Video), default is 0 /option");
```

解析加密模式和密钥。

```
parser.add_long_opt("decryptionMode", &decryptionMode, "decryption Mode, default is NULL/option");
parser.add_long_opt("secret", &secret, "input secret when enable decryptionMode/option");
```

解析空闲时间设置和录制文件目录。

```
parser.add_long_opt("idle", &idleLimitSec, "Default 300s, should be above 3s/option");
parser.add_long_opt("recordFileRootDir", &recordFileRootDir, "recording file root dir/option");
```

解析最高和最低 UDP 端口。

```
parser.add_long_opt("lowUdpPort", &lowUdpPort, "default is random value/option");
parser.add_long_opt("highUdpPort", &highUdpPort, "default is random value/option");
```

解析音视频帧参数。

```
parser.add_long_opt("getAudioFrame", &getAudioFrame, "default 0 (0:save as file, 1:aac frame, 2:pcm frame, 3:mixed pcm frame) (Can't combine with isMixingEnabled) /option");
parser.add_long_opt("getVideoFrame", &getVideoFrame, "default 0 (0:save as file, 1:h.264, 2:yuv, 3:jpg buffer, 4:jpg file, 5:jpg file and video file) (Can't combine with isMixingEnabled) /option");
```

解析截图时间间隔，配置文件路径。

```
parser.add_long_opt("captureInterval", &captureInterval, "default 5 (Video snapshot interval (second))");
parser.add_long_opt("cfgFilePath", &cfgFilePath, "config file path / option");
parser.add_long_opt("proxyServer", &proxyServer, "proxyServer:format ip:port, eg,\"127.0.0.1:1080\"/option");
```

解析数据流类型和触发模式。

```
parser.add_long_opt("streamType", &streamType, "remote video stream type(0:STREAM_HIGH,1:STREAM_LOW), default is 0/option");
parser.add_long_opt("triggerMode", &triggerMode, "triggerMode:(0: automatically mode, 1: manually mode) default is 0/option");
```

### 检查设置

检查` parser` 设置， `appID`， 和 `name` 参数。若任一参数不合法，终止程序。

```
if (!parser.parse_opts(argc, argv) || appId.empty() || name.empty()) {
  std::ostringstream sout;
  parser.print_usage(argv[0], sout);
  cout<<sout.str()<<endl;
  return -1;
}
```

如果采用手动触发模式，则需要设置额外的事件监听器。

```
if(triggerMode == agora::linuxsdk::MANUALLY_MODE) {
    signal(SIGUSR1, start_service);
    signal(SIGUSR2, stop_service);
}
```

检查录制文件目录和配置文件目录是否为空目录。若不为空目录，终止程序。

```
if(!recordFileRootDir.empty() && !cfgFilePath.empty()){
  LOG(ERROR,"Client can't set both recordFileRootDir and cfgFilePath");
  return -1;
}
```

若为设置录制文件目录和配置文件目录，将录制文件目录设置为当前目录。

```
if(recordFileRootDir.empty() && cfgFilePath.empty())
    recordFileRootDir = ".";
```

若已开启合图模式，设置合图参数，并检查合法性。

```
//Once recording video under video mixing model, client needs to config width, height, fps and kbps
if(isMixingEnabled && !isAudioOnly) {
   if(4 != sscanf(mixResolution.c_str(), "%d,%d,%d,%d", &width,
                &height, &fps, &kbps)) {
      LOG(ERROR, "Illegal resolution: %s", mixResolution.c_str());
      return -1;
   }
}
```

### 应用设置

当用户加入频道时，记录 log 信息。

```
LOG(INFO, "uid %" PRIu32 " from vendor %s is joining channel %s",
        uid, appId.c_str(), name.c_str());
```

定义 `recorder `对象和相应设置。

```
agora::AgoraSdk recorder;
agora::recording::RecordingConfig config;
```

应用空闲时间设置和频道设置。

```
config.idleLimitSec = idleLimitSec;
config.channelProfile = static_cast<agora::linuxsdk::CHANNEL_PROFILE_TYPE>(channelProfile);
```

应用视频，音频和合图设置。

```
config.isVideoOnly = isVideoOnly;
config.isAudioOnly = isAudioOnly;
config.isMixingEnabled = isMixingEnabled;
config.mixResolution = (isMixingEnabled && !isAudioOnly)? const_cast<char*>(mixResolution.c_str()):NULL;
config.mixedVideoAudio = mixedVideoAudio;
```

设置应用目录，录制文件目录和配置文件目录。

```
config.appliteDir = const_cast<char*>(applitePath.c_str());
config.recordFileRootDir = const_cast<char*>(recordFileRootDir.c_str());
config.cfgFilePath = const_cast<char*>(cfgFilePath.c_str());
```

应用加密设置。

```
config.secret = secret.empty()? NULL:const_cast<char*>(secret.c_str());
config.decryptionMode = decryptionMode.empty()? NULL:const_cast<char*>(decryptionMode.c_str());
config.proxyServer = proxyServer.empty()? NULL:const_cast<char*>(proxyServer.c_str());
```

应用 UDP 端口设置和截图时间间隔设置。

```
config.lowUdpPort = lowUdpPort;
config.highUdpPort = highUdpPort;
config.captureInterval = captureInterval;
```

应用音视频流格式设置和触发模式设置。

```
config.decodeAudio = static_cast<agora::linuxsdk::AUDIO_FORMAT_TYPE>(getAudioFrame);
config.decodeVideo = static_cast<agora::linuxsdk::VIDEO_FORMAT_TYPE>(getVideoFrame);
config.streamType = static_cast<agora::linuxsdk::REMOTE_VIDEO_STREAM_TYPE>(streamType);
config.triggerMode = static_cast<agora::linuxsdk::TRIGGER_MODE_TYPE>(triggerMode);
```

### 建立 Recorder 实例

设置合图模式。

```
recorder.updateMixModeSetting(width, height, isMixingEnabled ? !isAudioOnly:false);
```

创建录制引擎实例并加入频道。

```
if (!recorder.createChannel(appId, channelKey, name, uid, config)) {
  cerr << "Failed to create agora channel: " << name << endl;
  return -1;
}
```

更新录制文件目录。

```
cout << "Recording directory is " << recorder.getRecorderProperties()->storageDir << endl;
recorder.updateStorageDir(recorder.getRecorderProperties()->storageDir);
```

更新录制状态标志。

```
while (!recorder.stopped() && !g_bSignalStop) {
    if(g_bSignalStartService) {
        recorder.startService();
        g_bSignalStartService = false;
    }

    if(g_bSignalStopService) {
        recorder.stopService();
        g_bSignalStopService = false;
    }

    sleep(1);
}
```

录制终止时，离开频道并释放资源。

```
if (g_bSignalStop) {
  recorder.leaveChannel();
  recorder.release();
}

cerr << "Stopped \n";
return 0;
```

### 开始录制

你现在可以开始录制了。

## 参考文档

-   关于录制 SDK 的 API 的详细信息，参考 [录制 API](../../cn/API%20Reference/recording_cpp.md) 。

-   录制完成后，你可能需要使用转码脚本将录制的文件进行合成，详见 [录制音视频](../../cn/Quickstart%20Guide/recording_voice_video.md)  中关于转码脚本的说明。

-   录制的示例代码已包含在录制 SDK 包中。C++ 的示例代码在位于 ./samples/cpp 中。



