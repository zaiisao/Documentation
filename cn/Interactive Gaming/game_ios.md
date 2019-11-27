
---
title: 游戏 API
description: 
platform: iOS
updatedAt: Mon Nov 25 2019 10:12:10 GMT+0800 (CST)
---
# 游戏 API
<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Objective-C 接口类</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><a href="#agorartcenginekit">AgoraRtcEngineKit 接口类</a></td>
<td>包含 App 调用的主要方法</td>
</tr>
<tr><td><a href="#agorartcenginekitdelegate">AgoraRtcEngineKitDelegate 接口类</a></td>
<td>包含回调方法</td>
</tr>
</tbody>
</table>

<a id = "agorartcenginekit"></a>
## AgoraRtcEngineKit 接口类

### 初始化 (sharedEngineWithappId)

```
+ (instancetype)sharedEngineWithAppId:(NSString *)appId
delegate:(id<AgoraRtcEngineKitDelegate>)delegate;
```

该方法初始化 AgoraRtcEngineKit 为一个单例。使用 AgoraRtcEngineKit ，必须先调用该接口进行初始化。 Agora SDK 通过指定的 delegate 通知应用程序引擎运行时的事件。Delegate 中定义的所有方法都是可选实现的。

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
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a></td>
</tr>
<tr><td><code>delegate</code></td>
<td> </td>
</tr>
</tbody>
</table>



### 设置频道属性 (setChannelProfile)

```
- (int)setChannelProfile (AgoraChannelProfile) profile;
```

该方法用于设置频道模式 (Profile)。Agora RtcEngine 需知道应用程序的使用场景, 从而使用不同的优化手段。


> -   同一频道内只能同时设置一种模式。
> -   该方法必须在加入频道前调用和进行设置，进入频道后无法再设置


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
<div><ul>
<li>AgoraChannelProfileGame_freemode = 2: 自由发言模式</li>
<li>AgoraChannelProfileGame_commandmode = 3: 指挥模式</li>
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
- (int)setClientRole:(AgoraClientRole)role;
```

在加入频道前，用户需要通过本方法设置观众 (默认)或主播模式。在加入频道后，用户可以通过本方法切换用户模式。

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
<tr><td><code>profile</code></td>
<td><p>指挥模式频道的用户角色:</p>
<ul>
<li>CLIENT_ROLE_BROADCASTER = 1; 主播(既指挥者), 主播可以发言指挥</li>
<li>CLIENT_ROLE_AUDIENCE = 2; 观众(默认，被指挥者)，观众服务命令</li>
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



### 加入频道 (joinChannelByToken)

```
- (int)joinChannelByToken:(NSString * _Nullable)token
                channelId:(NSString * _Nonnull)channelId
                     info:(NSString * _Nullable)info
                      uid:(NSUInteger)uid
              joinSuccess:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))joinSuccessBlock;
```

该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。 使用不同 App ID 的应用程序是不能互通的。如果已在通话中，用户必须调用 `leaveChannel` 退出当前通话，才能进入下一个频道。 SDK 在通话中使用 iOS 系统的 AVAudioSession 共享对象进行录音和播放，应用程序对该对象的操作可能会影响 SDK 的音频相关功能。

调用该 API 后会触发回调，如果同时实现 `joinChannelSuccessBlock` 和 `didJoinChannel` 会出现冲突，`joinChannelSuccessBlock` 优先级高，2 个同时存在时，`didJoinChannel` 会被忽略。 需要有 `didJoinChannel` 回调时，请将 `joinChannelSuccessBlock` 设置为 nil 。

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
<tr><td><code>Token</code></td>
<td><ul>
<li>安全要求不高: 将值设为 nil</li>
<li>安全要求高: 将值设置为 Token。如果你已经启用了 App 证书, 请务必使用 Token。关于如何获取 Token，详见 <a href="../../cn/Agora%20Platform/token.md"><span>密钥说明</span></a></li>
</ul>
</td>
</tr>
<tr><td><code>channelId</code></td>
<td>标识通话的频道名称，长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>info</code></td>
<td>(非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户。</td>
</tr>
<tr><td><code>uid</code></td>
<td><p>(非必选项) 用户 ID，32 位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护。</p>
<div>uid 在 SDK 内部用 32 位无符号整数表示，由于 Java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，Java 会表示为负数，如有需要可以用 (uid&amp;0xffffffffL) 转换成 64 位整数。</div>
</td>
</tr>
<tr><td><code>joinSuccessBlock</code></td>
<td>成功加入频道的回调</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2)：传递的参数无效</li>
<li>ERR_NOT_READY (-3)：没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>

> 在 `joinChannel` 时，SDK 调用 `setCategoryAVAudioSessionCategoryPlayAndRecord `将 AVAudioSession 设置到 PlayAndRecord 模式，应用程序不应将其设置到其他模式。设置该模式时，正在播放的声音会被打断（比如正在播放的响铃声）。

### 离开频道 \(leaveChannel\)

```
- (int)leaveChannel;
```

离开频道，即挂断或退出通话。加入频道后，必须调用 `leaveChannel`以结束通话，否则不能进行下一次通话。

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



### 打开音频 \(enableAudio\)

```
- (int)enableAudio;
```

该方法打开音频\(默认为开启状态\)。

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



### 关闭音频 \(disableAudio\)

```
- (int)disableAudio;
```

该方法关闭音频模式。

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



### 将自己静音 \(muteLocalAudioStream\)

```
- (int)muteLocalAudioStream:(BOOL)mute;
```

该方法用于允许/禁止往网络发送本地音频流。

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
<li>False: 取消麦克风静音</li>
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



### 静音所有远端音频 \(muteAllRemoteAudioStreams\)

```
- (int)muteAllRemoteAudioStreams:(BOOL)mute;
```

该方法用于允许/禁止播放远端用户的音频流。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>True: 停止播放所接收的音频流</li>
<li>False: 恢复播放所接收的音频流</li>
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



### 静音指定用户音频 \(muteRemoteAudioStream\)

```
- (int)muteRemoteAudioStream:(NSUInteger)uid
                            :(BOOL)mute;
