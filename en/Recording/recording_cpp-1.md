
---
title: Enabling Recording
description: 
platform: CPP
updatedAt: Fri Nov 30 2018 07:01:27 GMT+0000 (UTC)
---
# Enabling Recording
In this quickstart, you will learn how to use the Agora Recording SDK to enable recording.

The Agora Recording SDK for Linux (Recording SDK) supports:

-   Recording communication and live broadcast content.
-   Recording the voice and video of all users in a channel.
-   Recording the voice and video of all users in multiple channels simultaneously.
-   Recording the voice of all users in a channel or in multiple channels simultaneously.
-   Recording an encrypted channel if the application has integrated Agora built-in encryption.


The Agora Recording SDK for Linux is integrated on your Linux server instead of your application:

<img alt="../_images/recording_linux_en.png" src="https://web-cdn.agora.io/docs-files/en/recording_linux_en.png" style="width: 640px;"/>


To record the content of a channel, a ‘special audience’ joins the channel, gets the content and stores the content on a Linux server. You must:

-   Implement the recording SDK on your Linux server.
-   Use the same App ID in the Agora Recording SDK and in other Agora SDKs implementing voice or video communication. For detailed information about App ID, see [Getting an App ID](../../en/Agora%20Platform/token.md).
-   Specify the channel to record.

> The Recording SDK must use the same channel profile as the Agora Native/Web SDK, otherwise issues may occur.
## Prerequisites

See [Setting up Your Environment](../../en/Quickstart%20Guide/recording_env.md) for the prerequisites.

## Quickstart

### Import the C++ Libraries

Import the C++ libraries for the variable definitions and streaming.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Library</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>&lt;csignal&gt;</span></code></td>
<td>Signal handling library.</td>
</tr>
<tr><td><code><span>&lt;cstdint&gt;</span></code></td>
<td>Defines a set of integral type
aliases.</td>
</tr>
<tr><td><code><span>&lt;iostream&gt;</span></code></td>
<td>Defines the standard input/output
stream objects.</td>
</tr>
<tr><td><code><span>&lt;sstream&gt;</span></code></td>
<td>Provides the string stream classes.</td>
</tr>
<tr><td><code><span>&lt;string&gt;</span></code></td>
<td>Defines the string types, character
traits, and a set of converting
functions.</td>
</tr>
<tr><td><code><span>&lt;vector&gt;</span></code></td>
<td>Defines the vector container
class.</td>
</tr>
<tr><td><code><span>&lt;algorithm&gt;</span></code></td>
<td>Defines a collection of functions
designed to be used on ranges of
elements.</td>
</tr>
</tbody>
</table>



```
#include <csignal>
#include <cstdint>
#include <iostream>
#include <sstream>
#include <string>
#include <vector>
#include <algorithm>
```

Import the Agora SDK libraries.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Library</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>IAgoraLinuxSdkCommon.h</span></code></td>
<td>Defines Agora variable types and classes.</td>
</tr>
<tr><td><code><span>IAgoraRecordingEngine.h</span></code></td>
<td>Defines the Agora recording engine class.</td>
</tr>
<tr><td><code><span>base/atomic.h</span></code></td>
<td>Defines the Agora namespace.</td>
</tr>
<tr><td><code><span>base/log.h</span></code></td>
<td>Agora logging library.</td>
</tr>
<tr><td><code><span>base/opt_parser.h</span></code></td>
<td>Agora communication helper library.</td>
</tr>
<tr><td><code><span>agorasdk/AgoraSdk.h</span></code></td>
<td>Agora Recording SDK.</td>
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

### Add the Namespaces and Global Variables

Define the standard and Agora classes, and specify the namespaces to use in the code.

```
using std::string;
using std::cout;
using std::cerr;
using std::endl;

using agora::base::opt_parser;
using agora::linuxsdk::VideoFrame;
using agora::linuxsdk::AudioFrame;
```

