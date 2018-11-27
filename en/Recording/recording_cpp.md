
---
title: Recording API
description: 
platform: CPP
updatedAt: Tue Nov 27 2018 07:04:27 GMT+0000 (UTC)
---
# Recording API
> Version: v2.2.2

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>C++ Interface Class</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr><td><a href="#irecordingengine">IRecordingEngine</a></td>
<td>The <code>IRecordingEngine</code> class provides the main methods that can be invoked by your application.</td>
</tr>
<tr><td><a href="#irecordingengineeventhandler">IRecordingEngineEventHandler</a></td>
<td>The <code>IRecordingEngineEventHandler</code> class enables callbacks to your application.</td>
</tr>
</tbody>
</table>



## <a name="irecordingengine"></a>IRecordingEngine

The <code>IRecordingEngine</code> class provides the following main methods that can be invoked by your application:

-   [Creates a Recording Engine (createAgoraRecordingEngine)](#createAgoraRecordingEngine)
-   [Allows an Application to Join a Channel (joinChannel)](#joinChannel)
-   [Sets the Video Mixing Layout (setVideoMixingLayout)](#setVideoMixingLayout)
-   [Allows the Application to Leave the Channel (leaveChannel)](#leaveChannel)
-   [Releases the IRecordingEngine Object (release)](#release)
-   [Retrieves the Recording Properties (getProperties)](#getProperties)
-   [Starts the Recording (startService)](#startService)
-    [Stops the Recording (stopService)](#stopService))
-    [Sets the User Background Image (setUserBackground)](#setUserBackground))
-   [Sets the Log Level (setLogLevel)](#setLogLevel)
-   [Enables the Module Log (enableModuleLog)](#enableModuleLog)


### <a name="createAgoraRecordingEngine"></a>Creates a Recording Engine (createAgoraRecordingEngine)

This method creates the Agora recording engine object.

```
public static IRecordingEngine* createAgoraRecordingEngine(const char * appId, IRecordingEngineEventHandler *eventHandler);
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
<tr><td><code>appId</code></td>
<td>The App ID used in the communications to be recorded. For details, see <a href="../../en/Agora%20Platform/token.md">Getting an App ID</a>.</td>
</tr>
<tr><td><code>eventHandler</code></td>
<td>The Agora Recording SDK notifies the application of the triggered callbacks found in <a href="#irecordingengineeventhandler">IRecordingEngineEventHandler</a>.</td>
</tr>
</tbody>
</table>



### <a name="joinChannel"></a>Allows an Application to Join a Channel (joinChannel)

This method allows the recording application to join a channel and start recording.

```
virtual int joinChannel(const char * token, const char *channelId, uid_t uid, const RecordingConfig &config) = 0;
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
</body>
<tr><td><code>token</code></td>
<td>The token used in the communications to be recorded. For details, see <a href="../../en/Agora%20Platform/token.md">Getting an App ID</a>.</td>
</tr>
<tr><td><code>channelId</code></td>
<td>Name of the channel to be recorded.</td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID. A 32-bit unsigned integer ranging from 1 to (2^32-1) that is unique in a channel.</td>
</tr>
<tr><td><code>config</code></td>
<td>Detailed recording configuration. See the definition in the table below.</td>
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

> -   In the Recording SDK, <code>requestToken</code> and <code>renewToken</code> are private methods. Make sure that you set <code>expireTimestamp</code> as 0 when generating a token, which means that the privilege, once generated, never expires.
> -   A channel does not accept duplicate uids. Otherwise, there will be unpredictable behaviors.


The structure of <code>RecordingConfig</code>:

```
typedef struct RecordingConfig {
    bool isAudioOnly;
    bool isVideoOnly;
    bool isMixingEnabled;
    agora::linuxsdk::MIXED_AV_CODEC_TYPE mixedVideoAudio;
    char * mixResolution;
    char * decryptionMode;
    char * secret;
    char * appliteDir;
    char * recordFileRootDir;
    char * cfgFilePath;
    agora::linuxsdk::VIDEO_FORMAT_TYPE decodeVideo;
    agora::linuxsdk::AUDIO_FORMAT_TYPE decodeAudio;
    int lowUdpPort;
    int highUdpPort;
    int idleLimitSec;
    int captureInterval;
    int audioIndicationInterval;
    agora::linuxsdk::CHANNEL_PROFILE_TYPE channelProfile;
    agora::linuxsdk::REMOTE_VIDEO_STREAM_TYPE streamType;
    agora::linuxsdk::TRIGGER_MODE_TYPE triggerMode;
    agora::linuxsdk::LANGUAGE_TYPE lang;
    char * proxyServer;
    agora::linuxsdk::AUDIO_PROFILE_TYPE audioProfile;
    char * defaultVideoBg;
    char * defaultUserBg;
    agora::linuxsdk::avsyncType avSyncMode;

    RecordingConfig(): channelProfile(agora::linuxsdk::CHANNEL_PROFILE_COMMUNICATION),
        isAudioOnly(false),
        isVideoOnly(false),
        isMixingEnabled(false),
        mixResolution(NULL),
        decryptionMode(NULL),
        secret(NULL),
        idleLimitSec(300),
        appliteDir(NULL),
        recordFileRootDir(NULL),
        cfgFilePath(NULL),
        lowUdpPort(0),
        highUdpPort(0),
        captureInterval(5),
        audioIndicationInterval(0),
        decodeAudio(agora::linuxsdk::AUDIO_FORMAT_DEFAULT_TYPE),
        decodeVideo(agora::linuxsdk::VIDEO_FORMAT_DEFAULT_TYPE),
        mixedVideoAudio(agora::linuxsdk::MIXED_AV_DEFAULT),
        streamType(agora::linuxsdk::REMOTE_VIDEO_STREAM_HIGH),
        triggerMode(agora::linuxsdk::AUTOMATICALLY_MODE),
        lang(agora::linuxsdk::CPP_LANG),
        proxyServer(NULL),
        audioProfile(agora::linuxsdk::AUDIO_PROFILE_DEFAULT),
        defaultVideoBg(NULL),
        defaultUserBg(NULL),
        avSyncMode(agora::linuxsdk::avsyncType::AVSYNC_V0)
    {}

    virtual ~RecordingConfig() {}
 } RecordingConfig;
```

<table>
<caption>Recording Configuration</caption>
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
<tr><td><code>channelProfile</code></td>
<td><p>Sets the channel mode:</p>
<ul>
<li>0: (Default) Communication mode (CHANNEL_PROFILE_COMMUNICATION). This is used in one-on-one or group calls, where all users in the channel can talk freely.</li>
<li>1: Live broadcast (CHANNEL_PROFILE_LIVE_BROADCAST). The host sends and receives voice/video, while the audience only receives voice/video. Host and audience roles can be set by calling <code>setClientRole</code>. </li>
</ul>
The Recording SDK must use the same channel profile as the Agora Native/Web SDK, otherwise issues may occur.
</td>
</tr>
<tr><td><code>isAudioOnly</code></td>
<td><p>Sets whether or not to record audio only:</p>
<ul>
<li>true: Enables audio recording only.</li>
<li>false: (Default) Records both audio and video.</li>
</ul>
</td>
</tr>
<tr><td><code>isVideoOnly</code></td>
<td><p>Sets whether or not to record video only:</p>
<ul>
<li>true: Enables video recording only.</li>
<li>false: (Default) Records both audio and video.</li>
</ul>
</td>
</tr>
<tr><td><code>isMixingEnabled</code></td>
<td><p>Enables the audio- or video-mixing mode:</p>
<ul>
<li>false: (Default) Enables the individual mode (audio). The bitrate and audio channel number of the recording file are the same as those of the original audio stream.</li>
<li>true: Enables the composite mode (video). The sample rate, bitrate, and audio channel number of the recording file are the same as the highest level of those of the original audio streams.</li>
</ul>
<p>If the composite mode is enabled:</p>
<div><ul>
<li>If <code>isAudioOnly</code> is <code>true</code> and <code>isVideoOnly</code> is <code>false</code>, only the audio is recorded.</li>
<li>If <code>isAudioOnly</code> is <code>false</code> and <code>isVideoOnly</code> is <code>true</code>,  only the video is recorded.</li>
<li>If both <code>isAudioOnly</code> and <code>isVideoOnly</code> are <code>false</code>, voice and video mixing are enabled (the audio and video of all uids are recorded respectively).</li>
	<li><code>isVideoOnly</code> and <code>isVideoOnly</code> cannot be set as <code>true</code> at the same time.</li>
</ul>
</p>
</div>
</td>
</tr>
<tr><td><code>mixResolution</code> <sup>[1]</sup></td>
<td>If you set <code>isMixingEnabled</code> as <code>true</code>, <code>mixResolution</code> allows you to set the resolution in the format of width, height, fps, and Kbps; representing the width, height, frame rate, and bitrate of the video stream.</td>
</tr>
<tr><td><code>decryptionMode</code></td>
<td><p>When the whole channel is encrypted, the recording SDK uses <code>decryptionMode</code> to enable the built-in decryption function:</p>

<div><ul>
<li>“aes-128-xts”: AES-128, XTS mode</li>
<li>“aes-128-ecb”: AES-128, ECB mode</li>
<li>“aes-256-xts”: AES-256, XTS mode</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>secret</code></td>
<td>The decryption password when decryption mode is enabled. The default value is NULL.</td>
</tr>
<tr><td><code>idleLimitSec</code></td>
<td>The Agora Recording SDK automatically stops recording when there is no user in the recorded channel after a time period of <code>idleLimitSec</code>.  The value must be &ge; three seconds. The default value is 300 seconds.</td>
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
<tr><td><code>lowUdpPort</code></td>
<td>The lowest UDP port. Ensure that the value of <code>highUdpPort</code> - <code>lowUdpPort</code> is &ge; 4. The default value is 0.</td>
</tr>
<tr><td><code>highUdpPort</code></td>
<td>The highest UDP port. Ensure that the value of <code>highUdpPort</code> - <code>lowUdpPort</code> is &ge; 4. The default value is 0.</td>
</tr>
<tr><td><code>captureInterval</code></td>
<td>The time interval of the screen captures. The value ranges from one second to five seconds. You need to use <code>captureInterval</code> with decodeVideo = 3/4 when joining a channel. See <code>joinChannel<code> to allow a user to join a channel.</td>
</tr>
<tr><td><code>audioIndicationInterval</code></td>
<td><p>Whether or not to detect the speakers:</p>

<div><ul>
<li>&le; 0: Disables detecting the speakers.</li>
<li>&gt; 0: The time interval (ms) of detecting the speakers. When a speaker is found, the SDK returns the user ID of the speaker in the <code>onActiveSpeaker</code> callback.</li>
</ul>
</div>
</td>
</tr>
	<tr><td><code>decodeAudio</code> <sup>[2]</sup></td>
		<td><p>Audio decoding format:</p>
<ul>
<li>0: Default audio format (AUDIO_FORMAT_DEFAULT_TYPE).</li>
<li>1: AAC format (AUDIO_FORMAT_AAC_FRAME_TYPE).</li>
<li>2: PCM format (AUDIO_FORMAT_PCM_FRAME_TYPE).</li>
<li>3: PCM audio-mixing format (AUDIO_FORMAT_MIXED_PCM_FRAME_TYPE).</li>
</ul>
</td>
</tr>
	<tr><td><code>decodeVideo</code> <sup>[2]</sup></td>
<td><p>Video decoding format:</p>
	<ul>
<li><p>0: Default video format (VIDEO_FORMAT_DEFAULT_TYPE).</p>
</li>
<li><p>1: H.264 format (VIDEO_FORMAT_H264_FRAME_TYPE).</p>
</li>
<li><p>2: YUV format (VIDEO_FORMAT_YUV_FRAME_TYPE).</p>
</li>
<li><p>3: JPEG format (VIDEO_FORMAT_JPG_FRAME_TYPE).</p>
</li>
<li><p>4: JPEG file format (VIDEO_FORMAT_JPG_FILE_TYPE).</p>
</li>
<li><p>5: JPEG video file format (VIDEO_FORMAT_JPG_VIDEO_FILE_TYPE).</p>

<div><ul>
<li>Individual Mode (<code>isMixingEnabled</code> is set as <code>false</code>): MPEG-4 video and JPEG files.</li>
<li>Mixing Mode (<code>isMixingEnabled</code> is set as <code>true</code>): MPEG-4 video file for combined streams and JPEG files for individual streams. </li>
</ul>
</div>
</li>
</ul>
</td>
</tr>
<tr><td><code>mixedVideoAudio</code></td>
<td><p>If you set <code>isMixingEnabled</code> as <code>true</code>, <code>mixedVideoAudio</code> allows you to mix audio and video in real time:</p>
<ul>
<li>0: (Default) Mixes the audio and video respectively.</li>
<li>1: Mixes the audio and video in real time into an MPEG-4 file. Supports limited players.</li>
<li>2: Mixes the audio and video in real time into an MPEG-4 file. Supports more players.</li>
</ul>
</td>
</tr>
<tr><td><code>streamType</code></td>
<td><p><code>streamType</code> takes effect only when the Agora Native SDK has enabled dual-stream mode (high stream by default).</p>

<div><ul>
<li>0: (Default) High stream.</li>
<li>1: Low stream.</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>triggerMode</code></td>
<td><p>Sets whether to record automatically or manually:</p>
<ul>
<li>0: Automatically.</li>
<li>1: Manually. To start and stop recording, call <code>startRecording</code> and <code>stopRecording</code> respectively.</li>
</ul>
</td>
</tr>
<tr><td><code>lang</code></td>
	<td><p>Sets the programming language:</p>
		<ul>
			<li>CPP_LANG (CPP)</li>
			<li>java (Java)</li>
		</ul></td>
</tr>
<tr><td><code>proxyserver</code></td>
<td><code>proxyserver</code> allows you to record the content with the Intranet server. For details, please contact <a href="mailto:sales%40agora.io">sales@agora.io</a>.</td>
</tr>
<tr><td><code>audioProfile</code></td>
<td><p>Audio profile of the recording file:</p>

<div><ul>
<li>0: (Default) Sampling rate of 48 kHz, communication encoding, mono (AUDIO_PROFILE_DEFAULT).</li>
<li>4: Sampling rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps (AUDIO_PROFILE_MUSIC_HIGH_QUALITY).</li>
<li>5: Sampling rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps (AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO).</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>defaultVideoBg</code></td>
<td>The default background image of the video.</td>
</tr>
<tr><td><code>defaultUserBg</code></td>
<td>The default background image of the user.</td>
</tr>
<tr><td><code>avSyncMode</code></td>
<td><p>Audio and video synchronization mode:</p>

<div><ul>
<li>-1: Synchronization error (UNKNOWN_AVSYNC).</li>
<li>0: Compatible with older versions (AVSYNC_V0).</li>
<li>1: New audio and video synchronization mode (AVSYNC_V1).</li>
</ul>
</div>
</td>
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
<tbody.
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
<td><b>Not Supported</b></td>
<td><b>Not Supported</b></td>
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
<td><b>Not Supported</b></td>
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
virtual int setVideoMixingLayout(const agora::linuxsdk::VideoMixingLayout &layout) = 0;
```

The structure of <code>VideoMixingLayout</code>:

```
 typedef struct VideoMixingLayout
 {
     struct Region {
       uid_t uid;
       double x;//[0,1]
       double y;//[0,1]
       double width;//[0,1]
       double height;//[0,1]
       int zOrder; //optional, [0, 100] //0 (default): bottom most, 100: top most

       //  Optional
       //  [0, 1.0] where 0 means transparent, 1.0 means opaque
       double alpha;

       int renderMode;//RENDER_MODE_HIDDEN: Crop, RENDER_MODE_FIT: Zoom to fit
       Region()
           :uid(0)
            , x(0)
            , y(0)
            , width(0)
            , height(0)
            , zOrder(0)
            , alpha(1.0)
            , renderMode(1)
      {}

   };
   int canvasWidth;
   int canvasHeight;
   const char* backgroundColor;//e.g. "#C0C0C0" in RGB
   uint32_t regionCount;
   const Region* regions;
   const char* appData;
   int appDataLength;
   VideoMixingLayout()
       :canvasWidth(0)
        , canvasHeight(0)
        , backgroundColor(NULL)
        , regionCount(0)
        , regions(NULL)
        , appData(NULL)
        , appDataLength(0)
   {}
} VideoMixingLayout;
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
	<li><code>renderMode</code>: Render mode<ul>
<li>1: Cropped (RENDER_MODE_HIDDEN).</li>
<li>2: Proportionate (RENDER_MODE_FIT).</li>
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


### <a name="leaveChannel"></a>Allows the Application to Leave the Channel  (leaveChannel)

This method allows the recording application to leave the channel and release the thread resources.

```
virtual int leaveChannel() = 0;
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



### <a name="release"></a>Releases the IRecordingEngine Object (release)

This method releases the IRecordingEngine object.

```
virtual int release() = 0;
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

> -   The recording properties only include the information of the path where the recording files are stored.
> -   This method is different from [onUserJoined](#onUserJoined). You must call <code>onUserJoined</code> after joining the channel.


```
virtual const RecordingEngineProperties* getProperties() = 0;
```

### <a name="startService"></a>Starts the Recording (startService)

This method manually starts recording.

The method is only valid when you set <code>triggerMode</code> to <code>1</code> (manually) when joining the channel. For more information, see [Allows an Application to Join a Channel (joinChannel)](#joinChannel) on <code>triggerMode</code>.

```
virtual int startService() = 0;
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

This method manually stops recording.

The method is only valid when you set <code>triggerMode</code> to <code>1</code> (manually) when joining the channel. For more information, see [Allows an Application to Join a Channel (joinChannel)](#joinChannel) on <code>triggerMode</code>.

```
virtual int stopService() = 0;
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
virtual int setUserBackground(uid_t uid, const char* img_path) = 0;
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
<td>The user ID of the user for the background image to be set.</td>
</tr>
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
virtual int setLogLevel(agora::linuxsdk::agora_log_level level) = 0;
```

This method sets the log level. Only log levels preceding the selected level are generated. The default value of the log level is 6.

### <a name="enableModuleLog"></a>Enables the Module Log (enableModuleLog)

```
virtual int enableModuleLog(uint32_t module, bool enable) = 0;
```

This method enables/disables generating logs for specified modules.

## <a name="irecordingengineeventhandler"></a>IRecordingEngineEventHandler Class

The IRecordingEngineEventHandler class provides the following callbacks for the recording engine:

-   [An Error has Occurred During SDK Runtime (onError)](#onError)

-   [A Warning has Occurred During SDK Runtime (onWarning)](#onWarning)

-   [The User has Joined the Specified Channel (onJoinChannelSuccess)](#onJoinChannelSuccess)

-   [The User has Left the Channel (onLeaveChannel)](#onLeaveChannel)

-   [A Remote User or a Host has Joined the Channel (onUserJoined)](#onUserJoined)

-   [A User has Left the Channel or Gone Offline (onUserOffline)](#onUserOffline)

-   [The Raw Audio Data has Been Received (audioFrameReceived)](#audioFrameReceived)

-   [The Raw Video Data has Been Received (videoFrameReceived)](#videoFrameReceived)

-   [Indicates the Speaker in the Channel (onActiveSpeaker)](#onActiveSpeaker)


### <a name="onError"></a>An Error has Occurred During SDK Runtime (onError)

This callback is triggered when an error has occurred during SDK runtime.

The SDK cannot fix the issue or resume running, which requires intervention from the application and informs the user on the issue.

```
virtual void onError(int error, agora::linuxsdk::STAT_CODE_TYPE stat_code) = 0;
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
<li>0: No error (ERR_OK).</li>
<li>1: General error with no classified reason (ERR_FAILED).</li>
<li>2: Invalid parameter is called (ERR_INVALID_ARGUMENT). For example, the specific channel name contains illegal characters.</li>
<li>3: The SDK module is not ready (ERR_NOT_READY). Agora recommends the following methods to solve this error:
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
<li>0: Everything is normal (STAT_OK).</li>
<li>1: Error from the engine (STAT_ERR_FROM_ENGINE).</li>
<li>2: Failure to join the channel (STAT_ERR_ARS_JOIN_CHANNEL).</li>
<li>3: Failure to create a process (STAT_ERR_CREATE_PROCESS).</li>
<li>4: Failure to mix the video (STAT_ERR_MIXED_INVALID_VIDEO_PARAM).</li>
<li>5: Null pointer (STAT_ERR_NULL_POINTER ).</li>
<li>6: Invalid parameters of the proxy server (STAT_ERR_PROXY_SERVER_INVALID_PARAM).</li>
<li>0x8: Error in polling (STAT_POLL_ERR).</li>
<li>0x10: Polling hangs up (STAT_POLL_HANG_UP).</li>
<li>0x20: Invalid polling request (STAT_POLL_NVAL).</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



### <a name="onWarning"></a>A Warning has Occurred During SDK Runtime (onWarning)

This callback is triggered when a warning has occurred during SDK runtime.

In most cases, the application can ignore the warnings reported by the SDK because the SDK can usually fix the issue and resume running.

```
virtual void onWarning(int warn) = 0;
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
<li>103: No channel resources are available (WARN_NO_AVAILABLE_CHANNEL). Maybe because the server cannot allocate any channel resource.</li>
<li>104: A timeout when looking up the channel (WARN_LOOKUP_CHANNEL_TIMEOUT). When joining a channel, the SDK looks up the specified channel. This warning usually occurs when the network conditions are too poor to connect to the server.</li>
<li>105: The server rejected the request to look up the channel (WARN_LOOKUP_CHANNEL_REJECTED). The server cannot process this request or the request is illegal.</li>
<li>106: A timeout occurred when opening the channel (WARN_OPEN_CHANNEL_TIMEOUT). Once the specific channel is found, the SDK opens the channel. This warning usually occurs when the network conditions are too poor to connect to the server.</li>
<li>107: The server rejected the request to open the channel (WARN_OPEN_CHANNEL_REJECTED). The server cannot process this request or the request is illegal.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onJoinChannelSuccess"></a>The User has Joined the Specified Channel (onJoinChannelSuccess)

This callback is triggered when the user has successfully joined the specified channel with an assigned Channel ID and user ID.

```
virtual void onJoinChannelSuccess(const char * channelId, uid_t uid) = 0;
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
<tr><td>channelId</td>
<td>Channel ID assigned based on the channel name specified in <code>joinChannel</code>.</td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID of the user.</td>
</tr>
</tbody>
</table>



### <a name="onLeaveChannel"></a>The User has Left the Channel (onLeaveChannel)

This callback is triggered when the user has left the channel.

```
virtual void onLeaveChannel(agora::linuxsdk::LEAVE_PATH_CODE code) = 0;
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
<tr><td><code>code</code></td>
<td>
<div>Reason:</div>
<ul>
<li>0: Initialization failure (LEAVE_CODE_INIT).</li>
<li>1: Signal triggered exit (LEAVE_CODE_SIG).</li>
<li>2: There is no user in the channel except for the recording application (LEAVE_CODE_NO_USERS).</li>
<li>3: Timer catch exit (LEAVE_CODE_TIMER_CATCH).</li>
<li>4: The client leaves the channel (LEAVE_CODE_CLIENT_LEAVE).</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onUserJoined"></a>A Remote User or a Host has Joined the Channel. (onUserJoined)

This callback is triggered when another user has joined the channel.

If other users are already in the channel, the SDK reports to the application on the existing users as well. This callback is called as many times as the number of users in the channel.

```
virtual void onUserJoined(uid_t uid, agora::linuxsdk::UserJoinInfos &infos) = 0;
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
<tr><td><code>infos</code></td>
<td>Information about the user.</td>
</tr>
</tbody>
</table>



The structure of <code>UserJoinInfos</code>:

```
typedef struct UserJoinInfos {
    const char* storageDir;
    //add information about the user below

    UserJoinInfos():
        storageDir(NULL)
    {}
}UserJoinInfos;
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
<tr><td><code>storageDir</code></td>
<td>Directory of the recorded files.</td>
</tr>
</tbody>
</table>



### <a name="onUserOffline"></a>A User has Left the Channel or Gone Offline (onUserOffline)

This callback is triggered when a user has left the channel or gone offline.

The SDK reads the timeout data to determine if a user has left the channel (or has gone offline). If no data package is received from the user within 15 seconds, the SDK assumes the user is offline. A poor network connection may lead to false detections, so use signaling for reliable offline detection.

```
virtual void onUserOffline(uid_t uid, agora::linuxsdk::USER_OFFLINE_REASON_TYPE reason) = 0;
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
<tr><td>uid</td>
<td>User ID of the user</td>
</tr>
<tr><td><code>reason</code></td>
<td><p>Reason:</p>
<ul>
<li>0: The user has quit the call (USER_OFFLINE_QUIT).</li>
<li>1: The SDK timed out and the user dropped offline because it has not received any data packet for a period of time (USER_OFFLINE_DROPPED). If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the user has dropped offline. </li>
<li>2: The client role has changed from the host to the audience (USER_OFFLINE_BECOME_AUDIENCE ). The option is only valid when you set the channel profile as live broadcast when calling <code>joinChannel</code>.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="audioFrameReceived"></a>The Raw Audio Data has Been Received (audioFrameReceived)

This callback is triggered when the raw audio data has been received.

```
virtual void audioFrameReceived(unsigned int uid, const agora::linuxsdk::AudioFrame *frame) const = 0;
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
<tr><td><code>frame</code></td>
<td>Received raw audio data in PCM or AAC format.</td>
</tr>
</tbody>
</table>



The structure of <code>AudioFrame</code>:

```
struct AudioFrame {
    AUDIO_FRAME_TYPE type;
    union {
        AudioPcmFrame *pcm;
        AudioAacFrame *aac;
    } frame;

    AudioFrame();
    ~AudioFrame();
    avsyncType avsync_type_;
    MEMORY_TYPE mType;
};
```

#### AudioPcmFrame

The structure of <code>AudioPcmFrame</code> is as follows:

```
class AudioPcmFrame {
    public:
    AudioPcmFrame(u64_t frame_ms, uint_t sample_rates, uint_t samples);
    ~AudioPcmFrame();
    public:
    u64_t frame_ms_;
    uint_t channels_; // 1
    uint_t sample_bits_; // 16
    uint_t sample_rates_; // 8k, 16k, 32k
    uint_t samples_;

    const uchar_t *pcmBuf_;
    uint_t pcmBufSize_;
};
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
class AudioAacFrame {
    public:
    explicit AudioAacFrame(u64_t frame_ms);
    ~AudioAacFrame();

    const uchar_t *aacBuf_;
    u64_t frame_ms_;
    uint_t aacBufSize_;

};
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

This callback is triggered for every received raw video frame and can be used to detect sexually explicit content, if necessary.

Agora recommends capturing the i frame only and neglecting the others.

```
virtual void videoFrameReceived(unsigned int uid, const agora::linuxsdk::VideoFrame *frame) const = 0;
```

The structure of <code>VideoFrame</code>:

```
struct VideoFrame {
    VIDEO_FRAME_TYPE type;
    union {
        VideoYuvFrame *yuv;
        VideoH264Frame *h264;
        VideoJpgFrame *jpg;
    } frame;

    int rotation_; // 0, 90, 180, 270
    VideoFrame();
    ~VideoFrame();

    MEMORY_TYPE mType;
};
```

#### VideoYuvFrame

The structure of <code>VideoYuvFrame</code>:

```
class VideoYuvFrame {
    public:
    VideoYuvFrame(u64_t frame_ms, uint_t width, uint_t height, uint_t ystride,
            uint_t ustride, uint_t vstride);
    ~VideoYuvFrame();

    u64_t frame_ms_;

    const uchar_t *ybuf_;
    const uchar_t *ubuf_;
    const uchar_t *vbuf_;

    uint_t width_;
    uint_t height_;

    uint_t ystride_;
    uint_t ustride_;
    uint_t vstride_;

    //all
    const uchar_t *buf_;
    uint_t bufSize_;
};
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
struct VideoH264Frame {
    public:
    VideoH264Frame():
        frame_ms_(0),
        frame_num_(0),
        buf_(NULL),
        bufSize_(0)
    {}

    ~VideoH264Frame(){}
    u64_t frame_ms_;
    uint_t frame_num_;

    //all
    const uchar_t *buf_;
    uint_t bufSize_;
};
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
struct VideoJpgFrame {
    public:
    VideoJpgFrame():
        frame_ms_(0),
        buf_(NULL),
        bufSize_(0){}

   ~VideoJpgFrame() {}
    u64_t frame_ms_;

    //all
    const uchar_t *buf_;
    uint_t bufSize_;
};
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
virtual void onActiveSpeaker(uid_t uid);
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










