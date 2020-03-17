
---
title: 游戏裁剪版 API
description: 
platform: Unity
updatedAt: Mon Nov 25 2019 10:13:59 GMT+0800 (CST)
---
# 游戏裁剪版 API
本文涵盖了 Agora AMG SDK for Unity 的 API 使用参考，适用于 Unity for iOS, Android, 和 Windows 三个平台。

-   对于 Android 平台，需要申请以下系统权限：

     -   <uses-permission android:name=”android.permission.INTERNET” /\>
     -   <uses-permission android:name=”android.permission.RECORD\_AUDIO” /\>
     -   <uses-permission android:name=”android.permission.BLUETOOTH” /\>
     -   <uses-permission android:name=”android.permission.MODIFY\_AUDIO\_SETTINGS” /\>
     -   <uses-permission android:name=”android.permission.ACCESS\_NETWORK\_STATE” /\>
     -   <uses-permission android:name=”android.permission.ACCESS\_WIFI\_STATE” /\>
     -   <uses-permission android:name=”android.permission.WAKE\_LOCK” /\>
     -   <uses-permission android:name=”android.permission.WRITE\_EXTERNAL\_STORAGE” /\>
     -   <uses-permission android:name=”android.permission.READ\_PHONE\_STATE” /\>
     -   <uses-permission android:name=”android.permission.READ\_LOGS” /\>
 
-   对于 iOS 平台，需要包涵以下库/框架：

    -   `CoreTelephony.framework`
    -   `VideoToolbox.framework`
    -   `libresolv.tbd`
    -   `AgoraAudioKit.framework`
    -   `libagoraSdkCWrapper.a`
    -   `libopencore-amrnb.a`


本文提供基于 C# 语言的游戏音视频 API 描述，包括 IRtcEngine 抽象类。

IRtcEngine 抽象类包含了以下相关方法：