### Define the Global Variables to Determine the Service Status.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Variable</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>g_bSignalStop</span></code></td>
<td>Used to define if a signal has stopped</td>
</tr>
<tr><td><code><span>g_bSignalStartService</span></code></td>
<td>Used to define if a service has started</td>
</tr>
<tr><td><code><span>g_bSignalStopService</span></code></td>
<td>Used to define if a service has stopped</td>
</tr>
</tbody>
</table>



```
atomic_bool_t g_bSignalStop;
atomic_bool_t g_bSignalStartService;
atomic_bool_t g_bSignalStopService;
```

### Create the Start and Stop Service Methods

The <code>start_service</code> and <code>stop_service</code> methods update the global <code>g_bSignalStartService</code> and <code>g_bSignalStopService</code> variables.

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

### Define the Variables

#### Define the Agora variables for the Agora SDK engine.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Variable</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>uid</span></code></td>
<td>User ID</td>
</tr>
<tr><td><code><span>appId</span></code></td>
<td>App ID</td>
</tr>
<tr><td><code><span>channelKey</span></code></td>
<td>Channel key</td>
</tr>
<tr><td><code><span>name</span></code></td>
<td>Channel name</td>
</tr>
<tr><td><code><span>channelProfile</span></code></td>
<td>Channel profile</td>
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

#### Define the Variables for the Recording Configurations.

Define the decryption mode <code>decryptionMode</code> and the decryption key <code>secret</code>.

```
string decryptionMode;
string secret;
```

Define the resolution for the video mix and the idle time limit (seconds).

```
string mixResolution("360,640,15,500");

int idleLimitSec=5*60;//300s
```

Define the paths for the application.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Variable</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>applitePath</span></code></td>
<td>Path to store AgoraCoreService</td>
</tr>
<tr><td><code><span>recordFileRootDir</span></code></td>
<td>Directory to save the recording files</td>
</tr>
<tr><td><code><span>cfgFilePath</span></code></td>
<td>Path to the configuration file</td>
</tr>
<tr><td><code><span>proxyServer</span></code></td>
<td>IP and port for the proxy server</td>
</tr>
</tbody>
</table>



```
string applitePath;
string appliteLogPath;
string recordFileRootDir = "";
string cfgFilePath = "";
string proxyServer;
```

Define the variables for the low and high UDP ports.

```
int lowUdpPort = 0;//40000;
int highUdpPort = 0;//40004;
```

Define the audio, video, and mixing variables for the settings in the Agora parser.

```
bool isAudioOnly=0;
bool isVideoOnly=0;
bool isMixingEnabled=0;
bool mixedVideoAudio=0;
```

Define the audio, video, and stream format types.

```
uint32_t getAudioFrame = agora::linuxsdk::AUDIO_FORMAT_DEFAULT_TYPE;
uint32_t getVideoFrame = agora::linuxsdk::VIDEO_FORMAT_DEFAULT_TYPE;
uint32_t streamType = agora::linuxsdk::REMOTE_VIDEO_STREAM_HIGH;
```

Define the video snapshot interval <code>captureInterval</code> (seconds). Set the trigger mode to automatic.

```
int captureInterval = 5;
int triggerMode = agora::linuxsdk::AUTOMATICALLY_MODE;
```

Define the following video parameters: Width, height, frame rate, and bitrate.

```
int width = 0;
int height = 0;
int fps = 0;
int kbps = 0;
```

Define the signal variables, which track the communication signal of the recording.

```
g_bSignalStop = false;
g_bSignalStartService = false;
g_bSignalStopService = false;
```

### Set the Signal Event Handlers

Use the <code>signal</code> method to set the signal event handlers.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Event</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code><span>SIGQUIT</span></code></td>
<td>The signal is terminated.</td>
</tr>
<tr><td><code><span>SIGABRT</span></code></td>
<td>The signal is aborted.</td>
</tr>
<tr><td><code><span>SIGINT</span></code></td>
<td>The signal is interrupted.</td>
</tr>
<tr><td><code><span>SIGPIPE</span></code></td>
<td>Broken pipe signal. Passing
<code><span>SIG_IGN</span></code> as a handler ignores
the broken pipe signal.</td>
</tr>
</tbody>
</table>