```

本方法用于允许/禁止播放远端用户的音频流。

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
<li>False: 恢复播放指定用户的音频流</li>
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



### 启用说话者音量提示 \(enableAudioVolumeIndication\)

```
- (int)enableAudioVolumeIndication:(NSInteger)interval
                            smooth:(NSInteger)smooth;
```

该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。

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
<td><p>指定音量提示的时间间隔</p>
<div><ul>
<li>&lt;=0: 禁用音量提示功能</li>
<li>&gt;0: 提示间隔，单位为毫秒。建议设置到大于 200 毫秒</li>
</ul>
</td>
</tr>
<tr><td><code>smooth</code></td>
<td>平滑系数。默认可以设置为 3</td>
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



### 设置仅限语音模式 \(setVoiceOnlyMode\)

```
- (int) setVoiceOnlyMode:(BOOL) enable;
```

该方法设置 仅限语音 模式，即仅传输语音流，而不传输其他流，例如，敲键盘的声音等。

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
</tbody>
</table>



### 设置远端用户的语音位置 \(setRemoteVoicePosition\)

```
- (int) setRemoteVoicePosition:(NSUInteger) uid
                           pan:(double) pan
                          gain:(double) gain;
```

该方法设置远端用户的语音位置。

此方法可以实现听声辨位功能。

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
<td><p>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。</p>
<p>默认值为 100.0。取值越小，音效的音量越低。</p>
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



### 设置本地语音音调 \(setLocalVoicePitch\)

```
- (int) setLocalVoicePitch:(double) pitch;
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



### 设置日志文件 \(setLogFile\)

```
- (int)setLogFile:(NSString*)filePath;
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
<tr><td><code>filePath</code></td>
<td>日志文件的完整路径。该日志文件为 UTF-8 编码。</td>
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



### 设置日志文件过滤器 \(setLogFilter\)

```
- (int)setLogFilter:(NSUInteger)filter;
```

设置SDK的输出日志过滤器。不同的过滤器可以用或组合。

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
<li>LOG_FILTER_CRITICAL = 0x0008;</li>
<li>LOG_FILTER_ERROR = 0x000c;</li>
<li>LOG_FILTER_WARN = 0x000e;</li>
<li>LOG_FILTER_INFO = 0x000f;</li>
<li>LOG_FILTER_CRITICAL = 0x080f;</li>
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



### 查询 SDK 版本号 \(getSdkVersion\)

```
+ (NSString *)getSdkVersion;
```

该方法返回 SDK 版本号的字符串。

### 更新 Token \(renewToken\)

```
- (int)renewToken:(NSString * _Nonnull)token;
```

该方法用于更新 Token。如果启用了 Token 机制，过一段时间后使用的 Token 会失效。

当：

-   发生 `rtcEngine:tokenPrivilegeWillExpire`: 回调时，

-  `rtcEngine:didOccurError`: 回调报告 ERR\_TOKEN\_EXPIRED\(109\) 时，

-   `rtcEngineRequestToken`: 回调报告 ERR\_TOKEN\_EXPIRED\(109\) 时，


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
<tr><td><code>Token</code></td>
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



### 开始客户端录音 \(startAudioRecording\)

```
- (int)startAudioRecording:(NSString*)filePath;
```

Agora SDK 支持通话过程中在客户端进行录音，且录音文件格式可以为:

-   .wav : 文件大，音质保真度高

-   .aac : 文件小，有一定的音质保真度损失


确保应用程序里指定的目录存在且可写。该接口需在加入频道之后调用。如果调用 `leaveChannel` 时还在录音，录音会自动停止。

### 停止客户端录音 \(stopAudioRecording\)

```
- (int)stopAudioRecording;
```

该方法停止客户端录音。该接口需要在 `leaveChannel` 之前调用，如果没有调用，在 `leaveChannel` 时会自动停止。

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



### 开始客户端本地混音 \(startAudioMixing\)

```
- (int) startAudioMixing: (NSString*) filePath
                loopback: (BOOL) loopback
                 replace: (BOOL) replace
                   cycle: (NSInteger) cycle
                playTime: (NSInteger) playTime;
