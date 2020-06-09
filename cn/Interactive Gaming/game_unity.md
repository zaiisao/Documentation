
---
title: 游戏 API
description: 
platform: Unity
updatedAt: Sun Jun 07 2020 06:20:10 GMT+0800 (CST)
---
# 游戏 API
<div class="alert note">互动游戏 Unity SDK 将不再维护，请使用 RTC Unity SDK 替代。详情见<a href="https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/unity/index.html">API 参考</a >。</div>

本文提供基于 C\# 语言的游戏音视频 API 描述，包括以下类:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><a href="#irtcengine">IRtcEngine 抽象类</a></td>
<td>包含应用程序需调用的主要方法</td>
</tr>
<tr><td><a href="#iaudioeffectmanager">IAudioEffectManager 接口类</a></td>
<td>包含应用程序用于管理音效的方法</td>
</tr>
</tbody>
</table>


<a id = "irtcengine"></a>
## IRtcEngine 抽象类

### 创建引擎实例

#### 创建并获取引擎实例 (GetEngine)

```
public static IRtcEngine GetEngine (string appId);
```

该方法获取引擎实例。如果暂无引擎实例，调用该方法会自动创建一个。 当该方法执行成功时，将返回 IRtcEngine 实例，而非空值。

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
<td>App ID，详见<a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a></td>
</tr>
</tbody>
</table>



#### 查询引擎实例 (QueryEngine)

```
public static IRtcEngine QueryEngine();
```

该方法查询引擎实例。与 `GetEngine` 不同的是，如果没有现成的实例，该方法不会自动创建一个实例。

<a name = "voice"></a>
### 实现语音功能

#### 设置频道属性 (SetChannelProfile)

```
public int SetChannelProfile(CHANNEL_PROFILE profile);
```

该方法用于设置频道模式 (Profile)。Agora RtcEngine 需知道应用程序的使用场景, 从而使用不同的优化手段。


> -   同一频道内只能同时设置一种模式。
> -   该方法必须在加入频道前调用和进行设置，进入频道后无法再设置。


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>profile</code></td>
<td><p>频道模式:</p>
<ul>
<li>CHANNEL_PROFILE_GAME_FREE_MODE = 0: 自由发言模式。频道中的所有用户都可以自由发言</li>
<li>CHANNEL_PROFILE_GAME_COMMAND_MODE = 1: 指挥模式。频道中有指挥和观众两种用户角色。指挥相当于主播，可以发送和接收音视频流；观众只能接收音视频流</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 设置用户角色 (SetClientRole)

```
public int SetClientRole(CLIENT_ROLE role);
```

该方法用于加入频道前设置用户角色，同时允许用户在加入频道后切换角色。

> 该方法仅适用于: 您已通过调用 `SetChannelProfile` 将频道模式设置为指挥模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>指挥模式频道的用户角色:</p>
<ul>
<li>CLIENT_ROLE_BROADCASTER = 1; 主播(既指挥者)，主播可以发言指挥</li>
<li>CLIENT_ROLE_AUDIENCE = 2; 观众(默认，被指挥者)，观众服从指挥</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a id = "joinChannel"></a>
#### 加入频道 (JoinChannel)

```
public int JoinChannel (string token, string channelName, string optionalInfo, uint optionalUid);
```

该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。 使用不同 App ID 的应用程序是不能互通的。如果已在通话中，用户必须调用 `leaveChannel` 退出当前通话，才能进入下一个频道。
 