-   [引擎实例相关](#engine)

-   [频道相关](#channel)

-   [音频相关](#audio)

-   [暂停发送音视频流相关](#muteStream)

-   [其他功能](#other)

-   [回调](#callback)

<a name = "engine"></a>
### 创建引擎实例 (GetEngine)

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
<tr><td>appId</td>
<td>App ID， 详见 <a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a></td>
</tr>
</tbody>
</table>



### 查询引擎实例 (QueryEngine)

```
puiblic static IRtcEngine QueryEngine();
```

该方法查询引擎实例。与 `GetEngine` 不同的是，如果没有现成的实例，该方法不会自动创建一个实例。

<a name = "channel"></a>
### 设置频道属性 \(SetChannelProfile\)

```
public abstract int SetChannelProfile(CHANNEL_PROFILE profile);
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
<td><p>频道模式：</p>
<div><ul>
<li>CHANNEL_PROFILE_GAME_FREE_MODE = 2: 自由发言模式。在此模式下，频道内所有人没有角色区分。</li>
<li>CHANNEL_PROFILE_GAME_COMMAND_MODE = 3: 指挥模式。在此模式下，频道内区分主播和观众。只有主播能够在频道内发言。</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置用户角色 (setClientRole)

```
public int setClientRole (int role, String permissionkey);
```

在加入频道前，用户需要通过本方法设置观众 (默认) 或主播模式。在加入频道后，用户可以通过本方法切换用户模式。

> 该方法仅适用于: 您已通过调用 `setChannelProfile` 将频道模式设置为指挥模式。

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
<tr><td><code>permissionKey</code></td>
<td>将其设置为空</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


### 加入频道 (joinChannel)

```
public abstract int joinChannel(String token,
                                String channelName,
                                String optionalInfo,
                                int optionalUid);
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
<li>安全要求高: 将值设置为 Token 值。 如果你已经启用了 App 证书, 请务必使用 Token。 关于如何获取 Token，详见 <a href="../../cn/Agora%20Platform/token.md"><span>密钥说明</span></a> </li>
</ul>
</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>optionalInfo</code></td>
<td>(非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户</td>
</tr>
<tr><td><code>optionalUid</code></td>
<td><p>(非必选项) 用户ID，32位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK不对该返回值进行维护</p>
<p>uid 在 SDK 内部用 32 位无符号整数表示，由于 java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，java 会表示为负数，如有需要可以用(uid&amp;0xffffffffL)转换成 64 位整数</p>
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


<a name = "audio"></a>
### 打开音频 (EnableAudio)

```
public abstract int EnableAudio();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 关闭音频 (DisableAudio)

```
public abstract int DisableAudio();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 离开频道 (LeaveChannel)

```
public abstract int LeaveChannel();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 修改语音路由的默认值 (setDefaultAudioRouteToSpeakerPhone)

```
public int SetDefaultAudioRouteToSpeakerphone(bool speakerphone)
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
<tr><td><code>defaultToSpeaker</code></td>
<td><ul>
<li>True: 将语音路由设置为扬声器(外放)</li>
<li>False: 将语音路由设置为听筒</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


> -   该方法只在纯音频模式下工作，在有视频的模式下不工作。
> -   如果插上耳机或连接蓝牙，语音路由会发生相应改变。拔出耳机或断开蓝牙后，语音路由将恢复成默认值。


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



### 将语音路由设置为扬声器外放 (setEnableSpeakerphone)

```
public int SetEnableSpeakerphone (bool speakerphone)
```

该方法将语音路由设置为 **扬声器 (外放)**。该方法对语音路由影响的优先级高于插上耳机或连接蓝牙的动作。

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
<tr><td><code>enabled</code></td>
<td><p>是否将语音路由设置为扬声器(外放)：</p>
<div><ul>
<li><p>True:</p>
<div><ul>
<li>如果用户已在频道内，调用该 API 后，会切换到从扬声器(外放)出声</li>
<li>如果用户尚未加入频道，调用该API后，在加入频道时会默认从扬声器(外放)出声</li>
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
<li>&lt; 1: 方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>


> 使用该方法后，插上耳机或连接蓝牙等动作不影响语音路由。

### 是否将语音路由设置为扬声器外放 (IsSpeakerphoneEnabled\)

```
public bool IsSpeakerphoneEnabled()
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设定扬声器音量 (SetSpeakerphoneVolume)

```
public int SetSpeakerphoneVolume(int volume)
```

使用该方法设定扬声器音量。

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
<tr><td><code>volume</code></td>
<td>设定音量，最小为 0，最大为 255</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


### 启用说话者音量提示 (EnableAudioVolumeIndication)

```
public abstract int EnableAudioVolumeIndication (int interval, int smooth);
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
<li>&lt;=0: 禁用音量提示功能</li>
<li>&gt;0: 提示间隔，单位为毫秒。建议设置到大于 200 毫秒</li>
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


<a name = "muteStream"></a>
### 将自己静音 \(MuteLocalAudioStream\)

```
public abstract int MuteLocalAudioStream (bool mute);
```

静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>muted</code></td>
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 静音所有远端音频 (MuteAllRemoteAudioStreams)

```
public abstract int MuteAllRemoteAudioStreams (bool mute);
```

该方法用于允许/禁止播放远端用户的音频流，即对所有远端用户进行静音与否。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 静音指定用户音频 (MuteRemoteAudioStream)

```
public abstract int MuteRemoteAudioStream (uint uid, bool mute);
```

静音指定远端用户/对指定远端用户取消静音。本方法用于允许/禁止播放远端用户的音频流。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


<a name = "other"></a>
### 设置日志过滤器 (SetLogFilter)

```
public abstract int SetLogFilter (LOG_FILTER filter);
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 设置日志文件 (SetLogFile)

```
public abstract int SetLogFile (string filePath);
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 更新 Token (renewToken)

```
public abstract int renewToken(String token);
```

该方法用于更新 Token。如果启用了 Token 机制，过一段时间后使用的 Token 会失效。

当：

-   发生 `onTokenPrivilegeWillExpire` 回调时，
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



### 触发 SDK 事件 (Poll)

```
public abstract void Poll ();
```

该方法根据垂直同步 (vertical synchronization) 例如 Update 触发 SDK 事件 (Unity3D)。

### 启动语音通话测试 (startEchoTest)

```
public int StartEchoTest()
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
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_REFUSED (-5): 不能启动测试，可能没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



### 终止语音通话测试 (StopEchoTest)

```
public int StopEchoTest()
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
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_REFUSED(-5): 停止 EchoTest 失败，可能是因为不在进行 EchoTest</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



### 查询 SDK 版本号 (GetSdkVersion)

```
public static string GetSdkVersion ()
```

该方法返回 SDK 版本号的字符串 (char 格式)。

### 获取错误描述 (GetErrorDescription)

```
public static string GetErrorDescription (int code)
```

SDK 运行时如果出错，该方法可以获取错误码。

### 获取通话 ID (GetCallId)

```
public string GetCallId()
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



### 给通话评分 (Rate)

```
public int Rate(string callId, int rating, string desc)
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
<td>从 getCallId() 获取的通话 ID</td>
</tr>
<tr><td><code>rating</code></td>
<td>给通话的评分，最低 1 分，最高 10 分。</td>
</tr>
<tr><td><code>description</code></td>
<td>[非必选项]给通话的描述，可选，长度应小于 800 字节。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): 传递的参数无效</li>
<li>ERR_NOT_READY (-3): 没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



### 投诉通话质量 (Complain)

```
public int Complain(string callId, string desc)
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
<td>从 getCallId() 获取的通话 ID</td>
</tr>
<tr><td><code>desc</code></td>
<td>[非必选项]给通话的描述，可选，长度应小于 800 字节。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): 传递的参数无效</li>
<li>ERR_NOT_READY (-3): 没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



### 暂停语音 (Pause)

```
public void Pause ()
```

该方法暂停语音，例如: 当进入后台模式，你可以调用该 API 暂停语音。

### 恢复语音 (Resume)

```
public void Resume ()
```

该方法恢复暂停的语音。

### 获取消息数量 (getMessageCount)

```
public int GetMessageCount ()
```

该方法获取消息队列里的消息数量。

### 销毁引擎实例 (Destroy)

```
public static void Destroy()
```

该方法释放 Agora SDK 使用的所有资源。有些应用程序只在用户需要时才使用音视频功能，不需要时则将资源释放出来用于其他操作，该方法对这类程序可能比较有用。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


<a name = "callback"></a>
### 加入频道回调 (JoinChannelSuccessHandler)

```
public delegate void JoinChannelSuccessHandler (string channelName, uint uid, int elapsed);
```

该方法表示用户已成功加入指定频道。频道 ID 的分配是根据 `JoinChannel` API 中指定的频道名称。如果调用 `JoinChannel` 时并未指定用户 ID，服务器就会分配一个。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
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



### 重新加入频道回调 (ReJoinChannelSuccessHandler)

```
public delegate void ReJoinChannelSuccessHandler (string channelName, uint uid, int elapsed);
```

有时候由于网络原因，客户端可能会和服务器失去连接，SDK 会进行自动重连，自动重连成功后触发此回调方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
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



### 网络中断回调 (ConnectionInterruptedHandler)

```
public delegate void ConnectionInterruptedHandler ();
```

在 SDK 和服务器失去了网络连接时，触发该回调。失去连接后，除非 APP 主动调用 `LeaveChannel`，SDK 会一直自动重连。

### 连接丢失回调 (ConnectionLostHandler)

```
public delegate void ConnectionLostHandler ();
```

在 SDK 和服务器失去了网络连接后，会触发 `ConnectionInterruptedHandler` 回调，并自动重连。 在一定时间内（默认 10 秒）如果没有重连成功，触发 `ConnectionLostHandler` 回调。 除非 APP 主动调用 `LeaveChanne`，SDK 仍然会自动重连。

### 其他用户加入当前频道回调 (UserJoinedHandler)

```
public delegate void UserJoinedHandler (uint uid, int elapsed);
```

提示有用户加入了频道。 如果该客户端加入频道时已经有人在频道中，SDK 也会向应用程序上报这些已在频道中的用户。

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
<tr><td><code>elapsed</code></td>
<td><code>JoinChannel</code> 开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>



### 其他用户离开当前频道回调 (UserOfflineHandler)

```
public delegate void UserOfflineHandler (uint uid, USER_OFFLINE_REASON reason);
```

提示有用户离开了频道（或掉线）。SDK 判断用户离开频道（或掉线）的依据是超时: 在一定时间内（15 秒）没有收到对方的任何数据包，判定为对方掉线。在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

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


### 离开频道回调 (LeaveChannelHandler)

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

### 启用说话者音量提示 (VolumeIndicationHandler)

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



### 用户静音回调 (UserMutedHandler)

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



### 发生警告回调 (SDKWarningHandler)

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



### 发生错误回调 (SDKErrorHandler)

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



### Rtc Engine 统计数据回调 (RtcStatsHandler)

```
public delegate void RtcStatsHandler (RtcStats stats);
```

该回调定期上报 Rtc Engine 的运行时的状态，每两秒触发一次。

### 语音路由变更回调 (AudioRouteChangedHandler)

```
public delegate void AudioRouteChangedHandler (AUDIO_ROUTE route);
```

SDK 会通过该回调通知 App 语音路由状态已发生变化。该回调返回当前的语音路由已切换至听筒，外放 (扬声器)，耳机或蓝牙。

### Token 已过期回调 (onRequestToken)

```
public void onRequestToken();
```

在调用 `joinChannel` 时如果指定了 Token，由于 Token 具有一定的时效，在通话过程中 SDK 可能由于网络原因和服务器失去连接，重连时可能需要新的 Token。该回调通知 APP 需要生成新的 Token，并需调用 `renewToken` 为 SDK 指定新的 Token。 之前的处理方法是在 `onError` 回调报告 ERR_TOKEN_EXPIRED (109\)、ERR_INVALID_TOKEN (110) 时，APP 需要生成新的 Token。在新版本中，原来的处理仍然有效，但建议把相关逻辑放进该回调里。

### 上下麦回调 (onClientRoleChanged)

```
public delegate void onClientRoleChangedHandler(int oldRole, int newRole)
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



## 错误码和警告码 - AMG SDK

详见 [错误码和警告码](../../cn/API%20Reference/the_error_game.md)。