```

指定本地音频文件来和麦克风采集的音频流进行混音和替换\(用音频文件替换麦克风采集的音频流\)，可以通过参数选择对方是否能听到本地播放的音频和指定循环播放的次数。

> -   如需使用 `startAudioMixing` API，要求 iOS 8.0 或以上设备。
> 
> -   请在频道内调用该方法，如果在频道外调用该方法可能会出现问题。


### 停止客户端本地混音 \(stopAudioMixing\)

```
- (int)stopAudioMixing;
```

停止本地音频文件混音。请在频道内调用该方法。

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



### 暂停播放伴奏 \(pauseAudioMixing\)

```
- (int)pauseAudioMixing;
```

该方法暂停播放伴奏。请在频道内调用该方法。

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



### 恢复播放伴奏 \(resumeAudioMixing\)

```
- (int)resumeAudioMixing;
```

该方法恢复混音, 继续播放伴奏。请在频道内调用该方法。

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



### 调节伴奏音量 \(adjustAudioMixingVolume\)

```
- (int)adjustAudioMixingVolume:(NSInteger) volume;
```

该方法调节混音里伴奏的音量大小。请在频道内调用该方法。

### 获取伴奏时长 \(getAudioMixingDuration\)

```
- (int)getAudioMixingDuration;
```

该方法获取伴奏时长，单位为毫秒。请在频道内调用该方法。如果返回值 <0，表明调用失败。

### 获取伴奏播放进度 \(getAudioMixingCurrentPosition\)

```
- (int)getAudioMixingCurrentPosition;
```

该方法获取当前伴奏播放进度，单位为毫秒。请在频道内调用该方法。

### 调节录音信号音量 \(adjustRecordingSignalVolume\)

```
- (int)adjustRecordingSignalVolume:(NSInteger)volume;
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
<ul>
<li>0: 静音</li>
<li>100: 原始音量</li>
<li>400: 最大可为原始音量的 4 倍(自带溢出保护)</li>
</ul>
</td>
</tr>
<tr/>
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



### 调节播放信号音量 \(adjustPlaybackSignalVolume\)

```
- (int)adjustPlaybackSignalVolume:(NSInteger)volume;
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
<td><p>播放信号音量可在 0~400 范围内进行调节:</p>
<ul>
<li>0: 静音</li>
<li>100: 原始音量</li>
<li>400: 最大可为原始音量的 4 倍(自带溢出保护)</li>
</ul>
</td>
</tr>
<tr/>
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



### 获取音效音量 \(getEffectsVolume\)

```
- (double) getEffectsVolume;
```

该方法获取音量的音量，范围为\[0.0, 1.0\]。

### 设置音效音量 \(setEffectsVolume\)

```
- (int) setEffectsVolume:(double) volume;
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
<td>取值范围为 [0.0, 100.0]。 100.0 为默认值</td>
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



### 播放音效 \(playEffect\)

```
- (int) playEffect: (int) soundId
          filePath: (NSString*) filePath
              loop: (BOOL) loop
             pitch: (double) pitch
               pan: (double) pan
              gain: (double) gain;
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
<td>指定音效的 ID。每个音效均有唯一的 ID <sup>[1]</sup></td>
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
 
> [1] 如果你已通过 `preloadEffect` 将音效加载至内存，确保这里设置的 `soundId` 与 `preloadEffect` 设置的 `soundId` 相同。

### 停止播放指定音效 \(stopEffect\)

```
- (int) stopEffect:(int) soundId;
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
</tbody>
</table>



### 停止播放所有的音效 \(stopAllEffects\)

