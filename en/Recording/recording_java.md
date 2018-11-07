
---
title: Recording API 
description: 
platform: Java
updatedAt: Wed Nov 07 2018 15:07:02 GMT+0000 (UTC)
---
# Recording API 
> Version: v2.2.3
> This Java API is a data encapsulation of the C++ sample code with JNI, and is therefore slightly different from that of the C++ API in structure. The Agora SDK (sample code shared by C++ and Java) implements the C++ recording API methods and callbacks, goes through data encapsulation in the JNI layer, and then works as the Java interface and class of the Native SDK through the JNI proxy.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
	<tr><th>Interface</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><a href="#native-interface">Native Interface</a></td>
<td>Main methods that can be invoked by your app.</td>
</tr>
<tr><td><a href="#callback-functions">Callbacks</a></td>
<td>Callbacks returned to your app.</td>
</tr>
</tbody>
</table>

## <a name="native-interface"></a>Native Interface

The Native Interface provides main methods that can be invoked by your app.

-   [Creates a Channel (createChannel)](#createChannel)

-   [Sets the Video Mixing Layout (setVideoMixingLayout)](#setVideoMixingLayout)

-   [Allows the App to Leave the Channel (leaveChannel)](#leaveChannel)

-   [Starts the Recording (startService)](#startService)

-   [Stops the Recording (stopService)](#stopService)

-   [Retrieves the Recording Properties (getProperties)](#getProperties)

-   [Sets the User Background Image (setUserBackground)](#setUserBackground)

-   [Sets the Log Level (setLogLevel)](#setLogLevel)

-   [Enables the Module Log (enableModuleLog)](#enableModuleLog)


### <a name="createChannel"></a>Creates a Channel (createChannel)

This method creates a channel and enables the app to join the channel.

```
public native boolean createChannel(String appId, String token, String name, int uid, RecordingConfig config, int logLevel, int logModules);
```

> -   In the Recording SDK, <code>requestToken</code> and <code>renewToken</code> are private interfaces. Make sure that you set <code>expireTimestamp</code> as 0 when generating a token, which means that the privilege, once generated, never expires.
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
<tr><td><code>isAudioOnly</code></td>
<td><p>Sets whether or not to record audio only:</p>
<ul>
<li>true: Enable audio recording only.</li>
<li>false: (Default) Record both audio and video.</li>
</ul>
</td>
</tr>
<tr><td><code>isVideoOnly</code></td>
<td><p>Sets whether or not to record video only:</p>
<ul>
<li>true: Enable video recording only.</li>
<li>false: (Default) Record both audio and video.</li>
</ul>
</td>
</tr>
<tr><td><code>isMixingEnabled</code></td>
<td><p>Enables the audio- or video-mixing mode:</p>
<ul>
<li>false: (Default) Enable the individual mode (audio). The bitrate and audio channel number of the recording file are the same as those of the original audio stream.</li>
<li>true: Enable the composite mode (video). The sample rate, bitrate, and audio channel number of the recording file are the same as the highest level of those of the original audio streams.</li>
</ul>
<p>If the composite mode is enabled:</p>
<div><ul>
<li>If <code>isAudioOnly</code> is true and <code>isVideoOnly</code> is false, only the audio is recorded.</li>
<li>If <code>isAudioOnly</code> is false and <code>isVideoOnly</code> is true, only the video is recorded.</li>
<li>If both <code>isAudioOnly</code> and <code>isVideoOnly</code> are false, voice and video mixing are enabled (the audio and video of all uids are recorded respectively).</li>
<li><code>isVideoOnly</code> and <code>isVideoOnly</code> cannot be set as <code>true</code> at the same time.</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>mixedVideoAudio</code></td>
<td><p>If you have set <code>isMixingEnabled</code> as <code>true</code>, <code>mixedVideoAudio</code> allows you to mix the audio and video in real time:</p>
<ul>
<li>0: (Default) Mix the audio and video respectively.</li>
<li>1: Mix the audio and video in real time into an MPEG-4 file. Supports limited players.</li>
<li>2: Mix the audio and video in real time into an MPEG-4 file. Supports more players.</li>
</ul>
</td>
</tr>
	<tr><td><code>mixResolution</code> <sup>[1]</sup></td>
<td>If you have <code>isMixingEnabled</code> as <code>true</code>, <code>mixResolution</code> sets the resolution in the format: width, height, fps, and Kbps; representing the width, height, frame rate, and bitrate of the video stream.</td>
</tr>
<tr><td><code>decryptionMode</code></td>
<td><p>When the whole channel is encrypted, the recording SDK uses <code>decryptionMode</code> to enable the built-in decryption function:</p>
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
<td>The root directory of the recording files. The default value is NULL. Do not set <code>recordFileRootDir</code> and <code>cfgFilePath</code> at the same time.</td>
</tr>
<tr><td><code>cfgFilePath</code></td>
<td>The path of the configuration file. The default value is NULL. In this configuration file, you can set the absolute path of the recording file, and the content in the configuration file must be in JSON format. Do not set <code>recordFileRootDir</code> and <code>cfgFilePath</code> at the same time.
For example, {“Recording_Dir” :”&lt;recording path&gt;”}, where <code>Recording_Dir</code> is fixed.</td>
</tr>
<tr><td><code>decodeAudio</code> <sup>[2]</sup></td>
<td><p>Audio Decoding Format:</p><ul>
<li>AUDIO_FORMAT_DEFAULT_TYPE = 0: Default audio format.</li>
<li>AUDIO_FORMAT_AAC_FRAME_TYPE = 1: AAC format.</li>
<li>AUDIO_FORMAT_PCM_FRAME_TYPE = 2: PCM format.</li>
<li>AUDIO_FORMAT_MIXED_PCM_FRAME_TYPE = 3: PCM audio-mixing format.</li>
</ul>
</td>
</tr>
<tr><td><code>decodeVideo</code> <sup>[2]</sup></td>
<td><p>Video Decoding Format:</p><ul>
<li>VIDEO_FORMAT_DEFAULT_TYPE = 0: Default video format.</li>
<li>VIDEO_FORMAT_H264_FRAME_TYPE = 1: H.264 format.</li>
<li>VIDEO_FORMAT_YUV_FRAME_TYPE = 2: YUV format.</li>
<li>VIDEO_FORMAT_JPG_FRAME_TYPE = 3: JPEG format.</li>
<li>VIDEO_FORMAT_JPG_FILE_TYPE = 4: JPEG file format.</li>
<li>VIDEO_FORMAT_JPG_VIDEO_FILE_TYPE = 5: JPEG video file format.</li>
</ul>
<div><ul>
<li>Individual Mode (isMixingEnabled=false): MPEG-4 video and JPEG files are supported.</li>
<li>Mixing Mode (isMixingEnabled=true): MPEG-4 video files for combined streams and JPEG files for individual streams are supported.</li>
</ul>
</div>
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
<li>&le; 0: Disables detecting the speakers.</li>
<li>&gt; 0: The time interval (ms) of detecting the speakers. When an active speaker is found, the SDK returns the user ID of the speaker in the <code>onActiveSpeaker</code> callback.</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>channelProfile</code></td>
<td><p>Sets the channel mode:</p>
<ul>
<li>CHANNEL_PROFILE_COMMUNICATION: (Default) Communication mode. This is used in one-on-one or group calls, where all users in the channel can talk freely.</li>
<li>CHANNEL_PROFILE_LIVE_BROADCAST: Live broadcast. The host sends and receives voice/video, while the audience only receives voice/video. Host and audience roles can be set by calling <code>setClientRole</code>. </li>
</ul>
</td>
</tr>
<tr><td><code>streamType</code></td>
<td><code>streamType</code> takes effect only when the Agora Native SDK has enabled the dual-stream mode (high stream by default).</td>
</tr>
<tr><td><code>triggerMode</code></td>
<td><p>Sets whether to record automatically or manually upon joining the channel:</p>
<ul>
<li>0: Automatically</li>
<li>1: Manually</li>
</ul>
	<p> If you wish to call <code>startRecording</code> and <code>stopRecording</code>, then choose <code>Manually</code>.
</td>
</tr>
<tr><td><code>proxyserver</code></td>
<td><code>proxyserver</code> allows you to record the content with the Intranet server. For details, please contact <a href="mailto:sales%40agora.io">sales@agora.io</a>.</td>
</tr>
<tr><td><code>audioProfile</code> </td>
<td><p>Audio profile of the recording file:</p>
<div><ul>
<li>AUDIO_PROFILE_DEFAULT = 0: (Default) Sampling rate of 48 kHz, communication encoding, mono.</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY = 4: Sampling rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps.</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO = 5: Sampling rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps.</li>
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

> [1] The <code>isAudioOnly</code> and <code>isVideoOnly</code> parameters are disabled by default. Do not set <code>isAudioOnly</code> and <code>isVideoOnly</code> as <code>true</code> at the same time.

> [2] If the raw data is enabled:
	> - Only audio mixing is supported. Video mixing is not supported. 
	> - Only raw video data in H.264 for the Web SDK is supported. VP8 is not supported.

**Video Profile for the Communication Mode:**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Resolution</th>
<th>Frame Rate (fps)</th>
<th>Minimum Bitrate (Kbps)</th>
<th>Maximum Bitrate (Kbps)</th>
<th>Recommended Bitrate (Kbps)</th>
</tr>
</thead>
<tbody>
<tr><td>3840 &times; 2160</td>
<td>15</td>
<td>3000</td>
<td>9000</td>
<td>6000</td>
</tr>
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



**Video Profile for the Live Broadcast Mode:**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Resolution</th>
<th>Frame Rate (fps)</th>
<th>Minimum Bitrate (Kbps)</th>
<th>Maximum Bitrate (Kbps)</th>
<th>Recommended Bitrate (Kbps)</th>
</tr>
</thead>
<tbody>
<tr><td>3840 &times; 2160</td>
<td>15</td>
<td>6000</td>
<td>18000</td>
<td>12000</td>
</tr>
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



**Supported Players:**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Platform</th>
<th>Player/Explorer</th>
<th>mixedVideoAudio=0</th>
<th>mixedVideoAudio=1</th>
</tr>
</thead>
<tbody>
<tr><td>Linux</td>
<td>Default Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Linux</td>
<td>VLC Media Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Linux</td>
<td>ffplay</td>
<td>Supported</td>
<td>Supported</td>
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
<td>Chrome (49.0.2623+)</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>QuickTime Player</td>
<td>Supported</td>
<td>Supported</td>
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
<th>Not Supported</th>
<th>Not Supported</th>
</tr>
<tr><td>macOS</td>
<td>Chrome (47.0.2526.111+)</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>macOS</td>
<td>Safari (11.0.3+)</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>Default Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>VLC</td>
<th>Not Supported</th>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>KMPlayer</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>iOS</td>
<td>Safari (9.0+)</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>Default Player</td>
<td>Supported</td>
<td>Supported</td>
</tr>
<tr><td>Android</td>
<td>MXPlayer</td>
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
<td>Chrome (49.0.2623+)</td>
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
    //[0, 1.0] where 0 denotes thoroughly transparent, 1.0 opaque
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
<td>The number of the hosts in the channel.</td>
</tr>
<tr><td><code>regions</code></td>
<td><p>The host list of <code>VideoMixingLayout</code>. Each host in the channel has a region to display the video on the screen with the following parameters to be set:</p>
<ul>
<li><code>uid</code>: User ID of the host displaying the video in the region.</li>
<li><code>x</code>: Relative horizontal position of the top-left corner of the region. The value is between 0.0 and 1.0.</li>
<li><code>y</code>: Relative vertical position of the top-left corner of the region. The value is between 0.0 and 1.0.</li>
<li><code>width</code>: Relative width of the region. The value is between 0.0 and 1.0.</li>
<li><code>height</code>: Actual height of the region. The value is between 0.0 and 1.0.</li>
<li><code>zOrder</code>: The index of the layer. The value is between 1 (bottom layer) and 100 (top layer).</li>
<li><code>alpha</code>: The transparency of the image. The value is between 0.0 (transparent) and 1.0 (opaque).</li>
<li><code>renderMode</code>: Render mode:<ul>
<li>RENDER_MODE_HIDDEN(1): Cropped.</li>
<li>RENDER_MODE_FIT(2): Proportionate.</li>
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


### <a name="leaveChannel"></a>Allows the App to Leave the Channel (leaveChannel)

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



### <a name="getProperties"></a>Retrieves the Properties (getProperties)

This method allows you to retrieve the recording properties without joining a channel.

> -   The recording properties only include the path of the recording files.
> -   This method is different from [onUserJoined](#onUserJoined). You must call <code>onUserJoined</code> after joining the channel.


```
private native RecordingEngineProperties getProperties(long nativeHandle);
```

### <a name="startService"></a>Starts a Recording (startService)

This method manually starts a recording.

The method is only valid when you set <code>triggerMode</code> to <code>Manually</code> when joining the channel. For more information, see [Creates a Channel (createChannel)](#createChannel) on <code>triggerMode</code>.

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



### <a name="stopService"></a>Stops the Recording (stopService)

This method manually stops the recording.

The method is only valid when you set <code>triggerMode</code> to <code>Manually</code> when joining the channel. For more information, see [Create a Channel (createChannel)](#createChannel) on <code>triggerMode</code>.

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
<li>AGORA_LOG_LEVEL_FATAL = 2</li>
<li>AGORA_LOG_LEVEL_ERROR = 3</li>
<li>AGORA_LOG_LEVEL_WARN = 4</li>
<li>AGORA_LOG_LEVEL_NOTICE = 5</li>
<li>AGORA_LOG_LEVEL_INFO = 6</li>
<li>AGORA_LOG_LEVEL_DEBUG = 7</li>
</ul>
</td>
</tr>
</tbody>
</table>



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
<li>AGORA_LOG_MODULE_MEDIA_FILE = 0x1</li>
<li>AGORA_LOG_MDOULE_RECORDING_ENGINE = 02</li>
<li>AGORA_LOG_MODULE_RTMP_RENDER = 0x4</li>
<li>AGORA_LOG_MODULE_IPC = 0x8</li>
<li>AGORA_LOG_MODULE_CORE_SERVGICE_HANDLER = 0x10</li>
<li>AGORA_LOG_MODULE_ANY = ~0</li>
</ul>
</td>
</tr>
<tr><td><code>enable</code></td>
<td>Whether or not to generate the module log:<ul>
<li>true: Generate the module log.</li>
<li>false: Do not generate the module log.</li>
</ul>
</td>
</tr>
</tbody>
</table>



## <a name="callback-functions"></a>Callbacks

The following callbacks are available to your app:

-   [Returns the JNI Instance (nativeObjectRef)](#nativeObjectRef)
-   [An Error has Occurred During SDK Runtime (onError)](#onError)
-   [A Warning has Occurred During SDK Runtime (onWarning)](#onWarning)
-   [The User has Left the Channel (onLeaveChannel)](#onLeaveChannel)
-   [The User/Host has Joined the Channel (onUserJoined)](#onUserJoined)
-   [A User has Left the Channel or Gone Offline (onUserOffline)](#onUserOffline)
-   [The Raw Audio Data has Been Received (audioFrameReceived)](#audioFrameReceived)
-   [The Raw Video Data has Been Received (videoFrameReceived)](#videoFrameReceived)
-   [The User who is Speaking in the Channel (onActiveSpeaker)](#onActiveSpeaker)


### <a name="nativeObjectRef"></a>Returns the JNI Instance (nativeObjectRef)

This callback returns the JNI instance. You need to pass this JNI instance when calling each main method, except [Creates a Channel (createChannel)](#createChannel).

```
private void nativeObjectRef(long nativeHandle){
  //your code
}
```

### <a name="onError"></a>An Error has Occurred During SDK Runtime (onError)

This callback is triggered when an error has occurred during SDK runtime.

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
<li>ERR_OK(0): No error.</li>
<li>ERR_FAILED(1): General error (no classified reason).</li>
<li>ERR_INVALID_ARGUMENT(2): Invalid parameter called. For example, the specific channel name contains illegal characters.</li>
<li>ERR_NOT_READY3): The SDK module is not ready. Agora recommends the following methods to solve this error:
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



### <a name="onWarning"></a>A Warning has Occurred During SDK Runtime (onWarning)

This callback is triggered when a warning has occurred during SDK runtime.

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
<li>WARN_LOOKUP_CHANNEL_TIMEOUT = 104: A timeout when looking up the channel. When joining a channel, the SDK looks up the specified channel. This warning usually occurs when the network conditions are too poor to connect to the server.</li>
<li>WARN_LOOKUP_CHANNEL_REJECTED = 105: The server rejected the request to look up the channel. The server cannot process this request or the request is illegal.</li>
<li>WARN_OPEN_CHANNEL_TIMEOUT = 106: A timeout occurred when opening the channel. Once the specific channel is found, the SDK opens the channel. This warning usually occurs when the network condition is too poor to connect to the server.</li>
<li>WARN_OPEN_CHANNEL_REJECTED = 107: The server rejected the request to open the channel. The server cannot process this request or the request is illegal.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onLeaveChannel"></a>The User has Left the Channel (onLeaveChannel)

```
private void onLeaveChannel(int reason){
  // your code
}
```

This callback is triggered when a user has left the channel.

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
<li>LEAVE_CODE_INIT(0): Initialization failure</li>
<li>LEAVE_CODE_SIG(1&lt;&lt;1): Signal triggered exit</li>
<li>LEAVE_CODE_NO_USERS(1&lt;&lt;2): There is no user in the channel except for the recording app.</li>
<li>LEAVE_CODE_TIMER_CATCH(1&lt;&lt;3): Timer catch exit</li>
<li>LEAVE_CODE_CLIENT_LEAVE(1&lt;&lt;4): The client leaves the channel.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onUserJoined"></a>The User/Host has Joined the Channel (onUserJoined)

This callback is triggered when another user has joined the channel.

If other users are already in the channel, the SDK reports to the app on the existing users as well. This callback is called as many times as the number of users in the channel.

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



### <a name="onUserOffline"></a>A User has Left the Channel or Gone Offline (onUserOffline)

This callback is triggered when a user has left the channel or gone offline.

The SDK reads the timeout data to determine if a user has left the channel (or has gone offline). If no data package is received from the user within 15 seconds, the SDK assumes the user is offline. A poor network connection may lead to false detections, so use signaling for reliable offline detection.

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
<li>USER_OFFLINE_DROPPED = 1: The SDK timed out and the user dropped offline because it has not received any data packet for a period of time. If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the user has dropped offline.
    </li>
<li>USER_OFFLINE_BECOME_AUDIENCE = 2: The client role has changed from the host to the audience. The option is only valid when you set the channel profile as live broadcast when calling <code>joinChannel</code>.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="audioFrameReceived"></a>The Raw Audio Data has Been Received (audioFrameReceived)

This callback is triggered when the raw audio data has been received.

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
<td>User ID of the user.</td>
</tr>
</thead>
<tbody>
<tr><td><code>type</code></td>
<td><p>Format of the received raw audio data:</p>
<div><ul>
<li>pcm (0)</li>
<li>aac (1)</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>frame</code></td>
<td>Received raw audio data in PCM or AAC format.</td>
</tr>
</tbody>
</table>



The structure of <code>AudioFrame</code>:

```
public class AudioFrame {
  public AUDIO_FRAME_TYPE type;
  public AudioPcmFrame pcm;
  public AudioAacFrame aac;
}
```

#### AudioPcmFrame

The structure of <code>AudioPcmFrame</code>:

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
<td>Bitrate of the sampling data.</td>
</tr>
<tr><td><code>sample_rates</code></td>
<td>Sampling rate.</td>
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

The structure of <code>AudioAacFrame</code>:

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



### <a name="videoFrameReceived"></a>The Raw Video Data has Been Received (videoFrameReceived)

This callback is triggered when the raw video data has been received. 

The callbacks are available for every frame of the video and this callback can be used to detect sexually explicit content, if necessary.

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
<td>User ID as specified in the <code>createChannel()</code> method. If no uid is previously assigned, the Agora server automatically assigns a uid.</td>
</tr>
<tr><td><code>type</code></td>
<td><p>The format of the received video data:</p>
<div><ul>
<li>yuv(0)</li>
<li>h.264(1)</li>
<li>jpg(2)</li>
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



The structure of <code>VideoFrame</code>:

```
public class VideoFrame {
  public VideoYuvFrame yuv;
  public VideoH264Frame h264;
  public VideoJpgFrame jpg;
  public int rotation; // 0, 90, 180, 270
}
```

#### VideoYuvFrame

The structure of <code>VideoYuvFrame</code>:

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

The structure of <code>VideoH264Frame</code>:

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

The structure of <code>VideoJpgFrame</code>:

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



### <a name="onActiveSpeaker"></a>Indicates the Speaker in the Channel (onActiveSpeaker)

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











