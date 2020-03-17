
---
title: 游戏 API
description: 
platform: Cocos Creator
updatedAt: Mon Nov 25 2019 10:14:30 GMT+0800 (CST)
---
# 游戏 API
Agora Cocos Creator js SDK 包含如下 API：

- [.init(appid)](#module_agora.init)
- [.setChannelProfile(profile)](#module_agora.setChannelProfile)
- [.setClientRole(role)](#module_agora.setClientRole)
- [.joinChannel(token, channelId, [info], [uid])](#module_agora.joinChannel)
- [.leaveChannel()](#module_agora.leaveChannel)
- [.enableAudio()](#module_agora.enableAudio)
- [.disableAudio()](#module_agora.disableAudio)
- [.muteLocalAudioStream(mute)](#module_agora.muteLocalAudioStream)
- [.enableLocalAudio(enabled)](#module_agora.enableLocalAudio)
- [.muteAllRemoteAudioStreams(mute)](#module_agora.muteAllRemoteAudioStreams)
- [.muteRemoteAudioStream(uid, mute)](#module_agora.muteRemoteAudioStream)
- [.enableAudioVolumeIndication(interval, smooth)](#module_agora.enableAudioVolumeIndication)
- [.adjustRecordingSignalVolume(volume)](#module_agora.adjustRecordingSignalVolume)
- [.adjustPlaybackSignalVolume(volume)](#module_agora.adjustPlaybackSignalVolume)
- [.setDefaultAudioRouteToSpeakerphone(bVal)](#module_agora.setDefaultAudioRouteToSpeakerphone)
- [.setParameters(profile)](#module_agora.setParameters)
- [.getVersion()](#module_agora.getVersion)
- [.setLogFile(filePath)](#module_agora.setLogFile)
- [.setLogFilter(filter)](#module_agora.setLogFilter)
- [.on(event, callback, target)](#module_agora.on)

<a id="module_agora.init"></a>

### 初始化 Agora `agora.init(appid)`

| Param | Type                | Description  |
| ----- | ------------------- | ------------ |
| appid | <code>String</code> | Agora App ID |

**Examples:**

```
require("js-agora");
agora.init("AGORA APP ID");
```

初始化 Agora
注意: 在整个应用全局，开发者只需要对引擎做一次初始化。另外引擎的异步调用的回调，如：加入频道成功、退出频道成功等消息，都在同一个对象里。

Agora 音频实时云传输服务，不区分调试环境和正式环境。

<a id="module_agora.setChannelProfile"></a>

### 设置频道属性 `agora.setChannelProfile(profile)`

| Param   | Type                | Description                                   |
| ------- | ------------------- | --------------------------------------------- |
| profile | <code>Number</code> | <li> 0 : 通信模式(默认)<br> <li> 1 : 直播模式 |

**Examples:**

```
agora.setChannelProfile(0)
```

该方法用于设置频道模式(Profile)。引擎需知道应用程序的使用场景(例如通信模式或直播模式), 从而使用不同的优化手段。

> 参数只可使用枚举类型对应的整型值。其它 API 类同。

目前 Agora Native SDK 支持以下几种模式:

| 模式     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| 通信     | 通信为默认模式，用于常见的一对一或群聊，频道中的任何用户可以自由说话 |
| 直播     | 直播模式有主播和观众两种用户角色，可以通过调用 setClientRole 设置。主播可收发语音和视频，但观众只能收，不能发 |
| 游戏语音 | 频道内任何用户可自由发言。该模式下默认使用低功耗低码率的编解码器 |

> - 同一频道内只能同时设置一种模式。
> - 不同的频道模式必须使用不同的 App ID。
> - 该方法必须在加入频道前调用和进行设置，进入频道后无法再设置。

<a id="module_agora.setClientRole"></a>

### 设置用户角色 `agora.setClientRole(role)`

```
agora.setClientRole(2);
```

| Param | Type                | Description                          |
| ----- | ------------------- | ------------------------------------ |
| role  | <code>Number</code> | <li>1 : 主播 <br> <li>2 : 观众(默认) |

直播模式下，在加入频道前，用户需要通过本方法设置观众(默认)或主播模式；在加入频道后，用户可以通过本方法切换用户模式。

<a id="module_agora.joinChannel"></a>

### 加入频道 `agora.joinChannel(token, channelId, [info], [uid])`

```
agora.joinChannel("", "CHANNEL_ID");
```

| Param     | Type                | Description                                                  |
| --------- | ------------------- | ------------------------------------------------------------ |
| token     | <code>String</code> | <li>安全要求不高：将值设置为空字符串<br><li>安全要求高: 将值设置为 Token 值。 如果你已经启用了 App 证书, 请务必使用 Token。 关于如何获取 Token，详见[密钥说明](https://docs.agora.io/cn/Voice/token?platform=All%20Platforms) |
| channelId | <code>String</code> | 标识通话的频道名称，长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^\_,{ |
| [info]    | <code>String</code> | (非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户 |
| [uid]     | <code>number</code> | (非必选项) 用户 ID，32 位无符号整数。建议设置范围：1 到 (2<sup>32</sup>-1)，并保证唯一性。如果不指定（即设为 0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护，uid 在 SDK 内部用 32 位无符号整数表示，由于 Java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，Java 会表示为负数，如有需要可以用(uid&amp;0xffffffffL)转换成 64 位整数 |

该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。 使用不同 App ID 的应用程序是不能互通的。如果已在通话中，用户必须调用 `agora.leaveChannel()` 退出当前通话，才能进入下一个频道。

> 同一个频道里不能出现两个相同的 UID。如果你的 App 支持多设备同时登录，即同一个用户账号可以在不同的设备上同时登录(例如微信支持在 PC 端和移动端同时登录)，请保证传入的 UID 不相同。 例如你之前都是用同一个用户标识作为 UID, 建议从现在开始加上设备 ID, 以保证传入的 UID 不相同 。如果你的 App 不支持多设备同时登录，例如在电脑上登录时，手机上会自动退出，这种情况下就不需要在 UID 上添加设备 ID。

<a id="module_agora.leaveChannel"></a>

### 离开频道 `agora.leaveChannel()`

```
agora.leaveChannel();
```

离开频道，即挂断或退出通话。
`joinChannel` 后，必须调用 `leaveChannel` 以结束通话，否则不能进行下一次通话。不管当前是否在通话中，都可以调用 `leaveChannel`，没有副作用。如果成功，则返回值为 0。`leaveChannel` 会把会话相关的所有资源释放掉。

`leaveChannel` 是异步操作，调用返回时并没有真正退出频道。在真正退出频道后，SDK 会触发 `onLeaveChannel` 回调。

<a id="module_agora.enableAudio"></a>

### 打开音频 `agora.enableAudio()`

```
agora.enableAudio();
```

> 该方法设置内部引擎为启用状态，在 `leaveChannel` 后仍然有效。

<a id="module_agora.disableAudio"></a>

### 关闭音频 `agora.disableAudio()`

```
agora.disableAudio();
```

> 该方法设置内部引擎为禁用状态，在 `leaveChannel` 后仍然有效。

<a id="module_agora.muteLocalAudioStream"></a>

### 将自己静音 `agora.muteLocalAudioStream(mute)`

```
agora.muteLocalAudioStream(true);
```

| Param | Type                 | Description                                       |
| ----- | -------------------- | ------------------------------------------------- |
| mute  | <code>Boolean</code> | <li>true: 麦克风静音<br><li>false: 取消麦克风静音 |

静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。

<a id="module_agora.enableLocalAudio"></a>

### 关闭/重启本地语音功能 `agora.enableLocalAudio(enabled)`

```
agora.enableLocalAudio(true);
```

| Param   | Type                 | Description                                                  |
| ------- | -------------------- | ------------------------------------------------------------ |
| enabled | <code>Boolean</code> | <li>true：重新开启本地语音功能，即开启本地语音采集或处理<br><li>false：关闭本地语音功能，即停止本地语音采集或处理 |

当用户加入频道时，语音功能默认是开启的。该方法可以关闭或重新开启本地语音功能，停止或重新开始本地音频采集及处理。
该方法不影响接收或播放远端音频流，适用于只听不发的用户场景。

> - 该方法需要在 `joinChannel` 之后调用才能生效。
> - 该方法与 `muteLocalAudioStream` 的区别在于：
>   - `enableLocalAudio`：开启或关闭本地语音采集及处理
>   - `muteLocalAudioStream`：停止或继续发送本地音频流

<a id="module_agora.muteAllRemoteAudioStreams"></a>

### 静音所有远端音频 `agora.muteAllRemoteAudioStreams(mute)`

| Param | Type                 | Description                                                  |
| ----- | -------------------- | ------------------------------------------------------------ |
| mute  | <code>Boolean</code> | <li>True: 停止接收和播放所有远端音频流<br><li>False: 允许接收和播放所有远端音频流 |

该方法用于允许/禁止播放远端用户的音频流，即对所有远端用户进行静音与否。

<a id="module_agora.muteRemoteAudioStream"></a>

### 静音指定用户音频 `agora.muteRemoteAudioStream(uid, mute)`

```
agora.muteRemoteAudioStream("UID", true);
```

| Param | Type                 | Description                                                  |
| ----- | -------------------- | ------------------------------------------------------------ |
| uid   | <code>String</code>  | 指定用户 ID                                                  |
| mute  | <code>Boolean</code> | <li>true: 停止接收和播放指定音频流<br><li>talse: 允许接收和播放指定音频流 |

静音指定远端用户/对指定远端用户取消静音。

> 如果之前有调用过 muteAllRemoteAudioStreams (true) 对所有远端音频进行静音，在调用本 API 之前请确保你已调用 muteAllRemoteAudioStreams (false) 。 muteAllRemoteAudioStreams 是全局控制，muteRemoteAudioStream 是精细控制。

<a id="module_agora.enableAudioVolumeIndication"></a>

### 启用说话者音量提示 `agora.enableAudioVolumeIndication(interval, smooth)`

```
agora.enableAudioVolumeIndication(1000, 3)
```

| Param    | Type                | Description                                                  |
| -------- | ------------------- | ------------------------------------------------------------ |
| interval | <code>Number</code> | 指定音量提示的时间间隔<br> <li>&lt;= 0：禁用音量提示功能<br><li>&gt; 0：返回音量提示的间隔，单位为毫秒。建议设置到大于 200 毫秒。最小不得少于 10 毫秒，否则会收不到 <code>onAudioVolumeIndication</code> 回调 |
| smooth   | <code>Number</code> | 平滑系数，指定音量提示的灵敏度。取值范围为 [0-10]，建议值为 3，数字越大，波动越灵敏；数字越小，波动越平滑。 |

该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。启用该方法后，无论频道内是否有人说话，都会在 `onAudioVolumeIndication` 回调中按设置的间隔时间返回音量提示。

<a id="module_agora.adjustRecordingSignalVolume"></a>

### agora.adjustRecordingSignalVolume(volume)

调节录音信号音量

| Param  | Type                | Description                              |
| ------ | ------------------- | ---------------------------------------- |
| volume | <code>Number</code> | 录音信号音量可在 0 ～ 400 范围内进行调节 |

<a id="module_agora.adjustPlaybackSignalVolume"></a>

### 调节播放信号音量 `agora.adjustPlaybackSignalVolume(volume)`

```
agora.adjustPlaybackSignalVolume(100)
```

| Param  | Type                | Description                                                  |
| ------ | ------------------- | ------------------------------------------------------------ |
| volume | <code>Number</code> | 录音信号音量可在 0~400 范围内进行调节:<li>0: 静音<br><li>100: 原始音量<br><li>400: 最大可为原始音量的 4 倍(自带溢出保护) |

该方法调节录音信号音量。

<a id="module_agora.setDefaultAudioRouteToSpeakerphone"></a>

### 修改默认的语音路由 `agora.setDefaultAudioRouteToSpeakerphone(bVal)`

```
agora.setDefaultAudioRouteToSpeakerphone(true)
```

| Param | Type                 | Description                                                  |
| ----- | -------------------- | ------------------------------------------------------------ |
| bVal  | <code>Boolean</code> | <li>true: 默认路由改为外放(扬声器)<br><li>false: 默认路由改为听筒<br>无论语音是从外放还是听筒出声，如果插上耳机或连接蓝牙，语音路由会发生相应改变。拔出耳机或断开蓝牙后语音路由将恢复成默认路由。 |

- 该方法只在纯音频模式下工作，在有视频的模式下不工作。
- 该方法需要在 `joinChannel` 前设置，否则不生效。

如有必要，调用该方法修改默认的语音路由。默认的语音路由如下:

| 频道模式 | 默认语音路由                                                 |
| -------- | ------------------------------------------------------------ |
| 通信     | <li>语音通话: 听筒<br><li>视频通话: 外放<br><li>视频通话中关闭视频 : 听筒 |
| 直播     | 外放                                                         |
| 游戏语音 | 外放                                                         |

> 已调用 `disableVideo` 或 已调用 `muteLocalVideoStream` 或 `muteAllRemoteVideoStreams`



<a id="module_agora.setParameters"></a>

### 自定义参数 `agora.setParameters(profile)`

```
agora.setParameters('{"key": "value"}');
```

| Param   | Type               | Description           |
| ------- | ------------------ | --------------------- |
| profile | <code>String/code> | JSON 字符串形式的参数 |

通过 JSON 配置 SDK 提供技术预览或特别定制功能。

JSON 选项默认不公开。声网工程师正在努力寻求以标准化方式公开 JSON 选项。

<a id="module_agora.getVersion"></a>

### 查询 SDK 版本号 `agora.getVersion()`

```
agora.getVersion();
```

<a id="module_agora.setLogFile"></a>
该方法返回 SDK 版本号的字符串。

### 设置日志文件 `agora.setLogFile(filePath)`

```
agora.setLogFile(filePath);
```

| Param    | Type                | Description                                   |
| -------- | ------------------- | --------------------------------------------- |
| filePath | <code>String</code> | 日志文件的完整路径。该日志文件为 UTF-8 编码。 |

设置 SDK 的输出 log 文件。SDK 运行时产生的所有 log 将写入该文件。应用程序必须保证指定的目录存在而且可写。

<a id="module_agora.setLogFilter"></a>

### 设置日志过滤器等级 `agora.setLogFilter(filter)`

```
agora.setLogFilter(0x80f);
```

| Param  | Type                | Description                                                  |
| ------ | ------------------- | ------------------------------------------------------------ |
| filter | <code>Number</code> | 设置过滤器等级:<br><li>LOG_FILTER_OFF = 0：不输出日志信息<br><li>LOG_FILTER_DEBUG = 0x80f：输出所有 API 日志信息<br><li>LOG_FILTER_INFO = 0x0f：输出 CRITICAL、ERROR、WARNING 和 INFO 级别的日志信息<br><li>LOG_FILTER_WARNING = 0x0e：输出 CRITICAL、ERROR 和 WARNING 级别的日志信息<br><li>LOG_FILTER_ERROR = 0x0c：输出 CRITICAL 和 ERROR 级别的日志信息<br><li>LOG_FILTER_CRITICAL = 0x08：输出 CRITICAL 级别的日志信息 |

设置 SDK 的输出日志过滤等级。不同的过滤等级可以单独或组合使用。

日志级别顺序依次为 _OFF_、_CRITICAL_、_ERROR_、_WARNING_、_INFO_ 和 _DEBUG_。选择一个级别，你就可以看到在该级别之前所有级别的日志信息。

例如，你选择 _WARNING_ 级别，就可以看到在 _CRITICAL_、_ERROR_ 和 _WARNING_ 级别上的所有日志信息。

<a id="module_agora.on"></a>

### 监听事件信息 `agora.on(event, callback, target)`

Agora SDK 通过该方法监听上述方法触发的事件，主要包含如下事件。更多事件操作请参考 <a href="https://docs.cocos.com/creator/api/zh/classes/EventTarget.html" target="_blank">EventTarget</a>。
	
- [agora.on('join-channel-success', (channel, uid, elapsed) => {}, this);](#joinChannelSuccess)
- [agora.on('audio-volume-indication', (speakers, speakerNumber, totalVolume) => {}, this);](#audioVolumeIndication)
- [agora.on('error', (err, msg) => {}, this);](#error)
- [agora.on('leave-channel', (stat) => {}, this);](#leaveChannel)
- [agora.on('user-joined', (uid, elapsed) => {}, this);](#userJoined)
- [agora.on('user-offline', (uid, reason) => {}, this);](#userOffline)
- [agora.on('user-mute-audio', (uid, muted) => {}, this);](#userMuteAudio)
- [agora.on('connection-interrupted', () => {}, this);](#connectionInterrupted)
- [agora.on('request-token', () => {}, this);](#requestToken)
- [agora.on('client-role-changed', (oldRole, newRole) => {}, this);](#clientRoleChanged)
- [agora.on('rejoin-channel-success', (channel, uid, elapsed) => {}, this);](#rejoinChannelSuccess)
- [agora.on('audio-quality', (uid, quality, delay, lost) => {}, this);](#audioQuality)
- [agora.on('warning', (warn, msg) => {}, this);](#warning)
- [agora.on('network-quality', (uid, txQuality, rxQuality) => {}, this);](#networkQuality)
- [agora.on('audio-routing-changed', (routing) => {}, this);](#audioRoutingChanged)
- [agora.on('connection-lost', () => {}, this);](#connectionLost)
- [agora.on('connection-banned', () => {}, this);](#connectionBanned)
- [agora.on("init-success", () => {}, this);](#initSuccess)
- [agora.on('recording-device-changed', (state, device) => {}, this);](#recordingDeviceChanged)

<a name="joinChannelSuccess"></a>

#### 加入频道成功回调 `agora.on('join-channel-success', (channel, uid, elapsed) => {}, this);`

**Kind**: global function  

| Param   | Type                | Description                                                  |
| ------- | ------------------- | ------------------------------------------------------------ |
| channel | <code>String</code> | 频道名称                                                     |
| uid     | <code>Number</code> | 用户ID。如果 joinChannel 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID |
| elapsed | <code>Number</code> | 从 joinChannel 开始到该事件产生的延迟（毫秒）                |

<a name="audioVolumeIndication"></a>

#### 说话声音音量提示回调 `agora.on('audio-volume-indication', (speakers, speakerNumber, totalVolume) => {}, this);`

**Kind**: global function  

| Param         | Type                | Description               |
| ------------- | ------------------- | ------------------------- |
| speakers      | <code>Array</code>  | 说话者（数组）。每个 speaker():<li>uid: 说话者的用户 ID。如果返回的 uid 为 0，则默认为本地用户<li>volume: 说话者的音量（0~255）     |
| speakerNumber | <code>Number</code> | speakers 数组的大小       |
| totalVolume   | <code>Number</code> | （混音后的）总音量(0~255) |

<a name="error"></a>

#### 发生错误回调 `agora.on('error', (err, msg) => {}, this);`

**Kind**: global function  

| Param | Type                | Description |
| ----- | ------------------- | ----------- |
| err   | <code>\*</code>     | 错误码    |
| msg   | <code>String</code> | 错误描述    |

<a name="leaveChannel"></a>

#### 离开频道回调 `agora.on('leave-channel', (stat) => {}, this);`

**Kind**: global function  

| Param | Type                | Description                                                  |
| ----- | ------------------- | ------------------------------------------------------------ |
| stat  | <code>Object</code> | 通话相关的统计信息。 <li>duration: 通话时长（秒） <li>txBytes: 发送字节数（bytes） <li>rxBytes: 接收字节数（bytes） <li>txKBitRate: 发送码率（kbps） <li>rxKBitRate: 接收码率（kbps） <li>rxAudioKBitRate: 音频接收码率 (kbps) <li>txAudioKBitRate: 音频发送频率 (kbps) <li>rxVideoKBitRate: 视频接收码率 (kbps) <li>txVideoKBitRate: 视频发送码率 (kbps) <li>userCount: 用户离开频道时频道内的瞬时人数 <li>cpuAppUsage: 当前应用程序的 CPU 使用率 (%) <li>cpuTotalUsage: 当前系统的 CPU 使用率 (%) |

<a name="userJoined"></a>

#### 其他用户加入当前频道回调 `agora.on('user-joined', (uid, elapsed) => {}, this);`

**Kind**: global function  

| Param   | Type                | Description                               |
| ------- | ------------------- | ----------------------------------------- |
| uid     | <code>Number</code> | 用户 ID                                   |
| elapsed | <code>Number</code> | joinChannel 开始到该回调触发的延迟（毫秒) |

<a name="userOffline"></a>

#### 其他用户离开当前频道回调 `agora.on('user-offline', (uid, reason) => {}, this);`

**Kind**: global function  

| Param  | Type                | Description |
| ------ | ------------------- | ----------- |
| uid    | <code>Number</code> | 用户 ID      |
| reason | <code>Number</code> | 离线原因    |

<a name="userMuteAudio"></a>

#### 用户被静音回调 `agora.on('user-mute-audio', (uid, muted) => {}, this);`

**Kind**: global function  

| Param | Type                 | Description |
| ----- | -------------------- | ----------- |
| uid   | <code>Number</code>  | 用户 ID     |
| muted | <code>Boolean</code> | 是否静音    |

<a name="connectionInterrupted"></a>

#### 网络中断回调 `agora.on('connection-interrupted', () => {}, this);`

**Kind**: global function  
<a name="requestToken"></a>

#### Token 过期回调 `agora.on('request-token', () => {}, this);`

**Kind**: global function  
<a name="clientRoleChanged"></a>

#### 上下麦回调 `agora.on('client-role-changed', (oldRole, newRole) => {}, this);`

**Kind**: global function  

| Param   | Type            | Description  |
| ------- | --------------- | ------------ |
| oldRole | <code>\*</code> | 切换前的角色 |
| newRole | <code>\*</code> | 切换后的角色 |

<a name="rejoinChannelSuccess"></a>

#### 重新加入频道成功回调 `agora.on('rejoin-channel-success', (channel, uid, elapsed) => {}, this);`

**Kind**: global function  

| Param   | Type                | Description                                   |
| ------- | ------------------- | --------------------------------------------- |
| channel | <code>String</code> | 频道名称                                      |
| uid     | <code>Number</code> | 用户ID。                                      |
| elapsed | <code>Number</code> | 从 joinChannel 开始到该事件产生的延迟（毫秒） |

<a name="audioQuality"></a>

#### 声音质量回调 `agora.on('audio-quality', (uid, quality, delay, lost) => {}, this);`

**Kind**: global function  

| Param   | Type            | Description          |
| ------- | --------------- | -------------------- |
| uid     | <code>\*</code> | 用户 ID              |
| quality | <code>\*</code> | 音频质量             |
| delay   | <code>\*</code> | 延迟，单位为毫秒     |
| lost    | <code>\*</code> | 丢包率，单位为百分比 |

<a name="warning"></a>

#### 发生警告回调 `agora.on('warning', (warn, msg) => {}, this);`

**Kind**: global function  

| Param | Type            | Description |
| ----- | --------------- | ----------- |
| warn  | <code>\*</code> | 警告码    |
| msg   | <code>\*</code> | 警告描述    |

<a name="networkQuality"></a>

#### 频道内网络质量报告回调 `agora.on('network-quality', (uid, txQuality, rxQuality) => {}, this);`

**Kind**: global function  

| Param     | Type            | Description  |
| --------- | --------------- | ------------ |
| uid       | <code>\*</code> | 用户ID       |
| txQuality | <code>\*</code> | 上行网络质量 |
| rxQuality | <code>\*</code> | 下行网络质量 |

<a name="audioRoutingChanged"></a>

#### 声音路由发生变化 `agora.on('audio-routing-changed', (routing) => {}, this);`

**Kind**: global function  

| Param   | Type            | Description |
| ------- | --------------- | ----------- |
| routing | <code>\*</code> | 路由信息    |

<a name="connectionLost"></a>

#### 连接丢失回调 `agora.on('connection-lost', () => {}, this);`

**Kind**: global function  
<a name="connectionBanned"></a>

#### 连接已被禁止回调 `agora.on('connection-banned', () => {}, this);`

**Kind**: global function  
<a name="initSuccess"></a>

#### agora 初始化成功 `agora.on("init-success", () => {}, this);`

**Kind**: global function  

<a name="recordingDeviceChanged"></a>

#### 录音设备发生变化回调 `agora.on('recording-device-changed', (state, device) => {}, this);`

**Kind**: global function  

| Param  | Type            | Description |
| ------ | --------------- | ----------- |
| state  | <code>\*</code> | 设备状态    |
| device | <code>\*</code> | 设备        |