```
- (int) stopAllEffects;
```

该方法停止播放所有的音效。

### 预加载音效 \(preloadEffect\)

```
- (int) preloadEffect:(int) soundId
             filePath:(NSString*) filePath;
```

该方法将指定音效文件 \(压缩的语音文件\) 预加载至内存。

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
</tbody>
</table>



### 释放音效 \(unloadEffect\)

```
- (int) unloadEffect:(int) soundId;
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
</tbody>
</table>



### 暂停音效播放 \(pauseEffect\)

```
- (int) pauseEffect:(int) soundId;
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
</tbody>
</table>



### 停止所有音效播放 \(pauseAllEffects\)

```
- (int)pauseAllEffects();
```

该方法暂停播放所有音效。

### 恢复播放指定音效 \(resumeEffect\)

```
- (int) resumeEffect:(int) soundId;
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
</tbody>
</table>



### 恢复播放所有音效 \(resumeAllEffects\)

```
- (int) resumeAllEffects;
```

该方法恢复播放所有音效。


<a id = "agorartcenginekitdelegate"></a>

## AgoraRtcEngineKitDelegate 接口类

### 发生警告回调 \(didOccurWarning\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
didOccurWarning:(AgoraRtcErrorCode)warningCode;
```

该回调方法表示SDK运行时出现了（网络或媒体相关的）警告。通常情况下，SDK上报的警告信息应用程序可以忽略，SDK会自动恢复。 比如和服务器失去连接时，SDK 可能会上报 AgoraRtc\_Error\_OpenChannelTimeout\(106\) 警告，同时自动尝试重连。

> 关于详细的警告代码，详见 Interactive Gaming Voice SDK。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>warningCode</code></td>
<td>警告代码</td>
</tr>
</tbody>
</table>



### 发生错误回调 \(didOccurError\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
didOccurError:(AgoraRtcErrorCode)errorCode;
```

该回调方法表示 SDK 运行时出现了（网络或媒体相关的）错误。通常情况下，SDK 上报的错误意味着SDK无法自动恢复，需要应用程序干预或提示用户。 比如启动通话失败时，SDK 会上报 AgoraRtc\_Error\_StartCall\(1002\) 错误。应用程序可以提示用户启动通话失败，并调用 `leaveChannel` 退出频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>engine</code></td>
<td>AgoraRtcEngineKit 对象</td>
</tr>
<tr><td><code>errorCode</code></td>
<td>错误码和警告代码</td>
</tr>
</tbody>
</table>



### 音量提示回调 \(reportAudioVolumeIndicationOfSpeakers\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine reportAudioVolumeIndicationOfSpeakers:
(NSArray*)speakers totalVolume:(NSInteger)totalVolume;
```

提示谁在说话及其音量，默认禁用。可通过 `enableAudioVolumeIndication` 方法设置。

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
<td><p>说话者(数组)。每个说话人:</p>
<div><ul>
<li>uid: 说话者的用户ID</li>
<li>volume: 说话者的音量（0~255, 从低到高)</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>totalVolume</code></td>
<td>(混音后的) 总音量（0~255）</td>
</tr>
</tbody>
</table>



### 加入频道成功回调 \(didJoinChannel\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didJoinChannel:
(NSString*)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
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
<tr><td><code>channel</code></td>
<td>频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID。如果<code>joinChannel</code>中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从<code>joinChannel</code>开始到该事件产生的延迟（毫秒）</td>
</tr>
</tbody>
</table>



### 重新加入频道回调 \(didRejoinChannel\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didRejoinChannel: (NSString*)channel
                                                                withUid: (NSUInteger)uid
                                                                elapsed: (NSInteger) elapsed;