```
signal(SIGQUIT, signal_handler);
signal(SIGABRT, signal_handler);
signal(SIGINT, signal_handler);
signal(SIGPIPE, SIG_IGN);
```

Define the <code>parser</code> object, using the <code>parser.add_long_opt</code> method to parse the following rtcEngine parameters:

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

Parse the audio, video, and mix settings.

```
parser.add_long_opt("isAudioOnly", &isAudioOnly, "Default 0:A/V, 1:AudioOnly (0:1)/option");
parser.add_long_opt("isVideoOnly", &isVideoOnly, "Default 0:A/V, 1:VideoOnly (0:1)/option");
parser.add_long_opt("isMixingEnabled", &isMixingEnabled, "Mixing Enable? (0:1)/option");
parser.add_long_opt("mixResolution", &mixResolution, "change default resolution for vdieo mix mode/option");
parser.add_long_opt("mixedVideoAudio", &mixedVideoAudio, "mixVideoAudio:(0:seperated Audio,Video) (1:mixed Audio & Video), default is 0 /option");
```

Parse the decryption mode <code>decryptionMode</code> and decryption key <code>secret</code>.

```
parser.add_long_opt("decryptionMode", &decryptionMode, "decryption Mode, default is NULL/option");
parser.add_long_opt("secret", &secret, "input secret when enable decryptionMode/option");
```

Parse the idle time limit and root directory for the recording files.

```
parser.add_long_opt("idle", &idleLimitSec, "Default 300s, should be above 3s/option");
parser.add_long_opt("recordFileRootDir", &recordFileRootDir, "recording file root dir/option");
```

Parse the low and high UDP ports.

```
parser.add_long_opt("lowUdpPort", &lowUdpPort, "default is random value/option");
parser.add_long_opt("highUdpPort", &highUdpPort, "default is random value/option");
```

Parse the audio and video frame settings.

```
parser.add_long_opt("getAudioFrame", &getAudioFrame, "default 0 (0:save as file, 1:aac frame, 2:pcm frame, 3:mixed pcm frame) (Can't combine with isMixingEnabled) /option");
parser.add_long_opt("getVideoFrame", &getVideoFrame, "default 0 (0:save as file, 1:h.264, 2:yuv, 3:jpg buffer, 4:jpg file, 5:jpg file and video file) (Can't combine with isMixingEnabled) /option");
```

Parse the video snapshot interval, configuration file path, and proxy server.

```
parser.add_long_opt("captureInterval", &captureInterval, "default 5 (Video snapshot interval (second))");
parser.add_long_opt("cfgFilePath", &cfgFilePath, "config file path / option");
parser.add_long_opt("proxyServer", &proxyServer, "proxyServer:format ip:port, eg,\"127.0.0.1:1080\"/option");
```

Parse the video stream type and trigger mode.

```
parser.add_long_opt("streamType", &streamType, "remote video stream type(0:STREAM_HIGH,1:STREAM_LOW), default is 0/option");
parser.add_long_opt("triggerMode", &triggerMode, "triggerMode:(0: automatically mode, 1: manually mode) default is 0/option");
```

Ensure that the <code>parser</code> settings, <code>appID</code>, and channel <code>name</code> are all valid. If any of these are invalid, terminate the application.

```
if (!parser.parse_opts(argc, argv) || appId.empty() || name.empty()) {
  std::ostringstream sout;
  parser.print_usage(argv[0], sout);
  cout<<sout.str()<<endl;
  return -1;
}
```

If the trigger mode is set to manual, add additional signal event listeners to start and stop the service.

```
if(triggerMode == agora::linuxsdk::MANUALLY_MODE) {
    signal(SIGUSR1, start_service);
    signal(SIGUSR2, stop_service);
}
```

Check if the recording file directory and configuration file path are empty. If the directories are not empty, log an error using the <code>LOG</code> method and terminate the application.

```
if(!recordFileRootDir.empty() && !cfgFilePath.empty()){
  LOG(ERROR,"Client can't set both recordFileRootDir and cfgFilePath");
  return -1;
}
```

