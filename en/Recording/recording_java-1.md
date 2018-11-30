
---
title: Enabling Recording
description: 
platform: Java
updatedAt: Fri Nov 30 2018 07:03:03 GMT+0000 (UTC)
---
# Enabling Recording
In this quickstart, you will learn how to use the Agora Recording SDK to enable recording.

The Agora Recording SDK for Linux (Recording SDK) supports:

-   Recording communication and live broadcast content
-   Recording the voice and video of all users in a channel
-   Recording the voice and video of all users in multiple channels simultaneously
-   Recording the voice of all users in a channel or in multiple channels simultaneously
-   Recording an encrypted channel if the application has integrated Agora built-in encryption


The Agora Recording SDK for Linux is integrated on your Linux server instead of your app:

<img alt="../_images/recording_linux_en.png" src="https://web-cdn.agora.io/docs-files/en/recording_linux_en.png" style="width: 640.0px;"/>


To record the content of a channel, a ‘special audience’ joins the channel, gets the content and stores the content on a Linux server. You must:

-   Implement the Agora Recording SDK on your Linux server.
-   Use the same App ID in the Agora Recording SDK and in other Agora SDKs implementing voice or video communication. For information about the App ID, see [Getting an App ID](../../en/Agora%20Platform/token.md).
-   Specify the channel to record.

> The Recording SDK must use the same channel profile as the Agora Native/Web SDK, otherwise issues may occur.
## Prerequisites

See [Setting up Your Environment](../../en/Quickstart%20Guide/recording_env.md) for the prerequisites.

## Quickstart

### Import the Java Libraries

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

### Define the AgoraJavaRecording Class

```
class AgoraJavaRecording{
...
}
```

### Define the Main Method

```
public static void main(String[] args){
...
}
```

### Define the Variables

#### Define the variables for the Agora Engine

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
int uid = 0;
String appId = "";
String channelKey = "";
String name = "";
int channelProfile = 0;
```

#### Define the Variables for the Recording Settings

Define the proxy mode and keys.

```
String decryptionMode = "";
String secret = "";
```

Define the resolution and idle time for video mixing.

```
String mixResolution = "360,640,15,500";
int idleLimitSec=5*60;//300s
```

Define the paths.

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
<td>Path to store AgoraCoreService.</td>
</tr>
<tr><td><code><span>recordFileRootDir</span></code></td>
<td>Directory to save the recording files.</td>
</tr>
<tr><td><code><span>cfgFilePath</span></code></td>
<td>Path to the configuration file.</td>
</tr>
</tbody>
</table>



```
String applitePath = "";
String recordFileRootDir = "";
String cfgFilePath = "";
```

Define the variables for the low and high UDP ports.

```
int lowUdpPort = 0;//40000;
int highUdpPort = 0;//40004;
```

Define the audio, video, and mixing variables for the settings in the Agora parser.

```
boolean isAudioOnly=false;
boolean isVideoOnly=false;
boolean isMixingEnabled=false;
boolean mixedVideoAudio=false;
```

Define the audio, video, and stream format types.

```
int getAudioFrame = AUDIO_FORMAT_TYPE.AUDIO_FORMAT_DEFAULT_TYPE.ordinal();
int getVideoFrame = VIDEO_FORMAT_TYPE.VIDEO_FORMAT_DEFAULT_TYPE.ordinal();
int streamType = REMOTE_VIDEO_STREAM_TYPE.REMOTE_VIDEO_STREAM_HIGH.ordinal();
```

Define the video snapshot interval <code>captureInterval</code> (seconds). Set the trigger mode to automatic.

```
int captureInterval = 5;
int triggerMode = 0;
```

Define the following video parameters: Width, height, frame rate, and bitrate.

```
int width = 0;
int height = 0;
int fps = 0;
int kbps = 0;
int count = 0;
```

### Parse and Check the Command Line Parameters

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

### Read the Command Line Parameters

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

### Create the Recording and Config Instances

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

### Set the Mixing Parameters

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

### Create and Join the Channel

```
ars.createChannel(appId, channelKey,name,uid,config);
```

### Start recording

Now you can start recording.

## Reference

Refer to [Recording Voice and Video](../../en/Quickstart%20Guide/recording_voice_video.md) for more functions of Agora Recording SDK.

For details of the APIs in Agora Recording SDK, please refer to [Recording API Beta](../../en/API%20Reference/recording_java.md).