> 同一个频道里不能出现两个相同的 UID。如果你的 App 支持多设备同时登录，即同一个用户账号可以在不同的设备上同时登录 (例如微信支持在 PC 端和移动端同时登录)，请保证传入的 UID 不相同。 例如你之前都是用同一个用户标识作为 UID, 建议从现在开始加上设备 ID, 以保证传入的 UID 不相同 。如果你的 App 不支持多设备同时登录，例如在电脑上登录时，手机上会自动退出，这种情况下就不需要在 UID 上添加设备 ID。

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
<td><ul>
<li>安全要求不高: 将值设为 null</li>
<li>安全要求高: 将值设置为 Token 值。 如果你已经启用了 App 证书, 请务必使用 Token。 关于如何获取 Token，详见<a href="../../cn/Quickstart%20Guide/token.md"><span>密钥说明</span></a> 。</li>
</ul>
</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）：
<ul>
<li>26 个小写英文字母 a-z</li>
<li>26 个大写英文字母 A-Z</li>
<li>10 个数字 0-9</li>
<li>空格</li>
<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","</li>
</ul></td>
</tr>
<tr><td><code>optionalInfo</code></td>
<td>(非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户</td>
</tr>
<tr><td><code>optionalUid</code></td>
<td><p>(非必选项) 用户 ID，32位无符号整数。建议设置范围：1 到(2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</p>
<p>uid 在 SDK 内部用 32 位无符号整数表示，由于 Java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，Java 会表示为负数，如有需要可以用(uid&amp;0xffffffffL)转换成 64 位整数</p>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2)：传递的参数无效</li>
<li>ERR_NOT_READY (-3)：没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 打开音频 (EnableAudio)

```
public int EnableAudio();
```

该方法打开音频 (默认为打开)。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 关闭音频 (DisableAudio)

```
public int DisableAudio();
```

该方法关闭音频。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 离开频道 (LeaveChannel)

```
public int LeaveChannel();
```

该方法离开频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置语音路由

#### 修改语音路由的默认值（SetDefaultAudioRouteToSpeakerPhone）

```c#
public int SetDefaultAudioRouteToSpeakerphone(bool speakerphone);
```

该方法将语音路由的默认值修改为 **扬声器 (外放)** / **听筒**。插上耳机或连接蓝牙的动作对语音路由影响的优先级高于该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>speakerphone</code></td>
<td><ul>
<li>True: 将语音路由设置为扬声器(外放)</li>
<li>False: 将语音路由设置为听筒</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

> -   在 Unity for iOS 中，该方法只在纯音频模式下工作，在有视频的模式下不工作。
> -   如果插上耳机或连接蓝牙，语音路由会发生相应改变。拔出耳机或断开蓝牙后，语音路由将恢复成默认值（自由说话模式下为外放）。


语音路由的默认值如下表:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>频道模式</strong></td>
<td><strong>默认语音路由</strong></td>
</tr>
<tr><td>自由说话模式</td>
<td>听筒</td>
</tr>
<tr><td>指挥模式</td>
<td>外放</td>
</tr>
</tbody>
</table>



#### 将语音路由设置为扬声器外放 (SetEnableSpeakerphone)

```
public int SetEnableSpeakerphone (bool speakerphone);
```

该方法将语音路由设置为 **扬声器(外放)**。该方法对语音路由影响的优先级高于插上耳机或连接蓝牙的动作。

调用该方法后，SDK 将返回 `onAudioRouteChanged` 回调提示状态已更改。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>speakerphone</code></td>
<td><p>是否将语音路由设置为扬声器(外放)：</p>
<div><ul>
<li><p>True:</p>
<div><ul>
<li>如果用户已在频道内，调用该 API 后，会切换到从扬声器(外放)出声</li>
<li>如果用户尚未加入频道，调用该 API 后，在加入频道时会默认从扬声器(外放)出声</li>
</ul>
</div>
</li>
<li><p>False: 语音会根据语音路由的默认值出声，详见 <code>setDefaultAudioRouteToSpeakerPhone</code></p>
</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 1: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 使用该方法后，插上耳机或连接蓝牙等动作不影响语音路由。

#### 是否将语音路由设置为扬声器外放 \(IsSpeakerphoneEnabled\)

```
public bool IsSpeakerphoneEnabled();
```

该方法检查是否将语音路由设置为扬声器外放。

> 该方法只在 Unity for iOS 和 Unity for Android 平台有效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置语音音量

#### 启用说话者音量提示 (EnableAudioVolumeIndication)

```
public int EnableAudioVolumeIndication (int interval, int smooth);
```

该方法启用或禁用音量提示。该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>interval</code></td>
<td><p>指定音量提示的时间间隔。</p>
<div><ul>
<li>&lt;= 0: 禁用音量提示功能</li>
<li>&gt; 0: 提示间隔，单位为毫秒。建议设置到大于 200 毫秒</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>smooth</code></td>
<td>平滑系数。默认可以设置为 3。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 暂停发送音视频流

<a name = "enableLocalAudio"></a>
#### 开关本地音频采集 (EnableLocalAudio)

```
public int EnableLocalAudio (bool enabled);
```

当 App 加入频道时，语音功能默认是开启的。该方法可以关闭或重新开启本地语音功能，停止或重新开始本地音频采集及处理。
语音功能关闭或重新开启后，会收到回调 [OnMicrophoneEnabledHandler](#onMicrophoneEnabledHandler)。
该方法不影响接收或播放远端音频流，适用于只听不发的用户场景。

> - 该方法需要在加入频道后调用才能生效。 
> - 该方法与  [MuteLocalAudioStream](#muteLocalAudioStream) 的区别在于：
>   - [EnableLocalAudio](#enableLocalAudio)：开启或关闭本地语音采集及处理 
>   - [MuteLocalAudioStream](#muteLocalAudioStream)：停止或继续发送本地音频流

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enabled</code></td>
<td><ul>
<li>True: 本地语音功能开启</li>
<li>False: 本地语音功能关闭</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

<a name = "muteLocalAudioStream"></a>
#### 将自己静音 (MuteLocalAudioStream)

```
public int MuteLocalAudioStream (bool mute);
```

静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。
> 该方法只在频道内有效。离开频道后，之前设置的静音状态会全部清除，请开发者自行检查代码逻辑。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 麦克风静音</li>
<li>False: 取消静音</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 静音所有远端音频 (MuteAllRemoteAudioStreams)

```
public int MuteAllRemoteAudioStreams (bool mute);
```

该方法用于允许/禁止播放远端用户的音频流，即对所有远端用户进行静音与否。
> 该方法只在频道内有效。离开频道后，之前设置的静音状态会全部清除，请开发者自行检查代码逻辑。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 停止播放所有收到的音频流</li>
<li>False: 允许播放所有收到的音频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 静音指定用户音频 (MuteRemoteAudioStream)

```
public int MuteRemoteAudioStream (uint uid, bool mute);
```

静音指定远端用户/对指定远端用户取消静音。本方法用于允许/禁止播放远端用户的音频流。
> 该方法只在频道内有效。离开频道后，之前设置的静音状态会全部清除，请开发者自行检查代码逻辑。

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
<td>指定用户的 UID</td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 停止播放指定用户的音频流</li>
<li>False: 允许播放指定用户的音频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "setDefaultMuteAllRemoteVideoStreams"></a>

#### 默认接收音频流 (SetDefaultMuteAllRemoteAudioStreams)

```c#
public int SetDefaultMuteAllRemoteAudioStreams(bool mute);
```

设置是否默认接收所有的远端音频流。

> 该方法在加入频道前后都可调用。如果在加入频道后调用，会接收不到后面加入频道的用户的音频流。

<table>
<thead>
<td>名称</td>
<td>描述</td>
</thead> 
<tbody>
<tr>
<td><code>mute</code></td>
<td>
是否默认不接收所有远端音频：
<ul>
<li>True：默认不接收所有远端音频流</li>
<li>False：默认接收所有远端音频流（默认）</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td>
<ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 查询 SDK 版本号 (GetSdkVersion)

```c#
public static string GetSdkVersion ();
```

该方法返回 SDK 版本号的字符串 (char 格式)。

#### 获取错误描述 (GetErrorDescription)

```
public static string GetErrorDescription (int code);
```

SDK 运行时如果出错，该方法可以获取错误码。

### 伴奏

#### 开始客户端本地混音 (StartAudioMixing)

```
public int StartAudioMixing (string filePath, bool loopback, bool replace, int cycle, int playTime = 0);
```

指定本地音频文件来和麦克风采集的音频流进行混音和替换 (用音频文件替换麦克风采集的音频流)，可以通过参数选择是否让对方听到本地播放的音频和指定循环播放的次数。

> 请在频道内调用该方法，如果在频道外调用该方法可能会出现问题。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filePath</code></td>
<td><p>指定需要混音的本地音频文件名和文件路径名:</p>
<ul>
<li>如果用户提供的目录以 /assets/ 开头，则去 assets 里面查找该文件</li>
<li>如果用户提供的目录不是以 /assets/ 开头，一律认为是在绝对路径里查找该文件</li>
</ul>
<p>支持以下音频格式: mp3，aac，m4a, 3gp, wav, flac</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>loopback</code></td>
<td><ul>
<li>True: 只有本地可以听到混音或替换后的音频流</li>
<li>False: 本地和对方都可以听到混音或替换后的音频流</li>
</ul>
</td>
</tr>
<tr/>
<tr><td><code>replace</code></td>
<td><ul>
<li>True: 音频文件内容将会替换本地录音的音频流</li>
<li>False: 音频文件内容将会和麦克风采集的音频流进行混音</li>
</ul>
</td>
</tr>
<tr/>
<tr><td><code>cycle</code></td>
<td><p>指定音频文件循环播放的次数:</p>
<ul>
<li>正整数: 循环的次数</li>
<li>-1：无限循环</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 停止客户端本地混音 (StopAudioMixing)

```
public int StopAudioMixing();
```

使用该方法停止伴奏播放。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 暂停伴奏播放 (PauseAudioMixing)

```
public int PauseAudioMixing();
```

使用该方法暂停伴奏播放。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 恢复伴奏播放 (ResumeAudioMixing)

```
public int ResumeAudioMixing();
```

使用该方法恢复混音，继续播放伴奏。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 调节伴奏音量 (AdjustAudioMixingVolume)

```
public int AdjustAudioMixingVolume (int volume);
```

使用该方法调节混音里伴奏的音量大小。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td>伴奏音量范围为 0~100，默认 100 为原始文件音量</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 获取伴奏时长 (GetAudioMixingDuration)

```
public int GetAudioMixingDuration();
```

该方法获取伴奏时长，单位为毫秒。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 获取伴奏播放进度 (GetAudioMixingCurrentPosition)

```
public int GetAudioMixingCurrentPosition();
```

该方法获取当前伴奏播放进度，单位为毫秒。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 录制

#### 开始客户端录音 (StartAudioRecording)

```
public int StartAudioRecording(string filePath);
```

Agora SDK 支持通话过程中在客户端进行录音，且录音文件格式可以为:

-   .wav : 文件大，音质保真度高
-   .aac : 文件小，有一定的音质保真度损失


确保应用程序里指定的目录存在且可写。该接口需在加入频道之后调用。如果调用 `LeaveChannel` 时还在录音，录音会自动停止。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filePath</code></td>
<td>录音文件路径名。该文件名字符串为 UTF-8 编码</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 停止客户端录音 (StopAudioRecording)

```
public int StopAudioRecording();
```

该方法停止客户端录音。该接口需要在调用 `LeaveChannel` 之前调用，如果没有调用，在调用 `LeaveChannel` 时会自动停止。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 调节录音信号音量 (AdjustRecordingSignalVolume)

```
public int AdjustRecordingSignalVolume (int volume);
```

该方法调节录音信号音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td><p>录音信号音量可在 0~400 范围内进行调节:</p>
<div><ul>
<li>0: 静音</li>
<li>100: 原始音量</li>
<li>400: 最大可为原始音量的4倍(自带溢出保护)</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 调节播放信号音量 \(AdjustPlaybackSignalVolume\)

```
public int AdjustPlaybackSignalVolume (int volume);
```

该方法调节播放信号音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td><p>录音信号音量可在 0~400 范围内进行调节:</p>
<div><ul>
<li>0: 静音</li>
<li>100: 原始音量</li>
<li>400: 最大可为原始音量的4倍(自带溢出保护)</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 启动语音通话测试 (StartEchoTest)

```
public int StartEchoTest();
```

该方法启动语音通话测试，目的是测试系统的音频设备（耳麦、扬声器等）和网络连接是否正常。 在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。

> 调用 `StartEchoTest` 后必须调用 `StopEchoTest` 以结束测试，否则不能进行下一次回声测试，或者调用 `JoinChannel` 进行通话。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
<ul>
<li>ERR_REFUSED (-5): 不能启动测试，可能没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 终止语音通话测试 (StopEchoTest)

```
public int StopEchoTest();
```

该方法停止语音通话测试。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
<ul>
<li>ERR_REFUSED(-5): 停止 EchoTest 失败，可能是因为不在进行 EchoTest</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 获取通话 ID (GetCallId)

```
public string GetCallId();
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
<tr><td>返回值</td>
<td>当前通话 ID</td>
</tr>
</tbody>
</table>



#### 给通话评分 (Rate)

```
public int Rate(string callId, int rating, string desc);
```

该方法能够让用户为通话评分，一般在通话结束后调用。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>callId</code></td>
<td>从 <code>getCallId</code> 获取的通话 ID</td>
</tr>
<tr><td><code>rating</code></td>
<td>给通话的评分，最低 1 分，最高 10 分。</td>
</tr>
<tr><td><code>desc</code></td>
<td>[非必选项]给通话的描述，可选，长度应小于 800 字节。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): 传递的参数无效</li>
<li>ERR_NOT_READY (-3): 没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 投诉通话质量 (Complain)

```
public int Complain(string callId, string desc);
```

该方法让用户就通话质量进行投诉。一般在通话结束后调用。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>callId</code></td>
<td>从 <code>getCallId</code> 获取的通话 ID</td>
</tr>
<tr><td><code>desc</code></td>
<td>[非必选项]给通话的描述，可选，长度应小于 800 字节。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): 传递的参数无效</li>
<li>ERR_NOT_READY (-3): 没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 暂停语音 (Pause)

```
public void Pause ();
```

该方法暂停语音，例如: 当进入后台模式，你可以调用该 API 暂停语音。

#### 恢复语音 (Resume)

```
public void Resume ();
```

该方法恢复暂停的语音。

> `Pause` 与 `Resume` 和 `EnableLocalAudio` 方法耦合。例如,使用 EnableLocalAudio(false) 禁用本地音频采集时，调用 `Resume` 会开启本地音频，请开发者自行检查代码逻辑。

#### 销毁引擎实例 (Destroy)

```
public static void Destroy();
```

该方法释放 Agora SDK 使用的所有资源。有些应用程序只在用户需要时才使用音视频功能，不需要时则将资源释放出来用于其他操作，该方法对这类程序可能比较有用。

> - 该方法需要在子线程中操作.
> - 该方法为同步调用。在等待 <a href="#irtcengine">IRtcEngine 对象</a>资源释放后再返回。APP 不应该在 SDK 产生的回调中调用该接口，否则由于 SDK 要等待回调返回才能回收相关的对象资源，会造成死锁。

### 实现视频功能

在实现视频功能前，请先阅读 [实现语音功能](#voice) ，因为 [实现语音功能](#voice) 里的 API 也是实现视频功能的基础。

#### 打开视频模式 (EnableVideo)

```
public int EnableVideo ();
```

该方法用于打开视频模式。可以在加入频道前或者通话中调用，在加入频道前调用，则自动开启视频模式，在通话中调用则由音频模式切换为视频模式。调用 `DisableVideo` 方法可关闭视频模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 关闭视频模式 (DisableVideo)

```
public int DisableVideo ();
```

该方法用于关闭视频。可以在加入频道前或者通话中调用，在加入频道前调用，则自动开启纯音频模式，在通话中调用则由视频模式切换为纯音频模式。调用 `EnableVideo` 方法可开启视频模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 设置视频属性 (SetVideoProfile)

```
public int SetVideoProfile(int profile, bool swapWidthAndHeight);
```

该方法设置视频编码属性（Profile）。每个属性对应一套视频参数，如分辨率、帧率、码率等。 当设备的摄像头不支持指定的分辨率时，SDK 会自动选择一个合适的摄像头分辨率，但是编码分辨率仍然用 `SetVideoProfile` 指定的。

该方法仅设置编码器编出的码流属性，可能跟最终显示的属性不一致，例如编码码流分辨率为 640x480，码流的旋转属性为 90 度，则显示出来的分辨率为竖屏模式。

> -   应在调用 `EnableVideo` 后设置视频属性
> -   应在调用 `joinChannel`/`startPreview` 前设置视频属性


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>profile</code></td>
<td>视频属性（Profile）。详见下表的定义。Profile 取值定义在下表</td>
</tr>
<tr><td><code>swapWidthAndHeight</code></td>
<td><p>是否交换宽和高:</p>
<ul>
<li>True:交换宽和高</li>
<li>False：不交换宽和高(默认)</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



**视频属性（Profile）定义**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>视频属性</strong></td>
<td><strong>枚举值</strong></td>
<td><strong>分辨率(宽x高)</strong></td>
<td><strong>帧率(fps)</strong></td>
<td><strong>码率(kbps)</strong></td>
</tr>
<tr><td>120P</td>
<td>0</td>
<td>160x120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_3</td>
<td>2</td>
<td>120x120</td>
<td>15</td>
<td>50</td>
</tr>
<tr><td>180P</td>
<td>10</td>
<td>320x180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_3</td>
<td>12</td>
<td>180x180</td>
<td>15</td>
<td>100</td>
</tr>
<tr><td>180P_4</td>
<td>13</td>
<td>240x180</td>
<td>15</td>
<td>120</td>
</tr>
<tr><td>240P</td>
<td>20</td>
<td>320x240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_3</td>
<td>22</td>
<td>240x240</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>240P_4</td>
<td>24</td>
<td>424x240</td>
<td>15</td>
<td>220</td>
</tr>
<tr><td>360P</td>
<td>30</td>
<td>640x360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_3</td>
<td>32</td>
<td>360x360</td>
<td>15</td>
<td>260</td>
</tr>
<tr><td>360P_4</td>
<td>33</td>
<td>640x360</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>360P_6</td>
<td>35</td>
<td>360x360</td>
<td>30</td>
<td>400</td>
</tr>
<tr><td>360P_7</td>
<td>36</td>
<td>480x360</td>
<td>15</td>
<td>320</td>
</tr>
<tr><td>360P_8</td>
<td>37</td>
<td>480x360</td>
<td>30</td>
<td>490</td>
</tr>
<tr><td>360P_9</td>
<td>38</td>
<td>640x360</td>
<td>15</td>
<td>800</td>
</tr>
<tr><td>360P_10</td>
<td>39</td>
<td>640x360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>360P_11</td>
<td>100</td>
<td>640x360</td>
<td>24</td>
<td>1000</td>
</tr>
<tr><td>480P</td>
<td>40</td>
<td>640x480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_3</td>
<td>42</td>
<td>480x480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>43</td>
<td>640x480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>45</td>
<td>480x480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>47</td>
<td>848x480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>48</td>
<td>848x480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>720P</td>
<td>50</td>
<td>1280x720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_3</td>
<td>52</td>
<td>1280x720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>54</td>
<td>960x720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>55</td>
<td>960x720</td>
<td>30</td>
<td>1380</td>
</tr>
<tr><td>1080P</td>
<td>60</td>
<td>1920x1080</td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_3</td>
<td>62</td>
<td>1920x1080</td>
<td>30</td>
<td>3150</td>
</tr>
<tr><td>1080P_5</td>
<td>64</td>
<td>1920x1080</td>
<td>60</td>
<td>4780</td>
</tr>
</tbody>
</table>

 
> 视频能否达到 1080P 的分辨率取决于设备的性能，在性能配备较低的设备上有可能无法实现。如果采用 1080P 分辨率而设备性能跟不上，则有可能出现帧率过低的情况。

#### 使用双流/单流模式 (EnableDualStreamMode)

```
public int EnableDualStreamMode(bool enabled);
```

使用该方法设置单流（默认）或者双流直播模式，只有当你在调用 `setChannelProfile` 将频道设为指挥模式时，该方法才有效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enabled</code></td>
<td><p>指定双流或者单流模式:</p>
<ul>
<li>True: 双流</li>
<li>False: 单流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 设置视频大小流 (SetRemoteVideoStreamType)

```
public int SetRemoteVideoStreamType(uint uid, int streamType);
```

当远端用户发送了双流 (视频大流和小流)时，使用该方法本地用户可以选择接收远端用户的视频大流还是小流。 使用该方法可以根据视频窗口的大小动态调整对应视频流的大小，以节约带宽和计算资源。

Agora SDK 默认收到视频大流。如需使用视频小流，调用本方法进行切换。

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
<td>用户 ID</td>
</tr>
<tr><td><code>streamType</code></td>
<td><p>设置视频流大小。视频流类型如下:</p>
<ul>
<li>REMOTE_VIDEO_STREAM_HIGH(0): 视频大流</li>
<li>REMOTE_VIDEO_STREAM_LOW(1): 视频小流</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 设置视频优化选项 (SetVideoQualityParameters)

```
public int SetVideoQualityParameters(bool preferFrameRateOverImageQuality);
```

设置视频优化选项。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>preferFrameRateOverImageQuality</code></td>
<td><p>支持两种优化选项:</p>
<div><ul>
<li>True: 画质和流畅度里，优先保证流畅度</li>
<li>False: 画质和流畅度里，优先保证画质(默认)</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 禁用本地视频功能 (EnableLocalVideo)

```
public int EnableLocalVideo (bool enabled);
```

禁用/启用本地视频功能。该方法用于只看不发的视频场景。该方法不需要本地有摄像头。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enabled</code></td>
<td><ul>
<li>True: 启用本地视频（默认）</li>
<li>False: 禁用本地视频</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 开启视频预览 (StartPreview)

```
public int StartPreview ();
```

使用该方法启动本地视频预览。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 停止视频预览 (StopPreview)

```
public int StopPreview ();
```

使用该方法停止本地视频预览。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 切换前置/后置摄像头 (SwitchCamera)

```
public int SwitchCamera();
```

使用该方法切换前置/后置摄像头。

> 该方法只在 Unity for iOS 和 Unity for Android 平台有效。

#### 直接发送图片给 App (EnableVideoObserver)

```
public int EnableVideoObserver ();
```

该方法直接将视频图片发送给 App，而无需经过传统的视图渲染器。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 禁止直接发送图片给 App (DisableVideoObserver)

```
public int DisableVideoObserver ();
```

该方法禁止直接将视频图片发送给 App。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 暂停发送本地视频流 (MuteLocalVideoStream)

```
public int MuteLocalVideoStream(bool mute);
```

暂停/恢复发送本地视频流。该方法用于允许/禁止往网络发送本地视频流。该方法不影响本地视频流获取，没有禁用摄像头。
> 该方法只在频道内有效。离开频道后，之前设置的静音状态会全部清除，请开发者自行检查代码逻辑。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 停止向网络发送本地视频流</li>
<li>False: 允许向网络发送本地视频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 暂停播放所有远端视频流 (MuteAllRemoteVideoStreams)

```
public int MuteAllRemoteVideoStreams(bool mute);
```

暂停/恢复所有人视频流。本方法用于允许/禁止播放所有人的视频流。
> 该方法只在频道内有效。离开频道后，之前设置的静音状态会全部清除，请开发者自行检查代码逻辑。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 停止播放所有收到的视频流</li>
<li>False: 允许播放所有收到的视频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 暂停指定远端视频流 (MuteRemoteVideoStream)

```
public int MuteRemoteVideoStream(uint uid, bool mute);
```

允许/禁止接收指定的远端视频流。
> 该方法只在频道内有效。离开频道后，之前设置的静音状态会全部清除，请开发者自行检查代码逻辑。

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
<td>指定远端用户的 UID</td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: 停止播放指定用户的视频流</li>
<li>False: 允许播放指定用户的视频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "setDefaultMuteAllRemoteVideoStreams"></a>

#### 默认接收视频流 (SetDefaultMuteAllRemoteVideoStreams)

```c#
public int SetDefaultMuteAllRemoteVideoStreams(bool mute);
```

设置是否默认接收所有的远端视频流。 

> 该方法在加入频道前后都可调用。如果在加入频道后调用，会接收不到后面加入频道的用户的视频流。

<table>
<thead>
<td>名称</td>
<td>描述</td>
</thead> 
<tbody>
<tr>
<td><code>mute</code></td>
<td>
是否默认不接收所有远端视频：
<ul>
<li>True：默认不接收所有远端视频流</li>
<li>False：默认接收所有远端视频流（默认）</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td>
<ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

### 加密

#### 启用内置的加密功能 (SetEncryptionSecret)

```
public int SetEncryptionSecret(string secret);
```

在加入频道之前，您的应用程序需调用 `SetEncryptionSecret` 指定 secret 来启用内置的加密功能，否则该通话未加密。 同一频道内的所有用户应设置相同的 secret。 当用户离开频道时，该频道的 secret 会自动清除。如果未指定 secret 或将 secret 设置为空，则无法激活加密功能。

<div class="alert note">Unity SDK 内默认无加密库。</div>

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>secret</code></td>
<td>加密密码</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 设置内置的加密方案 (SetEncryptionMode)

```
public int SetEncryptionMode(string encryptionMode);
```

Agora SDK 支持内置加密功能，默认使用 AES-128-XTS 加密方式。如需使用其他加密方式，可以调用该 API 设置。 同一频道内的所有用户必须设置相同的加密方式和 secret，才能进行通话。关于这几种加密方式的区别，请参考 AES 加密算法的相关资料。 调用本方法前，需调用 `SetEncryptionSecret` 启用内置的加密密码。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>encryptionMode</code></td>
<td><p>加密方式。目前支持以下几种:</p>
<div><ul>
<li><code>“aes-128-xts”</code>: 128 位 AES 加密，XTS 模式</li>
<li><code>“aes-128-ecb”</code>: 128 位 AES 加密，ECB 模式</li>
<li><code>“aes-256-xts”</code>: 256 位 AES 加密，XTS 模式</li>
<li><code>“”</code>: 设置为空字符串时，使用默认加密方式 “aes-128-xts”</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 其他

#### 设置日志过滤器 (SetLogFilter)

```
public int SetLogFilter (LOG_FILTER filter);
```

设置 SDK 的输出日志过滤器。不同的过滤器可以用或组合。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filter</code></td>
<td><p>设置过滤器等级:</p>
<div><ul>
<li>LOG_FILTER_OFF = 0;</li>
<li>LOG_FILTER_DEBUG = 0x80f;</li>
<li>LOG_FILTER_INFO = 0x0f;</li>
<li>LOG_FILTER_WARNING = 0x0e;</li>
<li>LOG_FILTER_ERROR = 0x0c;</li>
<li>LOG_FILTER_CRITICAL = 0x08;</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 设置日志文件 (SetLogFile)

```
public int SetLogFile (string filePath);
```

设置 SDK 输出的日志文件。SDK 运行时产生的所有日志将写入该文件。应用程序必须保证指定的目录存在而且可写。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>filepath</code></td>
<td>日志文件的完整路径</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 更新 Token (RenewToken\)

```
public override int RenewToken (string token);
```

该方法用于更新 Token。如果启用了 Token 机制，过一段时间后使用的 Token 会失效。

当：

-   `onError` 回调报告 ERR_TOKEN_EXPIRED (109) 时，
-   `onRequestToken` 回调报告 ERR_TOKEN_EXPIRED (109) 时，

应用程序应重新获取 Token，然后调用该 API 更新 Token，否则 SDK 无法和服务器建立连接。

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
<td>指定要更新的 Token</td>
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



### 回调

> 所有回调在主线程中返回。

#### 加入频道回调 (JoinChannelSuccessHandler)

```
public delegate void JoinChannelSuccessHandler (string channelName, uint uid, int elapsed);
```

该方法表示用户已成功加入指定频道。频道 ID 的分配是根据 `JoinChannel` API 中指定的频道名称。如果调用 `JoinChannel` 时并未指定用户 ID，服务器就会分配一个。

<table>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelName</code></td>
<td>频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 UID。如果 <code>joinChannel</code> 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从 <code>joinChannel</code> 开始到该事件产生的延迟（毫秒)</td>
</tr>
</tbody>
</table>



#### 重新加入频道回调 (ReJoinChannelSuccessHandler)

```
public delegate void ReJoinChannelSuccessHandler (string channelName, uint uid, int elapsed);
```

有时候由于网络原因，客户端可能会和服务器失去连接，SDK 会进行自动重连，自动重连成功后触发此回调方法。

<table>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelName</code></td>
<td>频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 UID, 如果调用 <code>JoinChannel</code> 时指定了 uid，则此处返回该 ID。不然请使用 Agora 服务器自动分配的 ID。</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从调用 <code>JoinChannel</code> 开始到该事件产生的延迟（毫秒)。</td>
</tr>
</tbody>
</table>



#### 网络中断回调 (ConnectionInterruptedHandler)

```
public delegate void ConnectionInterruptedHandler ();
```

在 SDK 和服务器失去了网络连接时，触发该回调。失去连接后，除非 APP 主动调用 `LeaveChannel`，SDK 会一直自动重连。

#### 连接丢失回调 (ConnectionLostHandler)

```
public delegate void ConnectionLostHandler ();
```

在 SDK 和服务器失去了网络连接后，会触发 `ConnectionInterruptedHandler` 回调，并自动重连。 在一定时间内（默认 10 秒）如果没有重连成功，触发 `ConnectionLostHandler` 回调。 除非 APP 主动调用 `LeaveChannel`，SDK 仍然会自动重连。

#### 其他用户加入当前频道回调 (UserJoinedHandler)

```
public delegate void UserJoinedHandler (uint uid, int elapsed);
```

提示有用户加入了频道。 如果该客户端加入频道时已经有人在频道中，SDK 也会向应用程序上报这些已在频道中的用户。

<table>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 UID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td><code>JoinChannel</code> 开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 其他用户离开当前频道回调 \(UserOfflineHandler\)

```
public delegate void UserOfflineHandler (uint uid, USER_OFFLINE_REASON reason);
```

提示有用户离开了频道（或掉线）。SDK 判断用户离开频道（或掉线）的依据是超时: 在一定时间内（15 秒）没有收到对方的任何数据包，判定为对方掉线。在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

<table>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>用户的 UID</td>
</tr>
<tr><td><code>reason</code></td>
<td><p>离线原因:</p>
<div><ul>
<li>USER_OFFLINE_QUIT: 用户主动离开</li>
<li>USER_OFFLINE_DROPPED: 因过长时间收不到对方数据包，超时掉线</li>
</ul>
<p>由于 SDK 使用的是不可靠通道，也有可能对方主动离开本方没收到对方离开消息而误判为超时掉线</p>
</div>
</td>
</tr>
</tbody>
</table>



#### 离开频道回调 (LeaveChannelHandler)

```
public delegate void LeaveChannelHandler (RtcStats stats);
```

离开频道，即挂断或退出通话。应用程序调用 `LeaveChannel` 方法时，SDK 提示应用程序离开频道成功。在该回调方法中，应用程序可以得到此次通话的总通话时长、SDK 收发数据的流量等信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>stat</code></td>
<td><p>通话相关的统计信息:</p>
<div><ul>
<li><code>duration</code>: 通话时长（秒），累计值</li>
<li><code>txBytes</code>: 发送字节数 (bytes), 累计值</li>
<li><code>rxBytes</code>: 接收字节数 (bytes), 累计值</li>
<li><code>txKBitRate</code>: 发送码率 (kbps), 瞬时值</li>
<li><code>rxKBitRate</code>: 接收码率 (kbps), 瞬时值</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



```
struct RtcStats {
                unsigned int duration;
                unsigned int txBytes;
                unsigned int rxBytes;
          unsigned short txKBitRate;
          unsigned short rxKBitRate;
        };
```

#### 启用说话者音量提示 (VolumeIndicationHandler)

```
public delegate void VolumeIndicationHandler (AudioVolumeInfo[] speakers, int speakerNumber, int totalVolume);
```

提示谁在说话及其音量。默认禁用。可以通过 `enableAudioVolumeIndication` 方法设置。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>speakers</code></td>
<td><p>说话者(数组)。每个 speaker() 包括:</p>
<ul>
<li><code>uid</code>: 说话者的用户 ID</li>
<li><code>volume</code>: 说话者的音量(0~255)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>speakerNumber</code></td>
<td>说话者数量</td>
</tr>
<tr><td><code>totalVolume</code></td>
<td>(混音后的) 总音量 (0~255)</td>
</tr>
</tbody>
</table>

<a name ="onFirstRemoteAudioFrameHandler"></a>

#### 接收到远端音频首帧回调 (OnFirstRemoteAudioFrameHandler)

```c#
public delegate void OnFirstRemoteAudioFrameHandler (uint userId, int elapsed);
```

接收到远端音频首帧时会触发该回调。

<table>
<thead>
<td>名称</td>
<td>描述</td>
</thead> 
<tbody>
<tr>
<td><code>userId</code></td>
<td>发送音频帧的远端用户的 ID</td>
</tr>
<tr>
<td><code>elapsed</code></td>
<td>从调用 <a href="#joinChannel">joinChannel</a> 方法直至该回调被触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>

<a name ="onFirstLocalAudioFrameHandler"></a>

#### 已发送本地音频首帧回调 (OnFirstLocalAudioFrameHandler)

```c#
public delegate void OnFirstLocalAudioFrameHandler (int elapsed);
```

已发送本地音频首帧时会触发该回调。

<table>
<thead>
<td>名称</td>
<td>描述</td>
</thead> 
<tbody>
<tr>
<td><code>elapsed</code></td>
<td>从调用 <a href="#joinChannel">joinChannel</a> 方法直至该回调被触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>


#### 用户静音回调 (UserMutedHandler)

```
public delegate void UserMutedHandler (uint uid, bool muted);
```

提示有其他用户将他的音频流静音/取消静音。

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
<td>用户的 UID</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: 该用户将音频静音</li>
<li>False: 该用户取消了音频静音</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 发生警告回调 (SDKWarningHandler)

```
public delegate void SDKWarningHandler (int warn, string msg);
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
<td>警告码</td>
</tr>
<tr><td><code>msg</code></td>
<td>警告消息</td>
</tr>
</tbody>
</table>



#### 发生错误回调 (SDKErrorHandler)

```
public delegate void SDKErrorHandler (int error, string msg);
```

表示 SDK 运行时出现了（网络或媒体相关的）错误。通常情况下，SDK 上报的错误意味着 SDK 无法自动恢复，需要 APP 干预或提示用户。 例如启动通话失败时，SDK 会上报 ERR_START_CALL 错误。APP 可以提示用户启动通话失败，并调用 `LeaveChannel` 退出频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>错误码</td>
</tr>
<tr><td><code>msg</code></td>
<td>错误消息</td>
</tr>
</tbody>
</table>



#### Rtc Engine 统计数据回调 (RtcStatsHandler)

```
public delegate void RtcStatsHandler (RtcStats stats);
```

该回调定期上报 Rtc Engine 的运行时的状态，每两秒触发一次。

#### 伴奏播放完成回调 (AudioMixingFinishedHandler)

```
public delegate void AudioMixingFinishedHandler ();
```

当伴奏文件播放完成时触发该回调。

#### 语音路由变更回调 (AudioRouteChangedHandler)

```
public delegate void AudioRouteChangedHandler (AUDIO_ROUTE route);
```

SDK 会通过该回调通知 App 语音路由状态已发生变化。该回调返回当前的语音路由已切换至听筒，外放 (扬声器)，耳机或蓝牙。

<a name = "onApiExecutedHandler"></a>

#### 已执行 API 方法回调 (OnApiExecutedHandler)

```c#
public delegate void OnApiExecutedHandler (int err, string api, string result);
```

执行API方法时会触发该回调。

<table>
<thead>
<td>名称</td>
<td>描述</td>
</thead> 
<tbody>
<tr>
<td><code>err</code></td>
<td>错误码。如果方法调用失败，会返回<a href="https://docs.agora.io/cn/Interactive%20Gaming/the_error_game?platform=All%20Platforms#errorcode">错误码</a>；如果返回 0，则表示方法调用成功</td>
</tr>
<tr>
<td><code>api</code></td>
<td>SDK 所调用的 API</td>
</tr>
<tr>
<td><code>result</code></td>
<td>SDK 调用 API 的调用结果</td>
</tr>
</tbody>
</table>



#### Token 过期回调 (RequestTokenHandler)

```
public delegate void RequestTokenHandler ();
```

在调用 `JoinChannel` 时如果指定了 Token，由于 Token 具有一定的时效，在通话过程中如果 Token 即将失效，SDK 会提前 30 秒触发该回调，提醒应用程序更新 Token。 当收到该回调时，用户需要重新在服务端生成新的 Token，然后调用 `RenewToken` 将新生成的 Token 传给 SDK。

#### 接收到远程视频并完成解码回调 (OnFirstRemoteVideoDecodedHandler)

```
public delegate void OnFirstRemoteVideoDecodedHandler (uint uid, int width, int height, int elapsed);
```

已完成远端视频首帧解码回调。

本地收到远端第一个视频帧并解码成功后，会触发该回调。有两种情况：

- 远端用户首次上线后发送视频
- 远端用户视频离线再上线后发送视频

其中，视频离线与用户离线不同。视频离线指本地在 15 秒内没有收到视频包，可能有如下原因：

- 远端用户离开频道
- 远端用户掉线
- 远端用户停止发送本地视频流（调用了 `muteLocalVideoStream` 方法）
- 远端用户关闭本地视频模块（调用了 `disableVideo` 方法）

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
<td>用户 ID，指定是哪个用户的视频流</td>
</tr>
<tr><td><code>width</code></td>
<td>视频流宽（像素）</td>
</tr>
<tr><td><code>height</code></td>
<td>视频流高（像素）</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从本地用户调用 <code>JoinChannel</code> 加入频道开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 视频大小已变更回调 (OnVideoSizeChangedHandler)

```
public delegate void OnVideoSizeChangedHandler (uint uid, int width, int height, int elapsed);
```

当指定用户的视频大小发生变化时会触发该回调。

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
<td>指定用户的 UID</td>
</tr>
<tr><td><code>width</code></td>
<td>视频流宽（像素）</td>
</tr>
<tr><td><code>height</code></td>
<td>视频流高（像素）</td>
</tr>
<tr><td><code>elapsed</code></td>
<td><code>JoinChannel</code> 开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>

<a name = onMicrophoneEnabledHandler></a>

#### 麦克风状态已改变回调 (OnMicrophoneEnabledHandler)

```
public delegate void OnMicrophoneEnabledHandler (bool isEnabled);
```

麦克风状态改变时会触发该回调。

<table>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>isEnabled</code></td>
<td>
<div><ul>
<li><code>True</code>: 麦克风已启用</li>
<li><code>False</code>: 麦克风已禁用</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

#### 上下麦回调 (onClientRoleChangedHandler)

```
public delegate void onClientRoleChangedHandler(int oldRole, int newRole);
```

直播场景下，当用户上下麦时会触发此回调，即主播切换为观众时，或观众切换为主播时。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>oldRole</code></td>
<td>切换前的角色</td>
</tr>
<tr><td><code>newRole</code></td>
<td>切换后的角色</td>
</tr>
</tbody>
</table>



#### 远端用户暂停发送视频流回调 (OnUserMuteVideoHandler)

```
public delegate void OnUserMuteVideoHandler (uint uid, bool muted);
```

远端用户暂停/恢复发送视频流时会触发此回调。

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
<td>远端用户的 UID</td>
</tr>
<tr><td><code>muted</code></td>
<td><p>远端用户操作：</p>
<div><ul>
<li>True: 远端用户暂停发送视频流</li>
<li>False: 远端用户恢复发送视频流</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


<a id = "iaudioeffectmanager"></a>
## IAudioEffectManager 接口类

### 设置仅限语音模式 (SetVoiceOnlyMode)

```
int SetVoiceOnlyMode(bool enable);
```

该方法设置 *仅限语音* 模式，即仅传输语音流，而不传输其他流，例如，敲键盘的声音等。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enable</code></td>
<td><ul>
<li>True: 启用 仅限语音 模式</li>
<li>False: 禁用 仅限语音 模式</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置远端用户的语音位置 (SetRemoteVoicePosition)

```
int SetRemoteVoicePosition(uint uid, double pan, double gain);
```

该方法设置远端用户的语音位置。

> 该方法只在耳机模式下有效，在外放模式下无效。

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
<td>远端用户的 UID</td>
</tr>
<tr><td><code>pan</code></td>
<td><p>设置是否改变音效的空间位置。取值范围为 [-1,1]:</p>
<ul>
<li>0: 表示该音效出现在正前方</li>
<li>-1: 表示该音效出现在左边</li>
<li>1: 表示该音效出现在右边</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>gain</code></td>
<td>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]: 默认值为 100.0。取值越小，音效的音量越低</td>
</tr>
</tbody>
</table>



### 设置本地语音音调 (SetLocalVoicePitch)

```
int SetLocalVoicePitch (double pitch);
```

该方法改变本地说话人声音的音调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>pitch</code></td>
<td>语音频率可以 [0.5, 2.0] 范围内设置。默认值为 1.0</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取音效音量 (GetEffectsVolume)

```
double GetEffectsVolume();
```

该方法获取音量的音量，范围为 [0.0, 100.0]。

### 设置音效音量 (SetEffectsVolume)

```
int SetEffectsVolume(double volume);
```

该方法设置音效的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>volume</code></td>
<td>取值范围为[0.0, 100.0]。 100.0 为默认值。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 播放音效 (PlayEffect)

```
int PlayEffect (int soundId,
                String filePath,
                bool loop = false,
                double pitch = 1.0D,
                double pan = 0.0D,
                double gain = 100.0D
);
```

该方法播放音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID <sup>[2]</sup></td>
</tr>
<tr><td><code>filePath</code></td>
<td>音效文件的绝对路径</td>
</tr>
<tr><td><code>loop</code></td>
<td><p>设置是否循环播放音效:</p>
<ul>
<li>True: 是的，循环播放音效</li>
<li>False: 不循环播放(默认)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pitch</code></td>
<td><p>设置音效的音效</p>
<p>取值范围为 [0.5，2]。默认值 1.0，表示不需要修改音调。取值越小，音调越低</p>
</td>
</tr>
<tr/>
<tr><td><code>pan</code></td>
<td><p>设置是否改变音效的空间位置。取值范围为 [-1, 1]:</p>
<ul>
<li>0: 表示该音效出现在正前方</li>
<li>-1: 表示该音效出现在左边</li>
<li>1: 表示该音效出现在右边</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>gain</code></td>
<td>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。默认值为 100.0。取值越小，音效的音量越低</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

> [2] 如果你已通过 `PreloadEffect` 将音效加载至内存，确保这里设置的 `soundId` 与 `PreloadEffect` 设置的 `soundId` 相同。

### 停止播放指定音效 (StopEffect)

```
int StopEffect(int soundId);
```

该方法停止播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止播放所有的音效 (StopAllEffects)

```
int StopAllEffects();
```

该方法停止播放所有的音效。

### 预加载音效 (PreloadEffect)

```
int PreloadEffect(int soundId, String filePath);
```

该方法将指定音效文件 (压缩的语音文件)预加载至内存。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td><code>filePath</code></td>
<td>音效文件的绝对路径</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 释放音效 (UnloadEffect)

```
int UnloadEffect(int soundId);
```

该方法将指定预加载的音效从内存里释放出来。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 暂停音效播放 (PauseEffect)

```
virtual int PauseEffect(int soundId);
```

该方法暂停播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止所有音效播放 (PauseAllEffects)

```
int PauseAllEffects();
```

该方法暂停播放所有音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 恢复播放指定音效 (ResumeEffect)

```
int ResumeEffect(int soundId);
```

该方法恢复播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 恢复播放所有音效 (ResumeAllEffects)

```
int ResumeAllEffects();
```

该方法恢复播放所有音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
	<tr><td><code>soundId</code></td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



## 错误码和警告码

详见 [错误码和警告码](../../cn/API%20Reference/the_error_game.md)。