Set the default path for recording files to <code>.</code>.

```
if(recordFileRootDir.empty() && cfgFilePath.empty())
    recordFileRootDir = ".";
```

If the directories are empty and mixing is enabled, set the video parameters. If the parameters are invalid, terminate the application.

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

Add a log to track the users that join the channel.

```
LOG(INFO, "uid %" PRIu32 " from vendor %s is joining channel %s",
        uid, appId.c_str(), name.c_str());
```

Define the Agora SDK <code>recorder</code> and configuration.

```
agora::AgoraSdk recorder;
agora::recording::RecordingConfig config;
```

Apply the idle time limit and channel profile.

```
config.idleLimitSec = idleLimitSec;
config.channelProfile = static_cast<agora::linuxsdk::CHANNEL_PROFILE_TYPE>(channelProfile);
```

Apply the video, audio, and mix settings.

```
config.isVideoOnly = isVideoOnly;
config.isAudioOnly = isAudioOnly;
config.isMixingEnabled = isMixingEnabled;
config.mixResolution = (isMixingEnabled && !isAudioOnly)? const_cast<char*>(mixResolution.c_str()):NULL;
config.mixedVideoAudio = mixedVideoAudio;
```

Apply the application directory, recording file directory, and configuration file path.

```
config.appliteDir = const_cast<char*>(applitePath.c_str());
config.recordFileRootDir = const_cast<char*>(recordFileRootDir.c_str());
config.cfgFilePath = const_cast<char*>(cfgFilePath.c_str());
```

Apply the decryption mode <code>decryptionMode</code>, decryption key <code>secret</code>, and proxy server.

```
config.secret = secret.empty()? NULL:const_cast<char*>(secret.c_str());
config.decryptionMode = decryptionMode.empty()? NULL:const_cast<char*>(decryptionMode.c_str());
config.proxyServer = proxyServer.empty()? NULL:const_cast<char*>(proxyServer.c_str());
```

Apply the low and high UDP ports and the video capture interval.

```
config.lowUdpPort = lowUdpPort;
config.highUdpPort = highUdpPort;
config.captureInterval = captureInterval;
```

Apply the audio, video, and stream format types and the trigger mode.

```
config.decodeAudio = static_cast<agora::linuxsdk::AUDIO_FORMAT_TYPE>(getAudioFrame);
config.decodeVideo = static_cast<agora::linuxsdk::VIDEO_FORMAT_TYPE>(getVideoFrame);
config.streamType = static_cast<agora::linuxsdk::REMOTE_VIDEO_STREAM_TYPE>(streamType);
config.triggerMode = static_cast<agora::linuxsdk::TRIGGER_MODE_TYPE>(triggerMode);
```

Set the mix mode for the <code>recorder</code>.

```
recorder.updateMixModeSetting(width, height, isMixingEnabled ? !isAudioOnly:false);
```

Create a recording engine instance and join the video channel. If it fails, terminate the application.

```
if (!recorder.createChannel(appId, channelKey, name, uid, config)) {
  cerr << "Failed to create agora channel: " << name << endl;
  return -1;
}
```

Update the recording storage directory.

```
cout << "Recording directory is " << recorder.getRecorderProperties()->storageDir << endl;
recorder.updateStorageDir(recorder.getRecorderProperties()->storageDir);
```

While the <code>recorder</code> is running, update the signal service using <code>recorder.startService</code> and <code>recorder.stopService</code>.

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

Once the signal stops, leave the channel using <code>recorder.leaveChannel</code> and release the <code>recorder</code> object.

```
if (g_bSignalStop) {
  recorder.leaveChannel();
  recorder.release();
}

cerr << "Stopped \n";
return 0;
```

### Start recording

Now you can start recording.

## Reference

Refer to [Recording Voice and Video](../../en/Quickstart%20Guide/recording_voice_video.md) for more functions of the Agora Recording SDK.

For details of the APIs in the Agora Recording SDK, please refer to [Recording API](../../en/API%20Reference/recording_cpp.md).




