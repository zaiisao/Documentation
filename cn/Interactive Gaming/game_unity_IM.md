
---
title: 游戏 API
description: 
platform: unity
updatedAt: Mon Nov 25 2019 10:13:35 GMT+0800 (CST)
---
# 游戏 API
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


本文提供基于 C\# 语言的游戏音视频 API 描述，包括以下类:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><a href="#irtcengine">IRtcEngine 抽象类</a></td>
<td>包含应用程序需调用的主要方法及其回调方法</td>
</tr>
<tr><td><a href="#iaudioeffectmanager">IAudioEffectManager 接口类</a></td>
<td>包含应用程序用于管理音效的方法</td>
</tr>
<tr><td><a href="#im">IM 接口类</a></td>
<td>包含应用程序用于发送及接收 IM 消息的方法</td>
</tr>
<tr><td><a href="#im-restful">IM RESTful 接口类</a></td>
<td>包含应用程序用于获取 IM Token 和敏感词服务的方法</td>
</tr>
<tr><td><a href="#im-callback">IM 回调接口类</a></td>
<td>包含 IM 接口类的回调方法</td>
</tr>
</tbody>
</table>

<a id = "irtcengine" ></a>
	
## IRtcEngine 抽象类

IRtcEngine 抽象类包含了以下相关方法：