```

有时候由于网络原因，客户端可能会和服务器失去连接，SDK 会进行自动重连，自动重连成功后触发此回调方法，提示有用户重新加入了频道，且频道 ID 和用户 ID 均已分配。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>从<code>joinChannel</code>开始到该事件产生的延迟（毫秒）</td>
</tr>
</tbody>
</table>



### 离开频道回调 \(didLeaveChannelWithStats\)

```
- (void)rtcKit:(AgoraRtcEngineKit *)rtcKit didLeaveChannelWithStats:(AgoraRtcStats*)stats;
```

应用程序调用`leaveChannel`方法时，SDK 提示应用程序离开频道成功。在该回调方法中，应用程序可以得到此次通话的总通话时长、SDK 收发数据的流量等信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>stats</code></td>
<td><p>通话相关的统计信息:</p>
<div><ul>
<li>通话时长，累计值</li>
<li><code>txBytes</code>: 发送字节数，累计值</li>
<li><code>rxBytes</code>: 接收字节数（bytes），累计值</li>
<li><code>txAudioKBitRate</code>: 发送码率（kbps），瞬时值</li>
<li><code>rxAudioKBitRate</code>: 接收码率（kbps），瞬时值</li>
<li><code>users</code>: 频道内的用户</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



### 上下麦回调 \(didClientRoleChanged\)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didClientRoleChanged:(AgoraRtcClientRole)oldRole newRole:(AgoraRtcClientRole)newRole;
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



```
typedef NS_ENUM(NSInteger, AgoraRtcClientRole) {
      AgoraRtc_ClientRole_Broadcaster = 1,
      AgoraRtc_ClientRole_Audience = 2,
};
```

### 语音路由变更回调 \(didAudioRouteChanged\)

```
- (void)rtcKit:(AgoraRtcEngineKit *)rtcKit didAudioRouteChanged:(AudioOutputRouting)routing;
```

SDK 会通过该回调通知 App 语音路由状态已发生变化。

该回调返回当前的语音路由已切换至听筒，外放\(扬声器\)，耳机或蓝牙。

### 用户音频静音回调 \(didAudioMuted\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
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
<tr><td><code>uid</code></td>
<td>用户 ID</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: 静音</li>
<li>False: 取消静音</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 用户加入回调 \(didJoinedOfUid\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didJoinedOfUid:
(NSUInteger)uid elapsed:(NSInteger)elapsed;
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
<td>用户 ID 。如果<code>joinChannel</code>中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>加入频道开始到该回调触发的延迟（毫秒)</td>
</tr>
</tbody>
</table>



### 用户离线回调 \(didOfflineOfUid\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOfflineOfUid:
(NSUInteger)uid reason:(AgoraUserOfflineReason)reason;
```

提示有用户离开了频道（或掉线）。SDK 判断用户离开频道\(或掉线\)的依据是超时: 在一定时间内 \(15秒\) 没有收到对方的任何数据包, 判定为对方掉线。 在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

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
<tr><td><code>reason</code></td>
<td><p>离线原因:</p>
<div><ul>
<li>AgoraUserOfflineReasonQuit: 用户主动离开</li>
<li>AgoraUserOfflineReasonDropped: 因过长时间收不到对方数据包，超时掉线。</li>
</ul>
<p>由于 SDK 使用的是不可靠通道，也有可能对方主动离开本方没收到对方离开消息而误判为超时掉线</p>
</div>
</td>
</tr>
</tbody>
</table>



### 网络连接中断回调 \(rtcEngineConnectionDidInterrupted\)

```
- (void)rtcEngineConnectionDidInterrupted:(AgoraRtcEngineKit *)rtcKit;
```

在 SDK 和服务器失去了网络连接时，触发该回调。失去连接后，除非 APP 主动调用 `leaveChannel`，SDK 会一直自动重连。

### 网络连接丢失回调 \(rtcEngineConnectionDidLost\)

```
- (void)rtcEngineConnectionDidLost:(AgoraRtcEngineKit *)rtcKit;
```

### 用户停止/重新发送视频回调 \(didVideoMuted\)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didVideoMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

提示有其他用户暂停发送/恢复发送其视频流。

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
<td>用户ID</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>Yes: 该用户已暂停发送其视频流</li>
<li>No: 该用户已恢复发送其视频流</li>
</ul>
</td>
</tr>
</tbody>
</table>



### Rtc Engine统计数据回调 \(reportRtcStats\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
reportRtcStats:(AgoraRtcStats*)stats;
```

该回调定期上报Rtc Engine的运行时的状态，每两秒触发一次。

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
<td><p>AgoraRtcStats, 详见如下描述:</p>
<div><ul>
<li><code>duration</code> 通话时长, 累计值</li>
<li><code>txBytes</code>发送字节数, 累计值</li>
<li><code>rxBytes</code>接收字节数, 累计值</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



### Token 服务即将过期回调 \(tokenPrivilegeWillExpire\)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine tokenPrivilegeWillExpire:(NSString *_Nonnull)token;
```

在调用 `joinChannel` 时如果指定了 Token，由于 Token 具有一定的时效，在通话过程中如果 Token 即将失效，SDK 会提前 30 秒触发该回调，提醒应用程序更新 Token。 当收到该回调时，用户需要重新在服务端生成新的 Token，然后调用 `renewToken` 将新生成的 Token 传给 SDK。

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
<td>即将服务失效的 Token</td>
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



## 错误码和警告代码 - Interactive Gaming SDK

详见 [错误码和警告代码](../../cn/API%20Reference/the_error_game.md)。


