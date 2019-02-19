
---
title: Recording API 
description: 
platform: Java
updatedAt: Thu Jan 10 2019 17:16:54 GMT+0000 (UTC)
---
# Recording API 
> Version: v2.2.3
> This Java API is a data encapsulation of the C++ sample code with JNI, and is therefore slightly different from that of the C++ API in structure. The Agora SDK (sample code shared by C++ and Java) implements the C++ recording API methods and callbacks, goes through data encapsulation in the JNI layer, and then works like the Java interface and class of the Native SDK through the JNI proxy.


<table>
<thead>
<tr><th>Interface</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><a href="#RecordingSDK">RecordingSDK</a></td>
<td>Main methods that can be invoked by your app.</td>
</tr>
<tr><td><a href="#RecordingEventHandler">RecordingEventHandler</a></td>
<td>Callbacks returned to your app.</td>
</tr>
</tbody>
</table>

## <a name="RecordingSDK"></a>RecordingSDK

RecordingSDK provides main methods that can be invoked by your app.

-   [Creates a Channel (createChannel)](#createChannel)

-   [Sets the Video Mixing Layout (setVideoMixingLayout)](#setVideoMixingLayout)

-   [Allows the Recording App to Leave the Channel (leaveChannel)](#leaveChannel)

-   [Starts the Recording (startService)](#startService)

-   [Pauses the Recording (stopService)](#stopService)

-   [Retrieves the Recording Properties (getProperties)](#getProperties)

-   [Sets the User Background Image (setUserBackground)](#setUserBackground)

-   [Sets the Log Level (setLogLevel)](#setLogLevel)

-   [Enables the Module Log (enableModuleLog)](#enableModuleLog)


### <a name="createChannel"></a>Creates a Channel (createChannel)

This method creates a channel and enables the app to join the channel.

```
public native boolean createChannel(String appId, String token, String name, int uid, RecordingConfig config, int logLevel, int logModules);
```

<table>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>appId</code></td>
<td>The App ID used in the communication to be recorded. See  <a href="../../en/Agora%20Platform/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td><code>token</code></td>
<td>The token used in the communication to be recorded. See  <a href="../../en/Agora%20Platform/token.md"><span>Security Keys</span></a>.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are:
<ul>
<li>The 26 lowercase English letters: a to z</li>
<li>The 26 uppercase English letters: A to Z</li>
<li>The 10 numbers: 0 to 9</li>
<li>Space</li>
<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","</li>
</ul>
</td>
</tr>
<tr><td><code>uid</code></td>
<td><p>User ID. A 32-bit unsigned integer ranging from 1 to (2^32-1) that is unique in a channel. Two Settings:</p>
<ul>
<li>Set as 0, the system automatically assigns a uid.</li>
<li>Set a unique uid (cannot be the same as any uid in the current channel).</li>
</ul>
</td>
</tr>
<tr><td><code>config</code></td>
<td>Detailed recording configuration. See the definition in the table below.</td>
</tr>
<tr><td><code>logLevel</code></td>
<td>Generate logs based on the selected level. Only logs with a level lower than <code>logLevel</code> are generated. See <a href="#setloglevel-recording-java"><span>Sets the Log Level (setLogLevel)</span></a>.</td>
</tr>
<tr><td><code>logModules</code></td>
<td>Generate logs of a module. Only logs of the specified module are generated. See <a href="#enablelogmodule-recording-java"><span>Enables the Module Log (enableModuleLog)</span></a>.</td>
</tr>
<tr><td>Returns:</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



> -   In the Recording SDK, <code>requestToken</code> and <code>renewToken</code> are private interfaces. Make sure that you set [expireTimestamp](../../en/Recording/token.md) as 0 when generating a token, which means that the privilege, once generated, never expires.
> -   A channel does not accept duplicate uids. Otherwise, there will be unpredictable behaviors.


The structure of <code>RecordingConfig</code>:

```
public class RecordingConfig {
  public RecordingConfig() {
    isAudioOnly = false;
    isVideoOnly = false;
    isMixingEnabled = false;
    mixedVideoAudio = MIXED_AV_CODEC_TYPE.MIXED_AV_DEFAULT;

    mixResolution = "";
    decryptionMode = "";
    secret = "";
    appliteDir = "";
    recordFileRootDir = "";
    cfgFilePath = "";
    proxyServer = "";
    defaultVideoBgPath = "";
    defaultUserBgPath = "";

    lowUdpPort = 0;//40000;
    highUdpPort = 0;//40004;
    idleLimitSec = 300;
    captureInterval = 5;
    triggerMode = 0;
    audioIndicationInterval = 0;
    audioProfile = 0;

    decodeVideo = VIDEO_FORMAT_TYPE.VIDEO_FORMAT_DEFAULT_TYPE;
    decodeAudio = AUDIO_FORMAT_TYPE.AUDIO_FORMAT_DEFAULT_TYPE;
    channelProfile = CHANNEL_PROFILE_TYPE.CHANNEL_PROFILE_COMMUNICATION;
    streamType = REMOTE_VIDEO_STREAM_TYPE.REMOTE_VIDEO_STREAM_HIGH;
  }

  public boolean isAudioOnly;
  public boolean isVideoOnly;
  public boolean isMixingEnabled;
  public MIXED_AV_CODEC_TYPE mixedVideoAudio;
  public String mixResolution;
  public String decryptionMode;
  public String secret;
  public String appliteDir;
  public String recordFileRootDir;
  public String cfgFilePath;
  //decodeVideo: default 0 (0:save as file, 1:h.264, 2:yuv, 3:jpg buffer, 4:jpg file, 5:jpg file and video file)
  public VIDEO_FORMAT_TYPE decodeVideo;
  //decodeAudio:  (default 0 (0:save as file, 1:aac frame, 2:pcm frame, 3:mixed pcm frame) (Can't combine with isMixingEnabled) /option)
  public AUDIO_FORMAT_TYPE decodeAudio;
  public int lowUdpPort;
  public int highUdpPort;
  public int idleLimitSec;
  public int captureInterval;
  public int audioIndicationInterval;
  //channelProfile:0 braodacast, 1:communicate; default is 1
  public CHANNEL_PROFILE_TYPE channelProfile;
  //streamType:0:get high stream 1:get low stream; default is 0
  public REMOTE_VIDEO_STREAM_TYPE streamType;
  public int triggerMode;
  public String proxyServer; //format ipv4:port
  public int audioProfile;
  public String defaultVideoBgPath;
  public String defaultUserBgPath;
}
```

<table>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>isAudioOnly</code></td>
<td><p>Sets whether or not to record audio only:</p>
<ul>
<li>true: Enables audio recording and disables video recording.</li>
<li>false: (Default) Enables both audio and video recording.</li>
</ul>
Used together with <code>isVideoOnly</code>:
<ul>
<li>If <code>isAudioOnly</code> is set as true and <code>isVideoOnly</code> is set as false, only record audio.</li>
<li>If <code>isAudioOnly</code> is set as false and <code>isVideoOnly</code> is set as true, only record video.</li>
<li>If <code>isAudioOnly</code> is set as false and <code>isVideoOnly</code> is set as false, record both audio and video.</li>
<li><code>isAudioOnly</code> and <code>isVideoOnly</code> cannot be set as true at the same time.</li>
</ul>
</td>
</tr>
<tr><td><code>isVideoOnly</code></td>
<td><p>Sets whether or not to record video only:</p>
<ul>
<li>true: Enables video recording and disables audio recording.</li>
<li>false: (Default) Enables both audio and video recording.</li>
</ul>
Used together with <code>isAudioOnly</code>:
<ul>
<li>If <code>isAudioOnly</code> is set as true and <code>isVideoOnly</code> is set as false, only record audio.</li>
<li>If <code>isAudioOnly</code> is set as false and <code>isVideoOnly</code> is set as true, only record video.</li>
<li>If <code>isAudioOnly</code> is set as false and <code>isVideoOnly</code> is set as false, record both audio and video.</li>
<li><code>isAudioOnly</code> and <code>isVideoOnly</code> cannot be set as true at the same time.</li>
</ul>
</td>
</tr>
<tr><td><code>isMixingEnabled</code></td>
<td><p>Sets whether or not to enable the audio- or video-composite mode.</p>
<ul>
<li>true: Enables the composite mode, which means the audio of all uids is mixed in an audio file and the video of all uids is mixed in a video file. The sample rate, bitrate, and number of audio channels of the recording file are the same as the highest level of those of the original audio streams.</li>
<li>false: (Default) Enables the individual mode, which means one audio or video file for a uid. The bitrate and number of audio channels of the recording file are the same as those of the original audio stream.</li>
</ul>
</td>
</tr>
<tr><td><code>mixedVideoAudio</code></td>
<td><p>If you set <code>isMixingEnabled</code> as <code>true</code>, <code>mixedVideoAudio</code> allows you to mix the audio and video in real time:</p>
<ul>
<li>0: (Default) Do not mix the audio and video.</li>
<li>1: Mix the audio and video in real time into an MPEG-4 file. Supports limited players.</li>
</ul>
</td>
</tr>
<tr><td><code>mixResolution</code></td>
<td>If you set <code>isMixingEnabled</code> as true, <code>mixResolution</code> allows you to set the resolution in the format of width, height, fps, and Kbps; representing the width, height, frame rate, and bitrate of the video stream.</td>
</tr>
<tr><td><code>decryptionMode</code></td>
<td><p>When the whole channel is encrypted, the Recording SDK uses <code>decryptionMode</code> to enable the built-in decryption function:</p>
<ul>
<li>“aes-128-xts”: AES-128, XTS mode</li>
<li>“aes-128-ecb”: AES-128, ECB mode</li>
<li>“aes-256-xts”: AES-256, XTS mode</li>
</ul>
</td>
</tr>
<tr><td><code>secret</code></td>
<td>The decryption password when decryption mode is enabled. The default value is NULL.</td>
</tr>
<tr><td><code>appliteDir</code></td>
<td>The directory of AgoraCoreService. The default value is NULL.</td>
</tr>
<tr><td><code>recordFilrRootDir</code></td>
<td>The root directory of the recording files. The default value is NULL. The sub-path is generated automatically.</td>
</tr>
<tr><td><code>cfgFilePath</code></td>
<td>The path of the configuration file. The default value is NULL. In the configuration file, you can set the absolute path of the recorded file, but the sub-path is not generated automatically. The content in the configuration file must be in JSON format, such as {“Recording_Dir” : “&lt;recording path&gt;”}. “Recording_Dir” cannot be changed.
</td>
</tr>
<tr><td><code>decodeAudio</code></td>
<td><p>Audio decoding format:</p><ul>
<li>AUDIO_FORMAT_DEFAULT_TYPE = 0: Default audio format.</li>
<li>AUDIO_FORMAT_AAC_FRAME_TYPE = 1: Audio frame in AAC format.</li>
<li>AUDIO_FORMAT_PCM_FRAME_TYPE = 2: Audio frame in PCM format (AUDIO_FORMAT_PCM_FRAME_TYPE).</li>
<li>AUDIO_FORMAT_MIXED_PCM_FRAME_TYPE = 3: Audio frame in PCM audio-mixing format.</li>
</ul>
When AUDIO_FORMAT_TYPE = 1, 2, or 3, <code>isMixingEnabled</code> cannot be set as true.
</td>
</tr>
<tr><td><code>decodeVideo</code></td>
<td><p>Video decoding format:</p><ul>
<li>VIDEO_FORMAT_DEFAULT_TYPE = 0: Default video format.</li>
<li>VIDEO_FORMAT_H264_FRAME_TYPE = 1: Video frame in H.264 format.</li>
<li>VIDEO_FORMAT_YUV_FRAME_TYPE = 2: Video frame in YUV format.</li>
<li>VIDEO_FORMAT_JPG_FRAME_TYPE = 3: Video frame in JPEG format.</li>
<li>VIDEO_FORMAT_JPG_FILE_TYPE = 4: JPEG file format.</li>
<li>VIDEO_FORMAT_JPG_VIDEO_FILE_TYPE = 5: Video frame in JPEG format + MPEG-4 video</li>
<ul>
<li>Individual Mode (<code>isMixingEnabled</code> is set as <code>false</code>): MPEG-4 video and JPEG files are supported.</li>
<li>Mixing Mode (<code>isMixingEnabled</code> is set as <code>true</code>): MPEG-4 video files for combined streams and JPEG files for individual streams are supported.</li>
</ul>	
</ul>	
When VIDEO_FORMAT_TYPE = 1, 2, 3, or 4, <code>isMixingEnabled</code> cannot be set as true.	

When the video is decoded into raw video data, that is, VIDEO_FORMAT_TYPE = 1, 2, 3, or 5, raw video data in YUV format for the Web SDK is supported while H.264 format is not supported.
</td>
</tr>
<tr><td><code>lowUdpPort</code></td>
<td>The lowest UDP port. The default value is 0. Ensure that the value of <code>highUdpPort</code> - <code>lowUdpPort</code> &ge; 4.</td>
</tr>
<tr><td><code>highUdpPort</code></td>
<td>The highest UDP port. The default value is 0. Ensure that the value of <code>highUdpPort</code> - <code>lowUdpPort</code> &ge; 4.</td>
</tr>
<tr><td><code>idleLimitSec</code></td>
<td>The Agora Recording SDK automatically stops recording when there is no user in the recorded channel after a time period of <code>idleLimitSec</code>. The value must be &ge; three seconds. The default value is 300 seconds.</td>
</tr>
<tr><td><code>captureInterval</code></td>
<td>Time interval of the screen capture. The value ranges between one second and five seconds. You need to use <code>captureInterval</code> with decodeVideo = 3/4 when joining the channel. See <code>joinChannel</code> to allow a user to join a channel.</td>
</tr>
<tr><td><code>audioIndicationInterval</code></td>
<td><p>Whether or not to detect the speakers:</p>
<div><ul>
<li>&le; 0: (Default) Disables the function of detecting the speakers.</li>
<li>&gt; 0: The time interval (ms) of detecting the speakers. Agora recommends setting the time interval to be longer than 200ms. When a speaker is found, the SDK returns the UID of the speaker in the <code>onActiveSpeaker</code> callback.</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>channelProfile</code></td>
<td><p>Sets the channel profile:</p>
<ul>
<li>CHANNEL_PROFILE_COMMUNICATION = 0: (Default) Communication profile. This is used in one-on-one or group calls, where all users in the channel can talk freely.</li>
<li>CHANNEL_PROFILE_LIVE_BROADCAST = 1: Live-broadcast profile. The host sends and receives voice/video, while the audience only receives voice/video. Host and audience roles can be set by calling <code>setClientRole</code>. </li>
</ul>
The Recording SDK must use the same channel profile as the Agora Native/Web SDK, otherwise issues may occur.
</td>
</tr>
<tr><td><code>streamType</code></td>
<td>Takes effect only when the Agora Native SDK enables the dual-stream mode (high stream by default).</td>
</tr>
<tr><td><a name="triggerMode"></a><code>triggerMode</code></td>
<td><p>Sets whether to record automatically or manually upon joining a channel:</p>
<ul>
<li>0: Automatically.</li>
<li>1: Manually. To start and stop recording, call <code>startRecording</code> and <code>stopRecording</code> respectively.</li>
</ul>
</td>
</tr>
<tr><td><code>proxyserver</code></td>
<td>Allows you to record the content with the Intranet server. For details, please contact <a href="mailto:sales%40agora.io">sales@agora.io</a>.</td>
</tr>
<tr><td><code>audioProfile</code> </td>
<td><p>Audio profile of the recording file:</p>
<div><ul>
<li>AUDIO_PROFILE_DEFAULT = 0: (Default) Sample rate of 48 kHz, communication encoding, mono.</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY = 4: Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps.</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO = 5: Sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps.</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>defaultVideoBgPath</code></td>
<td>The default background image of the video.</td>
</tr>
<tr><td><code>defaultUserBgPath</code></td>
<td>The default background image of the user.</td>
</tr>
</tbody>
</table>


**Video Profile for the Communication Profile:**

<table>
<thead>
<tr><th>Resolution</th>
<th>Frame Rate (fps)</th>
<th>Minimum Bitrate (Kbps)</th>
<th>Maximum Bitrate (Kbps)</th>
<th>Recommended Bitrate (Kbps)</th>
</tr>
</thead>
<tbody>
<tr><td>2560 &times; 1440</td>
<td>15</td>
<td>1600</td>
<td>4800</td>
<td>3200</td>
</tr>
<tr><td>1920 &times; 1080</td>
<td>15</td>
<td>1000</td>
<td>3000</td>
<td>2000</td>
</tr>
<tr><td>1280 &times; 720</td>
<td>15</td>
<td>600</td>
<td>1800</td>
<td>1200</td>
</tr>
<tr><td>960 &times; 720</td>
<td>15</td>
<td>480</td>
<td>1440</td>
<td>960</td>
</tr>
<tr><td>848 &times; 480</td>
<td>15</td>
<td>300</td>
<td>900</td>
<td>600</td>
</tr>
<tr><td>640 &times; 480</td>
<td>15</td>
<td>250</td>
<td>750</td>
<td>500</td>
</tr>
<tr><td>480 &times; 480</td>
<td>15</td>
<td>200</td>
<td>600</td>
<td>400</td>
</tr>
<tr><td>640 &times; 360</td>
<td>15</td>
<td>200</td>
<td>600</td>
<td>400</td>
</tr>
<tr><td>360 &times; 360</td>
<td>15</td>
<td>130</td>
<td>390</td>
<td>260</td>
</tr>
<tr><td>424 &times; 240</td>
<td>15</td>
<td>110</td>
<td>330</td>
<td>220</td>
</tr>
<tr><td>320 &times; 240</td>
<td>15</td>
<td>90</td>
<td>270</td>
<td>180</td>
</tr>
<tr><td>240 &times; 240</td>
<td>15</td>
<td>70</td>
<td>210</td>
<td>140</td>
</tr>
<tr><td>320 &times; 180</td>
<td>15</td>
<td>70</td>
<td>210</td>
<td>140</td>
</tr>
<tr><td>240 &times; 180</td>
<td>15</td>
<td>60</td>
<td>180</td>
<td>120</td>
</tr>
<tr><td>180 &times; 180</td>
<td>15</td>
<td>50</td>
<td>150</td>
<td>100</td>
</tr>
<tr><td>160 &times; 120</td>
<td>15</td>
<td>30</td>
<td>90</td>
<td>60</td>
</tr>
<tr><td>120 &times; 120</td>
<td>15</td>
<td>25</td>
<td>75</td>
<td>50</td>
</tr>
</tbody>
</table>



**Video Profile for the Live Broadcast Profile:**

<table>
<thead>
<tr><th>Resolution</th>
<th>Frame Rate (fps)</th>
<th>Minimum Bitrate (Kbps)</th>
<th>Maximum Bitrate (Kbps)</th>
<th>Recommended Bitrate (Kbps)</th>
</tr>
</thead>
<tbody>
<tr><td>2560 &times; 1440</td>
<td>15</td>
<td>3200</td>
<td>9600</td>
<td>6400</td>
</tr>
<tr><td>1920 &times; 1080</td>
<td>15</td>
<td>2000</td>
<td>6000</td>
<td>4000</td>
</tr>
<tr><td>1280 &times; 720</td>
<td>15</td>
<td>1200</td>
<td>3600</td>
<td>2400</td>
</tr>
<tr><td>960 &times; 720</td>
<td>15</td>
<td>960</td>
<td>2880</td>
<td>1920</td>
</tr>
<tr><td>848 &times; 480</td>
<td>15</td>
<td>600</td>
<td>1800</td>
<td>1200</td>
</tr>
<tr><td>640 &times; 480</td>
<td>15</td>
<td>500</td>
<td>1500</td>
<td>1000</td>
</tr>
<tr><td>480 &times; 480</td>
<td>15</td>
<td>400</td>
<td>1200</td>
<td>800</td>
</tr>
<tr><td>640 &times; 360</td>
<td>15</td>
<td>400</td>
<td>1200</td>
<td>800</td>
</tr>
<tr><td>360 &times; 360</td>
<td>15</td>
<td>260</td>
<td>780</td>
<td>520</td>
</tr>
<tr><td>424 &times; 240</td>
<td>15</td>
<td>220</td>
<td>660</td>
<td>440</td>
</tr>
<tr><td>320 &times; 240</td>
<td>15</td>
<td>180</td>
<td>540</td>
<td>360</td>
</tr>
<tr><td>240 &times; 240</td>
<td>15</td>
<td>140</td>
<td>420</td>
<td>280</td>
</tr>
<tr><td>320 &times; 180</td>
<td>15</td>
<td>140</td>
<td>420</td>
<td>280</td>
</tr>
<tr><td>240 &times; 180</td>
<td>15</td>
<td>120</td>
<td>360</td>
<td>240</td>
</tr>
<tr><td>180 &times; 180</td>
<td>15</td>
<td>100</td>
<td>300</td>
<td>200</td>
</tr>
<tr><td>160 &times; 120</td>
<td>15</td>
<td>60</td>
<td>180</td>
<td>120</td>
</tr>
<tr><td>120 &times; 120</td>
<td>15</td>
<td>50</td>
<td>150</td>
<td>100</td>
</tr>
</tbody>
</table>

<a name = "player_support_list"></a>

**Supported Players:**

<table>
<thead>
<tr><th>Platform</th>
<th>Player/Browser</th>
<th>mixedVideoAudio=0</th>
<th>mixedVideoAudio=1</th>
</tr>
</thead>
<tbody>
<tr><td>Linux</td>
<td>VLC Media Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Linux</td>
<td>ffplayer</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Linux</td>
<td>Google Chrome</td>
<td><strong>Not Supported</strong></td>
<td><strong>Not Supported</strong></td>
</tr>
<tr><td>Windows</td>
<td>Media Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Windows</td>
<td>KMPlayer</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Windows</td>
<td>VLC Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Windows</td>
<td>Google Chrome 49.0.2623 or later</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>QuickTime Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>VLC</td>
<td><strong>Not Supported</strong></td>
<td><strong>Not Supported</strong></td>
</tr>
<tr><td>macOS</td>
<td>Movist</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>MPlayerX</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>KMPlayer</td>
<th><strong>Not Supported</strong></th>
<th><strong>Not Supported</strong></th>
</tr>
<tr><td>macOS</td>
<td>Google Chrome 47.0.2526.111 or later</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>Safari 11.0.3 or later</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>Default Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>VLC for Mobile</td>
<th><strong>Not Supported</strong></th>
<td><strong>Not Supported</strong></td>
</tr>
<tr><td>iOS</td>
<td>KMPlayer</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>Safari 9.0 or later</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>Default Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>MX Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>VLC for Android</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>KMPlayer</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>Google Chrome 49.0.2623 or later</td>
<td>Supported</td>
<td>Supported</td>
</tr>
</tbody>
</table>



### <a name="setVideoMixingLayout"></a>Sets the Video Mixing Layout (setVideoMixingLayout)

This method sets the video mixing layout.

```
private native int setVideoMixingLayout(long nativeObject, VideoMixingLayout layout);
```

The structure of <code>VideoMixingLayout</code>:

```
public class VideoMixingLayout
{
  public int canvasWidth;
  public int canvasHeight;
  public String backgroundColor;//e.g. "#C0C0C0" in RGB
  public int regionCount;
  public Region[] regions;
  public String appData;
  public int appDataLength;
  public class Region {
    public long uid;
    public double x;//[0,1]
    public double y;//[0,1]
    public double width;//[0,1]
    public double height;//[0,1]
    public int zOrder; //optional, [0, 100] //0 (default): bottom most, 100: top most
    //Optional
    //[0, 1.0] where 0 means transparent, 1.0 means opaque
    public double alpha;
    public int renderMode;//RENDER_MODE_HIDDEN: Crop, RENDER_MODE_FIT: Zoom to fit
    public Region(){
      uid = 0;
      x = 0;
      y = 0;
      width = 0;
      height = 0;
      zOrder = 0;
      alpha = 1.0;
      renderMode = 0;
    }
  }
  public VideoMixingLayout() {
    canvasWidth = 0;
    canvasHeight = 0;
    backgroundColor = "";
    regionCount = 0;
    //regions = 0;
    appData = "";
    appDataLength = 0;
  }
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>canvasWidth</code></td>
<td>Width of the canvas (the display window or screen).</td>
</tr>
<tr><td><code>canvasHeight</code></td>
<td>Height of the canvas (the display window or screen).</td>
</tr>
<tr><td><code>backgroundColor</code></td>
<td>The background color of the canvas (the display window or screen) in RGB hex value.</td>
</tr>
<tr><td><code>regionCount</code></td>
<td>The number of the users (Communication profile) or hosts (Live-broadcast profile) in the channel.</td>
</tr>
<tr><td><code>regions</code></td>
<td><p>The user (Communication profile) or host (Live-broadcast profile) list of <code>VideoMixingLayout</code>. Each user (Communication mode) or host (Live-broadcast mode) in the channel has a region to display the video on the screen with the following parameters to be set:</p>
<ul>
<li><code>uid</code>: User IDs of the users (Communication profile) or host Live-broadcast profile) displaying the video in the region.</li>
<li><code>x</code>: Relative horizontal position of the top-left corner of the region. The value is between 0.0 and 1.0.</li>
<li><code>y</code>: Relative vertical position of the top-left corner of the region. The value is between 0.0 and 1.0.</li>
<li><code>width</code>: Relative width of the region. The value is between 0.0 and 1.0.</li>
<li><code>height</code>: Relative height of the region. The value is between 0.0 and 1.0.</li>
<li><code>zOrder</code>: The index of the layer. The value is between 1 (bottom layer) and 100 (top layer).</li>
<li><code>alpha</code>: The transparency of the image. The value is between 0.0 (transparent) and 1.0 (opaque).</li>
<li><code>renderMode</code>: Render mode:<ul>
<li>RENDER_MODE_HIDDEN(1): Cropped mode. Uniformly scale the video until it fills the visible boundaries (cropped). One dimension of the video may have clipped contents..</li>
<li>RENDER_MODE_FIT(2): Fit mode. Uniformly scale the video until one of its dimension fits the boundary (zoomed to fit). Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>appData</code></td>
<td>User-defined data.</td>
</tr>
<tr><td><code>appDataLength</code></td>
<td>The length of the user-defined data.</td>
</tr>
<tr><td>Returns</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



Here is an example to show the position and size of the host’s head portrait. x, y, width, and height are 0.5, 0.4, 0.2, and 0.3 respectively.

<img alt="../_images/sei_overview.png" src="https://web-cdn.agora.io/docs-files/en/sei_overview.png" style="width: 500.0px;"/>


### <a name="leaveChannel"></a>Allows the Recording App to Leave the Channel (leaveChannel)

This method allows the recording app to leave the channel and release the thread resources.

```
private native boolean leaveChannel(long nativeObject);
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Returns</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>


### <a name="startService"></a>Starts a Recording (startService)

This method manually starts a recording.

The method is only valid when you set [triggerMode](#triggerMode) as 1 (manually) when joining a channel. 

```
private native void startService(long nativeHandle);
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Returns</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="stopService"></a>Pauses the Recording (stopService)

This method manually pauses the recording.

The method is only valid when you set [triggerMode](#triggerMode) as 1 (manually) when joining a channel. 

```
private native void stopService(long nativeHandle);
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Returns</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

### <a name="getProperties"></a>Retrieves the Recording Properties (getProperties)

This method allows you to retrieve the recording properties without joining a channel.

> -   The recording properties only include the path of the recording files.
> -   This method is different from [onUserJoined](#onUserJoined). You must call <code>onUserJoined</code> after joining the channel.


```
private native RecordingEngineProperties getProperties(long nativeHandle);
```

### <a name="setUserBackground"></a>Sets the User Background Image (setUserBackground)

This method sets the background image of a specified user.

```
private native int setUserBackground(long nativeHandle, int uid, String image_path);
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
<tr><td><code>uid</code></td>
<td>The user ID of the user for the background image to be set.</td>
</tr>
</thead>
<tbody>
<tr><td><code>image_path</code></td>
<td>The path of the image file.</td>
</tr>
<tr><td>Returns</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a id="setloglevel-recording-java"></a>

### <a name="setLogLevel"></a>Sets the Log Level (setLogLevel)

```
private native void setLogLevel(long vativeHandle, int level)
```

This method sets the log level. Only log levels preceding the selected level are generated. The default value of <code>level</code> is 6.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>level</code></td>
<td>Log levels:<ul>
<li>2: AGORA_LOG_LEVEL_FATAL</li>
<li>3: AGORA_LOG_LEVEL_ERROR</li>
<li>4: AGORA_LOG_LEVEL_WARN</li>
<li>5: AGORA_LOG_LEVEL_NOTICE</li>
<li>6: AGORA_LOG_LEVEL_INFO</li>
<li>7: AGORA_LOG_LEVEL_DEBUG</li>
</ul>
</td>
</tr>
</tbody>
</table>

 <a id="enablelogmodule-recording-java"></a>

### <a name="enableModuleLog"></a>Enables the Module Log (enableModuleLog)

```
private native void enableLogModule(long vativeHandle, int module, int enable)
```

This method enables/disables generating logs for specified modules.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>module</code></td>
	<td>Module:<ul>
<li>0x1: AGORA_LOG_MODULE_MEDIA_FILE </li>
<li>0x2: AGORA_LOG_MDOULE_RECORDING_ENGINE</li>
<li>0x4: AGORA_LOG_MODULE_RTMP_RENDER</li>
<li>0x8: AGORA_LOG_MODULE_IPC</li>
<li>0x10: AGORA_LOG_MODULE_CORE_SERVICE_HANDLER</li>
<li>0: AGORA_LOG_MODULE_ANY</li>
</ul>
</td>
</tr>
<tr><td><code>enable</code></td>
<td>Whether or not to generate the module logs:<ul>
<li>true: Generate the module logs.</li>
<li>false: Do not generate the module logs.</li>
</ul>
</td>
</tr>
</tbody>
</table>



## <a name="RecordingEventHandler"></a>RecordingEventHandler

The RecordingEventHandler class enables the following callbacks to your app: 

-   [Returns the JNI Instance (nativeObjectRef)](#nativeObjectRef)
-   [Reports an Error During SDK Runtime (onError)](#onError)
-   [Reports a Warning During SDK Runtime (onWarning)](#onWarning)
-   [Occurs when the Recording App Leaves the Channel (onLeaveChannel)](#onLeaveChannel)
-   [Occurs when a Remote User or Host Joins the Channel (onUserJoined)](#onUserJoined)
-   [Occurs when a User Leaves the Channel or Goes Offline (onUserOffline)](#onUserOffline)
-   [Occurs when the Raw Audio Data is Received (audioFrameReceived)](#audioFrameReceived)
-   [Occurs when the Raw Video Data is Received (videoFrameReceived)](#videoFrameReceived)
-   [Occurs when a Speaker is Detected in the Channel (onActiveSpeaker)](#onActiveSpeaker)


### <a name="nativeObjectRef"></a>Returns the JNI Instance (nativeObjectRef)

This callback returns the JNI instance. You need to pass this JNI instance when calling each main method, except [createChannel](#createChannel).

```
private void nativeObjectRef(long nativeHandle){
  //your code
}
```

### <a name="onError"></a>Reports an Error During SDK Runtime (onError)

The SDK triggers this callback when an error occurs during SDK runtime.

The SDK cannot fix the issue or resume running, which requires intervention from the app and informs the user on the issue.

```
private void onError(int error, int stat_code) {
  //your code
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>error</code></td>
<td><p>Error codes:</p>
<div><ul>
<li>ERR_OK = 0: No error.</li>
<li>ERR_FAILED = 1: General error with no classified reason.</li>
<li>ERR_INVALID_ARGUMENT = 2: Invalid parameter is called. For example, the specific channel name contains illegal characters.</li>
<li>ERR_NOT_READY = 3: The SDK module is not ready. Agora recommends the following methods to solve this error:
<ul>
<li>Check the audio device.</li>
<li>Check the completeness of the app.</li>
<li>Re-initialize the SDK.</li>
</ul></li>
</ul>
</div>
</td>
</tr>
<tr><td><code>stat_code</code></td>
<td><p>State codes:</p>
<div><ul>
<li>STAT_ERR_FROM_ENGINE = 1: Error from the engine.</li>
<li>STAT_ERR_ARS_JOIN_CHANNEL = 2: Failure to join the channel.</li>
<li>STAT_ERR_CREATE_PROCESS = 3: Failure to create a process.</li>
<li>STAT_ERR_MIXED_INVALID_VIDEO_PARAM = 4: Failure to mix the video.</li>
<li>STAT_ERR_NULL_POINTER = 5: Null pointer.</li>
<li>STAT_ERR_PROXY_SERVER_INVALID_PARAM = 6: Invalid parameters of the proxy server.</li>
<li>STAT_POLL_ERR = 0x8: Error in polling.</li>
<li>STAT_POLL_HANG_UP = 0x10: Polling hangs up.</li>
<li>STAT_POLL_NVAL = 0x20: Invalid polling request.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



### <a name="onWarning"></a>Reports a Warning During SDK Runtime (onWarning)

The SDK triggers this callback when a warning occurs during SDK runtime.

In most cases, the app can ignore the warnings reported by the SDK because the SDK can usually fix the issue and resume running.

```
public void onWarning(int warn) {
//your code
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>warn</code></td>
<td><p>Warning codes:</p>
<ul>
<li>WARN_NO_AVAILABLE_CHANNEL = 103: No channel resources are available. Maybe because the server cannot allocate any channel resource.</li>
<li>WARN_LOOKUP_CHANNEL_TIMEOUT = 104: A timeout occurs when looking up the channel. When joining a channel, the SDK looks up the specified channel. This warning usually occurs when the network conditions are too poor to connect to the server.</li>
<li>WARN_LOOKUP_CHANNEL_REJECTED = 105: The server rejects the request to look up the channel. The server cannot process this request or the request is illegal.</li>
<li>WARN_OPEN_CHANNEL_TIMEOUT = 106: A timeout occurs when opening the channel. Once the specific channel is found, the SDK opens the channel. This warning usually occurs when the network condition is too poor to connect to the server.</li>
<li>WARN_OPEN_CHANNEL_REJECTED = 107: The server rejects the request to open the channel. The server cannot process this request or the request is illegal.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onLeaveChannel"></a>Occurs when the Recording App Leave the Channel (onLeaveChannel)

```
private void onLeaveChannel(int reason){
  // your code
}
```

The SDK triggers this callback when the recording app leaves the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>code</code></td>
<td>
<div>Reason:</div>
<ul>
<li>LEAVE_CODE_INIT = 0: Initialization fails.</li>
<li>LEAVE_CODE_SIG= 1: Signal triggered exit.</li>
<li>LEAVE_CODE_NO_USERS = 2: There is no user in the channel except for the recording app.</li>
<li>LEAVE_CODE_TIMER_CATCH = 3: Timer catch exit.</li>
<li>LEAVE_CODE_CLIENT_LEAVE = 4: The client leaves the channel.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onUserJoined"></a>Occurs when a Remote User or Host Joins the Channel (onUserJoined)

The SDK triggers this callback when another user joins the channel and returns the uid of the new user.

If there are other users in the channel before the recording app joins the channel, the SDK also reports on the uids of the existing users. The SDK triggers this callback as many times as the number of the users in the channel.

```
private void onUserJoined(long uid, String recordingDir){
  //your code
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID of the user.</td>
</tr>
<tr><td><code>recordingDir</code></td>
<td>Directory of the recorded files.</td>
</tr>
</tbody>
</table>



### <a name="onUserOffline"></a>Occurs when a User Leaves the Channel or Goes Offline (onUserOffline)

The SDK triggers this callback when a user leaves the channel or goes offline.

When no data package of a user is received for a certain period of time (15 seconds), the SDK assumes that the user has gone offline. Weak network connections may lead to misinformation, so Agora recommends using the signaling system for offline event detection.

```
private void onUserOffline(long uid, int reason) {
  //your code
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID of the user.</td>
</tr>
<tr><td><code>reason</code></td>
<td><p>Reason:</p>
<ul>
<li>USER_OFFLINE_QUIT = 0: The user has quit the call.</li>
<li>USER_OFFLINE_DROPPED = 1: The SDK timed out and the user dropped offline because it has not received any data packet for a period of time. If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the user has dropped offline.</li>
<li>USER_OFFLINE_BECOME_AUDIENCE = 2: The client role has changed from the host to the audience. This option is only valid when you set the channel profile as live broadcast when calling <code>joinChannel</code>.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="audioFrameReceived"></a>Occurs when the Raw Audio Data is Received (audioFrameReceived)

The SDK triggers this callback when the raw audio data is received.

```
private void audioFrameReceived(long uid, int type, AudioFrame frame) {
 //your code
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
<tr><td><code>uid</code></td>
<td>User ID of the remote user.</td>
</tr>
</thead>
<tbody>
<tr><td><code>type</code></td>
<td><p>Format of the received raw audio data:</p>
<div><ul>
<li>0: PCM</li>
<li>1: AAC</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>frame</code></td>
<td>Received raw audio data in PCM or AAC format.</td>
</tr>
</tbody>
</table>



The struct of <code>AudioFrame</code>:

```
public class AudioFrame {
  public AUDIO_FRAME_TYPE type;
  public AudioPcmFrame pcm;
  public AudioAacFrame aac;
}
```

#### AudioPcmFrame

The struct of <code>AudioPcmFrame</code>:

```
public class AudioPcmFrame {
  public AudioPcmFrame(long frame_ms, long sample_rates, long samples) {
  }
  public long frame_ms;
  public long channels; // 1
  public long sample_bits; // 16
  public long sample_rates; // 8k, 16k, 32k
  public long samples;

  public ByteBuffer pcmBuf;
  public long pcmBufSize;
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>frame_ms</code></td>
<td>Timestamp of the frame.</td>
</tr>
<tr><td><code>channels</code></td>
<td>Number of audio channels.</td>
</tr>
<tr><td><code>sample_bits</code></td>
<td>Bitrate of the sample data.</td>
</tr>
<tr><td><code>sample_rates</code></td>
<td>Sample rate.</td>
</tr>
<tr><td><code>samples</code></td>
<td>Number of samples of the frame.</td>
</tr>
<tr><td><code>pcmBuf</code></td>
<td>Audio frame buffer.</td>
</tr>
<tr><td><code>pcmBufSize</code></td>
<td>Size of the audio frame buffer.</td>
</tr>
</tbody>
</table>



#### AudioAacFrame

The struct of <code>AudioAacFrame</code>:

```
public class AudioAacFrame {
  public AudioAacFrame(long framems) {
    frame_ms = framems;
    aacBufSize = 0;
  }
  public ByteBuffer aacBuf;
  public long frame_ms;
  public long aacBufSize;
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>frame_ms</code></td>
<td>Timestamp of the frame.</td>
</tr>
<tr><td><code>aacBuf</code></td>
<td>Audio frame buffer.</td>
</tr>
<tr><td><code>aacBufSize</code></td>
<td>Size of the audio frame buffer.</td>
</tr>
</tbody>
</table>



### <a name="videoFrameReceived"></a>Occurs when the Raw Video Data is Received (videoFrameReceived)

The SDK triggers this callback when the raw video data is received. 

The SDK triggers this callback for every received raw video frame and can be used to detect sexually explicit content, if necessary.

Agora recommends capturing the i frame only and neglecting the others.

```
private void videoFrameReceived(long uid, int type, VideoFrame frame, int rotation) {
 //your code
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID of the remote user as specified in the <code>createChannel</code> method. If no uid is previously assigned, the Agora server automatically assigns a uid.</td>
</tr>
<tr><td><code>type</code></td>
<td><p>The format of the received video data:</p>
<div><ul>
<li>0: YUV</li>
<li>1: H.264</li>
<li>2: JPEG</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>frame</code></td>
<td>Received video data in YUV, H264, or JPEG format.</td>
</tr>
<tr><td><code>rotation</code></td>
<td>Rotational angle: 0, 90, 180, or 270.</td>
</tr>
</tbody>
</table>



The struct of <code>VideoFrame</code>:

```
public class VideoFrame {
  public VideoYuvFrame yuv;
  public VideoH264Frame h264;
  public VideoJpgFrame jpg;
  public int rotation; // 0, 90, 180, 270
}
```

#### VideoYuvFrame

The struct of <code>VideoYuvFrame</code>:

```
public class VideoYuvFrame {
  VideoYuvFrame(long framems, int width, int height, int ystride,int ustride, int vstride){
    this.frame_ms = framems;
    this.width = width;
    this.height = height;
    this.ystride = ystride;
    this.ustride = ustride;
    this.vstride = vstride;
  }
  public long frame_ms;

  public ByteBuffer ybuf;
  public ByteBuffer ubuf;
  public ByteBuffer vbuf;

  public int width;
  public int height;

  public int ystride;
  public int ustride;
  public int vstride;
  //all
  public ByteBuffer buf;
  public long bufSize;
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>frame_ms</code></td>
<td>Timestamp of the frame.</td>
</tr>
<tr><td><code>ybuf</code></td>
<td>Y buffer pointer.</td>
</tr>
<tr><td><code>ubuf</code></td>
<td>U buffer pointer.</td>
</tr>
<tr><td><code>vbuf</code></td>
<td>V buffer pointer.</td>
</tr>
<tr><td><code>width</code></td>
<td>Width of the video in the number of pixels.</td>
</tr>
<tr><td><code>height</code></td>
<td>Height of the video in the number of pixels.</td>
</tr>
<tr><td><code>ystride</code></td>
<td>Line span of the Y buffer.</td>
</tr>
<tr><td><code>ustride</code></td>
<td>Line span of the U buffer.</td>
</tr>
<tr><td><code>vstride</code></td>
<td>Line span of the V buffer.</td>
</tr>
<tr><td><code>buf</code></td>
<td>Video frame buffer.</td>
</tr>
<tr><td><code>bufSize</code></td>
<td>Size of the video frame buffer.</td>
</tr>
</tbody>
</table>



#### VideoH264Frame

The struct of <code>VideoH264Frame</code>:

```
public class VideoH264Frame {
  VideoH264Frame(){
    frame_ms = 0;
    frame_num = 0;
    bufSize = 0;
  }
  public long frame_ms;
  public long frame_num;
  public ByteBuffer buf;
  public long bufSize;
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>frame_ms</code></td>
<td>Timestamp of the frame.</td>
</tr>
<tr><td><code>frame_num</code></td>
<td>Index of the frame.</td>
</tr>
<tr><td><code>buf</code></td>
<td>Video frame buffer.</td>
</tr>
<tr><td><code>bufSize</code></td>
<td>Size of the video frame buffer.</td>
</tr>
</tbody>
</table>



#### VideoJpgFrame

The struct of <code>VideoJpgFrame</code>:

```
public class VideoJpgFrame {
  VideoJpgFrame(){
    frame_ms = 0;
    bufSize = 0;
  }
  public long frame_ms;
  public ByteBuffer buf;
  public long bufSize;
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>frame_ms</code></td>
<td>Timestamp of the frame.</td>
</tr>
<tr><td><code>buf</code></td>
<td>Video frame buffer.</td>
</tr>
<tr><td><code>bufSize</code></td>
<td>Size of the video frame buffer.</td>
</tr>
</tbody>
</table>



### <a name="onActiveSpeaker"></a>Occurs when a Speaker is Detected in the Channel (onActiveSpeaker)

```
void onActiveSpeaker(long uid){
 //your code
}
```

This callback returns the user ID of the active speaker.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>The user ID of the active speaker.</td>
</tr>
</tbody>
</table>



## Error Codes and Warning Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_native.md).