-   [引擎实例相关](#engine)
-   [频道相关](#channel)
-   [音频相关](#audio)
-   [视频相关](#video)
-   [暂停发送音视频流相关](#mute)
-   [伴奏相关](#mixing)
-   [录制相关](#recording)
-   [加密相关](#encryption)
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
<tr><td><code>appId</code></td>
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
### 设置频道属性 (SetChannelProfile)

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



### 设置用户角色 (SetClientRole)

```
public abstract int SetClientRole(CLIENT_ROLE role, string permissionKey);
```

该方法用于加入频道前设置用户角色，同时允许用户在加入频道后切换角色。

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



### 加入频道 (JoinChannel)

```
public abstract int JoinChannel (string channelName, string info, uint uid);
```

使用该方法加入频道。如果尚未创建频道，该方法自动创建频道。在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。

> -   该 UID 不能与频道内现有的其他用户 UID 相同，否则频道内持有相同 UID 用户的服务将被迫停止。
> -   加入频道之前，请确保已经成功连接服务器。


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
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,!#$%&amp;,()+, -,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>info</code></td>
<td>(非必选项) 开发者需加入的任何附加信息。该信息不会传递给频道内的其他用户。</td>
</tr>
<tr><td><code>uid</code></td>
<td>(非必选项) 用户 ID，32 位无符号整数。建议设置范围：1到(2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护。</td>
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



### 带密钥加入频道 (JoinChannelWithKey)

```
public abstract int JoinChannelWithKey (string channelKey, string channelName, string info, uint uid);
```

该方法跟上文的 加入频道 `joinChannel` 功能相同, 唯一的不同在于该方法需要带上 Channel Key。大部分情况下，直接使用 加入频道 `joinChannel` 方法即可满足需求。对于有较高安全性要求的用户，推荐使用本方法，携带 Channel Key，提高安全性。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelKey</code></td>
<td>关于如何获取 Channel Key, 请参考 <a href="../../cn/Agora%20Platform/token.md"><span>密钥说明</span></a></td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,!#$%&amp;,()+, -,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>info</code></td>
<td>(非必选项) 开发者需加入的任何附加信息。该信息不会传递给频道内的其他用户。</td>
</tr>
<tr><td><code>uid</code></td>
<td>(非必选项) 用户 ID，32 位无符号整数。建议设置范围：1到(2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 `onJoinChannelSuccess` 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护。</td>
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
<li><p>False: 语音会根据语音路由的默认值出声，详见 `setDefaultAudioRouteToSpeakerPhone`</p>
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

### 是否将语音路由设置为扬声器外放 (IsSpeakerphoneEnabled)

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


> -   在实现视频功能前，请先阅读 [音频相关](#audio) ，因为 [音频相关](#audio) 里的 API 也是实现视频功能的基础；
> -   在 iOS 和 Android 平台使用视频功能时，选择 Player Settings\> Other Settings\> Graphics API\> OpenGLES2 。

<a name = "video"></a>
### 打开视频模式 (EnableVideo)

```
public int EnableVideo ();
```

该方法用于打开视频模式。可以在加入频道前或者通话中调用，在加入频道前调用，则自动开启视频模式，在通话中调用则由音频模式切换为视频模式。调用 `Disablevideo` 方法可关闭视频模式。

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


### 关闭视频模式 (DisableVideo)

```
public int DisableVideo ()
```

该方法用于关闭视频。可以在加入频道前或者通话中调用，在加入频道前调用，则自动开启纯音频模式，在通话中调用则由视频模式切换为纯音频频模式。调用 `EnableVideo` 方法可开启视频模式。

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
<tr/>
</tbody>
</table>



### 设置视频属性 (SetVideoProfile)

```
public int SetVideoProfile(int profile, bool swapWidthAndHeight)
```

该方法设置视频编码属性（Profile）。每个属性对应一套视频参数，如分辨率、帧率、码率等。 当设备的摄像头不支持指定的分辨率时，SDK 会自动选择一个合适的摄像头分辨率，但是编码分辨率仍然用 `SetVideoProfile` 指定的。

该方法仅设置编码器编出的码流属性，可能跟最终显示的属性不一致，例如编码码流分辨率为 640x480，码流的旋转属性为 90 度，则显示出来的分辨率为竖屏模式。


> -   应在调用 `EnableVideo` 后设置视频属性；
> -   应在调用 `joinChannel`/`startPreview` 前设置视频属性。


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
<li>true:交换宽和高</li>
<li>false：不交换宽和高(默认)</li>
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
<td>1280x720 *</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_3</td>
<td>52</td>
<td>1280x720 *</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>54</td>
<td>960x720 *</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>55</td>
<td>960x720 *</td>
<td>30</td>
<td>1380</td>
</tr>
<tr><td>1080P</td>
<td>60</td>
<td>1920x1080  <sup>[1]</sup></td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_3</td>
<td>62</td>
<td>1920x1080  <sup>[1]</sup></td>
<td>30</td>
<td>3150</td>
</tr>
<tr><td>1080P_5</td>
<td>64</td>
<td>1920x1080  <sup>[1]</sup></td>
<td>60</td>
<td>4780</td>
</tr>
</tbody>
</table>

> [1]  视频能否达到 1080P 的分辨率取决于设备的性能，在性能配备较低的设备上有可能无法实现。如果采用 1080P 分辨率而设备性能跟不上，则有可能出现帧率过低的情况。Agora.io 将继续在后续版本中为较低端设备进行视频优化。 

### 使用双流/单流模式 (EnableDualStreamMode)

```
public int EnableDualStreamMode(bool enabled)
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
<li>true: 双流</li>
<li>false: 单流</li>
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



### 设置视频大小流 (SetRemoteVideoStreamType)

```
public int SetRemoteVideoStreamType(uint uid, int streamType)
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



视频大流的分辨率有三种配置分别为 1:1, 4:3, 16:9，小流与大流的画面长宽比一致，参数配置如下:

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>分辨率</strong></td>
<td><strong>帧率</strong></td>
<td><strong>关键帧间隔</strong></td>
<td><strong>码率(kbps)</strong></td>
</tr>
<tr><td>160*160</td>
<td>5</td>
<td>5</td>
<td>45</td>
</tr>
<tr><td>160*120</td>
<td>5</td>
<td>5</td>
<td>32</td>
</tr>
<tr><td>160*90</td>
<td>5</td>
<td>5</td>
<td>28</td>
</tr>
</tbody>
</table>



### 设置视频优化选项 (SetVideoQualityParameters)

```
public int SetVideoQualityParameters(bool preferFrameRateOverImageQuality)
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 禁用本地视频功能 (EnableLocalVideo)

```
public int EnableLocalVideo (bool enabled)
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
<li>true: 启用本地视频（默认）</li>
<li>false: 禁用本地视频</li>
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



### 开启视频预览 (StartPreview)

```
public int StartPreview ()
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 停止视频预览 (StopPreview)

```
public int StopPreview ()
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 切换前置/后置摄像头 (SwitchCamera)

```
public int SwitchCamera()
```

使用该方法切换前置/后置摄像头。

> 该方法只在 Unity for iOS 和 Unity for Android 平台有效。

#### 允许直接发送图片给 App \(EnableVideoObserver\)

```
public int EnableVideoObserver ()
```

该方法直接将允许视频图片发送给 app，而无需经过传统的视图渲染器。

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



### 禁止直接发送图片给 App (DisableVideoObserver)

```
public int DisableVideoObserver ()
```

该方法禁止直接将视频图片发送给 app。

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


<a name = "mute"></a>
### 将自己静音 (MuteLocalAudioStream)

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



### 暂停发送本地视频流 (MuteLocalVideoStream)

```
public int MuteLocalVideoStream(bool mute)
```

暂停/恢复发送本地视频流。该方法用于允许/禁止往网络发送本地视频流。该方法不影响本地视频流获取，没有禁用摄像头。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 暂停播放所有远端视频流 (MuteAllRemoteVideoStreams)

```
public int MuteAllRemoteVideoStreams(bool mute)
```

暂停/恢复所有人视频流。本方法用于允许/禁止播放所有人的视频流。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 暂停播放指定远端用户视频 (MuteRemoteVideoStream)

```
public int MuteRemoteVideoStream(uint uid, bool mute)
```

暂停/恢复指定用户视频流。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


<a name = "mixing"></a>
### 开始客户端本地混音 (StartAudioMixing)

```
public abstract int StartAudioMixing (string filePath, bool loopback, bool replace, int cycle, int playTime = 0);
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止客户端本地混音 (StopAudioMixing)

```
public abstract int StopAudioMixing();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 暂停伴奏播放 (PauseAudioMixing)

```
public abstract int PauseAudioMixing();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 恢复伴奏播放 (ResumeAudioMixing)

```
public abstract int ResumeAudioMixing();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 调节伴奏音量 (AdjustAudioMixingVolume)

```
public abstract int AdjustAudioMixingVolume (int volume);
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取伴奏时长 (GetAudioMixingDuratio)

```
public abstract int GetAudioMixingDuration();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取伴奏播放进度 (GetAudioMixingCurrentPosition)

```
public abstract int GetAudioMixingCurrentPosition();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


<a name = "recording"></a>
### 开始客户端录音 (StartAudioRecording)

```
public abstract int StartAudioRecording(string filePath);
```

Agora SDK 支持通话过程中在客户端进行录音，且录音文件格式可以为:

-   .wav : 文件大，音质保真度高
-   .aac : 文件小，有一定的音质保真度损失


确保应用程序里指定的目录存在且可写。该接口需在加入频道之后调用。如果调用 `leaveChannel` 时还在录音，录音会自动停止。

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
	<tr><td><code>quality</code></td>
<td><p>录音音质:</p>
<div><ul>
<li>AUDIO_RECORDING_QUALITY_LOW = 0: 低音质。录制 10 分钟的文件大小为 1.2 M 左右</li>
<li>AUDIO_RECORDING_QUALITY_MEDIUM = 1: 中音质。录制 10 分钟的文件大小为 2 M 左右</li>
<li>AUDIO_RECORDING_QUALITY_HIGH = 2: 高音质。录制 10 分钟的文件大小为 3.75 M 左右</li>
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



### 停止客户端录音 (StopAudioRecording)

```
public abstract int StopAudioRecording();
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 调节录音信号音量 (AdjustRecordingSignalVolume)

```
public abstract int AdjustRecordingSignalVolume (int volume);
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
<li>400: 最大可为原始音量的4倍 (自带溢出保护)</li>
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



### 调节播放信号音量 (AdjustPlaybackSignalVolume)

```
public abstract int AdjustPlaybackSignalVolume (int volume);
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
<li>400: 最大可为原始音量的4倍 (自带溢出保护)</li>
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


<a name = "encryption"></a>
### 启用内置的加密功能 (setEncryptionSecret)

```
public int SetEncryptionSecret(string secret)
```

在加入频道之前，您的应用程序需调用 `setEncryptionSecret` 指定 secret 来启用内置的加密功能，否则该通话未加密。 同一频道内的所有用户应设置相同的secret。 当用户离开频道时，该频道的 secret 会自动清除。如果未指定 secret 或将 secret 设置为空，则无法激活加密功能。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


### 设置内置的加密方案 (SetEncryptionMode)

```
public int SetEncryptionMode(string encryptionMode)
```

Agora SDK 支持内置加密功能，默认使用 AES-128-XTS 加密方式。如需使用其他加密方式，可以调用该 API 设置。 同一频道内的所有用户必须设置相同的加密方式和 secret，才能进行通话。关于这几种加密方式的区别，请参考 AES 加密算法的相关资料。 调用本方法前，需调用 `setEncryptionSecret` 启用内置的加密密码。

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



#### 设置日志文件 (SetLogFile)

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



### 更新 Channel Key (RenewChannelKey)

```
public override int RenewChannelKey (string channelKey);
```

如果启用了 Channel Key 机制，过一段时间后 Key 会失效。当触发 `RequestChannelKeyHandler` 回调时，调用该 API 更新 Channel Key，否则 SDK 无法和服务器建立连接。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelKey</code></td>
<td>指定要更新的 Channel Key</td>
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

该犯法返回 SDK 版本号的字符串 (char 格式)。

### 获取错误描述 (GetErrorDescription\)

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
<td>从 <code>getCallId</code> 获取的通话 ID</td>
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
### 加入频道回调 (onJoinChannelSuccess)

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
<tr><td>频道</td>
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
<tr><td>channel</td>
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

在 SDK 和服务器失去了网络连接后，会触发 `ConnectionInterruptedHandler` 回调，并自动重连。 在一定时间内（默认 10 秒）如果没有重连成功，触发 `ConnectionLostHandler` 回调。 除非 APP 主动调用 `LeaveChannel`，SDK 仍然会自动重连。

#### 其他用户加入当前频道回调 `UserJoinedHandler`

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



### 其他用户加入当前频道回调 (UserJoinedHandler)

```
public delegate void UserJoinedHandler (uint uid, int elapsed);
```

提示有用户加入了频道。如果该客户端加入频道时已经有人在频道中，SDK 也会向应用程序上报这些已在频道中的用户。

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

提示有用户离开了频道（或掉线）。SDK 判断用户离开频道（或掉线）的依据是超时: 在一定时间内（15秒）没有收到对方的任何数据包，判定为对方掉线。 在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

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
<ul>
<li>USER_OFFLINE_QUIT: 用户主动离开</li>
<li>USER_OFFLINE_DROPPED: 因过长时间收不到对方数据包，超时掉线</li>
<li>由于 SDK 使用的是不可靠通道，也有可能对方主动离开本方没收到对方离开消息而误判为超时掉线</li>
</ul>
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

### 说话者音量回调 (VolumeIndicationHandler)

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



### 发生error callback (SDKErrorHandler)

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

### 伴奏播放完成回调 (AudioMixingFinishedHandler)

```
public delegate void AudioMixingFinishedHandler ();
```

当伴奏文件播放完成时触发该回调。

### 语音路由变更回调 (AudioRouteChangedHandler)

```
public delegate void AudioRouteChangedHandler (AUDIO_ROUTE route);
```

SDK 会通过该回调通知 App 语音路由状态已发生变化。该回调返回当前的语音路由已切换至听筒，外放 (扬声器)，耳机或蓝牙。

### Channel Key 过期回调 (RequestChannelKeyHandler)

```
public delegate void RequestChannelKeyHandler ();
```

在调用 `JoinChannel` 时如果指定了 Channel Key，由于 Channel Key 具有一定的时效，在通话过程中 SDK 可能由于网络原因和服务器失去连接，重连时可能需要新的 Channel Key。 该回调通知 APP 需要生成新的 Channel Key，并需调用 `RenewChannelKey` 为 SDK 指定新的 Channel Key。

### 接收到远程视频并完成解码回调 (OnFirstRemoteVideoDecodedHandler\)

```
public delegate void OnFirstRemoteVideoDecodedHandler (uint uid, int width, int height, int elapsed);
```

收到第一帧远程视频流并解码成功时，触发此调用。应用程序可在此回调中设置该用户的视图。

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
<td><code>JoinChannel</code> 开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>


### 视频大小已变更回调 (OnVideoSizeChangedHandler)

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
<td>JoinChannel</code> 开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>



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


<a id = "iaudioeffectmanager"></a>
## IAudioEffectManager 接口类

### 获取音效音量 (getEffectsVolume)

```
double GetEffectsVolume();
```

该方法获取音量的音量，范围为 [0.0, 100.0]。

### 设置音效音量 (SetEffectsVolume)

```
int SetEffectsVolume(double volume)
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
<li>&lt;0: 方法调用失败</li>
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

> [2] 如果你已通过 `preloadEffect` 将音效加载至内存，确保这里设置的 `soundId` 与 `preloadEffect 设置的 `soundId` 相同。

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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 停止播放所有的音效 (stopAllEffects)

```
int stopAllEffects();
```

该方法停止播放所有的音效。

### 预加载音效 (preloadEffect)

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
<li>&lt;0: 方法调用失败</li>
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### 暂停音效播放 (PauseEffect)

```
virtual int PauseEffect(int soundId)
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
<li>&lt;0: 方法调用失败</li>
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
<li>&lt;0: 方法调用失败</li>
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
<li>&lt;0: 方法调用失败</li>
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



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
<li>&lt;0: 方法调用失败</li>
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
<tr><td>uid</code></td>
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
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


<a id = "im"></a>
## IM 接口类

该类接口为应用程序提供实现接收和发送 IM 消息的方法。

### 创建 C# 层的 IRtcEngine 对象 (GetEngine)

```
mRtcEngine = IRtcEngine.GetEngine (appId);
```

该方法在 C# 层创建一个 IRtcEngine 对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>appId</code></td>
<td>你所创建的程序的 App ID，详见 <a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a> 。</td>
</tr>
</tbody>
</table>



### 初始化 IM 资源

```
AgoraIMInit(appKey);
```

该方法通过传入 App Key 来初始化与 IM 服务相关的资源。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>appKey</code></td>
<td>你所创建的程序的 App Key，如 “82hegw5u8y3dx” 。请联系 <a href="mailto:sales%40agora.io">sales<span>@</span>agora<span>.</span>io</a> 获取 App key 。</td>
</tr>
</tbody>
</table>



### 连接 IM 服务器

```
AgoraIMConnect(token);
```

该方法通过传入 IM Token 来连接 IM 服务器。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>token</code></td>
<td>IM 的服务 Token，如 “j9UjgwnHPVLCeh5sc/27VR0pyBBs8RAFmk/S9ysb6uMoJKDc/Mv5qwjsPNb/nqiSP/msWr6S3EqVWLSM5oxiuA==” 。要获取 IM Token ，你需要在服务器端调用 <a href="#im-token-gettoken">获取 IM Token (getToken)</a> 方法。</td>
</tr>
</tbody>
</table>


<a id = "agoraimsendmessage"></a>
### 发送文字消息 (AgoraIMSendMessage)

```
public void AgoraIMSendMessage(String targetId,string message,int whichConversationType){
    agoraIMSendMessage(targetId,message,conversationType);
}
```

该方法向指定的用户发送文字消息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>targetId</code></td>
<td>接收文字消息的指定用户的 ID</td>
</tr>
<tr><td><code>message</code></td>
<td>发送的文字消息</td>
</tr>
<tr><td><code>conversationType</code></td>
<td><p>消息类型，具体包含以下几种：</p>
<div><ul>
<li>public static int NONE = 0：无消息</li>
<li>public static int PRIVATE = 1：点对点消息</li>
<li>public static int DISCUSSION = 2：讨论组消息</li>
<li>public static int GROUP = 3：群消息</li>
<li>public static int CHATROOM = 4：聊天室消息</li>
<li>public static int CUSTOMER_SERVICE = 5：客服消息</li>
<li>public static int SYSTEM = 6：系统消息</li>
<li>public static int APP_PUBLIC_SERVICE = 7：应用公共服务消息</li>
<li>public static int PUBLIC_SERVICE = 8：公共服务消息</li>
<li>public static int PUSH_SERVICE = 9：PUSH 消息</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


> 发送文字消息之前，请确保已经成功连接服务器。

**ConversationType 结构体**

```
public class ConversationType
{
    public static int NONE = 0;
    public static int PRIVATE = 1;
    public static int DISCUSSION = 2;
    public static int GROUP = 3;
    public static int CHATROOM = 4;
    public static int CUSTOMER_SERVICE = 5;
    public static int SYSTEM = 6;
    public static int APP_PUBLIC_SERVICE = 7;
    public static int PUBLIC_SERVICE = 8;
    public static int PUSH_SERVICE = 9;
}
```

### 初始化消息监听器 (AgoraIMInitMessageReceiveListener)

```
public void AgoraIMInitMessageReceiveListener(){
    agoraIMInitMessageReceiveListener();
}
```

该方法初始化消息监听器。

### 发送语音消息 (AgoraIMSendVoiceMessage)

```
public void AgoraIMSendVoiceMessage(string targetId, int conversationType, string filepath. int lengthOfVoiceMessage){
    agoraIMSendMessage(targetId,conversationType,filePath,lengthOfVoiceMessage);
}
```
该方法发送语音消息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>targetId</code></td>
<td>接收语音消息的指定用户的 ID</td>
</tr>
<tr><td><code>conversationType</code></td>
<td><p>发送的语音消息的类型，具体包含以下几种：</p>
<ul>
<li>public static int NONE = 0：无消息</li>
<li>public static int PRIVATE = 1：私人消息</li>
<li>public static int DISCUSSION = 2：讨论组消息</li>
<li>public static int GROUP = 3：群消息</li>
<li>public static int CHATROOM = 4：聊天室消息</li>
<li>public static int CUSTOMER_SERVICE = 5：客服消息</li>
<li>public static int SYSTEM = 6：系统消息</li>
<li>public static int APP_PUBLIC_SERVICE = 7：应用公共服务消息</li>
<li>public static int PUBLIC_SERVICE = 8：公共服务消息</li>
<li>public static int PUSH_SERVICE = 9：PUSH 消息</li>
</ul>
</td>
</tr>
<tr><td><code>filePath</code></td>
<td>语音消息的存储路径</td>
</tr>
<tr><td><code>lengthOfVoiceMessage</code></td>
<td>语音消息长度，单位为秒</td>
</tr>
</tbody>
</table>

> -   发送语音消息之前，请确保已经成功连接服务器。
> -   语音消息大小不能超过 128K 。超出 128K 的语音消息会发送失败。
> -   iOS 发送的消息的格式是 .wav; Android 发送的消息的格式是 .amr。


### 开始采集语音 (AgoraIMStartRecordAudio)

```
public void AgoraIMStartRecordAudio(){
    AgoraIMStartRecordAudio();
}
```

该方法开启语音采集。如果发送的是语音消息，可以通过该方法进行采集。

### 停止采集语音 (AgoraIMStopRecordAudio)

```
public void AgoraIMStopRecordAudio(){
    AgoraIMStopRecordAudio();
}
```

该方法停止语音采集。

### 获取采集的语音的存储路径 (AgoraIMGetRecordAudioFilePath)

```
public string AgoraIMGetRecordAudioFilePath(){
    string result = AgoraIMGetRecordAudioFilePath();
    return result;
}
```

该方法获取采集到的语音的存储路径。

### 获取采集的语音的时长 (AgoraIMGetRecordAudioLength)

```
public int AgoraIMGetRecordAudioLength(){
    int result = AgoraIMGetRecordAudioLength();
    return result;
}
```

该方法获取采集到的语音的时长。

> 请按顺序依次调用如上 `AgoraIMStartRecordAudio` 、`AgoraIMStopRecordAudio`、`AgoraIMGetRecordAudioFilePath` 和 `AgoraIMGetRecordAudioLength` 四个方法，否则可能出现方法调用不成功的情况。

### 获取历史消息 (AgoraIMGetHistoryMessages)

```
public void AgoraIMGetHistoryMessages(int conversationType, string targetId, int latestMessageId, int count){
   agoraIMGetHistoryMessages(conversationType, targetId, latestMessageId, count);
}
```
该方法获取指定的历史消息之前的指定条消息。获取的消息的类型和消息来源的对象分别由 `conversationType` 和 `string targetId` 确定。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>conversationType</code></td>
<td>消息类型，详见 <a href="#agoraimsendmessage">AgoraIMSendMessage</a> 中关于 <code>conversationType</code> 的描述</td>
</tr>
<tr><td><code>targetID</code></td>
<td>你想要获取的消息所在的对话的 ID。和消息类型相关，如私人对话 ID，讨论组 ID，群 ID，聊天室 ID 等。</td>
</tr>
<tr><td><code>latestMessageId</code></td>
<td>你想获取的最后一条消息的 ID</td>
</tr>
<tr><td><code>count</code></td>
<td>获取消息的数量</td>
</tr>
</tbody>
</table>


### 加入 IM 聊天室 (AgoraIMJoinChatRoom)

```
public void AgoraIMJoinChatRoom(string chatRoomId, int defMessageCount){
    AgoraIMJoinChatRoom(chatRoomId, defMessageCount);
}
```

该方法加入 IM 聊天室。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>chatRoomId</code></td>
<td>加入的 IM 聊天室号</td>
</tr>
<tr><td><code>defMessageCount</code></td>
<td>默认接收的历史消息数量</td>
</tr>
</tbody>
</table>

> -   发送语音消息之前，请确保已经成功连接服务器。


### 创建讨论组 (AgoraIMCreateDiscussion)

```
public void AgoraIMCreateDiscussion(string name, string userIdList){
    AgoraIMCreateDiscussion(name, userIdList);
}
```

该方法创建讨论组。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>name</code></td>
<td>创建的讨论组的名称</td>
</tr>
<tr><td><code>userIdList</code></td>
<td>讨论组内包含的人员的 ID 列表。不同人员 ID 之间用英文的逗号 “,” 隔开，不需要引号</td>
</tr>
</tbody>
</table>

> 调用讨论组相关 API ，操作对应的讨论组时，需要获取对应的讨论组 ID。此 ID 在创建讨论组成功的回调里获取。

### 添加人员到讨论组 (AgoraIMAddMemberToDiscussion)

```
public void AgoraIMAddMemberToDiscussion(string name, string userIdList){
    AgoraIMAddMemberToDiscussion(name, userIdList);
}
```

该方法将指定的人员添加到已有的讨论组。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>name</code></td>
<td>讨论组的名称</td>
</tr>
<tr><td><code>userIdList</code></td>
<td>添加到讨论组的人员 ID 列表。不同人员 ID 之间用英文的逗号 “,” 隔开，不需要引号</td>
</tr>
</tbody>
</table>



### 将讨论组成员移出讨论组 (AgoraIMRemoveMemberFromDiscussion)

```
public void AgoraIMRemoveMemberFromDiscussion(string name, string userIdList){
    agoraIMRemoveMemberFromDiscussion(name, userIdList);
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>name</code></td>
<td>讨论组的名称</td>
</tr>
<tr><td><code>userIdList</code></td>
<td>需要移出讨论组的人员 ID 列表。不同人员 ID 之间用英文的逗号 “,” 隔开，不需要引号</td>
</tr>
</tbody>
</table>



### 解散讨论组 (AgoraIMQuitDiscussion)

```
public void AgoraIMQuitDiscussion(string discussionId){
    agoraIMQuitDiscussion(discussionId);
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>discussionId</code></td>
<td>需要解散的讨论组 ID</td>
</tr>
</tbody>
</table>

### 设置讨论组名称 (AgoraIMSetDiscussionName)

```
public void AgoraIMSetDiscussionName(string discussionName,string discussionId){
    agoraIMSetDiscussionName(discussionName,discussionId);
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td><code>discussionName</code></td>
<td>想要设置的讨论组名称</td>
</tr>
<tr><td><code>discussionId</code></td>
<td>需要设置名称的讨论组 ID。一个讨论组只有一个 ID，此 ID 是固定的</td>
</tr>
</tbody>
</table>



### 翻译 IM 消息 (GetTranslate)

此方法翻译 IM 消息。

```
public void GetTranslate(string message, string original_language, string target_language){
    GetTranslate(message,original_language,target_language);
}
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
<tr><td><code>message</code></td>
<td>需要翻译的消息内容</td>
</tr>
<tr><td><code>original_language</code></td>
<td>消息的原语言简写</td>
</tr>
<tr><td><code>target_language</code></td>
<td>消息翻译的目标语言简写</td>
</tr>
</tbody>
</table>

语言简写与语言的对照关系：

源语言不确定时可设置为 auto，目标语言不可设置为 auto。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>语言简写</strong></td>
<td><strong>语言名称</strong></td>
</tr>
<tr><td>auto</td>
<td>自动检测</td>
</tr>
<tr><td>zh</td>
<td>中文</td>
</tr>
<tr><td>en</td>
<td>英语</td>
</tr>
<tr><td>yue</td>
<td>粤语</td>
</tr>
<tr><td>wyw</td>
<td>文言文</td>
</tr>
<tr><td>jp</td>
<td>日语</td>
</tr>
<tr><td>kor</td>
<td>韩语</td>
</tr>
<tr><td>fra</td>
<td>法语</td>
</tr>
<tr><td>spa</td>
<td>西班牙语</td>
</tr>
<tr><td>th</td>
<td>泰语</td>
</tr>
<tr><td>ara</td>
<td>阿拉伯语</td>
</tr>
<tr><td>ru</td>
<td>俄语</td>
</tr>
<tr><td>pt</td>
<td>葡萄牙语</td>
</tr>
<tr><td>de</td>
<td>德语</td>
</tr>
<tr><td>it</td>
<td>意大利语</td>
</tr>
<tr><td>el</td>
<td>希腊语</td>
</tr>
<tr><td>nl</td>
<td>荷兰语</td>
</tr>
<tr><td>pl</td>
<td>波兰语</td>
</tr>
<tr><td>bul</td>
<td>保加利亚语</td>
</tr>
<tr><td>est</td>
<td>爱沙尼亚语</td>
</tr>
<tr><td>dan</td>
<td>丹麦语</td>
</tr>
<tr><td>fin</td>
<td>芬兰语</td>
</tr>
<tr><td>cs</td>
<td>捷克语</td>
</tr>
<tr><td>rom</td>
<td>罗马尼亚语</td>
</tr>
<tr><td>slo</td>
<td>斯洛文尼亚语</td>
</tr>
<tr><td>swe</td>
<td>瑞典语</td>
</tr>
<tr><td>hu</td>
<td>匈牙利语</td>
</tr>
<tr><td>cht</td>
<td>繁体中文</td>
</tr>
<tr><td>vie</td>
<td>越南语</td>
</tr>
</tbody>
</table>



### 语音转文字 (voiceToText)

此方法将语音消息转为文字。

```
public void voiceToText(string filepath){
    voiceToText(filepath);
}
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
<tr><td><code>filepath</code></td>
<td>语音文件的路径</td>
</tr>
</tbody>
</table>

> 语音文件必须满足以下三个条件：

> -   文件格式必须为 pcm; （iOS 的录制文件格式默认为 pcm ，Android 的录制文件格式默认为 AMR，因此 Android 的录制文件在转换为文字前需要先调用 ` imEngineManager.MAMRToPCM (filePath);` 转换为 pcm 格式）
> -   pcm 文件大小不得超过 2M;
> -   pcm 文件时长不得超过 60 秒。


### 断开 IM 连接 (agoraIMDisconnect)

此方法断开 IM 连接。断开连接后，你仍能收到 IM 新消息的通知。

```
private static extern void agoraIMDisconnect();
```

### 注销 IM 服务 (agoraIMLogout)

此方法注销 IM 服务。注销后，你不会收到任何 IM 相关的通知。

```
private static extern void agoraIMLogout();
```
	
<a id = "im-restful"></a>
## IM RESTful 接口类

该类接口提供在服务端获取 IM Token 和设置敏感词的方法。

### 接口调用规则

服务端在调用应用服务器接口推送数据时，会添加 3 个 GET 请求参数 (在 URL 上添加的参数)，具体如下：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>nonce</td>
<td>String</td>
<td>随机数，无长度限制</td>
</tr>
<tr><td>timestamp</td>
<td>String</td>
<td>时间戳，从 1970 年 1 月 1 日 0 点 0 分 0 秒开始到现在的毫秒数</td>
</tr>
<tr><td>signature</td>
<td>String</td>
<td>数据签名。计算方法：将获取的 App Secret、Nonce (随机数)、Timestamp (时间戳)三个字符串按先后顺序拼接成一个字符串并进行 SHA 1 哈希计算得到</td>
</tr>
</tbody>
</table>


> 要获取您的 App Secret, 请联系 [sales@agora.io](mailto:sales@agora.io) 。

### IM Token

#### IM Token 生成原理

客户端通过 Agora Mobile Gaming SDK 每次连接服务器时，都需要向服务器提供 IM Token，以便验证身份，流程如下：

![app_id_im_1.jpg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537408790682)


后续登录过程中就不必再请求新的 IM Token，而由 App Server 直接提供以前保存过的 IM Token。

![app_id_im_2.jpg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537411604091)

	
如果你的 App 是免登录设计，也可以将 Token 保存在 App 本地，直接登录。此时请注意保证本地数据存储安全。

![app_id_im_3.jpg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537411633943)


App 获取 Token 后，根据情况可选择在 App 本地保留当前用户的 Token，如果 Token 失效，还需要提供相应的代码重新向服务器获取 Token。你也可以设置 Token 有效期，默认永久有效。

<a id = "im-token-gettoken"></a>
#### 获取 IM Token \(getToken\)

-   方法名：/user/getToken
-   URL：http://api.cn.ronghub.com/user/getToken.format ，其中 format 表示返回格式，可以为 json 或 xml 。
-   HTTP 方法：POST
-   参数：userId, name, portrait Uri

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>userId</td>
<td>String</td>
<td>(必填) 用户 Id，最大长度 64 字节。是用户在 App 中的唯一标识，必须保证在同一个 App 内不重复。重复的用户 Id 将被当作是同一用户。您可以使用系统的用户 Id，或者单独建立 Id 体系。格式不限，可以为数字、GUID、或者任意字符串（中文除外）</td>
</tr>
<tr><td>name</td>
<td>String</td>
<td>(必填) 用户名称，最大长度 128 字节。用来在推送时显示用户的名称</td>
</tr>
<tr><td>portraitUri</td>
<td>String</td>
<td>(必填) 用户头像 URI，最大长度 1024 字节</td>
</tr>
</tbody>
</table>



-   返回值：

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>返回值，200 为正常。如果您正在使用开发环境的 App Key，您的应用只能注册不超过 100 名用户。否则，将返回错误码 2007。如果您需要更多的测试账户数量，您需要在应用配置中申请“增加测试人数”</td>
</tr>
<tr><td>token</td>
<td>String</td>
<td>用户 Token，可以保存在应用内，最大长度 256 字节</td>
</tr>
<tr><td>userId</td>
<td>String</td>
<td>用户 Id，与输入的用户 Id 相同</td>
</tr>
</tbody>
</table>

> 你可以在 App Key 中对 Token 有效期进行设置，默认为永久有效。 一个用户 Id，可以多次获取 Token。这些 Token 都可以正常使用。Token 只在超过设置的有效期后失效。

-   示例：

 - HTTP 请求：

    ```
    POST /user/getToken.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    userId=jlk456j5&name=Ironman&portraitUri=http%3A%2F%2Fabc.com%2Fmyportrait.jpg
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200, "userId":"jlk456j5", "token":"sfd9823ihufi"}
    ```


### 敏感词服务

应用敏感词服务后，App 中用户不会收到含有敏感词的消息内容。

此服务默认最多设置 50 个敏感词，设置后 2 小时内生效，目前只支持应用于内置消息类型的文本消息。如需应用于通过 Server API 发送的消息，请联系 [sales@agora.io](mailto:sales@agora.io) 。

敏感词过滤方式有两种：屏蔽包含敏感词的消息和在消息中替换敏感词。

#### 敏感词过滤规则

-   支持对于中文简体、繁体自动智能过滤。即设置中文简体敏感词后，对应繁体敏感词也会自动识别过滤。
-   匹配敏感词时，智能忽略标题符号。即敏感词中含有标点符号时，会忽略中间的标点符号，对敏感词进行过滤。如：设置敏感词 “反动”，当出现 “反、动” 中间含有标点符号的聊天信息时，也会自动过滤。
-   英文、数字敏感词，支持全角、半角、大、小写自动匹配
-   对英文单词进行智能识别过滤功能。如：设置敏感词 “AV”，当出现含有 “Java” 的聊天信息时，因为 “Java” 为英文单词，所以不会对单词中包含的 “av” 进行过滤。


#### 添加敏感词 (add)

-   方法名：/sensitiveword/add
-   URL：http://api.cn.ronghub.com/sensitiveword/add.format ，其中 format 表示返回格式，可以为 json 或 xml 。
-   HTTP 方法：POST
-   参数：word, replaceWord

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>word</td>
<td>String</td>
<td>(必填) 敏感词，最长不超过 32 个字符，格式为汉字、数字、字母</td>
</tr>
<tr><td>replaceWord</td>
<td>String</td>
<td>(可选) 需要替换的敏感词，最长不超过 32 个字符。
如未设置替换的敏感词，当消息中含有敏感词时，消息将被屏蔽，用户不会收到消息。
如果设置了替换敏感词，当消息中含有敏感词时，将被替换为指定的字符进行发送。敏感词替换目前只支持单聊、群聊、聊天室会话。</td>
</tr>
</tbody>
</table>



-   返回值：

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>返回值，200 为正常</td>
</tr>
</tbody>
</table>

其他返回值请参考 [IM 返回值说明](https://docs.agora.io/cn/Interactive%20Gaming/cn/API%20Reference/the_error_im) 。

-   示例：

 - HTTP 请求：

    ```
    POST /conversation/notification/set.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    conversationType=1&requestId=b5NwvIrW8&targetId=UAhIaLkR0&isMuted=0
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200}
    ```


#### 移除敏感词 (delete)

从敏感词列表中，移除某一敏感词，移除后 2 小时内生效。

-   方法名：/sensitiveword/delete
-   URL：http://api.cn.ronghub.com/sensitiveword/delete.format ， 其中 format 表示返回格式，可以为 json 或 xml 。
-   HTTP 方法：POST
-   参数：word

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>word</td>
<td>String</td>
<td>(必填) 敏感词，最长不超过 32 个字符</td>
</tr>
</tbody>
</table>



-   返回值：

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>返回值，200 为正常</td>
</tr>
</tbody>
</table>

其他返回值请参考 [IM 返回值说明](https://docs.agora.io/cn/Interactive%20Gaming/cn/API%20Reference/the_error_im) 。

-   示例

 - HTTP 请求：

    ```
    POST /sensitiveword/delete.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    word=money
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200}
    ```


#### 批量移除敏感词 (batch/delete)

从敏感词列表中，批量移除某些敏感词，移除后 2 小时内生效。

-   方法名：/sensitiveword/batch/delete
-   URL：http://api.cn.ronghub.com/sensitiveword/batch/delete.format ，其中 format 表示返回格式，可以为 json 或 xml 。
-   HTTP 方法：POST
-   参数：words

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>words</td>
<td>String[]</td>
<td>(必填) 敏感词数组，一次最多移除 50 个敏感词。</td>
</tr>
</tbody>
</table>



-   返回值：

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>返回值，200 为正常</td>
</tr>
</tbody>
</table>

其他返回值请参考 [IM 返回值说明](https://docs.agora.io/cn/Interactive%20Gaming/cn/API%20Reference/the_error_im) 。

-   示例：

 - HTTP 请求：

    ```
    POST /sensitiveword/batch/delete.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    words=money&words=aaa&words=bbb
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200}
    ```


#### 查询敏感词列表 (list)

-   方法名：/sensitiveword/list
-   URL:  http://api.cn/ronghub.com/sensitiveword/list.format ， 其中，format 表示返回格式，可以为 json 或 xml 。
-   HTTP 方法：POST
-   参数：type

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>type</td>
<td>String</td>
<td><p>(可选) 查询敏感词的类型：</p>
<div><ul>
<li>0：查询替换敏感词</li>
<li>1：(默认) 查询屏蔽敏感词</li>
<li>2：查询全部敏感词</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



-   返回值：

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>返回值，200 为正常</td>
</tr>
<tr><td>word</td>
<td>String</td>
<td>敏感词内容</td>
</tr>
<tr><td>replaceWord</td>
<td>String</td>
<td>替换敏感词的内容，为空时对应 Word 敏感词类型为屏蔽敏感词</td>
</tr>
</tbody>
</table>

其他返回值请参考 [IM 返回值说明](../../cn/Interactive%20Gaming/the_error_im.md) 。

-   查询敏感词示例

 - HTTP 请求：

    ```
    POST /sensitiveword/list.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    type=1
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200,"words":[{"word":"环保"},{"word":"承诺"}]}
    ```

-   查询替换敏感词示例

 - HTTP 请求：

    ```
    POST /sensitiveword/list.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    type=0
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200,"words":[{"type":"0","word":"黄赌毒","replaceWord":"***"},{"type":"0","word":"qqq","replaceWord":"ddd"}]}
    ```

-   查询全部敏感词示例

 - HTTP 请求：

    ```
    POST /sensitiveword/list.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    type=2
    ```

 - HTTP 响应：

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200,"words":[{"type":"0","word":"黄赌毒","replaceWord":"***"},{"type":"1","word":"qqq","replaceWord":""}]}
    ```

<a id = "im-callback"></a>
## IM 回调接口类

该类为 IM 接口类的回调类。

### Token error callback (OnTokenIncorrectHandler)

```
public delegate void OnTokenIncorrectHandler ();
public OnTokenIncorrectHandler OnTokenIncorrect;
```

Token 错误时，触发此回调。

### 连接服务器success callback (OnConnectSuccessHandler)

```
public delegate void OnConnectSuccessHandler (string s);
public OnConnectSuccessHandler OnConnectSuccess;
```

连接服务器成功时，触发此回调。

### 连接服务器失败回调 (OnConnectErrorHander)

```
public delegate void OnConnectErrorHander (string s);
public OnConnectErrorHander OnConnectError;
```

连接服务器失败时，触发此回调。

### 消息已发送回调 (OnSendMessageAttachedHandler)

```
public delegate void OnSendMessageAttachedHandler (Message message);
public OnSendMessageAttachedHandler OnSendMessageAttached;
```

消息已发送时，触发此回调。

> 消息发送成功与否不能通过此回调判断。

### 发送消息success callback (OnSendMessageSuccessHandler)

```
public delegate void OnSendMessageSuccessHandler (Message message);
public OnSendMessageSuccessHandler OnSendMessageSuccess;
```

发送消息成功时，触发此回调。

### 发送消息失败回调 (OnSendMessageErrorHandler)

```
public delegate void OnSendMessageErrorHandler (string errCode);
public OnSendMessageErrorHandler OnSendMessageError;
```

发送消息失败时，触发此回调。

### 收到消息回调 (OnMessageReceivedHandler)

```
public delegate void OnMessageReceivedHandler (Message message);
public OnMessageReceivedHandler OnMessageReceived;
```

收到消息时，触发此回调。

> 收到点对点消息，聊天室消息，或讨论组消息都会触发此回调。具体消息类别需要通过 message 中的 objectName 字段判断：
 	
> -  语音消息对应的字段是 RC:VcMsg;
> - 文字消息对应的字段是 RC:TxtMsg。
                                    
### 加入聊天室success callback (OnJoinChatRoomSuccessHandler)

```
public delegate void OnJoinChatRoomSuccessHandler ();
public OnJoinChatRoomSuccessHandler OnJoinChatRoomSuccess;
```

加入聊天室成功时，触发此回调。

### 加入聊天室失败回调 (OnJoinChatRoomErrorHandler)

```
public delegate void OnJoinChatRoomErrorHandler (string errCode);
public OnJoinChatRoomErrorHandler OnJoinChatRoomError;
```

加入聊天室失败时，触发此回调。

### 获取历史消息success callback (OnGetHistoryMessageSuccessHandler)

```
public delegate void OnGetHistoryMessageSuccessHandler (Message[] messageList);
public OnGetHistoryMessageSuccessHandler OnGetHistoryMessageSuccess;
```

获取历史消息成功时，触发此回调。

### 获取历史消息失败回调 (OnGetHistoryMessageErrorHandler)

```
public delegate void OnGetHistoryMessageErrorHandler (string errCode);
public OnGetHistoryMessageErrorHandler OnGetHistoryMessageError;
```

获取历史消息失败时，触发此回调。

### 清除消息success callback (OnClearMessageSuccessHandler)

```
public delegate void OnClearMessageSuccessHandler (bool success);
public OnClearMessageSuccessHandler OnClearMessageSuccess;
```

清除消息成功时，触发此回调。

### 清除消息失败回调 (OnClearMessageErrorHandler)

```
public delegate void OnClearMessageErrorHandler (string errCode);
public OnClearMessageErrorHandler OnClearMessageError;
```

清除消息失败时，触发此回调。

### 发出语音消息回调 (OnSendVoiceMessageAttachedHandler)

```
public delegate void OnSendVoiceMessageAttachedHandler (Message message);
public OnSendVoiceMessageAttachedHandler OnSendVoiceMessageAttached;
```

发出语音消息时，触发此回调。

> 语音消息发送成功与否不能通过此回调判断。

### 发送语音消息success callback (OnSendVoiceMessageSuccessHandler)

```
public delegate void OnSendVoiceMessageSuccessHandler ();
public OnSendVoiceMessageSuccessHandler OnSendVoiceMessageSuccess;
```

发送语音消息成功时，触发此回调。

### 发送语音消息失败回调 (OnSendVoiceMessageErrorHandler)

```
public delegate void OnSendVoiceMessageErrorHandler (string errMessage);
public OnSendVoiceMessageErrorHandler OnSendVoiceMessageError;
```

发送语音消息失败时，触发此回调。

### 加入已经存在的聊天室success callback (OnJoinExistChatRoomSuccessHandler)

```
public delegate void OnJoinExistChatRoomSuccessHandler ();
public OnJoinExistChatRoomSuccessHandler OnJoinExistChatRoomSuccess;
```

加入聊天室，若此聊天室已经存在，且加入此聊天室成功，则触发此回调。

### 加入已经存在的聊天室失败回调 (OnJoinExistChatRoomErrorHandler)

```
public delegate void OnJoinExistChatRoomErrorHandler (string errMessage);
public OnJoinExistChatRoomErrorHandler OnJoinExistChatRoomError;
```

加入聊天室，若此聊天室已经存在，且加入此聊天室失败，则触发此回调。

### 解散聊天室success callback (OnQuitChatRoomSuccessHandler)

```
public delegate void OnQuitChatRoomSuccessHandler ();
public OnQuitChatRoomSuccessHandler OnQuitChatRoomSuccess;
```

解散聊天室成功时，触发此回调。

### 解散聊天室失败回调 (OnQuitChatRoomErrorHandler)

```
public delegate void OnQuitChatRoomErrorHandler (string errMessage);
public OnQuitChatRoomErrorHandler OnQuitChatRoomError;
```

解散聊天室失败时，触发此回调。

### 创建讨论组success callback (OnCreateDiscussionSuccessHandler)

```
public delegate void OnCreateDiscussionSuccessHandler (string s);
public OnCreateDiscussionSuccessHandler OnCreateDiscussionSuccess;
```

创建讨论组成功时，触发此回调。

### 创建讨论组失败回调 (OnCreateDiscussionErrorHandler)

```
public delegate void OnCreateDiscussionErrorHandler (string s);
public OnCreateDiscussionErrorHandler OnCreateDiscussionError;
```

创建讨论组失败时，触发此回调。

### 添加成员到讨论组success callback (OnAddMemberToDiscussionSuccessHandler)

```
public delegate void OnAddMemberToDiscussionSuccessHandler ();
public OnAddMemberToDiscussionSuccessHandler OnAddMemberToDiscussionSuccess;
```

添加成员到讨论组成功时，触发此回调。

### 添加成员到讨论组失败回调 (OnAddMemberToDiscussionErrorHandler)

```
public delegate void OnAddMemberToDiscussionErrorHandler (string s);
public OnAddMemberToDiscussionErrorHandler OnAddMemberToDiscussionError;
```

添加成员到讨论组失败时，触发此回调。

### 从讨论组移除成员success callback (OnRemoveMemberFromDiscussionSuccessHandler)

```
public delegate void OnRemoveMemberFromDiscussionSuccessHandler ();
public OnRemoveMemberFromDiscussionSuccessHandler OnRemoveMemberFromDiscussionSuccess;
```

从讨论组移除成员成功时，触发此回调。

### 从讨论组移除成员失败回调 (OnRemoveMemberFromDiscussionErrorHandler)

```
public delegate void OnRemoveMemberFromDiscussionErrorHandler (string s);
public OnRemoveMemberFromDiscussionErrorHandler OnRemoveMemberFromDiscussionError;
```

从讨论组移除成员失败时，触发此回调。

### 解散讨论组success callback (OnQuitDiscussionSuccessHandler)

```
public delegate void OnQuitDiscussionSuccessHandler ();
public OnQuitDiscussionSuccessHandler OnQuitDiscussionSuccess;
```

解散讨论组成功时，触发此回调。

### 解散讨论组失败回调 (OnQuitDiscussionErrorHandler)

```
public delegate void OnQuitDiscussionErrorHandler (string s);
public OnQuitDiscussionErrorHandler OnQuitDiscussionError;
```

解散讨论组失败时，触发此回调。

### 获取当前讨论组相关信息失败回调 (OnGetDiscussionErrorHandler)

```
public delegate void OnGetDiscussionErrorHandler (string s);
public OnGetDiscussionErrorHandler OnGetDiscussionError;
```

获取当前讨论组相关信息失败时，触发此回调。

### 获取当前讨论组相关信息success callback (OnGetDiscussionSuccessHandler)

```
public delegate void OnGetDiscussionSuccessHandler (Discussion s);
public OnGetDiscussionSuccessHandler OnGetDiscussionSuccess;
```

获取当前讨论组相关信息成功时，触发此回调。

### 设置讨论组名称success callback (OnSetDiscussionNameSuccessHandler)

```
public delegate void OnSetDiscussionNameSuccessHandler ();
public OnSetDiscussionNameSuccessHandler OnSetDiscussionNameSuccess;
```

设置讨论组名称成功时，触发此回调。

### 设置讨论组名称失败回调 (OnSetDiscussionNameErrorHandler)

```
public delegate void OnSetDiscussionNameErrorHandler (string s);
public OnSetDiscussionNameErrorHandler OnSetDiscussionNameError;
```

设置讨论组名称失败时，触发此回调。

### 设置讨论组邀请状态success callback (OnSetDiscussionInviteStatusSuccessHandler)

```
public delegate void OnSetDiscussionInviteStatusSuccessHandler ();
public OnSetDiscussionInviteStatusSuccessHandler OnSetDiscussionInviteStatusSuccess;
```

设置讨论组邀请状态成功时，触发此回调。

### 设置讨论组邀请状态失败回调 (OnSetDiscussionInviteStatusErrorHandler)

```
public delegate void OnSetDiscussionInviteStatusErrorHandler ();
public OnSetDiscussionInviteStatusErrorHandler OnSetDiscussionInviteStatusError;
```

设置讨论组邀请状态失败时，触发此回调。

### 翻译 IM 消息成功回调 (OnTranslateCallBackSuccess)

```
public delegate void OnTranslateCallBackSuccessHandler ();
public OnTranslateCallBackSuccessHandler OnTranslateCallBackSuccess;
```

翻译 IM 消息成功时，触发此回调。

### 翻译 IM 消息失败回调 (OnTranslateCallBackError)

```
public delegate void OnTranslateCallBackErrorHandler ();
public OnTranslateCallBackErrorHandler OnTranslateCallBackError;
```

翻译 IM 消息失败时，触发此回调。

### 语音转文字成功回调 (OnVoiceToTextSuccesss)

```
public delegate void OnVoiceToTextSuccesssHandler ();
public OnVoiceToTextSuccessHandler OnVoiceToTextSuccess;
```

语音转文字成功时，触发此回调。

### 语音转文字失败回调 (OnVoiceToTextError)

```
public delegate void OnVoiceToTextErrorHandler ();
public OnVoiceToTextErrorHandler OnVoiceToTextError;
```

语音转文字失败时，触发此回调。

## 错误码和警告码 - AMG SDK

详见 [错误码和警告码](../../cn/API%20Reference/the_error_game.md)。


