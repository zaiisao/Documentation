
---
title: 录制 API
description: 
platform: C++
updatedAt: Thu Nov 01 2018 07:57:03 GMT+0000 (UTC)
---
# 录制 API
# 录制 API

> 版本：v2.2.3

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>接口类</strong></td>
<td><strong>描述</strong></td>
</tr>
	<tr><td><a href="#IRecordingEngine">IRecordingEngine 类</a></td>
<td>用于管理 <code>Recording engine</code> 的类</td>
</tr>
<tr><td><a href="#id12">IRecordingEngineEventHandler类</a></td>
<td>为<code> Recording engine </code>提供回调的类</td>
</tr>
</tbody>
</table>



<a id = "IRecordingEngine"></a>
​	 

## IRecordingEngine 类

该类用于管理 Agora SDK 的录制引擎，主要包含以下方法：

- [创建录制引擎 (createAgorarecordingEngine)](#createAgoraRecordingEngine)
- [加入频道 \(joinChannel\)](#joinChannel)
- [设置视频合图布局 \(setVideoMixingLayout\)](#setVideoMixingLayout)
- [退出频道 \(leaveChannel\)](#leaveChannel)
- [销毁 IRecordingEngine 对象 \(release\)](#release)
- [获取属性 \(getProperties\)](#getProperties)
- [开始录制 \(startService\)](#startService)
- [停止录制 \(stopService\)](#stopService)
- [设置生成 log 的等级 \(setLogLevel\)](#setLogLevel)
- [设置生成 log 的模块 \(enableModuleLog\)](#enableModuleLog)

<a name = "createAgoraRecordingEngine"></a>

### 创建录制引擎 \(createAgoraRecordingEngine\)

```C++
public static IRecordingEngine* createAgoraRecordingEngine(const char * appId, IRecordingEngineEventHandler *eventHandler);
```

该方法创建录制引擎对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>appId</code></td>
<td>希望录制的音视频通话中使用的 App ID ，详见 <a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a></td>
</tr>
<tr><td><code>eventHandler</code></td>
<td>录制 SDK 所触发的事件通过 <a href="#irecordingengineeventhandler">IRecordingEngineEventHandler</a> 通知应用程序</td>
</tr>
</tbody>
</table>



<a name = "joinChannel"></a>

### 加入频道 \(joinChannel\)

```
virtual int joinChannel(const char * token, const char *channelId, uid_t uid, const RecordingConfig &config) = 0;
```

该方法让录制 App 加入频道，并开始录制。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>token</code></td>
<td>希望录制的音视频通话中使用的 token ，详见 <a href="../../cn/Agora%20Platform/token.md"><span>密钥说明</span></a></td>
</tr>
<tr><td><code>channelId</code></td>
<td>希望录制的视频通话的频道名称</td>
</tr>
<tr><td><code>uid</code></td>
	<td><p>录制使用的uid。取值范围：1 到（2<sup>32</sup>-1），并保证频道内 uid 不重复。有两种设置方法：</p>
<ul>
<li>设置为 0，系统将自动分配一个uid</li>
<li>设置一个唯一的uid（不能与现在频道内的任何 uid 重复）</li>
</ul>
</td>
</tr>
<tr><td><code>config</code></td>
<td>录制的详细设置，详见下表的定义</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



> - 由于录制端暂时未开放 `requestToken `和 `renewToken`接口，在获取 Token 时，请务必将服务到期时间（`expireTimestamp`\) 设置为 0，表示权限一旦生成，永不过期。
> - 同一个频道里不能出现两个相同的 `uid`，否则会产生未定义行为。

`RecordingConfig` 结构如下:

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
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelProfile</code></td>
<td><p>使用场景。Agora RtcEngine 针对不同的使用场景进行了不同的优化。目前支持：</p>
<ul>
<li>CHANNEL_PROFILE_COMMUNICATION：通信（默认），即常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话</li>
<li>CHANNEL_PROFILE_LIVE_BROADCAST：直播，有两种用户角色：主播和观众</li>
</ul>
</td>
</tr>
<tr><td><code>isAudioOnly</code></td>
<td><p>仅启用音频录制功能，关闭视频录制：</p>
<ul>
<li>true：仅启用音频录制功能</li>
<li>false：音视频同时录制</li>
</ul>
</td>
</tr>
<tr><td><code>isVideoOnly</code></td>
<td><p>仅启用视频录制功能，关闭音频录制：</p>
<ul>
<li>true：仅启动视频录制功能</li>
<li>false：音视频同时录制</li>
</ul>
</td>
</tr>
<tr><td><code>isMixingEnabled</code></td>
<td><p>是否启用合流（混音或合图）模式：</p>
<ul>
<li>false(默认)：启用单流模式录制。录制文件的声道数和码率与原始音频流的声道数和码率保持一致。</li>
<li>true：启用合流模式录制。录制文件的采样率，码率和声道数与录制前各音视频流的最高值保持一致。<ul>
<li>如果 <code>isAudioOnly</code>为 true，且 <code>isVideoOnly</code>为 false，则表示启用纯音频混合</li>
<li>如果 <code>isAudioOnly</code>为 false，且 <code>isVideoOnly</code>为 true，则表示启用纯视频混合</li>
<li>如果<code>isAudioOnly</code>为 false，且<code>isVideoOnly</code> 为 false，则表示音视频模式混合，即多个 <code>uid </code>的音频混合、多个 <code>uid</code> 的视频混合</li>
<li>不能将 <code>isAudioOnly</code> 和 <code>isVideoOnly </code>同时设置为 true</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>mixResolution</code> <sup>[1]</sup></td>
<td>如果你启用了合流模式，可以通过该参数设置合图的视频属性，格式为：width，hight，fps，kbps，分别对应合图的宽、高、帧率和码率</td>
</tr>
<tr><td><code>decryptionMode</code></td>
<td><p>当整个频道进行加密时，录制 SDK 启用内置的解密功能。目前支持以下几种解密方式：</p>
<ul>
<li>“aes-128-xts”：128 位 AES 加密，XTS 模式</li>
<li>“aes-128-ecb”：128 位 AES 加密，ECB 模式</li>
<li>“aes-256-xts”：256 位 AES 加密，XTS 模式</li>
</ul>
</td>
</tr>
<tr><td><code>secret</code></td>
<td>启用解密模式后，设置的解密密码</td>
</tr>
<tr><td><code>idleLimitSec</code></td>
<td>设置空闲频道的超时退出时间。该值需大于等于 3 秒，默认值为 300 秒。若频道空闲（无用户）超过设定的时间，录制程序自动退出。该时间也会纳入计费。</td>
</tr>
<tr><td><code>appliteDir</code></td>
<td>AgoraCoreService 所在的目录</td>
</tr>
<tr><td><code>recordFileRootDir</code></td>
<td>设置录制文件存放的根目录。<em><code>recordFileRootDir</code></em> 和 <em><code>cfgFilePath</code></em> 不能同时设置。</td>
</tr>
<tr><td><code>cfgFilePath</code></td>
<td>设置配置文件的路径。格式为 JSON，例如：{“Recording_Dir” : “&lt;recording path&gt;”}。<em><code>recordFileRootDir</code></em> 和 <em><code>cfgFilePath</code></em> 不能同时设置。</td>
</tr>
<tr><td><code>lowUdpPort</code></td>
<td>最低 UDP 端口。高 UDP 端口与低 UDP 端口差值不能小于 4</td>
</tr>
<tr><td><code>highUdpPort</code></td>
<td>最高 UDP 端口。高 UDP 端口与低 UDP 端口差值不能小于 4</td>
</tr>
<tr><td><code>captureInterval</code></td>
<td>截屏的时间间隔，最小值为 1 秒，默认为 5 秒。仅当 <code>decodeVideo</code>= 3/4 时有效。</td>
</tr>
<tr><td><code>audioIndicationInterval</code></td>
<td><p>说话者检测的时间间隔。</p>
<div><ul>
<li>&lt;= 0: 禁用说话者检测的功能；</li>
<li>&gt; 0: 说话者检测的时间间隔，单位为 ms 。一旦检测到频道内有人说话，就会在 <a href="#onactivespeaker-recording-cpp"><span>启用说话者提示 (onActiveSpeaker)</span></a> 回调中返回说话者 id 。</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>decodeAudio</code> <sup>[2]</sup></td>
<td><ul>
<li>AUDIO_FORMAT_DEFAULT_TYPE = 0：默认音频格式</li>
<li>AUDIO_FORMAT_AAC_FRAME_TYPE = 1：AAC 格式</li>
<li>AUDIO_FORMAT_PCM_FRAME_TYPE = 2：PCM 格式</li>
<li>AUDIO_FORMAT_MIXED_PCM_FRAME_TYPE = 3：PCM 混音格式</li>
</ul>
</td>
</tr>
<tr><td><code>decodeVideo</code> <sup>[2]</sup></td>
<td><ul>
<li>VIDEO_FORMAT_DEFAULT_TYPE = 0：默认视频格式</li>
<li>VIDEO_FORMAT_H264_FRAME_TYPE = 1：H.264 帧格式</li>
<li>VIDEO_FORMAT_YUV_FRAME_TYPE = 2：YUV 帧格式</li>
<li>VIDEO_FORMAT_JPG_FRAME_TYPE = 3：JPG 帧格式</li>
<li>VIDEO_FORMAT_JPG_FILE_TYPE = 4：JPG 文件格式</li>
<li>VIDEO_FORMAT_JPG_VIDEO_FILE_TYPE = 5：JPG + 视频文件格式<ul>
<li>单流模式 (<code>isMixingEnabled</code>=false) 下，录制得 MP4 视频文件，并截图获得 JPG 文件</li>
<li</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>mixedVideoAudio</code></td>
<td><p>实时混合语音和视频。仅当 <code>isMixingEnabled</code> = 1 时有效。该参数有三种模式：</p>
<ul>
<li>0：音视频分别混合（默认）</li>
<li>1：音频和视频一起混合，录制文件格式为 mp4，但播放器支持有限</li>
<li>2：音频和视频一起混合，录制文件格式为 mp4，支持更多播放器</li>
</ul>
</td>
</tr>
<tr><td><code>streamType</code></td>
<td><p>选择音视频流。仅在 Agora Native SDK 选择的是双流模式时有效：</p>
<div><ul>
<li>0: 大流(默认值)</li>
<li>1: 小流</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>triggerMode</code></td>
<td><p>选择录制启动模式：</p>
<ul>
<li>0：<code>automatically</code> 模式，即自动模式</li>
<li>1：<code>mannually</code> 模式，即手动模式。此模式下，可以调用 <em><code>startService</code></em> 和 <em><code>stopService</code></em> 方法灵活开始、结束录制。</li>
</ul>
</td>
</tr>
<tr><td><code>lang</code></td>
<td>录制使用的语言 (CPP_LANG 或 java)</td>
</tr>
<tr><td><code>proxyServer</code></td>
<td>使用该功能可以在内网进行录制。具体使用方法请联系 
	<a href="mailto:sales%40agora.io">sales<span>@</span>agora<span>.</span>io</a> 。</td>
</tr>
<tr><td><code>audioProfile</code> </td>
<td><p>录制文件的音频设置。</p>
<div><ul>
<li>AUDIO_PROFILE_DEFAULT = 0: 默认设置。指定 48k 采样率，通信编码，单声道，编码码率约 48 kbps</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY = 1: 指定 48 KHz 采样率，音乐编码, 单声道，编码码率约 128 kbps</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO = 2, // 指定 48KHz采样率，音乐编码, 双声道，编码码率约 192 kbps</li>
</ul>
</td>
</tr>
<tr><td><code>defaultVideoBg </code></td>
<td>默认视频背景图片的路径</td>
</tr>
<tr><td><code>defaultUserBg</code></td>
<td>默认用户背景图片的路径</td>
</tr>
<tr><td><code>avSyncMode</code></td>
<td><p>可选音画同步模式</p>
<div><ul>
<li>UNKNOWN_AVSYNC = -1, 音画同步报错</li>
<li>AVSYNC_V0 = 0, 和老版本兼容</li>
<li>AVSYNC_V1 = 1, 新版本的音画同步选项</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



> [1] 关于设置视频合图的推荐分辨率，详见下表。

> [2] 开启裸数据时，有以下限制：
>
> - 合流模式仅支持纯音频录制，不支持视频。
> - Web 端录制仅支持 H.264 格式的视频裸数据，不支持 VP8 格式。

### 视频属性

**视频属性 \(通信场景\):**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>分辨率</td>
<td>帧率</td>
<td>最低码率</td>
<td>最高码率</td>
<td>推荐码率</td>
</tr>
<tr><td>3840x2160</td>
<td>15</td>
<td>3000</td>
<td>9000</td>
<td>6000</td>
</tr>
<tr><td>2560x1440</td>
<td>15</td>
<td>1600</td>
<td>4800</td>
<td>3200</td>
</tr>
<tr><td>1920x1080</td>
<td>15</td>
<td>1000</td>
<td>3000</td>
<td>2000</td>
</tr>
<tr><td>1280x720</td>
<td>15</td>
<td>600</td>
<td>1800</td>
<td>1200</td>
</tr>
<tr><td>960x720</td>
<td>15</td>
<td>480</td>
<td>1440</td>
<td>960</td>
</tr>
<tr><td>848x480</td>
<td>15</td>
<td>300</td>
<td>900</td>
<td>600</td>
</tr>
<tr><td>640x480</td>
<td>15</td>
<td>250</td>
<td>750</td>
<td>500</td>
</tr>
<tr><td>480x480</td>
<td>15</td>
<td>200</td>
<td>600</td>
<td>400</td>
</tr>
<tr><td>640x360</td>
<td>15</td>
<td>200</td>
<td>600</td>
<td>400</td>
</tr>
<tr><td>360x360</td>
<td>15</td>
<td>130</td>
<td>390</td>
<td>260</td>
</tr>
<tr><td>424x240</td>
<td>15</td>
<td>110</td>
<td>330</td>
<td>220</td>
</tr>
<tr><td>320x240</td>
<td>15</td>
<td>90</td>
<td>270</td>
<td>180</td>
</tr>
<tr><td>240x240</td>
<td>15</td>
<td>70</td>
<td>210</td>
<td>140</td>
</tr>
<tr><td>320x180</td>
<td>15</td>
<td>70</td>
<td>210</td>
<td>140</td>
</tr>
<tr><td>240x180</td>
<td>15</td>
<td>60</td>
<td>180</td>
<td>120</td>
</tr>
<tr><td>180x180</td>
<td>15</td>
<td>50</td>
<td>150</td>
<td>100</td>
</tr>
<tr><td>160x120</td>
<td>15</td>
<td>30</td>
<td>90</td>
<td>60</td>
</tr>
<tr><td>120x120</td>
<td>15</td>
<td>25</td>
<td>75</td>
<td>50</td>
</tr>
</tbody>
</table>



**视频属性 \(直播场景\):**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>分辨率</td>
<td>帧率</td>
<td>最低码率</td>
<td>最高码率</td>
<td>推荐码率</td>
</tr>
<tr><td>3840x2160</td>
<td>15</td>
<td>6000</td>
<td>18000</td>
<td>12000</td>
</tr>
<tr><td>2560x1440</td>
<td>15</td>
<td>3200</td>
<td>9600</td>
<td>6400</td>
</tr>
<tr><td>1920x1080</td>
<td>15</td>
<td>2000</td>
<td>6000</td>
<td>4000</td>
</tr>
<tr><td>1280x720</td>
<td>15</td>
<td>1200</td>
<td>3600</td>
<td>2400</td>
</tr>
<tr><td>960x720</td>
<td>15</td>
<td>960</td>
<td>2880</td>
<td>1920</td>
</tr>
<tr><td>848x480</td>
<td>15</td>
<td>600</td>
<td>1800</td>
<td>1200</td>
</tr>
<tr><td>640x480</td>
<td>15</td>
<td>500</td>
<td>1500</td>
<td>1000</td>
</tr>
<tr><td>480x480</td>
<td>15</td>
<td>400</td>
<td>1200</td>
<td>800</td>
</tr>
<tr><td>640x360</td>
<td>15</td>
<td>400</td>
<td>1200</td>
<td>800</td>
</tr>
<tr><td>360x360</td>
<td>15</td>
<td>260</td>
<td>780</td>
<td>520</td>
</tr>
<tr><td>424x240</td>
<td>15</td>
<td>220</td>
<td>660</td>
<td>440</td>
</tr>
<tr><td>320x240</td>
<td>15</td>
<td>180</td>
<td>540</td>
<td>360</td>
</tr>
<tr><td>240x240</td>
<td>15</td>
<td>140</td>
<td>420</td>
<td>280</td>
</tr>
<tr><td>320x180</td>
<td>15</td>
<td>140</td>
<td>420</td>
<td>280</td>
</tr>
<tr><td>240x180</td>
<td>15</td>
<td>120</td>
<td>360</td>
<td>240</td>
</tr>
<tr><td>180x180</td>
<td>15</td>
<td>100</td>
<td>300</td>
<td>200</td>
</tr>
<tr><td>160x120</td>
<td>15</td>
<td>60</td>
<td>180</td>
<td>120</td>
</tr>
<tr><td>120x120</td>
<td>15</td>
<td>50</td>
<td>150</td>
<td>100</td>
</tr>
</tbody>
</table>



### 播放器列表

**播放器列表**:

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>平台</strong></td>
<td><strong>播放器/浏览器</strong></td>
<td><strong><code>mixedVideoAudio</code>=0</strong></td>
<td><strong><code>mixedVideoAudio</code>=1</strong></td>
</tr>
<tr><td>Linux</td>
<td>默认播放器</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Linux</td>
<td>VLC Media Player</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Linux</td>
<td>ffplay</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Windows</td>
<td>Media Player</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Windows</td>
<td>KMPlayer</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Windows</td>
<td>VLC Player</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Windows</td>
<td>Chrome (&gt;=49.0.2623)</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>macOS</td>
<td>QuickTime Player</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>macOS</td>
<td>Movist</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>macOS</td>
<td>MPlayerX</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>macOS</td>
<td>KMPlayer</td>
<td><strong>不支持</strong></td>
<td><strong>不支持</strong></td>
</tr>
<tr><td>macOS</td>
<td>Chrome (&gt;=47.0.2526.111)</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>macOS</td>
<td>Safari (&gt;=11.0.3)</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>iOS</td>
<td>iOS 默认播放器</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>iOS</td>
<td>VLC</td>
<td><strong>app 找不到视频文件，无法播放</strong></td>
<td>支持</td>
</tr>
<tr><td>iOS</td>
<td>KMPlayer</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>iOS</td>
<td>Safari (&gt;=9.0)</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Android</td>
<td>Android 默认播放器</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Android</td>
<td>MXPlayer</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Android</td>
<td>VLC for Android</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Android</td>
<td>KMPlayer</td>
<td>支持</td>
<td>支持</td>
</tr>
<tr><td>Android</td>
<td>Chrome (&gt;=49.0.2623)</td>
<td>支持</td>
<td>支持</td>
</tr>
</tbody>
</table>



<a name = "setVideoMixingLayout"></a>

### 设置视频合图布局 \(setVideoMixingLayout\)

```
virtual int setVideoMixingLayout(const agora::linuxsdk::VideoMixingLayout &layout) = 0;
```

该方法设置视频合图布局。

`VideoMixingLayout `结构如下:

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
       //  [0, 1.0] where 0 denotes throughly transparent, 1.0 opaque
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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>canvasWidth</code></td>
<td>整个屏幕（画布）的宽度</td>
</tr>
<tr><td><code>canvasHeight</code></td>
<td>整个屏幕（画布）的高度</td>
</tr>
<tr><td><code>backgroundColor</code></td>
<td>屏幕（画布）的背景颜色。可根据所需颜色填写对应的 6 位 RGB 值</td>
</tr>
<tr><td><code>regionCount</code></td>
<td>频道内显示头像或视频的主播的数量</td>
</tr>
<tr><td><code>regions</code></td>
<td><p>频道内每位主播在屏幕上显示自己的头像或视频的区域。详细参数包括：</p>
<ul>
<li><code>uid</code>：待显示在该区域的主播用户 uid：</li>
<li><code>x</code>：屏幕里该区域左上角的横坐标的相对值，取值范围是 [0.0,1.0]</li>
<li><code>y</code>：屏幕里该区域左上角的纵坐标的相对值，取值范围是 [0.0,1.0]</li>
<li><code>width</code>：该区域宽度的相对值，取值范围是 [0.0,1.0]</li>
<li><code>height</code>：该区域高度的相对值，取值范围是 [0.0,1.0]</li>
<li><code>zOrder</code>：所在图层编号，取值范围是 [1, 100]。1 表示该区域图像位于最下层图层，而 100 表示该区域图像位于最上层图层</li>
<li><code>alpha</code>：图像的透明度。取值范围是 [0.0,1.0] 。0 表示图像为透明的，1 表示图像为完全不透明的。</li>
<li><code>renderMode</code>：<ul>
<li>RENDER_MODE_HIDDEN(1)：裁减到合适大小</li>
<li>RENDER_MODE_FIT(2)：缩放到合适大小</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>appData</code></td>
<td>应用程序自定义的数据</td>
</tr>
<tr><td><code>appDataLength</code></td>
<td>应用程序自定义的数据的长度</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



下面的例子用来描述主播图像的位置和尺寸: x, y, width, height 分别为 0.5， 0.4， 0.2， 0.3

<img alt="../_images/sei_overview.png" src="https://web-cdn.agora.io/docs-files/cn/sei_overview.png" style="width: 561.4px; height: 484.4px;"/>

<a name = "leaveChannel"></a>

### 退出频道 \(leaveChannel\)

```
virtual int leaveChannel() = 0;
```

录制 App 退出频道，并销毁占用的线程资源。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



<a name = "release"></a>

### 销毁 IRecordingEngine 对象 \(release\)

```
virtual int release() = 0;
```

该方法销毁 `IRecordingEngine` 对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



<a name = "getProperties"></a>

### 获取属性 \(getProperties\)

```
virtual const RecordingEngineProperties* getProperties() = 0;
```

获取录制属性信息。无需加入任何频道。

> - 目前仅支持获取录制路径信息;

> - 该方法与 `onUserJoined `区别是: `onUserJoined` 需要在加入频道后使用。

<a name = "startService"></a>

### 开始录制 \(startService\)

```
virtual int startService() = 0;
```

该方法手动启动录制。

仅在参数 `triggerMode`为`manually` 时有效。详见 [加入频道 \(joinChannel\)](#joinChannel) 中关于 `triggerMode` 的描述。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



<a name = "stopService"></a>

### 停止录制 \(stopService\)

```
virtual int stopService() = 0;
```

该方法手动停止录制。

仅在参数 `triggerMode`为`manually`时有效。详见 [加入频道 \(joinChannel\)](#joinChannel) 中关于 `triggerMode`的描述。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置背景图片 \(setUserBackground\)

该方法为指定用户设置背景图片。

```
virtual int setUserBackground(uid_t uid, const char* img_path) = 0;
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>该用户的 ID 。</td>
</tr>
<tr><td><code>image_path</code></td>
<td>背景图片路径。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



<a name = "setLogLevel"></a>

### 设置生成 log 的等级 \(setLogLevel\)

```
virtual int setLogLevel(agora::linuxsdk::agora_log_level level) = 0;
```

该方法设置生成 log 的等级。设置完成后，只有等级低于 `level` 的 log 才会被生成。 `level`的默认值为 6。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>level</code></td>
<td><ul>
<li>AGORA_LOG_LEVEL_FATAL=2</li>
<li>AGORA_LOG_LEVEL_ERROR =3</li>
<li>AGORA_LOG_LEVEL_WARN = 4</li>
<li>AGORA_LOG_LEVEL_NOTICE = 5</li>
<li>AGORA_LOG_LEVEL_INFO = 6</li>
<li>AGORA_LOG_LEVEL_DEBUG = 7</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



<a name = "enableModuleLog"></a>

### 设置生成 log 的模块 \(enableModuleLog\)

```
virtual int enableModuleLog(uint32_t module, bool enable) = 0;
```

该方法开启/关闭指定模块的 log 生成功能。

<a id = "id12"></a>

<a id = "irecordingengineeventhandler"></a>

## IRecordingEngineEventHandler 类

该类用于为` Recording engine `提供回调。该类包含如下回调：

- [发生错误回调 \(onError\)](#onError)
- [发生警告回调 \(onWarning\)](#onWarning)
- [加入频道回调 \(onJoinChannelSuccess\)](#onJoinChannelSuccess)
- [离开频道回调 \(onLeaveChannel\)](#onLeaveChannel)
- [其他用户加入当前频道回调 \(onUserJoined\)](#onUserJoined)
- [其他用户离开当前频道回调 \(onUserOffline\)](#onUserOffline)
- [获取音频裸数据回调 \(audioFrameReceived\)](#audioFrameReceived)
- [获取视频裸数据回调 \(videoFrameReceived\)](#videoFrameReceived)
- [启用说话者提示 \(onActiveSpeaker\)](#onActiveSpeaker)

<a name = "onError"></a>

<a name = "onError"></a>

### 发生错误回调 \(onError\)

```
virtual void onError(int error, agora::linuxsdk::STAT_CODE_TYPE stat_code) = 0;
```

表示 SDK 运行时出现了（网络或媒体相关的）错误。通常情况下，SDK 上报的错误意味着 SDK 无法自动恢复，需要 APP 干预或提示用户。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>error</code></td>
<td><p>错误代码</p>
<div><ul>
<li>ERR_OK = 0：没有错误</li>
<li>ERR_FAILED = 1：一般性的错误（没有明确归类的错误原因）</li>
<li>ERR_INVALID_ARGUMENT = 2：API 调用了无效的参数。例如指定的频道名含有非法字符</li>
<li>ERR_INTERNAL_FAILED = 3：SDK 内部启动失败</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>stat_code</code></td>
<td><p>状态代码</p>
<div><ul>
<li>STAT_OK = 0：一切正常</li>
<li>STAT_ERR_FROM_ENGINE = 1：Engine 产生的错误</li>
<li>STAT_ERR_ARS_JOIN_CHANNEL = 2：加入频道失败</li>
<li>STAT_ERR_CREATE_PROCESS = 3：创建进程失败</li>
<li>STAT_ERR_MIXED_INVALID_VIDEO_PARAM = 4：合图参数失败</li>
<li>STAT_ERR_NULL_POINTER = 5：无效的指针</li>
<li>STAT_POLL_ERR = 0x8：轮询出错</li>
<li>STAT_POLL_HANG_UP = 0x10：轮询挂起</li>
<li>STAT_POLL_NVAL = 0x20：轮询请求无效</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

<a name = "onWarning"></a>

### 发生警告回调 \(onWarning\)

```
virtual void onWarning(int warn) = 0;
```

该回调方法表示 SDK 运行时出现了（网络或媒体相关的）警告。通常情况下，SDK 上报的警告信息应用程序可以忽略，SDK 会自动恢复。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>warn</code></td>
<td><p>警告代码：</p>
<ul>
<li>WARN_NO_AVAILABLE_CHANNEL = 103：没有可用的频道资源。可能是因为服务端没法分配频道资源</li>
<li>WARN_LOOKUP_CHANNEL_TIMEOUT = 104：查找频道超时。在加入频道时SDK先要查找指定的频道，出现该警告一般是因为网络太差，连接不到服务器</li>
<li>WARN_LOOKUP_CHANNEL_REJECTED = 105：查找频道请求被服务器拒绝。服务器可能没有办法处理这个请求或请求是非法的</li>
<li>WARN_OPEN_CHANNEL_TIMEOUT = 106：打开频道超时。查找到指定频道后，SDK接着打开该频道，超时一般是因为网络太差，连接不到服务器</li>
<li>WARN_OPEN_CHANNEL_REJECTED = 107: 打开频道请求被服务器拒绝。服务器可能没有办法处理该请求或该请求是非法的</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "onJoinChannelSuccess"></a>

### 加入频道回调 \(onJoinChannelSuccess\)

```
virtual void onJoinChannelSuccess(const char * channelId, uid_t uid) = 0;
```

该回调方法表示该客户端成功加入了指定的频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelId</code></td>
<td>和调用 <em><code>joinChannel()</code></em> 时设置的频道名一致</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 uid。</td>
</tr>
</tbody>
</table>

<a name = "onLeaveChannel"></a>

### 离开频道回调 \(onLeaveChannel\)

```
virtual void onLeaveChannel(agora::linuxsdk::LEAVE_PATH_CODE code) = 0;
```

此回调提示离开频道成功。在该回调方法中，应用程序可以得到此次通话的总通话时长、SDK 收发数据的流量等信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>code</code></td>
<td>
<div>原因：</div>
<ul>
<li>LEAVE_CODE_INIT(0)：初始化失败</li>
<li>LEAVE_CODE_SIG(1&lt;&lt;1)：由信号触发的退出</li>
<li>LEAVE_CODE_NO_USERS(1&lt;&lt;2)：频道内除录制外，没有其他用户</li>
<li>LEAVE_CODE_TIMER_CATCH(1&lt;&lt;3)：捕获到信号错误</li>
<li>LEAVE_CODE_CLIENT_LEAVE(1&lt;&lt;4)：wrapper 层主动退出</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "onUserJoined"></a>

### 其他用户加入当前频道回调 \(onUserJoined\)

```
virtual void onUserJoined(uid_t uid, agora::linuxsdk::UserJoinInfos &infos) = 0;
```

此回调提示有用户加入了频道。

如果该客户端加入频道时已经有人在频道中，SDK 也会向应用程序上报这些已在频道中的用户。频道内有多少用户，该回调就会调用几次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 uid</td>
</tr>
<tr><td><code>infos</code></td>
<td>用户加入频道信息</td>
</tr>
</tbody>
</table>



`UserJoinInfos` 结构如下:

```
typedef struct UserJoinInfos {
    const char* storageDir;
    //new attached info add below

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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>storageDir</code></td>
<td>录制文件所在的目录</td>
</tr>
</tbody>
</table>

<a name = "onUserOffline"></a>

### 其他用户离开当前频道回调 \(onUserOffline\)

```
virtual void onUserOffline(uid_t uid, agora::linuxsdk::USER_OFFLINE_REASON_TYPE reason) = 0;
```

提示有用户离开了频道（或掉线）。

SDK 判断用户离开频道（或掉线）的依据是：在一定时间内（15 秒）没有收到对方的任何数据包。 在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 uid</td>
</tr>
<tr><td><code>reason</code></td>
<td><p>离线原因</p>
<ul>
<li>USER_OFFLINE_QUIT = 0：用户主动离开</li>
<li>USER_OFFLINE_DROPPED = 1：因过长时间收不到对方数据包，超时掉线。注意：可能有误判</li>
<li>USER_OFFLINE_BECOME_AUDIENCE = 2： 用户身份从主播切换为观众时触发。该选项仅适用于当你在调用 joinChannel() 时将频道模式设置为直播的场景</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "audioFrameReceived"></a>

### 获取音频裸数据回调 \(audioFrameReceived\)

```
virtual void audioFrameReceived(unsigned int uid, const agora::linuxsdk::AudioFrame *frame) const = 0;
```

当返回音频裸数据时，会触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 uid</td>
</tr>
<tr><td><code>frame</code></td>
<td>返回的音频裸数据，格式为 pcm 或 aac</td>
</tr>
</tbody>
</table>



`AudioFrame` 结构如下：

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

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>avsync_type</code></td>
<td><p>音画同步参数</p>
<ul>
<li>UNKNOWN_AVSYNC = -1, 音画同步报错</li>
<li>AVSYNC_V0 = 0, 和老版本兼容</li>
<li>AVSYNC_V1 = 1, 新版本的音画同步选项</li>
</ul>
</td>
</tr>
<tr><td><code>mType</code></td>
<td><p>存储方式：</p>
<div><ul>
<li>STACK_MEM_TYPE = 0, 栈</li>
<li>HEAP_MEM_TYPE = 1, 堆</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### AudioPcmFrame

`AudioPcmFrame `结构如下：

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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>frame_ms</code></td>
<td>该帧的时间戳</td>
</tr>
<tr><td><code>channels</code></td>
<td>声道数量，例如双声道等</td>
</tr>
<tr><td><code>sample_bits</code></td>
<td>采样的数据位宽</td>
</tr>
<tr><td><code>sample_rates</code></td>
<td>采样率</td>
</tr>
<tr><td><code>samples</code></td>
<td>该帧的采样样本的数量</td>
</tr>
<tr><td><code>pcmBuf</code></td>
<td>音频帧缓冲区</td>
</tr>
<tr><td><code>pcmBufSize</code></td>
<td>音频帧缓冲区的大小</td>
</tr>
</tbody>
</table>



#### AudioAacFrame

`AudioAacFrame `结构如下：

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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>frame_ms</code></td>
<td>该帧的时间戳</td>
</tr>
<tr><td><code>aacBuf</code></td>
<td>音频祯缓冲区</td>
</tr>
<tr><td><code>aacBuffSize</code></td>
<td>音频祯缓冲区大小</td>
</tr>
</tbody>
</table>

<a name = "videoFrameReceived"></a>

### 获取视频裸数据回调 \(videoFrameReceived\)

```
virtual void videoFrameReceived(unsigned int uid, const agora::linuxsdk::VideoFrame *frame) const = 0;
```

当返回视频裸数据时，会触发该回调。该回调可用于实现高级功能，如鉴黄。这些功能可以通过采集并分析 I 帧实现。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 uid</td>
</tr>
<tr><td><code>frame</code></td>
<td>返回的视频裸数据，格式为 YUV、H264 或 JPG</td>
</tr>
</tbody>
</table>



`VideoFrame` 结构如下：

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

    avsyncType avsync_type_;
    MEMORY_TYPE mType;
};
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>avsync_type</code></td>
<td><p>音画同步参数</p>
<ul>
<li>UNKNOWN_AVSYNC = -1, 音画同步报错</li>
<li>AVSYNC_V0 = 0, 和老版本兼容</li>
<li>AVSYNC_V1 = 1, 新版本的音画同步选项</li>
</ul>
</td>
</tr>
<tr><td><code>mType</code></td>
<td><p>存储方式：</p>
<div><ul>
<li>STACK_MEM_TYPE = 0, 栈</li>
<li>HEAP_MEM_TYPE = 1, 堆</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### VideoYuvFrame

`VideoYuvFrame `结构如下：

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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>frame_ms</code></td>
<td>该帧的时间戳</td>
</tr>
<tr><td><code>ybuf</code></td>
<td>指向 YUV 数据中的 Y 缓冲区指针</td>
</tr>
<tr><td><code>ubuf</code></td>
<td>指向 YUV 数据中的 U 缓冲区指针</td>
</tr>
<tr><td><code>vbuf</code></td>
<td>指向 YUV 数据中的 V 缓冲区指针</td>
</tr>
<tr><td><code>width</code></td>
<td>视频像素宽度</td>
</tr>
<tr><td><code>height</code></td>
<td>视频像素高度</td>
</tr>
<tr><td><code>ystride</code></td>
<td>YUV 数据中的 Y 缓冲区的行跨度</td>
</tr>
<tr><td><code>ustride</code></td>
<td>YUV 数据中的 U 缓冲区的行跨度</td>
</tr>
<tr><td><code>vstride</code></td>
<td>YUV 数据中的 V 缓冲区的行跨度</td>
</tr>
<tr><td><code>buf`</code></td>
<td>视频帧缓冲区</td>
</tr>
<tr><td><code>bufSize</code></td>
<td>频帧缓冲区的大小</td>
</tr>
</tbody>
</table>



#### VideoH264Frame

`VideoH264Frame `结构如下:

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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>frame_ms</code></td>
<td>该帧的时间戳</td>
</tr>
<tr><td><code>frame_num</code></td>
<td>帧的序号</td>
</tr>
<tr><td><code>buf</code></td>
<td>视频帧缓冲区</td>
</tr>
<tr><td><code>bufSize</code></td>
<td>视频帧缓冲区的大小</td>
</tr>
</tbody>
</table>



#### VideoJpgFrame

`VideoJpgFrame `结构如下：

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
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>frame_ms</code></td>
<td>该帧的时间戳</td>
</tr>
<tr><td><code>buf</code></td>
<td>视频帧缓冲区</td>
</tr>
<tr><td><code>bufSize</code></td>
<td>视频帧缓冲区的大小</td>
</tr>
</tbody>
</table>

<a id="onactivespeaker-recording-cpp"></a>	
​	
<a name = "onActiveSpeaker"></a>	

### 启用说话者提示 \(onActiveSpeaker\)

```
virtual void onActiveSpeaker(uid_t uid);
```

返回说话者的 uid 。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>说话者的 uid</td>
</tr>
</tbody>
</table>



## 错误代码和警告代码

相关错误代码见 [发生错误回调 \(onError\)](#onError)。更多详见 [错误代码和警告代码](https://docs.agora.io/cn/recording/API%20Reference/the_error_native)。
