
---
title: 游戏 API
description: 
platform: Objective-C
updatedAt: Mon Jan 28 2019 04:07:39 GMT+0000 (UTC)
---
# 游戏 API
游戏 API 由 **Objective-C 接口** 和 **C++ 接口** 部分组成，提供游戏 SDK 在 iOS 平台上的主要方法和回调。

- Objective-C 接口：
   * [主要方法](#irtcengine)
   * [Delegate 方法](#irtcenginedelegate)
   * [Block 回调事件](#block) 
- C++ 接口：
   * [主要方法](#irtcengine)
   * [参数方法](#rtcengineparameters)
   * [主回调事件](#irtcengineeventhandler)


## Objective-C 接口

<a id = "rtcenginekit"></a>
### 主要方法 (AgoraRtcEngineKit)

`AgoraRtcEngineKit` 类包含应用程序调用的主要方法。

#### 初始化 (sharedEngineWithAppId)

```
+ (instancetype _Nonnull)sharedEngineWithAppId:(NSString * _Nonnull)appId
                                      delegate:(id<AgoraRtcEngineDelegate> _Nullable)delegate;
```

该方法初始化 `AgoraRtcEngineKit` 为一个单例。使用 `AgoraRtcEngineKit`，必须先调用该接口进行初始化。 Agora Native SDK 通过指定的 `delegate` 通知应用程序引擎运行时的事件。Delegate 中定义的所有方法都是可选实现的。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>模式</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appId</td>
<td><p>Agora 为应用程序开发者签发的 App ID。</p></td>
</tr>
</tbody>
</table>


#### 设置频道属性 (setChannelProfile)

```
- (int)setChannelProfile:(AgoraChannelProfile)profile;
```

该方法用于设置频道模式(Profile)。`AgoraRtcEngineKit` 需知道应用程序的使用场景(例如通信模式或直播模式), 从而使用不同的优化手段。

目前 Agora Native SDK 支持以下几种模式:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>模式</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>通信</td>
<td>通信为默认模式，用于常见的一对一或群聊，频道中的任何用户可以自由说话</td>
</tr>
<tr><td>直播</td>
<td>直播模式有主播和观众两种用户角色，可以通过调用 setClientRole 设置。主播可收发语音和视频，但观众只能收，不能发</td>
</tr>
<tr><td>游戏语音</td>
<td>频道内任何用户可自由发言。该模式下默认使用低功耗低码率的编解码器</td>
</tr>
</tbody>
</table>


> -   同一频道内只能同时设置一种模式。如果想要切换模式，则需要先调用 destroy 销毁当前引擎，然后在初始化 `sharedEngineWithAppId` 方法中传入 **不同** 的 App ID 重新创建引擎，再调用该方法切换频道模式。
> -   不同的频道模式必须使用不同的 App ID。
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
<tr><td>profile</td>
<td><p>频道模式：</p>
<ul>
<li>AgoraChannelProfileCommunication = 0: 通信模式 (默认)</li>
<li>AgoraChannelProfileBroadcasting = 1: 直播模式</li>
<li>AgoraChannelProfileGame = 2: 游戏语音模式</li>
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



#### 设置用户角色 (setClientRole)

```
- (int)setClientRole:(AgoraClientRole)role;
```

直播模式下，在加入频道前，用户需要通过本方法设置观众(默认)或主播模式；在加入频道后，用户可以通过本方法切换用户模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>role</td>
<td><p>直播场景里的用户角色</p>
<div><ul>
<li>AgoraClientRoleBroadcaster = 1; 主播</li>
<li>AgoraClientRoleAudience = 2; 观众(默认)</li>
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



#### 打开音频 (enableAudio)

```
- (int)enableAudio;
```

打开音频(默认为开启状态)。

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


> 该方法设置内部引擎为启用状态，在 `leaveChannel` 后仍然有效。


#### 关闭/重新开启本地语音功能 (enableLocalAudio)

```
- (int)enableLocalAudio:(BOOL)enabled;
```

当 App 加入频道时，它的语音功能默认是开启的。该方法可以关闭或重新开启本地语音功能，停止或重新开始本地音频采集及处理。
该方法不影响接收或播放远端音频流，适用于只听不发的用户场景。
语音功能关闭或重新开启后，会收到回调 `didMicrophoneEnabled`。

> - 该方法需要在 `joinChannelByToken` 之后调用才能生效。
> - 该方法与 `muteLocalAudioStream` 的区别在于：
>    *  `enableLocalAudio`：开启或关闭本地语音采集及处理
>    *  `muteLocalAudioStream`：停止或继续发送本地音频流

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enabled</td>
<td>
<ul>
<li>true：重新开启本地语音功能，即开启本地语音采集或处理</li>
<li>false：关闭本地语音功能，即停止本地语音采集或处理</li>
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


#### 关闭音频 (disableAudio)

```
- (int)disableAudio;
```

关闭音频。

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

> 该方法设置内部引擎为禁用状态，在 `leaveChannel` 后仍然有效。

#### 设置音频属性 (setAudioProfile)

```
- (int)setAudioProfile:(AgoraAudioProfile)profile
              scenario:(AgoraAudioScenario)scenario;
```

该方法用于设置音质。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>profile</td>
<td><p>设置采样率，码率，编码模式和声道数:</p>
<div><ul>
<li>AgoraAudioProfileSpeechStandard = 1: 指定 32 KHz采样率，语音编码, 单声道，编码码率约 18 kbps</li>
<li>AgoraAudioProfileMusicStandard = 2: 指定 48 KHz采样率，音乐编码, 单声道，编码码率约 48 kbps</li>
<li>AgoraAudioProfileMusicStandardStereo = 3: 指定 48 KHz采样率，音乐编码,  双声道，编码码率约 56 kbps</li>
<li>AgoraAudioProfileMusicHighQuality = 4: 指定 48 KHz 采样率，音乐编码, 单声道，编码码率约 128 kbps</li>
<li>AgoraAudioProfileMusicHighQualityStereo = 5: 指定 48 KHz采样率，音乐编码,  双声道，编码码率约 192 kbps</li>
</ul>
</div>
</td>
</tr>
<tr><td>scenario</td>
<td><p>设置音频应用场景:</p>
<div><ul>
<li>AgoraAudioScenarioDefault = 0: 默认设置</li>
<li>AgoraAudioScenarioChatRoomEntertainment = 1: 娱乐应用，需要频繁上下麦的场景</li>
<li>AgoraAudioScenarioEducation = 2: 教育应用，流畅度和稳定性优先</li>
<li>AgoraAudioScenarioGameStreaming = 3: 游戏直播应用，需要外放游戏音效也直播出去的场景。如需实现高保真的音乐传输，建议选择该场景</li>
<li>AgoraAudioScenarioShowRoom = 4: 秀场应用，音质优先和更好的专业外设支持</li>
<li>AgoraAudioScenarioChatRoomGaming = 5: 游戏开黑</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


> -   该方法需要在 `joinChannelByToken` 之前设置好，`joinChannelByToken` 之后设置不生效
> -   通信模式下，该方法设置 `profile` 生效，设置 `scenario` 不生效
> -   通信和直播模式下，音质（码率）会有网络自适应的调整，通过该方法设置的是一个最高码率
> -   音乐教学场景下，建议将 profile 设置为 MusicHighQuality（4），scenario 设置为 GameStreaming（3）
> -   scenario 中的 ChatRoomEntertainment（1）和 ChatRoomGaming（5）为游戏开黑场景，建议仅在纯音频的场景下使用


#### 设置音频高音质选项 (setHighQualityAudioParametersWithFullband)

```
- (int)setHighQualityAudioParametersWithFullband:(BOOL)fullband
                                          stereo:(BOOL)stereo
                                     fullBitrate:(BOOL)fullBitrate;
```

该方法设置音频高音质选项。请在加入频道前调用本方法并一次性设置好三个模式。切勿在加入频道后再次调用本方法。

> Agora 不推荐使用该方法设置高音质选项。如果想要设置音质，可以调用设置音频属性 `setAudioProfile`) 方法。

#### 加入频道 (joinChannelByToken)

```
- (int)joinChannelByToken:(NSString * _Nullable)token
                channelId:(NSString * _Nonnull)channelId
                     info:(NSString * _Nullable)info
                      uid:(NSUInteger)uid
              joinSuccess:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))joinSuccessBlock;
```

该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。 使用不同 App ID 的应用程序是不能互通的。如果已在通话中，用户必须调用 `leaveChannel()` 退出当前通话，才能进入下一个频道。 SDK 在通话中使用 iOS 系统的 AVAudioSession 共享对象进行录音和播放，应用程序对该对象的操作可能会影响 SDK 的音频相关功能。

调用该 API 后会触发回调，如果同时实现 joinChannelSuccessBlock 和 didJoinChannel 会出现冲突，joinChannelSuccessBlock 优先级高，2 个同时存在时，didJoinChannel 会被忽略。 需要有 didJoinChannel 回调时，请将 joinChannelSuccessBlock 设置为 nil 。

> 同一个频道里不能出现两个相同的 UID。如果你的 App 支持多设备同时登录，即同一个用户账号可以在不同的设备上同时登录(例如微信支持在 PC 端和移动端同时登录)，请保证传入的 UID 不相同。 例如你之前都是用同一个用户标识作为 UID, 建议从现在开始加上设备 ID, 以保证传入的 UID 不相同 。如果你的 App 不支持多设备同时登录，例如在电脑上登录时，手机上会自动退出，这种情况下就不需要在 UID 上添加设备 ID。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>Token</td>
<td><ul>
<li>安全要求不高: 将值设为 nil</li>
<li>安全要求高: 将值设置为 Token。如果你已经启用了 App Certificate, 请务必使用 Token。关于如何获取 Token，详见 <a href="../../cn/Agora%20Platform/token.md"><span>密钥说明</span></a></li>
</ul>
</td>
</tr>
<tr><td>channelId</td>
<td>标识通话的频道名称，长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td>info</td>
<td>(非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户。</td>
</tr>
<tr><td>uid</td>
<td><p>(非必选项) 用户 ID，32 位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 onJoinChannelSuccess 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护。</p>
<div>uid 在 SDK 内部用 32 位无符号整数表示，由于 Java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，Java 会表示为负数，如有需要可以用 (uid&amp;0xffffffffL) 转换成 64 位整数。</div>
</td>
</tr>
<tr><td>joinSuccessBlock</td>
<td>成功加入频道回调</td>
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



> 在 `joinChannel()` 时，SDK 调用 setCategoryAVAudioSessionCategoryPlayAndRecord 将 AVAudioSession 设置到 PlayAndRecord 模式，应用程序不应将其设置到其他模式。设置该模式时，正在播放的声音会被打断（比如正在播放的响铃声）。

#### 离开频道 (leaveChannel)

```
- (int)leaveChannel:(void(^ _Nullable)(AgoraChannelStats * _Nonnull stat))leaveChannelBlock;
```

离开频道，即挂断或退出通话。

当调用 `joinChannelByToken()` API 方法后，必须调用 `leaveChannel()` 结束通话，否则无法开始下一次通话。 不管当前是否在通话中，都可以调用 `leaveChannel()`，没有副作用。该方法会把会话相关的所有资源释放掉。该方法是异步操作，调用返回时并没有真正退出频道。在真正退出频道后，SDK 会触发 `didLeaveChannelWithStats` 回调。


> 如果你调用了 `leaveChannel()` 后立即调用 `destroy()`，SDK 将无法触发 `didLeaveChannelWithStats` 回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>leaveChannelBlock</td>
<td>成功离开频道的回调</td>
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


#### 设置本地语音音调 (setLocalVoicePitch)

```
- (int)setLocalVoicePitch:(double)pitch;
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
<tr><td>pitch</td>
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

#### 设置远端用户的语音位置 (setRemoteVoicePosition)

```
- (int)setRemoteVoicePosition:(int)uid pan:(double)pan gain:(double)gain;
```

该方法设置远端用户的语音位置。

> 该方法只在耳机模式下有效，在外放模式下无效。

<table>
<colgroup>
<col>
</col>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td><p>远端用户的 UID</p>
</td></tr>
<tr><td>pan</td>
<td><p>设置是否改变音效的空间位置。取值范围为 [-1, 1]：
<ul>
<li>0：表示该音效出现在正前方</li>
<li>-1：表示该音效出现在左边</li>
<li>1：表示该音效出现在右边</li>
</ul>
</td>
</tr>
<tr><td>gain</td>
<td><p>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。
默认值为 100.0。取值越小，音效的音量越低。</p>
</td></tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### 设置仅语音模式 (setVoiceOnlyMode)

```
- (int)setVoiceOnlyMode:(bool)enable;
```

该方法设置仅语音模式。即仅传输语音流，而不传输其他流，例如，敲键盘的声音等。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enable</td>
<td>
<ul>
<li>true：启用仅语音模式</li>
<li>false：禁用仅语音模式</li>
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

#### 设置本地语音音效均衡 (setLocalVoiceEqualizationOfBandFrequency)

```
- (int)setLocalVoiceEqualizationOfBandFrequency:(AgoraAudioEqualizationBandFrequency)bandFrequency withGain:(NSInterger)gain;
```

该方法设置本地语音音效均衡。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>bandFrequency</td>
<td>取值范围是 [0-9]，分别代表音效的 10 个 band 的中心频率 [31，62，125，250，500，1k，2k，4k，8k，16k]Hz</td>
</tr>
<tr><td>gain</td>
<td>每个 band 的增益，单位是 dB，每一个值的范围是 [-15，15]</td>
</tr>
</tbody>
</table>



#### 设置本地音效混响 (setLocalVoiceReverbOfType)

```
- (int)setLocalVoiceReverbOfType:(AgoraAudioReverbType)reverbType withValue:(NSInterger)value;
```

该方法设置本地音效混响。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>reverbType</td>
<td>混响音效类型。该方法共有 5 个混响音效类型，分别如 value 栏列出</td>
</tr>
<tr><td>value</td>
<td><p>各混响音效 Key 所对应的值：</p>
<div><ul>
<li>AgoraAudioReverbDryLevel = 0：(dB, [-20,10])，原始声音效果，即所谓的 dry signal</li>
<li>AgoraAudioReverbWetLevel = 1：(dB, [-20,10])，早期反射信号效果，即所谓的 wet signal</li>
<li>AgoraAudioReverbRoomSize = 2：([0，100])， 所需混响效果的房间尺寸</li>
<li>AgoraAudioReverbWetDelay = 3：(ms, [0, 200])，wet signal 的初始延迟长度，以毫秒为单位</li>
<li>AgoraAudioReverbStrength = 4：([0，100])，后期混响长度</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


#### 打开与 Web SDK 的互通 (enableWebSdkInteroperability)

```
- (int) enableWebSdkInteroperability:(BOOL)enabled;
```

该方法打开或关闭与 Agora Web SDK 的互通。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enabled</td>
<td><p>是否已打开与 Agora Web SDK 的互通：</p>
<div><ul>
<li>true：打开互通</li>
<li>false：关闭互通</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 设置默认的语音路径 (setDefaultAudioRouteToSpeakerPhone)

```
- (int)setDefaultAudioRouteToSpeakerphone:(BOOL)defaultToSpeaker;
```

> -  该方法只在纯音频模式下工作，在有视频的模式下不工作。
> -  该方法需要在 `joinChannelbyToken` 前设置，否则不生效。


该方法设置接收到的语音从听筒或扬声器出声。如果用户不调用本方法，语音默认从听筒出声。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>defaultToSpeaker</td>
<td><ul>
<li>YES: 从扬声器出声</li>
<li>NO: 从听筒出声</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>返回值</td>
<td>
<ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>




#### 打开外放 (setEnableSpeakerphone)

```
- (int)setEnableSpeakerphone:(BOOL)enableSpeaker;
```

该方法将语音路由强制设置为扬声器（外放）。调用该方法后，SDK 将返回 `didAudioRouteChanged` 回调提示状态已更改。


> 在调用该方法前，请先阅读 `setDefaultAudioRouteToSpeakerPhone` 里关于默认语音路由的说明，确认是否需要调用该方法。

#### 是否是扬声器状态 (isSpeakerphoneEnabled)

```
- (BOOL)isSpeakerphoneEnabled;
```

该方法检查扬声器是否已开启。

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
<li>Yes: 表明输出到扬声器</li>
<li>No: 表明输出到非扬声器(听筒，耳机等)</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 启用耳返功能 (enableInEarMonitoring)

```
- (int)enableInEarMonitoring:(BOOL)enabled;
```

该方法打开或关闭耳返功能。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enabled</td>
<td><ul>
<li>YES: 启用耳返监听功能</li>
<li>NO: 禁用耳返监听功能(默认)</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### 启用说话者音量提示 (enableAudioVolumeIndication)

```
- (int)enableAudioVolumeIndication:(NSInteger)interval
                            smooth:(NSInteger)smooth;
```

该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。启用该方法后，无论频道内是否有人说话，都会在 `reportAudioVolumeIndicationOfSpeakers` 及 `audioVolumeIndicationBlock` 回调中按设置的时间间隔返回音量提示

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td>interval</td>
<td><p>指定音量提示的时间间隔：</p>
<ul>
<li>&lt;= 0：禁用音量提示功能</li>
<li>&gt; 0：提示间隔，单位为毫秒。建议设置到大于 200 毫秒。最小不得少于 10 毫秒，否则会收不到 <code>reportAudioVolumeIndicationOfSpeakers</code> 回调</li>
</ul>
</td>
</tr>
<tr><td>smooth</td>
<td>平滑系数。默认可以设置为 3</td>
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



#### 设置耳返音量 (setInEarMonitoringVolume)

```
- (int)setInEarMonitoringVolume:(NSInteger)volume;
```

该方法设置耳返的音量。

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
<td> </td>
</tr>
<tr><td>volume</td>
<td>设置耳返音量，取值范围在 [0.100]</td>
<td>默认值为 100</td>
</tr>
</tbody>
</table>


#### 获取音效音量 (getEffectsVolume)

```
- (double)getEffectsVolume;
```

该方法获取音效的音量，范围为 \[0.0, 100.0\]。

#### 设置音效音量 (setEffectsVolume)

```
- (int)setEffectsVolume:(double)volume;
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
<tr><td>volume</td>
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



#### 实时调整音效音量 (setVolumeOfEffect)

```
- (int)setVolumeOfEffect:(int)soundId
              withVolume:(double)volume;
```

该方法实时调整指定音效的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>volume</td>
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



#### 播放指定音效 (playEffect)

```
- (int)playEffect:(int)soundId
         filePath:(NSString * _Nullable)filePath
        loopCount:(int)loopCount
            pitch:(double)pitch
              pan:(double)pan
             gain:(double)gain
          publish:(BOOL)publish;
```

该方法播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID <sup>[3]</sup></td>
</tr>
<tr><td>filepath</td>
<td>音效文件的绝对路径</td>
</tr>
<tr><td>loopCount</td>
<td><p>设置音效循环播放的次数：</p>
<div><ul>
<li>0：播放音效一次</li>
<li>1：播放音效两次</li>
<li>-1：无限循环播放音效，直至调用 <code>stopEffect</code> 或 <code>stopAllEffects</code> 后停止</li>
</ul>
</div>
</td>
</tr>
<tr><td>pitch</td>
<td>设置音效的音调
取值范围为 [0.5, 2]。默认值为 1.0，表示不需要修改音调。取值越小，则音调越低</td>
</tr>
<tr><td>pan</td>
<td><p>设置是否改变本地听到的音效的空间位置。取值范围为 [-1.0, 1.0]：</p>
<div><ul>
<li>0.0：音效出现在正前方</li>
<li>-1.0：音效出现在左边</li>
<li>1.0：音效出现在右边</li>
</ul>
</div>
</td>
</tr>
<tr><td>gain</td>
<td>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。默认值为 100.0。取值越小，则音效的音量越低</td>
</tr>
<tr><td>publish</td>
<td><p>设置是否将音效传到远端：</p>
<div><ul>
<li>YES：音效在本地播放的同时，会发布到 Agora 云上，因此远端用户也能听到该音效</li>
<li>NO：音效不会发布到 Agora 云上，因此只能在本地听到该音效</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：该方法调用成功</li>
<li>&lt; 0：该方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> [3] 如果你已通过 `preloadEffect` 将音效加载至内存，确保这里设置的 `soundId` 与 `preloadEffect` 设置的 `soundId` 相同。
> 该接口在 v2.1.x 及之前的版本中如下所示，不推荐使用。

```
- (int) playEffect: (int) soundId
          filePath: (NSString*) filePath
              loop: (BOOL) loop
             pitch: (double) pitch
               pan: (double) pan
              gain: (double) gain;
```

#### 停止播放指定音效 (stopEffect)

```
- (int)stopEffect:(int)soundId;
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
<tr><td>soundId</td>
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



#### 停止播放所有音效 (stopAllEffects)

```
- (int)stopAllEffects;
```

该方法停止播放所有音效。

#### 预加载音效 (preloadEffect)

```
- (int)preloadEffect:(int)soundId
            filePath:(NSString * _Nullable) filePath;
```

该方法将指定音效文件 (压缩的语音文件) 预加载至内存。

> 为保证通信畅通，请注意预加载音效文件的大小，并在 `joinChannelByToken` 前就使用该方法完成音效预加载。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>filePath</td>
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



#### 释放音效 (unloadEffect)

```
- (int)unloadEffect:(int)soundId;
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
<tr><td>soundId</td>
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



#### 暂停音效播放 (pauseEffect)

```
- (int)pauseEffect:(int)soundId;
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
<tr><td>soundId</td>
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



#### 暂停所有音效播放 (pauseAllEffects)

```
- (int)pauseAllEffects;
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



#### 恢复播放指定音效 (resumeEffect)

```
- (int)resumeEffect:(int)soundId;
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
<tr><td>soundId</td>
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



#### 恢复播放所有音效 (resumeAllEffects)

```
- (int)resumeAllEffects;
```

该方法恢复播放所有音效。


#### 将自己静音 (muteLocalAudioStream)

```
- (int)muteLocalAudioStream:(BOOL)muted;
```

静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。


> 该方法不影响录音状态，并没有禁用麦克风。

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
<li>Yes: 麦克风静音</li>
<li>No: 取消静音</li>
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



#### 静音所有远端音频 (muteAllRemoteAudioStreams)

```
- (int)muteAllRemoteAudioStreams:(BOOL)mute;
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
<tr><td>muted</td>
<td><ul>
<li>YES: 停止接收和播放所有远端音频流</li>
<li>NO: 允许接收和播放所有远端音频流</li>
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



#### 静音指定用户音频 (muteRemoteAudioStream)

```
- (int)muteRemoteAudioStream:(NSUInteger)uid mute:(BOOL)mute;
```

静音指定远端用户/对指定远端用户取消静音。


> 如果之前有调用过 muteAllRemoteAudioStreams (true) 对所有远端音频进行静音，在调用本 API 之前请确保你已调用 muteAllRemoteAudioStreams (false) 。 muteAllRemoteAudioStreams 是全局控制，muteRemoteAudioStream 是精细控制。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>指定用户 ID</td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>YES: 停止接收和播放指定音频流</li>
<li>NO: 允许接收和播放指定音频流</li>
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


#### 开始播放伴奏 (startAudioMixing)

```
- (int)startAudioMixing:(NSString *  _Nonnull)filePath
               loopback:(BOOL)loopback
                replace:(BOOL)replace
                  cycle:(NSInteger)cycle;
```

指定本地音频文件来和麦克风采集的音频流进行混音和替换(用音频文件替换麦克风采集的音频流)， 可以通过参数选择是否让对方听到本地播放的音频和指定循环播放的次数。该 API 也支持播放在线音乐。


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
<tr><td>filePath</td>
<td>指定需要混音的本地音频文件名和文件路径名:
支持以下音频格式: mp3, aac, m4a, 3gp, wav, flac</td>
</tr>
<tr><td>loopback</td>
<td><ul>
<li>YES: 只有本地可以听到混音或替换后的音频流</li>
<li>NO: 本地和对方都可以听到混音或替换后的音频流</li>
</ul>
</td>
</tr>
<tr><td>replace</td>
<td><ul>
<li>YES: 音频文件内容将会替换本地录音的音频流</li>
<li>NO: 音频文件内容将会和麦克风采集的音频流进行混音</li>
</ul>
</td>
</tr>
<tr><td>cycle</td>
<td>指定音频文件循环播放的次数：
<ul>
<li>正整数: 循环的次数</li>
<li>-1: 无限循环</li>
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




#### 停止播放伴奏 (stopAudioMixing)

```
- (int)stopAudioMixing;
```

该方法停止播放伴奏。请在频道内调用该方法。

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
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 暂停播放伴奏 (pauseAudioMixing)

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
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 恢复播放伴奏 (resumeAudioMixing)

```
- (int)resumeAudioMixing;
```

该方法恢复混音，继续播放伴奏。请在频道内调用该方法。

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
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 调节伴奏音量 (adjustAudioMixingVolume)

```
- (int)adjustAudioMixingVolume:(NSInteger)volume;
```

该方法调节混音里伴奏的音量大小。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>volume</td>
<td>伴奏音量范围为 0~100。默认 100 为原始文件音量</td>
</tr>
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



#### 获取伴奏时长 (getAudioMixingDuration)

```
- (int)getAudioMixingDuration;
```

该方法获取伴奏时长，单位为毫秒。请在频道内调用该方法。如果返回值 <0，表明调用失败。

#### 获取伴奏播放进度 (getAudioMixingCurrentPosition)

```
- (int)getAudioMixingCurrentPosition;
```

该方法获取当前伴奏播放进度，单位为毫秒。请在频道内调用该方法。

#### 拖动语音进度条 (setAudioMixingPosition)

```
- (int)setAudioMixingPosition:(NSInteger)pos;
```

该方法可以拖动播放音频文件的进度条，这样你可以根据实际情况播放文件，而不是非得从头到尾播放一个文件。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>pos</td>
<td>整数。进度条位置，单位为毫秒</td>
</tr>
</tbody>
</table>

#### 开始客户端录音 (startAudioRecording)

```
- (int)startAudioRecording:(NSString * _Nonnull)filePath
                   quality:(AgoraAudioRecordingQuality)quality;
```

Agora SDK 支持通话过程中在客户端进行录音。该方法录制频道内所有用户的音频，并生成一个包含所有用户声音的录音文件，录音文件格式可以为:

-   wav : 文件大，音质保真度高

-   aac : 文件小，有一定的音质保真度损失


请确保应用程序里指定的目录存在且可写。该接口需在加入频道之后调用。如果调用 `leaveChannel()` 时还在录音，录音会自动停止。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>filePath</td>
<td>录音文件的本地保存路径，由用户自行指定，需精确到文件名及格式，例如：<code>/dir1/dir2/dir3/audio.aac</code></td>
</tr>
<tr><td>quality</td>
<td><p>录音音质：</p>
<ul>
<li>AgoraAudioRecordingQualityLow = 0: 低音质。录制 10 分钟的文件大小为 1.2 M 左右</li>
<li>AgoraAudioRecordingQualityMedium = 1: 中音质。录制 10 分钟的文件大小为 2 M 左右</li>
<li>AgoraAudioRecordingQualityHigh = 2: 高音质。录制 10 分钟的文件大小为 3.75 M 左右</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 停止客户端录音 (stopAudioRecording)

```
- (int)stopAudioRecording;
```

停止录音。该接口需要在 `leaveChannel()` 之前调用，不然会在调用 `leaveChannel()` 时自动停止。

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



#### 调节录音信号音量 (adjustRecordingSignalVolume)

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
<tr><td>volume</td>
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



#### 调节播放信号音量 (adjustPlaybackSignalVolume)

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
<tr><td>volume</td>
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


#### 启用内置加密，并设置数据加密密码 (setEncryptionSecret)

```
- (int)setEncryptionSecret:(NSString * _Nullable)secret;
```

在加入频道之前，应用程序需调用 `setEncryptionSecret()` 指定 secret 来启用内置的加密功能，同一频道内的所有用户应设置相同的 secret。 当用户离开频道时，该频道的 secret 会自动清除。如果未指定 secret 或将 secret 设置为空，则无法激活加密功能。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>secret</td>
<td>加密密码</td>
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



#### 设置内置的加密方案 (setEncryptionMode)

```
- (int)setEncryptionMode:(NSString * _Nullable)encryptionMode;
```

Agora Native SDK 支持内置加密功能，默认使用 AES-128-XTS 加密方式。如需使用其他加密方式，可以调用该 API 设置。

同一频道内的所有用户必须设置相同的加密方式和 secret 才能进行通话。关于这几种加密方式的区别，请参考 AES 加密算法的相关资料。


> 在调用本方法前，请先调用 `setEncryptionSecret()` 启用内置加密功能。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>encryptionMode</td>
<td><p>加密方式。目前支持以下几种:</p>
<ul>
<li>“aes-128-xts”: 128 位 AES 加密， XTS 模式</li>
<li>“aes-128-ecb”: 128 位 AES 加密， ECB 模式</li>
<li>“aes-256-xts”: 256 位 AES 加密，XTS 模式</li>
<li>“”: 设置为空字符串时，使用默认加密方式 “aes-128-xts”</li>
</ul>
</td>
</tr>
<tr/>
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


#### 创建数据流 (createDataStream)

```
- (int)createDataStream:(NSInteger * _Nonnull)streamId
               reliable:(BOOL)reliable
                ordered:(BOOL)ordered;
```

该方法用于创建数据流。频道内每人最多只能创建 5 个数据流。频道内数据通道最多允许数据延迟 5 秒，若超过 5 秒接收方尚未收到数据流，则数据通道会向应用程序报错。

> 请将 `reliable` 和 `ordered` 同时设置为 YES 或 NO, 暂不支持交叉设置。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>reliable</td>
<td><ul>
<li>YES: 接收方会在 5 秒内收到发送方所发送的数据，否则会收到 onStreamMessageError 回调获得相应报错信息。</li>
<li>NO: 接收方不保证收到，就算数据丢失也不会报错。</li>
</ul>
</td>
</tr>
<tr><td>ordered</td>
<td><ul>
<li>YES: 接收方 5 秒内会按照发送方发送的顺序收到数据包。</li>
<li>NO: 接收方不保证按照发送方发送的顺序收到数据包</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>&lt;0: 创建数据流失败的错误码 <sup>[4]</sup></li>
<li>&gt; 0: 数据流 ID</li>
</ul>
</td>
</tr>
</tbody>
</table>


> [4] 返回的错误码是负数，对应错误代码和警告代码里的正整数。例如返回的错误码为-2，则对应错误代码和警告代码里的 2: ERR_INVALID_ARGUMENT 。

#### 发送数据流 (sendStreamMessage)

```
- (int)sendStreamMessage:(NSInteger)streamId
                    data:(NSData * _Nonnull)data;
```

该方法发送数据流消息到频道内所有用户。频道内每秒最多能发送 30 个包，且每个包最大为 1 KB。 API 须对数据通道的传送速率进行控制: 每个客户端每秒最多能发送 6 KB 数据。频道内每人最多能同时有 5 个数据通道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>streamId</td>
<td>数据流 ID，<code>createDataStream()</code> 的返回值。</td>
</tr>
<tr><td>message</td>
<td>待发送的数据</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 发送成功</li>
<li>&lt; 0: 发送失败，返回错误码：
<ul>
<li>ERR_SIZE_TOO_LARGE (114)</li>
<li>ERR_TOO_OFTEN (12)</li>
<li>ERR_BITRATE_LIMIT (115)</li>
<li>ERR_INVALID_ARGUMENT (2)</li>
</ul>
</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 开始语音直播测试 (startEchoTest)

```
- (int)startEchoTest:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))successBlock;
```

该方法启动语音直播测试，目的是测试系统的音频设备（耳麦、扬声器等）和网络连接是否正常。 在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。

> -   调用 startEchoTest 后必须调用 stopEchoTest 以结束测试，否则不能进行下一次回声测试，或者调用 `joinChannel()` 进行通话。
> -   直播模式下，只有主播用户才能调用。如果用户由通信模式切换到直播模式，请务必调用 `setClientRole()` 方法将用户的橘色设置为。

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
<li>ERR_REFUSED (-5)：无法启动测试，可能没有成功初始化</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>




#### 停止语音直播测试 (stopEchoTest)

```
- (int)stopEchoTest;
```

该方法停止语音直播测试。

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
<li>ERR_REFUSED (-5)：不能启动测试，可能语音直播测试没有成功启动</li> 
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>




#### 启动网络测试 (enableLastmileTest)

```
- (int)enableLastmileTest;
```

该方法启用网络连接质量测试，用于检测用户网络接入质量。默认该功能为关闭状态。该方法主要用于以下场景:

用户加入频道前，可以调用该方法判断和预测目前的上行网络质量是否足够好。启用该方法会消耗一定的网络流量，影响通话质量。在收到 `lastmileQuality` 回调后须调用 `disableLastmileTest()` 停止测试，再加入频道。

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



#### 禁用网络测试 (disableLastmileTest)

```
- (int)disableLastmileTest;
```

该方法禁用网络测试。

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


#### 获取通话 ID (getCallId)

```
- (NSString * _Nullable)getCallId;
```

获取当前的通话 ID。

客户端在每次 `joinChannel()` 后会生成一个对应的 CallId，标识该客户端的此次通话。有些方法如rate, complain需要在通话结束后调用，向SDK提交反馈，这些方法必须指定CallId参数。使用这些反馈方法，需要在通话过程中调用getCallId方法获取CallId，在通话结束后在反馈方法中作为参数传入。

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
<td>通话 ID</td>
</tr>
</tbody>
</table>



#### 给通话评分 (rate)

```
- (int)rate:(NSString * _Nonnull)callId
     rating:(NSInteger)rating
description:(NSString * _Nullable)description;
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
<tr><td>callId</td>
<td>通话 getCallId 函数获取的通话 ID</td>
</tr>
<tr><td>rating</td>
<td>给通话的评分，最低 1 分，最高 10 分</td>
</tr>
<tr><td>description</td>
<td>(非必选项) 给通话的描述，可选，长度应小于 800 字节</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT(-2): 传入的参数无效，例如 callId 无效</li>
<li>ERR_NOT_READY(-3)：SDK 不在正确的状态，可能是因为没有成功初始化</li>      
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>




#### 投诉通话质量 (complain)

```
- (int)complain:(NSString * _Nonnull)callId
    description:(NSString * _Nullable)description;
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
<tr><td>callId</td>
<td>通话 getCallId 函数获取的通话 ID</td>
</tr>
<tr><td>description</td>
<td>(非必选项) 给通话的描述，可选，长度应小于 800 字节</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT(-2): 传入的参数无效，例如 callId 无效</li>
<li>ERR_NOT_READY(-3)：SDK 不在正确的状态，可能是因为没有成功初始化</li>       
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>


#### 更新 Token (renewToken)

```
- (int)renewToken:(NSString * _Nonnull)token;
```

该方法用于更新 Token。如果启用了 Token 机制，过一段时间后使用的 Token 会失效。

当：

-   `rtcEngine:didOccurError`：回调报告 ERR_TOKEN_EXPIRED(109) 时，

-   `rtcEngineRequestToken`：回调报告 ERR_TOKEN_EXPIRED(109) 时，


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
<tr><td>Token</td>
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



#### 设置日志文件 (setLogFile)

```
- (int)setLogFile:(NSString * _Nonnull)filePath;
```

设置 SDK 的输出 log 文件。SDK 运行时产生的所有 log 将写入该文件。应用程序必须保证指定的目录存在而且可写。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>filePath</td>
<td>日志文件的完整路径。该日志文件为 UTF-8 编码。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0:  方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> iOS 平台下日志文件的默认地址为：Library/caches/agorasdk.log。

#### 设置日志过滤器 (setLogFilter)

```
- (int)setLogFilter:(NSUInteger)filter;
```

设置 SDK 的输出日志过滤等级。不同的过滤等级可以单独或组合使用。

日志级别顺序依次为 *Off*、*Critical*、*Error*、*Warning*、*Info* 和 *Debug*。选择一个级别，你就可以看到在该级别之前所有级别的日志信息。

例如，你选择 *Warning* 级别，就可以看到在 *Critical*、*Error* 和 *Warning* 级别上的所有日志信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>filter</td>
<td><p>设置过滤器等级:</p>
<div><ul>
<li>AgoraLogFilterOff = 0：不输出日志信息</li>
<li>AgoraLogFilterDebug = 0x080f：输出所有 API 日志信息</li>
<li>AgoraLogFilterInfo = 0x000f：输出 Critical、Error、Warning 和 Info 级别的日志信息</li>
<li>AgoraLogFilterWarning = 0x000e：输出 Critical、Error 和 Warning 级别的日志信息</li>
<li>AgoraLogFilterError = 0x000c：输出 Critical 和 Error 级别的日志信息</li>
<li>AgoraLogFilterCritical = 0x0008：输出 Critical 级别的日志信息</li>
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

#### 销毁引擎实例 (destroy)

```
+ (void)destroy;
```

该方法释放 Agora SDK 使用的所有资源。有些应用程序只在用户需要时才进行视频直播，不需要时则将资源释放出来用于其他操作，该方法对这类程序可能比较有用。 只要调用了 `destroy()`, 用户将无法再使用和回调该 SDK 内的其它方法。如需再次使用直播功能，必须重新初始化 sharedEngineWithappId 来新建一个 AgoraRtcEngineKit 实例（instance）。

> -   该方法需要在子线程中操作。
> -   该方法为同步调用。在等待 IRtcEngine 对象资源释放后再返回。APP 不应该在 SDK 产生的回调中调用该接口，否则由于 SDK 要等待回调返回才能回收相关的对象资源，会造成死锁。


#### 查询 SDK 版本号 (getSdkVersion)

```
+ (NSString * _Nonnull)getSdkVersion;
```

该方法返回 SDK 版本号字符串。

<a id = "rtcenginedelegate"></a>
### Delegate 方法 (AgoraRtcEngineDelegate)

SDK 会通过代理方法 AgoraRtcEngineDelegate 向应用程序上报一些运行时事件。从 1.1 版本开始，SDK 使用 Delegate 代替原有的部分 Block 回调。 原有的 Block 回调被标为废弃，目前仍然可以使用，但是建议用相应的 Delegate 方法代替。如果同一个回调 Block 和 Delegate 方法都有定义，则 SDK 只回调 Block 方法。

#### 发生警告回调 (didOccurWarning)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOccurWarning:(AgoraRtcErrorCode)warningCode;
```

该回调方法表示 SDK 运行时出现了（网络或媒体相关的）警告。通常情况下，SDK 上报的警告信息应用程序可以忽略，SDK 会自动恢复。比如和服务器失去连接时，SDK 可能会上报 `AgoraRtc_Error_OpenChannelTimeout(106)` 警告，同时自动尝试重连。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td>WarningCode</td>
<td>警告代码</td>
</tr>
</tbody>
</table>


#### 发生错误回调 (didOccurError)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOccurError:(AgoraRtcErrorCode)errorCode;
```

该回调方法表示 SDK 运行时出现了（网络或媒体相关的）错误。通常情况下，SDK 上报的错误意味着 SDK 无法自动恢复，需要应用程序干预或提示用户。 比如启动通话失败时，SDK 会上报 `AgoraRtc_Error_StartCall(1002)` 错误。应用程序可以提示用户启动通话失败，并调用 `leaveChannel()` 退出频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td>Engine</td>
<td><code>AgoraRtcEngineKit</code> 对象</td>
</tr>
<tr><td>errorCode</td>
<td>错误代码</td>
</tr>
</tbody>
</table>


#### API 已执行回调 (didApiCallExecute)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didApiCallExecute:(NSInteger)error api:(NSString * _Nonnull)api result:(NSString * _Nonnull)result;
```

当 SDK 执行相应的 API 后，会触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>error</td>
<td>错误码。如果方法调用失败，会返回错误码；如果返回 0，则表示方法调用成功</td>
</tr>
<tr><td>api</td>
<td>SDK 所调用的 API</td>
</tr>
<tr><td>result</td>
<td>SDK 调用 API 的调用结果</td>
</tr>
</tbody>
</table>

#### 加入频道回调 (didJoinChannel)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didJoinChannel:(NSString * _Nonnull)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
```

该回调方法表示该客户端成功加入了指定的频道。同 `joinChannelByToken()` API 的 `joinSuccessBlock` 回调。

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
<tr><td>uid</td>
<td>用户 ID。如果 <code>joinChannelByToken</code> 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
<tr><td>elapsed</td>
<td>从 <code>joinChannelByToken</code> 开始到该事件产生的延迟（毫秒）</td>
</tr>
</tbody>
</table>

#### 重新加入频道回调 (didRejoinChannel)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didRejoinChannel:(NSString * _Nonnull)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
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
<tr><td>uid</td>
<td>用户 ID。如果 <code>joinChannelByToken</code> 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
<tr><td>elapsed</td>
<td>从 <code>joinChannelByToken</code> 开始到该事件产生的延迟（毫秒）</td>
</tr>
</tbody>
</table>

#### 已离开频道回调 (didLeaveChannelWithStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didLeaveChannelWithStats:(AgoraChannelStats * _Nonnull)stats;
```

当用户调用 leaveChannel 离开频道后，SDK 会触发该回调。该回调与 `leaveChannelBlock` 相同，且 `leaveChannelBlock` 优先级更高。

**AgoraChannelStats 定义**

```
__attribute__((visibility("default"))) @interface AgoraChannelStats: NSObject
@property (assign, nonatomic) NSInteger duration;
@property (assign, nonatomic) NSInteger txBytes;
@property (assign, nonatomic) NSInteger rxBytes;
@property (assign, nonatomic) NSInteger txAudioKBitrate;
@property (assign, nonatomic) NSInteger rxAudioKBitrate;
@property (assign, nonatomic) NSInteger txVideoKBitrate;
@property (assign, nonatomic) NSInteger rxVideoKBitrate;
@property (assign, nonatomic) NSInteger lastmileDelay;
@property (assign, nonatomic) NSInteger userCount;
@property (assign, nonatomic) double cpuAppUsage;
@property (assign, nonatomic) double cpuTotalUsage;
@end
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
<tr><td>stats</td>
<td><p>通话相关的统计信息：</p>
<div><ul>
<li>duration：通话时长，单位为秒，累计值</li>
<li>txBytes：发送字节数 (bytes)，累计值</li>
<li>rxBytes：接收字节数 (bytes)，累计值</li>
<li>txAudioKBitRate：音频发送码率 (kbps)，瞬时值</li>
<li>rxAudioKBitRate：音频接收码率 (kbps)，瞬时值</li>
<li>txVideoKBitRate：视频发送码率 (kbps)，瞬时值</li>
<li>rxVideoKBitRate：视频接收码率 (kbps)，瞬时值</li>
<li>lastmileDelay：客户端到 vos 服务器的延迟（毫秒）</li>
<li>users：用户离开频道时频道内的瞬时人数</li>
<li>cpuTotalUsage：当前系统的 CPU 使用率 (%)</li>
<li>cpuAppUsage：当前应用程序的 CPU 使用率 (%)</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


#### 语音路由已发生变化回调(didAudioRouteChanged)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioRouteChanged:(AgoraAudioOutputRouting)routing;
```

当语音路由发生变化时，SDK 会触发此回调。

其中 AgoraAudioOutputRouting 定义如下:

```
typedef NS_ENUM(NSInteger, AgoraAudioOutputRouting)
{
    AgoraAudioOutputRoutingDefault = -1,
    AgoraAudioOutputRoutingHeadset = 0,
    AgoraAudioOutputRoutingEarpiece = 1,
    AgoraAudioOutputRoutingHeadsetNoMic = 2,
    AgoraAudioOutputRoutingSpeakerphone = 3,
    AgoraAudioOutputRoutingLoudspeaker = 4,
    AgoraAudioOutputRoutingHeadsetBluetooth = 5
};
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>routing</td>
<td><p>设置语音路由：</p>
<div><ul>
<li>-1：使用默认的语音路由</li>
<li>0：使用耳机为语音路由</li>
<li>1：使用听筒为语音路由</li>
<li>2：使用不带麦的耳机为语音路由</li>
<li>3：使用手机的扬声器为语音路由</li>
<li>4：使用外接的扬声器为语音路由</li>
<li>5：使用蓝牙耳机为语音路由</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

#### 麦克风状态已改变回调 (didMicrophoneEnabled)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didMicrophoneEnabled:(BOOL)enabled;
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
<tr><td>enabled</td>
<td>
<ul>
<li>true：麦克风已弃用</li>
<li>false：麦克风已禁用</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 语音质量回调 (audioQualityOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine audioQualityOfUid:(NSUInteger)uid quality:(AgoraNetworkQuality)quality delay:(NSUInteger)delay lost:(NSUInteger)lost;
```

同 audioQualityBlock。在通话中，该回调方法每两秒触发一次，报告当前通话的（嘴到耳）音频质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID 。每次回调上报一个用户当前的语音质量（不包含自己），不同用户在不同的回调中上报。</td>
</tr>
<tr><td>quality</td>
<td><p>声音质量评分:</p>
<div><ul>
<li>AgoraNetworkQualityUnknown(0)：网络质量未知</li>
<li>AgoraNetworkQualityExcellent(1)：网络质量极好</li>
<li>AgoraNetworkQualityGood(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>AgoraNetworkQualityPoor(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>AgoraNetworkQualityBad(4)：勉强能沟通但不顺畅</li>
<li>AgoraNetworkQualityVBad(5)：网络质量非常差，基本不能沟通</li>
<li>AgoraNetworkQualityDown(6)：完全无法沟通</li>
</ul>
</div>
</td>
</tr>
<tr><td>delay</td>
<td>延迟（毫秒）</td>
</tr>
<tr><td>lost</td>
<td>丢包率（百分比）</td>
</tr>
</tbody>
</table>



#### 音量提示回调 (reportAudioVolumeIndicationOfSpeakers)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine reportAudioVolumeIndicationOfSpeakers:(NSArray<AgoraRtcAudioVolumeInfo *> * _Nonnull)speakers totalVolume:(NSInteger)totalVolume;
```

同 `audioVolumeIndicationBlock` 。该回调提示频道内谁在说话以及说话者的音量。默认禁用。可通过启用说话者音量提示 `enableAudioVolumeIndication` 方法开启；开启后，无论频道内是否有人说话，都会按方法中设置的时间间隔返回提示音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>speakers</td>
<td><p>说话者（数组）。每个 speaker()：</p>
<ul>
<li>uid：说话者的用户 ID。如果返回的 uid 为 0，则默认为本地用户</li>
<li>volume：说话者的音量（0~255）</li>
</ul>
</td>
</tr>
<tr><td>totalVolume</td>
<td>（混音后的）总音量（0~255）</td>
</tr>
</tbody>
</table>



在返回的 speakers 数组中：

-   如果返回的 uid 为 0，即当频道内的说话者为本地用户时，说话者的音量 `volume` 为 `totalVolume`，即频道内的总音量。

-   如果返回的 uid 不是 0，且 volume 为 0，则默认该 uid 对应的远端用户没有说话。

-   如果有 uid 出现在上次返回的数组中，但不在本次返回的数组中，则默认该 uid 对应的远端用户没有说话。


#### 主播加入回调 (didJoinedOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didJoinedOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

同 userJoinedBlock。该回调提示有主播加入了频道，并返回该主播的 ID。如果在加入之前，已经有主播在频道中了，新加入的用户也会收到已有主播加入频道的回调。Agora 建议连麦主播不超过 17 人。


> 直播场景下:
> -   主播间能相互收到新主播加入频道的回调，并能获得该主播的 uid
> -   观众也能收到新主播加入频道的回调，并能获得该主播的 uid
> -   当 Web 端加入直播频道时，只要 Web 端有推流，SDK 会默认该 Web 端为主播，并触发该回调


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>主播 ID。如果 joinChannelByToken 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td>elapsed</td>
<td>加入频道开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 主播离线回调 (didOfflineOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didOfflineOfUid:(NSUInteger)uid reason:(AgoraUserOfflineReason)reason;
```

同 userOfflineBlock。提示有主播离开了频道（或掉线）。SDK 判断用户离开频道（或掉线）的依据是超时: 在一定时间内（15 秒）没有收到对方的任何数据包，判定为对方掉线。 在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

#### 用户音频静音回调 (didAudioMuted)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

同 userMuteAudioBlock。提示有用户将通话静音/取消静音。

> 现阶段，当频道内主播数超过 20 人，该回调会失效。后续会改进。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>Yes:静音</li>
<li>No: 取消静音</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Rtc Engine统计数据回调 (reportRtcStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine reportRtcStats:(AgoraChannelStats * _Nonnull)stats;
```

同 rtcStatsBlock。该回调定期上报 Rtc Engine 的运行时的状态，每两秒触发一次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stat</td>
<td>AgoraRtcStats，详见 <a href="#didleavechannelwithstats-live-ios"><span>已离开频道回调 (didLeaveChannelWithStats)</span></a> 中的描述</td>
</tr>
</tbody>
</table>



#### 网络质量回调 (LastmileQuality)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine lastmileQuality:(AgoraNetworkQuality)quality;
```

报告本地用户的网络质量。当你调用 enableLastmileTest 之后，该回调函数每 2 秒触发一次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>quality</td>
<td><p>网络质量评分:</p>
<div><ul>
<li>AgoraNetworkQualityUnknown(0)：网络质量未知</li>
<li>AgoraNetworkQualityExcellent(1)：网络质量极好</li>
<li>AgoraNetworkQualityGood(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>AgoraNetworkQualityPoor(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>AgoraNetworkQualityBad(4)：勉强能沟通但不顺畅</li>
<li>AgoraNetworkQualityVBad(5)：网络质量非常差，基本不能沟通</li>
<li>AgoraNetworkQualityDown(6)：完全无法沟通</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### 频道内网络质量报告回调 (networkQuality)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine networkQuality:(NSUInteger)uid txQuality:(AgoraNetworkQuality)txQuality rxQuality:(AgoraNetworkQuality)rxQuality;
```

该回调每 2 秒触发，向APP报告频道内所有用户当前的上行、下行网络质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID。表示该回调报告的是持有该ID的用户的网络质量 。当 uid 为 0 时，返回的是本地用户的网络质量</td>
</tr>
<tr><td>txQuality</td>
<td><p>该用户的上行网络质量。</p>
<div><ul>
<li>AgoraNetworkQualityUnknown(0)：网络质量未知</li>
<li>AgoraNetworkQualityExcellent(1)：网络质量极好</li>
<li>AgoraNetworkQualityGood(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>AgoraNetworkQualityPoor(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>AgoraNetworkQualityBad(4)：勉强能沟通但不顺畅</li>
<li>AgoraNetworkQualityVBad(5)：网络质量非常差，基本不能沟通</li>
<li>AgoraNetworkQualityDown(6)：完全无法沟通</li>
</ul>
</div>
</td>
</tr>
<tr><td>rxQuality</td>
<td><p>该用户的下行网络质量。</p>
<div><ul>
<li>AgoraNetworkQualityUnknown(0)：网络质量未知</li>
<li>AgoraNetworkQualityExcellent(1)：网络质量极好</li>
<li>AgoraNetworkQualityGood(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>AgoraNetworkQualityPoor(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>AgoraNetworkQualityBad(4)：勉强能沟通但不顺畅</li>
<li>AgoraNetworkQualityVBad(5)：网络质量非常差，基本不能沟通</li>
<li>AgoraNetworkQualityDown(6)：完全无法沟通</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### 网络连接中断回调 (rtcEngineConnectionDidInterrupted)

```
- (void)rtcEngineConnectionDidInterrupted:(AgoraRtcEngineKit * _Nonnull)engine;
```

该回调方法表示 SDK 和服务器失去了网络连接。

与 rtcEngineConnectionDidLost 回调的区别是: rtcEngineConnectionDidInterrupted 回调在 SDK 刚失去和服务器连接时触发，rtcEngineConnectionDidLost 在失去连接且尝试自动重连失败后才触发。 失去连接后，除非 APP 主动调用 leaveChannel()，不然 SDK 会一直自动重连。

#### 网络连接丢失回调 (rtcEngineConnectionDidLost)

```
- (void)rtcEngineConnectionDidLost:(AgoraRtcEngineKit * _Nonnull)engine;
```

在 SDK 和服务器失去了网络连接后，会触发 rtcEngineConnectionDidInterrupted 回调，并自动重连。如果重连一定时间内（默认 10 秒）后仍未连上，会触发该回调。 如果 SDK 在调用 joinChannel 后 10 秒内没有成功加入频道，也会触发该回调。 该回调触发后，SDK 仍然会尝试重连，重连成功后会触发 didRejoinChannel 回调。

#### 连接已被禁止回调 (rtcEngineConnectionDidBanned)

```
- (void)rtcEngineConnectionDidBanned:(AgoraRtcEngineKit * _Nonnull)engine;
```

当你被服务端禁掉连接的权限时，会触发该回调。



#### 收到远端音频回调 (firstRemoteAudioFrameOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstRemoteAudioFrameOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

当接收到远端音频第一帧时，触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 UID，表示收到的是哪个用户的音频流</td>
</tr>
<tr><td>elapsed</td>
<td>加入频道开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>


#### 接收到对方数据流消息的回调 (receiveStreamMessageFromUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine receiveStreamMessageFromUid:(NSUInteger)uid streamId:(NSInteger)streamId data:(NSData * _Nonnull)data;
```

提示本地用户已在5秒内收到对方发送的数据包。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>streamId</td>
<td>数据流 ID</td>
</tr>
<tr><td>data</td>
<td>接收到的数据</td>
</tr>
</tbody>
</table>



#### 接收对方数据流消息错误的回调 (didOccurStreamMessageErrorFromUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didOccurStreamMessageErrorFromUid:(NSUInteger)uid streamId:(NSInteger)streamId error:(NSInteger)error missed:(NSInteger)missed cached:(NSInteger)cached;
```

提示本地用户没有在5秒内收到对方发送的数据包。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>streamId</td>
<td>数据流 ID</td>
</tr>
<tr><td>error</td>
<td><dl>
<dt>错误码</dt>
<dd><ul>
<li>ERR_OK = 0, 没有错误</li>
<li>ERR_NOT_IN_CHANNEL=113，用户不在频道内</li>
<li>ERR_BITRATE_LIMIT=115， 码率受限</li>
</ul>
</dd>
</dl>
</td>
</tr>
<tr><td>missed</td>
<td>丢失的消息数量</td>
</tr>
<tr><td>cached</td>
<td>数据流中断时，后面缓存的消息数量</td>
</tr>
</tbody>
</table>

#### 监测到活跃用户回调 (activeSpeaker)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine activeSpeaker:(NSUInteger)speakerUid;
```

如果用户开启了 `enableAudioVolumeIndication` 功能，则当音量检测模块监测到频道内有新的活跃用户说话时，会通过本回调返回该用户的 uid。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>当前时间段声音最大的用户的 uid。如果返回的 uid 为 0，则默认为本地用户</td>
</tr>
</tbody>
</table>


> -   你需要开启 `enableAudioVolumeIndication` 方法才能收到该回调。
> -   uid 返回的是当前时间段内声音最大的用户 uid，而不是瞬时声音最大的用户 uid。


请参考以下伪代码理解逻辑:

```
 - (void)rtcEngine:(AgoraRtcEngineKit *)engine activeSpeaker:(NSUInteger)speakerUid {
[self switchToMainViewOfSpeaker:speakerUid];
}
```

#### 上下麦回调 (didClientRoleChanged)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didClientRoleChanged:(AgoraClientRole)oldRole newRole:(AgoraClientRole)newRole;
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
<tr><td>oldRole</td>
<td>切换前的角色</td>
</tr>
<tr><td>newRole</td>
<td>切换后的角色</td>
</tr>
</tbody>
</table>


#### Token 过期回调 (rtcEngineRequestToken)

```
- (void)rtcEngineRequestToken:(AgoraRtcEngineKit * _Nonnull)engine;
```

在调用 joinChannelByToken 时如果指定了 Token，由于 Token 具有一定的时效，在通话过程中SDK可能由于网络原因和服务器失去连接，重连时可能需要新的 Token。 该回调通知APP需要生成新的 Token，并需调用 renewToken() 为 SDK 指定新的 Token。

之前的处理方法是在 didOccurError() 回调报告 ERR_TOKEN_EXPIRED(109)、ERR_INVALID_TOKEN(110) 时，APP 需要生成新的 Token。 在新版本中，原来的处理仍然有效，但建议把相关逻辑放进该回调里。

#### 本地伴奏播放已结束回调 (rtcEngineLocalAudioMixingDidFinish)

```
- (void)rtcEngineLocalAudioMixingDidFinish:(AgoraRtcEngineKit * _Nonnull)engine;
```

当调用 startAudioMixing 播放伴奏音乐结束后，会触发该回调。如果调用 startAudioMixing 失败，会在 didOccurError 回调里，返回错误码 WARN_AUDIO_MIXING_OPEN_ERROR 。

#### 本地音效播放已结束回调 (rtcEngineDidAudioEffectFinish)

```
- (void)rtcEngineDidAudioEffectFinish:(AgoraRtcEngineKit * _Nonnull)engine soundId:(NSInteger)soundId;
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
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
</tbody>
</table>



#### 远端伴奏播放已开始回调 (rtcEngineRemoteAudioMixingDidStart)

```
- (void)rtcEngineRemoteAudioMixingDidStart:(AgoraRtcEngineKit * _Nonnull)engine;
```

当远端有用户调用 startAudioMixing 播放伴奏音乐，会触发该回调。在合唱应用中可以利用这个回调作为本端歌词播放的触发条件。

#### 远端伴奏播放已结束回调 (rtcEngineRemoteAudioMixingDidFinish)

```
- (void)rtcEngineRemoteAudioMixingDidFinish:(AgoraRtcEngineKit * _Nonnull)engine;
```

当远端有用户调用 stopAudioMixing 停止伴奏音乐，会触发该回调。

<a id = "block"></a>
### Block 回调

SDK 会通过 Block 给应用程序上报一些运行时事件，以下为 SDK 中的回调方法说明。从 1.1 版本开始，SDK 使用 Delegate 代替原有的部分 Block 回调。原有的 Block 回调被标为废弃，目前仍然可以使用，但是建议用相应的 Delegate 方法代替。如果需要使用 Delegate 回调，请将方法中的 Block 函数设置为 nil。

#### 主播加入回调 (userJoinedBlock)

```
- (void)userJoinedBlock:(void(^)
(NSUInteger uid, NSInteger elapsed))userJoinedBlock;
```

提示有主播加入了频道。如果该客户端加入频道时已经有主播在频道中，SDK 也会向应用程序上报这些已在频道中的用户。


> 直播场景下:
> -   主播间能相互收到新主播加入频道的回调，并能获得该主播的 uid
> -   观众也能收到新主播加入频道的回调，并能获得该主播的 uid
> -   当 Web 端加入直播频道时，只要 Web 端有推流，SDK 会默认该 Web 端为主播，并触发该回调


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>主播 ID。如果 joinChannelByToken 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td>elapsed</td>
<td>加入频道开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 主播离线回调 (userOfflineBlock)

```
- (void)userOfflineBlock:(void(^)
(NSUInteger uid))userOfflineBlock;
```

提示有主播离开了频道（或掉线）。

SDK判断用户离开频道（或掉线）的依据是超时: 在一定时间内（15 秒）没有收到对方的任何数据包，判定为对方掉线。 在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>主播 ID</td>
</tr>
</tbody>
</table>



#### 重新加入频道回调 (rejoinChannelSuccessBlock)

```
 - (void)rejoinChannelSuccessBlock:(void(^)(NSString* channel,
NSUInteger uid, NSInteger elapsed))rejoinChannelSuccessBlock;
```

有时候由于网络原因，客户端可能会和服务器失去连接，SDK会进行自动重连，自动重连成功后触发此回调方法，提示有用户重新加入了频道，且频道ID和用户ID均已分配。

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
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>elapsed</td>
<td>加入延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 离开频道回调 (leaveChannelBlock)

```
- (void)leaveChannelBlock:(void(^)
(AgoraRtcStats* stat))leaveChannelBlock;
```

显示用户离开了频道，提供会话数据信息，包括通话时长和tx/rx字节数。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stat</td>
<td><p>AgoraRtcStats, 描述如下:</p>
<ul>
<li>duration: 通话时长，累计值</li>
<li>txBytes: 发送字节数，累计值</li>
<li>rxBytes: 接收字节数，累计值</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Rtc Engine统计数据回调 (rtcStatsBlock)

```
- (void)rtcStatsBlock:(void(^)
(AgoraRtcStats* stat))rtcStatsBlock;
```

该回调定期上报 Rtc Engine 的运行时的状态，每两秒触发一次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stat</td>
<td><p>AgoraRtcStats, 描述如下:</p>
<ul>
<li>duration: 通话时长，累计值</li>
<li>txBytes: 发送字节数，累计值</li>
<li>rxBytes: 接收字节数，累计值</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 音量提示回调 (audioVolumeIndicationBlock)

```
- (void)audioVolumeIndicationBlock:(void(^)(NSArray *speakers,
NSInteger totalVolume))audioVolumeIndicationBlock;
```

提示谁在说话及其音量，默认禁用。可通过启用说话者音量提示 `enableAudioVolumeIndication` 方法开启；开启后，无论频道内是否有人说话，都会按方法中设置的时间间隔返回提示音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>speakers</td>
<td><p>说话者（数组）。每个speaker():</p>
<div><ul>
<li>uid: 说话者的用户 ID。如果返回的 uid 为 0，则默认为本地用户</li>
<li>volume: 说话者的音量（0~255）</li>
</ul>
</div>
</td>
</tr>
<tr><td>totalVolume</td>
<td>（混音后的）总音量（0~255）</td>
</tr>
</tbody>
</table>


#### 用户音频静音回调 (userMuteAudioBlock)

```
- (void)userMuteAudioBlock:(void(^)
(NSUInteger uid, bool muted))userMuteAudioBlock;
```

提示有用户将通话静音/取消静音。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>Yes: 静音</li>
<li>No: 取消静音</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 语音质量回调 (audioQualityBlock)

```
- (void)audioQualityBlock:(void(^)(NSUInteger uid,
AgoraNetworkQuality quality,  NSUInteger delay,
NSUInteger lost))audioQualityBlock;
```

在通话中，该回调方法每两秒触发一次，报告当前通话的（嘴到耳）音频质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID 。每次回调上报一个用户当前的语音质量（不包含自己），不同用户在不同的回调中上报。</td>
</tr>
<tr><td>quality</td>
<td><p>声音质量评分:</p>
<div><ul>
<li>AgoraNetworkQualityUnknown(0)：网络质量未知</li>
<li>AgoraNetworkQualityExcellent(1)：网络质量极好</li>
<li>AgoraNetworkQualityGood(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>AgoraNetworkQualityPoor(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>AgoraNetworkQualityBad(4)：勉强能沟通但不顺畅</li>
<li>AgoraNetworkQualityVBad(5)：网络质量非常差，基本不能沟通</li>
<li>AgoraNetworkQualityDown(6)：完全无法沟通</li>
</ul>
</div>
</td>
</tr>
<tr><td>delay</td>
<td>延迟（毫秒）</td>
</tr>
<tr><td>lost</td>
<td>丢包率（百分比）</td>
</tr>
</tbody>
</table>



#### 网络质量回调 (LastmileQualityBlock)

```
- (void)lastmileQualityBlock:(void(^)
(AgoraNetworkQuality quality))lastmileQualityBlock;
```

报告网络质量，该回调方法每两秒触发一次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>quality</td>
<td><p>网络质量评分:</p>
<ul>
<li>AgoraRtc_Quality_Unknown = 0</li>
<li>AgoraRtc_Quality_Excellent = 1</li>
<li>AgoraRtc_Quality_Good = 2</li>
<li>AgoraRtc_Quality_Poor = 3</li>
<li>AgoraRtc_Quality_Bad = 4</li>
<li>AgoraRtc_Quality_VBad = 5</li>
<li>AgoraRtc_Quality_Down = 6</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



#### 网络连接丢失回调 (connectionLostBlock)

```
- (void)connectionLostBlock:(void(^)())connectionLostBlock;
```

该回调方法表示SDK与服务器失去了网络连接。

##  C++ 接口

<a id = "irtcengine"></a>
### 主要方法 (IRtcEngine)

#### 创建 agora::IRtcEngine 对象 (create)

```
agora::rtc::IRtcEngine* AGORA_CALL createAgoraRtcEngine();
```

创建 IRtcEngine 对象。返回值为 agora::IRtcEngine 对象。IRtcEngine 对象提供的接口方法，如无特殊说明，都是异步调用，对接口的调用建议在同一个线程进行。 所有返回值为 int 型的 API，如无特殊说明，返回值 0 为调用成功，返回值小于 0 为调用失败。

#### 初始化 (initialize)

```
virtual int initialize(const RtcEngineContext& context) = 0;
```

该方法用来进行初始化 Agora Media 服务。传入 Agora 为开发者签发的厂商秘钥进行初始化。 在创建 agora::IRtcEngine 对象后，必须先调用该方法进行初始化，才能使用其他方法。初始化成功后，默认处于语音通话模式。 使用视频功能需要额外调用一次 enableVideo API 启用视频服务。

RTCEngineContext 定义如下:

```
struct RtcEngineContext
{
    IRtcEngineEventHandler* eventHandler;
    /** App ID issued to the developers by Agora. Apply for a new one from Agora if it is missing from your kit.
    */
    const char* appId;
    RtcEngineContext()
    :eventHandler(NULL)
    ,appId(NULL)
    {}
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
<tr><td>appId</td>
<td>Agora 为应用程序开发者签发的厂商秘钥。详见 <a href="../../cn/Agora%20Platform/token.md"><span>获取 App ID</span></a></td>
</tr>
<tr><td>eventHandler</td>
<td>IRtcEngineEventHandler 是一个提供了缺省实现的抽象类，SDK 通过该抽象类向应用程序报告 SDK 运行时的各种事件</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_VENDOR_KEY(-101): 传入的 App ID 无效</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 设置频道属性 (setChannelProfile)

```
virtual int setChannelProfile(CHANNEL_PROFILE_TYPE profile) = 0;
```

该方法用于设置频道模式(Profile)。`AgoraRtcEngineKit` 需知道应用程序的使用场景(例如通信模式或直播模式), 从而使用不同的优化手段。

目前 Agora Native SDK 支持以下几种模式:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>模式</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>通信</td>
<td>通信为默认模式，用于常见的一对一或群聊，频道中的任何用户可以自由说话</td>
</tr>
<tr><td>直播</td>
<td>直播模式有主播和观众两种用户角色，可以通过调用 setClientRole 设置。主播可收发语音和视频，但观众只能收，不能发</td>
</tr>
<tr><td>游戏语音</td>
<td>频道内任何用户可自由发言。该模式下默认使用低功耗低码率的编解码器</td>
</tr>
</tbody>
</table>

> -   同一频道内只能同时设置一种模式。如果想要切换模式，则需要先调用 release 销毁当前引擎，然后重新调用 `create`、并在 `initialize` 方法中传入 **不同** 的 App ID，再调用该方法切换频道模式。
> -   不同的频道模式必须使用不同的 App ID。
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
<tr><td>profile</td>
<td><p>频道模式：</p>
<ul>
<li>CHANNEL_PROFILE_COMMUNICATION = 0: 通信模式 (默认)</li>
<li>CHANNEL_PROFILE_LIVE_BROADCASTING = 1: 直播模式</li>
<li>CHANNEL_PROFILE_GAME = 2: 游戏语音模式</li>
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



#### 设置用户角色 (setClientRole)

```
virtual int setClientRole(CLIENT_ROLE_TYPE role) = 0;
```

直播模式下，在加入频道前，用户需要通过本方法设置观众(默认)或主播模式；在加入频道后，用户可以通过本方法切换用户模式。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>role</td>
<td><p>直播场景里的用户角色。</p>
<ul>
<li>CLIENT_ROLE_BROADCASTER = 1; 主播</li>
<li>CLIENT_ROLE_AUDIENCE = 2; 观众(默认)</li>
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



#### 打开音频 (enableAudio)

```
virtual int enableAudio() = 0;
```

打开音频(默认为开启状态)。返回值:

-   0: 方法调用成功

-   <0: 方法调用失败

> 该方法设置内部引擎为启用状态，在 `leaveChannel` 后仍然有效。

#### 关闭/重启本地语音功能 (enableLocalAudio)

```
virtual int enableLocalAudio(bool enabled) = 0;
```

当用户加入频道时，语音功能默认是开启的。该方法可以关闭或重新开启本地语音功能，停止或重新开始本地音频采集及处理。
该方法不影响接收或播放远端音频流，适用于只听不发的用户场景。
语音功能关闭或重新开启后，会收到回调 `onMicrophoneEnabled`。

> - 该方法需要在 `joinChannel` 之后调用才能生效。
> - 该方法与 `muteLocalAudioStream` 的区别在于：
>    *  `enableLocalAudio`：开启或关闭本地语音采集及处理
>    *  `muteLocalAudioStream`：停止或继续发送本地音频流

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enabled</td>
<td>
<ul>
<li>true：重新开启本地语音功能，即开启本地语音采集或处理</li>
<li>false：关闭本地语音功能，即停止本地语音采集或处理</li>
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

#### 关闭音频 (disableAudio)

```
virtual int disableAudio() = 0;
```

关闭音频。返回值:

-   0: 方法调用成功

-   <0: 方法调用失败


> 该方法设置内部引擎为禁用状态，在 `leaveChannel` 后仍然有效。

#### 设置音频属性 (setAudioProfile)

```
virtual int setAudioProfile(AUDIO_PROFILE_TYPE profile, AUDIO_SCENARIO_TYPE scenario) = 0;
```

该方法用于设置音质。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>profile</td>
<td><p>设置采样率，码率，编码模式和声道数:</p>
<div><ul>
<li>AUDIO_PROFILE_SPEECH_STANDARD = 1: 指定 32 KHz采样率，语音编码, 单声道，编码码率约 18 kbps</li>
<li>AUDIO_PROFILE_MUSIC_STANDARD = 2: 指定 48 KHz采样率，音乐编码, 单声道，编码码率约 48 kbps</li>
<li>AUDIO_PROFILE_MUSIC_STANDARD_STEREO = 3: 指定 48 KHz采样率，音乐编码,  双声道，编码码率约 56 kbps</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY = 4: 指定 48 KHz 采样率，音乐编码, 单声道，编码码率约 128 kbps</li>
<li>AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO = 5: 指定 48 KHz采样率，音乐编码,  双声道，编码码率约 192 kbps</li>
</ul>
</div>
</td>
</tr>
<tr><td>scenario</td>
<td><p>设置音频应用场景:</p>
<div><ul>
<li>AUDIO_SCENARIO_DEFAULT = 0: 默认设置</li>
<li>AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT = 1: 娱乐应用，需要频繁上下麦的场景</li>
<li>AUDIO_SCENARIO_EDUCATION = 2: 教育应用，流畅度和稳定性优先</li>
<li>AUDIO_SCENARIO_GAME_STREAMING = 3: 游戏直播应用，需要外放游戏音效也直播出去的场景</li>
<li>AUDIO_SCENARIO_SHOWROOM = 4: 秀场应用，音质优先和更好的专业外设支持</li>
<li>AUDIO_SCENARIO_CHATROOM_GAMING = 5: 游戏开黑</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


> -   该方法需要在 `joinChannel` 之前设置好，`joinChannel` 之后设置不生效
> -   通信模式下，该方法设置 `profile` 生效，设置 `scenario` 不生效
> -   通信和直播模式下，音质（码率）会有网络自适应的调整，通过该方法设置的是一个最高码率
> -   音乐教学场景下，建议将 profile 设置为 MUSIC_HIGH_QUALITY（4），scenario 设置为 GAME_STREAMING（3）
> -   scenario 中的 CHATROOM_ENTERTAINMENT（1）和 CHATROOM_GAMING（5）为游戏开黑场景，建议仅在纯音频的场景下使用


#### 设置音频高音质选项 (setHighQualityAudioParametersWithFullband)

```
int setHighQualityAudioParameters(boolean fullband, boolean stereo, boolean fullBitrate)
```

该方法设置音频高音质选项。请在加入频道前调用本方法并一次性设置好三个模式。切勿在加入频道后再次调用本方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>fullband</td>
<td><p>全频带编解码器（48 kHz 采样率）, 不兼容 1.7.4 以前版本</p>
<ul>
<li>True: 启用全频带编解码器</li>
<li>False: 禁用全频带编解码器</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>stereo</td>
<td><p>立体声编解码器，不兼容 1.7.4 以前版本</p>
<ul>
<li>True: 启用立体声编解码器</li>
<li>False: 禁用立体声编解码器</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>fullBitrate</td>
<td><p>高码率模式，建议仅在纯音频模式下使用</p>
<ul>
<li>True: 启用高码率模式</li>
<li>False: 禁用高码率模式</li>
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


> 该方法目前已废弃，Agora 不推荐使用。如果想要设置音质，请参考 `setAudioProfile` 中的用法。

#### 加入频道 (joinChannel)

```
virtual int joinChannel(const char* token, const char* channelId, const char* info, uid_t uid) = 0;
```

该方法让用户加入通话频道，在同一个频道内的用户可以互相通话，多个用户加入同一个频道，可以群聊。 使用不同 App ID 的应用程序是不能互通的。如果已在通话中，用户必须调用 `leaveChannel()` 退出当前通话，才能进入下一个频道。


> 同一个频道里不能出现两个相同的 UID。如果你的 App 支持多设备同时登录，即同一个用户账号可以在不同的设备上同时登录(例如微信支持在 PC 端和移动端同时登录)，请保证传入的 UID 不相同。 例如你之前都是用同一个用户标识作为 UID, 建议从现在开始加上设备 ID, 以保证传入的 UID 不相同 。如果你的 App 不支持多设备同时登录，例如在电脑上登录时，手机上会自动退出，这种情况下就不需要在 UID 上添加设备 ID。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>token</td>
<td><ul>
<li>安全要求不高: 将值设为 null</li>
<li>安全要求高: 将值设置为 Token 值。 如果你已经启用了 App Certificate, 请务必使用 Token。 关于如何获取 Token，详见 <a href="../../cn/Agora%20Platform/token.html.md"><span>密钥说明</span></a> 。</li>
</ul>
</td>
</tr>
<tr><td>channelId</td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td>info</td>
<td>(非必选项) 开发者需加入的任何附加信息。一般可设置为空字符串，或频道相关信息。该信息不会传递给频道内的其他用户</td>
</tr>
<tr><td>uid</td>
<td>(非必选项) 用户ID，32位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK不对该返回值进行维护
uid 在 SDK 内部用 32 位无符号整数表示，由于 Java 不支持无符号整数，uid 被当成 32 位有符号整数处理，对于过大的整数，Java 会表示为负数，如有需要可以用(uid&amp;0xffffffffL)转换成 64 位整数</td>
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




> 在 `joinChannel()` 时，SDK 调用 `setCategoryAVAudioSessionCategoryPlayAndRecord` 将 `AVAudioSession` 设置到 `PlayAndRecord` 模式，应用程序不应将其设置到其他模式。设置该模式时，正在播放的声音会被打断（比如正在播放的响铃声）。

#### 离开频道 (leaveChannel)

```
virtual int leaveChannel() = 0;
```

离开频道，即挂断或退出通话。joinChannel后，必须调用 leaveChannel 以结束通话，否则不能进行下一次通话。 不管当前是否在通话中，都可以调用 leaveChannel，没有副作用。如果成功，则返回值为0。leaveChannel 会把会话相关的所有资源释放掉。

leaveChannel 是异步操作，调用返回时并没有真正退出频道。在真正退出频道后，SDK 会触发 onLeaveChannel 回调。

> 如果你调用了 `leaveChannel()` 后立即调用 `release()`，SDK 将无法触发 `onLeaveChannel` 回调。

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
<li>ERR_REFUSED (-5): 离开频道失败，当前状态不在通话中，或者正在离开频道</li>       
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### 设置本地语音音调 (setLocalVoicePitch)

```
int setLocalVoicePitch(double pitch);
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
<tr><td>pitch</td>
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

#### 设置远端用户的语音位置 (setRemoteVoicePosition)

```
virtual int setRemoteVoicePosition(int uid, double pan, double gain) = 0;
```

该方法设置远端用户的语音位置。

> 该方法只在耳机模式下有效，在外放模式下无效。

<table>
<colgroup>
<col>
</col>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td><p>远端用户的 UID</p>
</td></tr>
<tr><td>pan</td>
<td><p>设置是否改变音效的空间位置。取值范围为 [-1, 1]：
<ul>
<li>0：表示该音效出现在正前方</li>
<li>-1：表示该音效出现在左边</li>
<li>1：表示该音效出现在右边</li>
</ul>
</td>
</tr>
<tr><td>gain</td>
<td><p>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。
默认值为 100.0。取值越小，音效的音量越低。</p>
</td></tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt; 0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### 设置仅限语音模式 (setVoiceOnlyMode)

```
virtual int setVoiceOnlyMode(bool enable) = 0;
```

该方法设置仅语音模式。即仅传输语音流，而不传输其他流，例如，敲键盘的声音等。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enable</td>
<td>
<ul>
<li>true：启用仅语音模式</li>
<li>false：禁用仅语音模式</li>
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


#### 设置本地语音音效均衡 (setLocalVoiceEqualization)

```
int setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY bandFrequency, int bandGain);
```

该方法设置本地语音音效均衡。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>bandFrequency</td>
<td>取值范围是 [0-9]，分别代表音效的 10 个 band 的中心频率 [31，62，125，250，500，1k，2k，4k，8k，16k]Hz</td>
</tr>
<tr><td>bandGain</td>
<td>每个 band 的增益，单位是 dB，每一个值的范围是 [-15，15]</td>
</tr>
</tbody>
</table>



#### 设置本地音效混响 (setLocalVoiceReverb)

```
int setLocalVoiceReverb(AUDIO_REVERB_TYPE reverbKey, int value);
```

该方法设置本地音效混响。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>reverbKey</td>
<td>混响音效 Key。该方法共有 5 个混响音效 Key，分别如 value 栏列出</td>
</tr>
<tr><td>value</td>
<td><p>各混响音效 Key 所对应的值：</p>
<div><ul>
<li>AUDIO_REVERB_DRY_LEVEL = 0：(dB, [-20,10])，原始声音效果，即所谓的 dry signal</li>
<li>AUDIO_REVERB_WET_LEVEL = 1：(dB, [-20,10])，早期反射信号效果，即所谓的 wet signal</li>
<li>AUDIO_REVERB_ROOM_SIZE = 2：([0，100])， 所需混响效果的房间尺寸</li>
<li>AUDIO_REVERB_WET_DELAY = 3：(ms, [0, 200])，wet signal 的初始延迟长度，以毫秒为单位</li>
<li>AUDIO_REVERB_STRENGTH = 4：([0，100])，后期混响长度</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


#### 获取音效音量 (getEffectsVolume)

```
int getEffectsVolume();
```

该方法获取音效的音量，范围为 \[0.0, 100.0\]。

#### 设置音量音效 (setEffectsVolume)

```
int setEffectsVolume(int volume);
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
<tr><td>volume</td>
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



#### 实时调整音效音量 (setVolumeOfEffect)

```
int setVolumeOfEffect(int soundId, int volume);
```

该方法实时调整指定音效的音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>volume</td>
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



#### 播放指定音效 (playEffect)

```
int playEffect(int soundId, const char* filePath, int loopCount, double pitch, double pan, int gain, bool publish);
```

该方法播放指定音效。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID <sup>[1]</sup></td>
</tr>
<tr><td>filepath</td>
<td>音效文件的绝对路径</td>
</tr>
<tr><td>loopCount</td>
<td><p>设置音效循环播放的次数：</p>
<div><ul>
<li>0：播放音效一次</li>
<li>1：播放音效两次</li>
<li>-1：无限循环播放音效，直至调用 <code>stopEffect</code> 或 <code>stopAllEffects</code> 后停止</li>
</ul>
</div>
</td>
</tr>
<tr><td>pitch</td>
<td>设置音效的音调
取值范围为 [0.5, 2]。默认值为 1.0，表示不需要修改音调。取值越小，则音调越低</td>
</tr>
<tr><td>pan</td>
<td><p>设置是否改变本地听到的音效的空间位置。取值范围为 [-1.0, 1.0]：</p>
<div><ul>
<li>0.0：音效出现在正前方</li>
<li>-1.0：音效出现在左边</li>
<li>1.0：音效出现在右边</li>
</ul>
</div>
</td>
</tr>
<tr><td>gain</td>
<td>设置是否改变单个音效的音量。取值范围为 [0.0, 100.0]。默认值为 100.0。取值越小，则音效的音量越低</td>
</tr>
<tr><td>publish</td>
<td><p>设置是否将音效传到远端：</p>
<div><ul>
<li>true：音效在本地播放的同时，会发布到 Agora 云上，因此远端用户也能听到该音效</li>
<li>false：音效不会发布到 Agora 云上，因此只能在本地听到该音效</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：该方法调用成功</li>
<li>&lt; 0：该方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



> [1] 如果你已通过 `preloadEffect` 将音效加载至内存，确保这里设置的 `soundId` 与 `preloadEffect` 设置的 `soundId` 相同。

#### 停止播放指定音效 (stopEffect)

```
int stopEffect(int soundId);
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
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
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



#### 停止播放所有音效 (stopAllEffects)

```
int stopAllEffects();
```

该方法停止播放所有音效。

#### 预加载音效 (preloadEffect)

```
int preloadEffect(int soundId, char* filePath);
```

该方法将指定音效文件（压缩的语音文件）预加载至内存。


> 为保证通信畅通，请注意预加载音效文件的大小，并在 `joinChannel` 前就使用该方法完成音效预加载。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
<tr><td>filePath</td>
<td>音效文件的绝对路径</td>
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



#### 释放音效 (unloadEffect)

```
int unloadEffect(int soundId);
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
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
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



#### 暂停音效播放 (pauseEffect)

```
int pauseEffect(int soundId);
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
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
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



#### 暂停所有音效播放 (pauseAllEffects)

```
int pauseAllEffects();
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
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 恢复播放指定音效 (resumeEffect)

```
int resumeEffect(int soundId);
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
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
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



#### 恢复播放所有音效 (resumeAllEffects)

```
int resumeAllEffects();
```

该方法恢复播放所有音效。

#### 打开与 Web SDK 的互通 (enableWebSdkInteroperability)

```
int enableWebSdkInteroperability(bool enabled);
```

该方法打开或关闭与 Agora Web SDK 的互通。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enabled</td>
<td><p>是否已打开与 Agora Web SDK 的互通：</p>
<div><ul>
<li>true：打开互通</li>
<li>false：关闭互通</li>
</ul>
</div>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 修改默认的语音路由 (setDefaultAudioRouteToSpeakerphone)

```
int setDefaultAudioRouteToSpeakerphone(bool defaultToSpeaker) = 0;
```

> -  该方法只在纯音频模式下工作，在有视频的模式下不工作。
> -  该方法需要在 `joinChannel` 前设置，否则不生效。


如有必要，调用该方法修改默认的语音路由。默认的语音路由如下:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>频道模式</strong></td>
<td><strong>默认语音路由</strong></td>
</tr>
<tr><td>通信</td>
<td><ul>
<li>语音通话: 听筒</li>
<li>视频通话: 外放</li>
<li>视频通话中关闭视频 <sup>[2]</sup> : 听筒</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>直播</td>
<td>外放</td>
</tr>
<tr><td>游戏语音</td>
<td>外放</td>
</tr>
</tbody>
</table>



> [2] 已调用 disableVideo 或 已调用 muteLocalVideoStream/muteAllRemoteVideoStreams

**如有需要**， 根据下表修改上述默认的语音路由:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>defaultToSpeaker</td>
<td><ul>
<li>true: 默认路由改为外放(扬声器)</li>
<li>false: 默认路由改为听筒</li>
</ul>
<p>无论语音是从外放还是听筒出声，如果插上耳机或连接蓝牙，语音路由会发生相应改变。拔出耳机或断开蓝牙后语音路由将恢复成默认路由。</p>
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



#### 打开扬声器 (setEnableSpeakerphone)

```
int setEnableSpeakerphone(bool speakerOn) = 0;
```

该方法将语音路由强制设置为扬声器（外放）。调用该方法后，SDK 将返回 `onAudioRouteChanged` 回调提示状态已更改。


> 在调用该方法前，请先阅读 `setDefaultAudioRouteToSpeakerphone` 里关于默认语音路由的说明，确认是否需要调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>enabled</td>
<td><ul>
<li><p>true:</p>
<div><ul>
<li>如果用户已在频道内，无论之前语音是路由到耳机，蓝牙，还是听筒，调用该 API 后均会默认切换到从外放(扬声器)出声。</li>
<li>如果用户尚未加入频道，调用该API后，无论用户是否有耳机或蓝牙设备，在加入频道时均会默认从外放(扬声器)出声。</li>
</ul>
</div>
</li>
<li><p>false: 语音会根据默认路由出声，详见 <code>setDefaultAudioRouteToSpeakerphone</code></p>
</li>
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





#### 启用内置加密，并设置数据加密密码 (setEncryptionSecret)

```
virtual int setEncryptionSecret(const char* secret) = 0;
```

在加入频道之前，应用程序需调用 `setEncryptionSecret()` 指定 secret 来启用内置的加密功能，同一频道内的所有用户应设置相同的 secret。 当用户离开频道时，该频道的 secret 会自动清除。如果未指定 secret 或将 secret 设置为空，则无法激活加密功能。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>secret</td>
<td>加密密码</td>
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



#### 设置内置的加密方案 (setEncryptionMode)

```
virtual int setEncryptionMode(const char* encryptionMode) = 0;
```

Agora Native SDK 支持内置加密功能，默认使用 AES-128-XTS 加密方式。如需使用其他加密方式，可以调用该 API 设置。

同一频道内的所有用户必须设置相同的加密方式和 secret 才能进行通话。关于这几种加密方式的区别，请参考 AES 加密算法的相关资料。

> 在调用本方法前，请先调用 `setEncryptionSecret()` 启用内置加密功能。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>encryptionMode</td>
<td><p>加密方式。目前支持以下几种:</p>
<ul>
<li>“aes-128-xts”: 128 位 AES 加密， XTS 模式</li>
<li>“aes-128-ecb”: 128 位 AES 加密， ECB 模式</li>
<li>“aes-256-xts”: 256 位 AES 加密，XTS 模式</li>
<li>“”: 设置为空字符串时，使用默认加密方式 “aes-128-xts”</li>
</ul>
</td>
</tr>
<tr/>
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


#### 创建数据流 (createDataStream)

```
virtual int createDataStream(int* streamId, bool reliable, bool ordered) = 0;
```

该方法用于创建数据流。频道内每人最多只能创建 5 个数据流。频道内数据通道最多允许数据延迟 5 秒，若超过 5 秒接收方尚未收到数据流，则数据通道会向应用程序报错。


> 请将 `reliable` 和 `ordered` 同时设置为 true 或 false, 暂不支持交叉设置。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td>reliable</td>
<td><ul>
<li>true：接收方会在 5 秒内收到发送方所发送的数据，否则会收到 onStreamMessageError 回调获得相应报错信息。</li>
<li>false：接收方不保证收到，就算数据丢失也不会报错。</li>
</ul>
</td>
</tr>
<tr><td>ordered</td>
<td><ul>
<li>true：接收方 5 秒内会按照发送方发送的顺序收到数据包。</li>
<li>false：接收方不保证按照发送方发送的顺序收到数据包。</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>&lt; 0: 创建数据流失败的错误码 <sup>[3]</sup></li>
<li>&gt; 0: 数据流 ID</li>
</ul>
</td>
</tr>
</tbody>
</table>



> [3] 返回的错误码是负数，对应错误代码和警告代码里的正整数。例如返回的错误码为 -2，则对应错误代码和警告代码里的 2: ERR_INVALID_ARGUMENT 。

#### 发送数据流 (sendStreamMessage)

```
virtual int sendStreamMessage(int streamId, const char* data, size_t length) = 0;
```

该方法发送数据流消息到频道内所有用户。频道内每秒最多能发送 30 个包，且每个包最大为 1 KB。 API 须对数据通道的传送速率进行控制: 每个客户端每秒最多能发送 6 KB 数据。频道内每人最多能同时有 5 个数据通道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>streamId</td>
<td>数据流 ID，<code>createDataStream()</code> 的返回值。</td>
</tr>
<tr><td>message</td>
<td>待发送的数据</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 发送成功</li>
<li>&lt; 0: 发送失败，返回错误码：
<ul>
<li>ERR_SIZE_TOO_LARGE (114)</li>
<li>ERR_TOO_OFTEN (12)</li>
<li>ERR_BITRATE_LIMIT (115)</li>
<li>ERR_INVALID_ARGUMENT (2)</li>
</ul>
</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 开始语音直播测试 (startEchoTest)

```
virtual int startEchoTest() = 0;
```

该方法启动语音直播测试，目的是测试系统的音频设备（耳麦、扬声器等）和网络连接是否正常。 在测试过程中，用户先说一段话，在 10 秒后，声音会回放出来。如果 10 秒后用户能正常听到自己刚才说的话，就表示系统音频设备和网络连接都是正常的。


> -   调用 startEchoTest 后必须调用 stopEchoTest 以结束测试，否则不能进行下一次回声测试，或者调用 `joinChannel()` 进行通话。
>-   直播模式下，只有主播用户才能调用。如果用户由通信模式切换到直播模式，请务必调用 `setClientRole()` 方法将用户的橘色设置为。

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
<li>ERR_REFUSED (-5)：无法启动测试，可能没有成功初始化</li> 
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>




#### 停止语音直播测试 (stopEchoTest)

```
virtual int stopEchoTest() = 0;
```

该方法停止语音直播测试。

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
<li>ERR_REFUSED (-5)：不能启动测试，可能语音直播测试没有成功启动</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>




#### 启动网络测试 (enableLastmileTest)

```
virtual int enableLastmileTest() = 0;
```

该方法启用网络连接质量测试，用于检测用户网络接入质量。默认该功能为关闭状态。该方法主要用于以下场景:

-   用户加入频道前，可以调用该方法判断和预测目前的上行网络质量是否足够好。

-   观众切换为主播进行连麦前，可以调用该方法判断和预测目前的上行网络质量是否足够好。


在这两种场景下，启用该方法均会消耗一定的网络流量，影响通话质量。用户必须在收到 onLastmileQuality 回调后立即调用 disableLastmileTest 停止测试，再加入频道或切换用户角色。


> 当前频道内的主播(包括已切换为主播角色的高级观众)，请勿调用该方法。

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



#### 禁用网络测试 (disableLastmileTest)

```
virtual int disableLastmileTest() = 0;
```

该方法禁用网络测试。

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


#### 获取通话 ID (getCallId)

```
virtual int getCallId(agora::util::AString& callId) = 0;
```

获取当前的通话 ID。

客户端在每次 `joinChannel()` 后会生成一个对应的 CallId，标识该客户端的此次通话。有些方法如rate, complain需要在通话结束后调用，向SDK提交反馈，这些方法必须指定CallId参数。使用这些反馈方法，需要在通话过程中调用getCallId方法获取CallId，在通话结束后在反馈方法中作为参数传入。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>callId</td>
<td>返回当前的通话ID</td>
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



#### 给通话评分 (rate)

```
virtual int rate(const char* callId, int rating, const char* description) = 0;
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
<tr><td>callId</td>
<td>通话 getCallId 函数获取的通话 ID</td>
</tr>
<tr><td>rating</td>
<td>给通话的评分，最低 1 分，最高 10 分</td>
</tr>
<tr><td>description</td>
<td>(非必选项) 给通话的描述，可选，长度应小于 800 字节</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT(-2): 传入的参数无效，例如 callId 无效</li>
<li>ERR_NOT_READY(-3)：SDK 不在正确的状态，可能是因为没有成功初始化</li>      
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>




#### 投诉通话质量 (complain)

```
virtual int complain(const char* callId, const char* description) = 0;
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
<tr><td>callId</td>
<td>通话 getCallId 函数获取的通话 ID</td>
</tr>
<tr><td>description</td>
<td>(非必选项) 给通话的描述，可选，长度应小于 800 字节</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
<ul>
<li>ERR_INVALID_ARGUMENT(-2): 传入的参数无效，例如 callId 无效</li>
<li>ERR_NOT_READY(-3)：SDK 不在正确的状态，可能是因为没有成功初始化</li>       
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



#### 更新 Token (renewToken)

```
virtual int renewToken(const char* token) = 0;
```

该方法用于更新 Token。如果启用了 Token 机制，过一段时间后使用的 Token 会失效。

当：

-   `onError` 回调报告 ERR_TOKEN_EXPIRED(109) 时，

-   `onRequestToken` 回调报告 ERR_TOKEN_EXPIRED(109) 时，


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
<tr><td>token</td>
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



#### 设置日志文件 (setLogFile)

```
int setLogFile(const char* filePath);
```

设置 SDK 的输出 log 文件。SDK 运行时产生的所有 log 将写入该文件。应用程序必须保证指定的目录存在而且可写。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>filePath</td>
<td>日志文件的完整路径。该日志文件为 UTF-8 编码。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0:  方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> Android 平台下日志文件的默认地址为：sdcard/<appname>/agorasdk.log。appname 为应用名称。

#### 设置日志过滤器 (setLogFilter)

```
int setLogFilter(unsigned int filter);
```

设置 SDK 的输出日志过滤等级。不同的过滤等级可以单独或组合使用。

日志级别顺序依次为 *OFF*、*CRITICAL*、*ERROR*、*WARNING*、*INFO* 和 *DEBUG*。选择一个级别，你就可以看到在该级别之前所有级别的日志信息。

例如，你选择 *WARNING* 级别，就可以看到在 *CRITICAL*、*ERROR* 和 *WARNING* 级别上的所有日志信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>filter</td>
<td><p>设置过滤器等级:</p>
<div><ul>
<li>LOG_FILTER_OFF = 0：不输出日志信息</li>
<li>LOG_FILTER_DEBUG = 0x80f：输出所有 API 日志信息</li>
<li>LOG_FILTER_INFO = 0x0f：输出 CRITICAL、ERROR、WARNING 和 INFO 级别的日志信息</li>
<li>LOG_FILTER_WARNING = 0x0e：输出 CRITICAL、ERROR 和 WARNING 级别的日志信息</li>
<li>LOG_FILTER_ERROR = 0x0c：输出 CRITICAL 和 ERROR 级别的日志信息</li>
<li>LOG_FILTER_CRITICAL = 0x08：输出 CRITICAL 级别的日志信息</li>
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



#### 销毁IRtcEngine对象 (release)

```
virtual void release(bool sync=false) = 0;
```

该方法用于销毁 IRtcEngine 对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>sync</td>
<td><p>指明调用方式为同步或异步:</p>
<ul>
<li>True: 同步调用。在等待 IRtcEngine 对象资源释放后再返回。APP 不应该在 SDK 产生的回调中调用该接口，否则由于 SDK 要等待回调返回才能回收相关的对象资源，会造成死锁。SDK 会自动检测这种死锁并转为异步调用，但是检测本身会消耗额外的时间</li>
<li>False: 异步调用。不等 IRtcEngine 对象资源释放就立即返回。SDK 会自行释放所有资源。使用异步调用时要注意，不要在该调用后立即卸载 SDK 动态库，否则可能会因为 SDK 的清理线程还没有退出而崩溃</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>返回值</td>
<td><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 错误代码</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 查询 SDK 版本号 (getVersion)

```
virtual const char* getVersion(int* build) = 0;
```

该方法返回 SDK 版本号的字符串。


<a id = "rtcengineparameters"></a>
### 参数方法 (RtcEngineParameters)

该辅助类用于对 SDK 进行参数设置，如下为该类的具体方法说明。


#### 将自己静音 (muteLocalAudioStream)

```
int muteLocalAudioStream(bool mute);
```

静音/取消静音。该方法用于允许/禁止往网络发送本地音频流。


> 该方法不影响录音状态，并没有禁用麦克风。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>bool mute</td>
<td><ul>
<li>true：麦克风静音</li>
<li>false：取消麦克风静音</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 静音所有远端音频 (muteAllRemoteAudioStreams)

```
int muteAllRemoteAudioStreams(bool mute);
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
<tr><td>muted</td>
<td><ul>
<li>True: 停止接收和播放所有远端音频流</li>
<li>False: 允许接收和播放所有远端音频流</li>
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



#### 静音指定用户音频 (muteRemoteAudioStream)

```
int muteRemoteAudioStream(uid_t uid, bool mute);
```

静音指定远端用户/对指定远端用户取消静音。


> 如果之前有调用过 muteAllRemoteAudioStreams (true) 对所有远端音频进行静音，在调用本 API 之前请确保你已调用 muteAllRemoteAudioStreams (false) 。 muteAllRemoteAudioStreams 是全局控制，muteRemoteAudioStream 是精细控制。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>指定用户 ID</td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>True: 停止接收和播放指定音频流</li>
<li>False: 允许接收和播放指定音频流</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td>
<div><ul>
<li>0: 方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 启用说话者音量提示 (enableAudioVolumeIndication)

```
int enableAudioVolumeIndication(int interval, int smooth);
```

该方法允许 SDK 定期向应用程序反馈当前谁在说话以及说话者的音量。启用该方法后，无论频道内是否有人说话，都会在 `onAudioVolumeIndication` 回调中按设置的间隔时间返回音量提示。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>interval</td>
<td><p>指定音量提示的时间间隔：</p>
<ul>
<li>&lt;= 0：禁用音量提示功能</li>
<li>&gt; 0：返回音量提示的间隔，单位为毫秒。建议设置到大于 200 毫秒。最小不得少于 10 毫秒，否则会收不到 <code>onAudioVolumeIndication</code> 回调</li>
</ul>
</td>
</tr>
<tr><td>smooth</td>
<td>平滑系数，指定音量提示的灵敏度。取值范围为 [0-10]，建议值为 3，数字越大，波动越灵敏；数字越小，波动越平滑。</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 开始播放伴奏 (startAudioMixing)

```
int startAudioMixing(const char* filePath, bool loopback, bool replace, int cycle);
```

指定本地音频文件来和麦克风采集的音频流进行混音和替换(用音频文件替换麦克风采集的音频流)， 可以通过参数选择是否让对方听到本地播放的音频和指定循环播放的次数。


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
<tr><td>filePath</td>
<td>指定需要混音的本地音频文件名和文件路径名:
支持以下音频格式: mp3, aac, m4a, 3gp, wav, flac</td>
</tr>
<tr><td>loopback</td>
<td><ul>
<li>true: 只有本地可以听到混音或替换后的音频流</li>
<li>false: 本地和对方都可以听到混音或替换后的音频流</li>
</ul>
</td>
</tr>
<tr><td>replace</td>
<td><ul>
<li>true: 音频文件内容将会替换本地录音的音频流</li>
<li>false: 音频文件内容将会和麦克风采集的音频流进行混音</li>
</ul>
</td>
</tr>
<tr><td>cycle</td>
<td>指定音频文件循环播放的次数：
<ul>
<li>正整数: 循环的次数</li>
<li>-1: 无限循环</li>
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



#### 停止播放伴奏 (stopAudioMixing)

```
int stopAudioMixing();
```

该方法停止播放伴奏。请在频道内调用该方法。

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
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 暂停播放伴奏 (pauseAudioMixing)

```
int pauseAudioMixing();
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
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 恢复播放伴奏 (resumeAudioMixing)

```
int resumeAudioMixing();
```

该方法恢复混音，继续播放伴奏。请在频道内调用该方法。

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
<li>0：方法调用成功</li>
<li>&lt;0: 方法调用失败</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### 调节伴奏音量 (adjustAudioMixingVolume)

```
int adjustAudioMixingVolume(int volume);
```

该方法调节混音里伴奏的音量大小。请在频道内调用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>volume</td>
<td>伴奏音量范围为 0~100。默认 100 为原始文件音量</td>
</tr>
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



#### 获取伴奏时长 (getAudioMixingDuration)

```
int getAudioMixingDuration();
```

该方法获取伴奏时长，单位为毫秒。请在频道内调用该方法。如果返回值 <0，表明调用失败。

#### 获取伴奏播放进度 (getAudioMixingCurrentPosition)

```
int getAudioMixingCurrentPosition();
```

该方法获取当前伴奏播放进度，单位为毫秒。请在频道内调用该方法。

#### 拖动语音进度条 (setAudioMixingPosition)

```
int setAudioMixingPosition(int pos /*in ms*/);
```

该方法可以拖动播放音频文件的进度条，这样你可以根据实际情况播放文件，而不是非得从头到尾播放一个文件。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>pos</td>
<td>整数。进度条位置，单位为毫秒</td>
</tr>
</tbody>
</table>


#### 开始客户端录音 (startAudioRecording)

```
int startAudioRecording(const char* filePath, AUDIO_RECORDING_QUALITY_TYPE quality);
```

Agora SDK 支持通话过程中在客户端进行录音。该方法录制频道内所有用户的音频，并生成一个包含所有用户声音的录音文件，录音文件格式可以为:

-   wav : 文件大，音质保真度高

-   aac : 文件小，有一定的音质保真度损失


请确保应用程序里指定的目录存在且可写。该接口需在加入频道之后调用。如果调用 `leaveChannel()` 时还在录音，录音会自动停止。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>filePath</td>
<td>录音文件的本地保存路径，由用户自行指定，需精确到文件名及格式，例如：<code>/dir1/dir2/dir3/audio.aac</code></td>
</tr>
<tr><td>quality</td>
<td><p>录音音质：</p>
<ul>
<li>AUDIO_RECORDING_QUALITY_LOW = 0: 低音质。录制 10 分钟的文件大小为 1.2 M 左右</li>
<li>AUDIO_RECORDING_QUALITY_MEDIUM = 1: 中音质。录制 10 分钟的文件大小为 2 M 左右</li>
<li>AUDIO_RECORDING_QUALITY_HIGH = 2: 高音质。录制 10 分钟的文件大小为 3.75 M 左右</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用失败</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 停止客户端录音 (stopAudioRecording)

```
int stopAudioRecording();
```

停止录音。该接口需要在 `leaveChannel()` 之前调用，不然会在调用 `leaveChannel()` 时自动停止。

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



#### 调节录音信号音量 (adjustRecordingSignalVolume)

```
int adjustRecordingSignalVolume(int volume);
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
<tr><td>volume</td>
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



#### 调节播放信号音量 (adjustPlaybackSignalVolume)

```
int adjustPlaybackSignalVolume(int volume);
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
<tr><td>volume</td>
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


<a id = "irtcengineeventhandler2"></a>
### 主回调事件 (IRtcEngineEventHandler)

agora::IRtcEngineEventHandler 接口类用于 SDK 向应用程序发送回调事件通知，应用程序通过继承该接口类的方法获取 SDK 的事件通知。接口类的所有方法都有缺省（空）实现，应用程序可以根据需要只继承关心的事件。

#### 加入频道回调 (onJoinChannelSuccess)

```
virtual void onJoinChannelSuccess(const char* channel, uid_t uid, int elapsed);
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
<tr><td>channel</td>
<td>频道名</td>
</tr>
<tr><td>uid</td>
<td>用户 ID。如果 joinChannel 中指定了 uid，则此处返回该 ID；否则使用 Agora 服务器自动分配的 ID</td>
</tr>
<tr><td>elapsed</td>
<td>从 joinChannel 开始到该事件产生的延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 重新加入频道回调 (onRejoinChannelSuccess)

```
virtual void onRejoinChannelSuccess(const char* channel, uid_t uid, int elapsed);
```

有时候由于网络原因，客户端可能会和服务器失去连接，SDK会进行自动重连，自动重连成功后触发此回调方法。

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
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>elapsed</td>
<td>从 joinChannel 开始到该事件产生的延迟（毫秒）</td>
</tr>
</tbody>
</table>



#### 发生警告回调 (onWarning)

```
virtual void onWarning(int warn, const char* msg);
```

该回调方法表示SDK运行时出现了（网络或媒体相关的）警告。通常情况下，SDK上报的警告信息应用程序可以忽略，SDK会自动恢复。比如和服务器失去连接时，SDK可能会上报ERR_OPEN_CHANNEL_TIMEOUT警告，同时自动尝试重连。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>warn</td>
<td>警告代码</td>
</tr>
<tr><td>msg</td>
<td>警告描述，详见 <a href="../../cn/Interactive%20Gaming/the_error_native.html.md"><span>错误代码和警告代码</span></a></td>
</tr>
</tbody>
</table>

#### 发生错误回调 (onError)

```
virtual void onError(int err, const char* msg);
```

该回调方法表示 SDK 运行时出现了（网络或媒体相关的）错误。通常情况下，SDK上报的错误意味着SDK无法自动恢复，需要应用程序干预或提示用户。 比如启动通话失败时，SDK 会上报 ERR_START_CALL 错误。应用程序可以提示用户启动通话失败，并调用 leaveChannel 退出频道。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>err</td>
<td>错误代码</td>
</tr>
<tr><td>msg</td>
<td>错误描述，详见 <a href="../../cn/Interactive%20Gaming/the_error_native.html.md"><span>错误代码和警告代码</span></a></td>
</tr>
</tbody>
</table>

#### API 已执行回调 (onApiCallExecuted)

```
virtual void onApiCallExecuted(int err, const char* api, const char* result);
```

当 SDK 执行相应的 API 后，会触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>error</td>
<td>错误码。如果方法调用失败，会返回错误码；如果返回 0，则表示方法调用成功</td>
</tr>
<tr><td>api</td>
<td>SDK 所调用的 API</td>
</tr>
<tr><td>result</td>
<td>SDK 调用 API 的调用结果</td>
</tr>
</tbody>
</table>

#### 麦克风状态已改变回调 (onMicrophoneEnabled)

```
virtual void onMicrophoneEnabled(bool enabled)
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
<tr><td>enabled</td>
<td>
<ul>
<li>true：麦克风已启用</li>
<li>false：麦克风已禁用</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 声音质量回调 (onAudioQuality)

```
virtual void onAudioQuality(uid_t uid, int quality, unsigned short delay, unsigned short lost);
```

在通话中，该回调方法每两秒触发一次，报告当前通话的（嘴到耳）音频质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>quality</td>
<td><p>语音质量：</p>
<ul>
<li>QUALITY_UNKNOWN(0)：网络质量未知</li>
<li>QUALITY_EXCELLENT(1)：网络质量极好</li>
<li>QUALITY_GOOD(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>QUALITY_POOR(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>QUALITY_BAD(4)：勉强能沟通但不顺畅</li>
<li>QUALITY_VBAD(5)：网络质量非常差，基本不能沟通</li>
<li>QUALITY_DOWN(6)：完全无法沟通</li>
</ul>
</td>
</tr>
<tr><td>delay</td>
<td>延迟，单位为毫秒</td>
</tr>
<tr><td>lost</td>
<td>丢包率，单位为百分比</td>
</tr>
</tbody>
</table>



#### 说话声音音量提示回调 (onAudioVolumeIndication)

```
virtual void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume);
```

该回调提示频道内谁在说话以及说话者的音量。默认禁用。可以通过 `enableAudioVolumeIndication` 方法开启；开启后，无论频道内是否有人说话，都会按方法中设置的时间间隔返回提示音量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>speakers</td>
<td><p>说话者（数组）。每个 speaker():</p>
<div><ul>
<li>uid: 说话者的用户 ID。如果返回的 uid 为 0，则默认为本地用户</li>
<li>volume: 说话者的音量（0~255）</li>
</ul>
</div>
</td>
</tr>
<tr><td>speakerNumber</td>
<td>Speakers 数组的大小</td>
</tr>
<tr><td>totalVolume</td>
<td>（混音后的）总音量（0~255）</td>
</tr>
</tbody>
</table>



在返回的 speakers 数组中：

-   如果返回的 uid 为 0，即当频道内的说话者为本地用户时，说话者的音量 `volume` 为 `totalVolume`，即频道内的总音量。

-   如果返回的 uid 不是 0，且 volume 为 0，则默认该 uid 对应的远端用户没有说话。

-   如果有 uid 出现在上次返回的数组中，但不在本次返回的数组中，则默认该 uid 对应的远端用户没有说话。


#### 离开频道回调 (onLeaveChannel)

```
virtual void onLeaveChannel(const RtcStats& stat);
```

应用程序调用 leaveChannel()方法时，SDK提示应用程序离开频道成功。在该回调方法中，应用程序可以得到此次通话的总通话时长、SDK收发数据的流量等信息。

**RtcStats 定义**

```
struct RtcStats
{
    unsigned int duration;
    unsigned int txBytes;
    unsigned int rxBytes;
    unsigned short txKBitRate;
    unsigned short rxKBitRate;

    unsigned short rxAudioKBitRate;
    unsigned short txAudioKBitRate;

    unsigned short rxVideoKBitRate;
    unsigned short txVideoKBitRate;
    unsigned int userCount;
    double cpuAppUsage;
    double cpuTotalUsage;
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
<tr><td>stat</td>
<td><p>通话相关的统计信息。</p>
<div><ul>
<li>duration: 通话时长（秒）</li>
<li>txBytes: 发送字节数（bytes）</li>
<li>rxBytes: 接收字节数（bytes）</li>
<li>txKBitRate: 发送码率（kbps）</li>
<li>rxKBitRate: 接收码率（kbps）</li>
<li>rxAudioKBitRate: 音频接收码率 (kbps)</li>
<li>txAudioKBitRate: 音频发送频率 (kbps)</li>
<li>rxVideoKBitRate: 视频接收码率 (kbps)</li>
<li>txVideoKBitRate: 视频发送码率 (kbps)</li>
<li>uerCount: 用户离开频道时频道内的瞬时人数</li>
<li>cpuAppUsage: 当前应用程序的 CPU 使用率 (%)</li>
<li>cpuTotalUsage: 当前系统的 CPU 使用率 (%)</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### 其他用户加入当前频道回调 (onUserJoined)

```
virtual void onUserJoined(uid_t uid, int elapsed);
```

该回调提示有主播加入了频道，并返回该主播的 ID。如果在加入之前，已经有主播在频道中了，新加入的用户也会收到已有主播加入频道的回调。Agora 建议连麦主播不超过 17 人。

> 直播场景下:
> -   主播间能相互收到新主播加入频道的回调，并能获得该主播的 uid
> -   观众也能收到新主播加入频道的回调，并能获得该主播的 uid
> -   当 Web 端加入直播频道时，只要 Web 端有推流，SDK 会默认该 Web 端为主播，并触发该回调


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>主播 ID</td>
</tr>
<tr><td>elapsed</td>
<td>joinChannel 开始到该回调触发的延迟（毫秒)</td>
</tr>
</tbody>
</table>



#### 其他用户离开当前频道回调 (onUserOffline)

```
virtual void onUserOffline(uid_t uid, USER_OFFLINE_REASON_TYPE reason);
```

提示有主播离开了频道（或掉线）。SDK 判断用户离开频道（或掉线）的依据是超时：在一定时间内（15秒）没有收到对方的任何数据包，判定为对方掉线。 在网络较差的情况下，可能会有误报。建议可靠的掉线检测应该由信令来做。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>主播 ID</td>
</tr>
<tr><td>reason</td>
<td><p>离线原因:</p>
<ul>
<li>USER_OFFLINE_QUIT(0): 用户主动离开</li>
<li>USER_OFFLINE_DROPPED(1): 因过长时间收不到对方数据包，超时掉线。注意：由于 SDK 使用的是不可靠通道，也有可能对方主动离开本方没收到对方离开消息而误判为超时掉线</li>
<li>USER_OFFLINE_BECOME_AUDIENCE(2): 用户身份从主播切换为观众时触发</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



#### 当前通话统计回调 (onRtcStats)

```
virtual void onRtcStats(const RtcStats& stats);
```

SDK定期向应用程序报告当前通话的统计信息，每两秒触发一次。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>stat</td>
<td>详见 <a href="#onleavechannel-live-windows"><span>离开频道回调 (onLeaveChannel)</span></a> 中的描述</td>
</tr>
</tbody>
</table>



#### 收到远端音频回调 (onFirstRemoteAudioFrame)

```
virtual void onFirstRemoteAudioFrame(uid_t uid, int elapsed)
```

当接收到远端音频第一帧时，触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 UID，表示收到的是哪个用户的音频流</td>
</tr>
<tr><td>elapsed</td>
<td>加入频道开始到该回调触发的延迟（毫秒）</td>
</tr>
</tbody>
</table>


#### 音频设备变化回调 (onAudioDeviceStateChanged)

```
virtual void onAudioDeviceStateChanged(const char* deviceId, int deviceType, int deviceState);
```

提示系统音频设备状态发生改变，比如耳机被拔出。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>deviceId</td>
<td>设备 ID</td>
</tr>
<tr><td>deviceType</td>
<td><p>设备类型定义如下：</p>
<div><ul>
<li>UNKNOWN_DEVICE(-1)：设备类型未知</li>
<li>AUDIO_PLAYOUT_DEVICE(0)：播放设备</li>
<li>AUDIO_RECORDING_DEVICE(1)：录制设备</li>
</ul>
</div>
</td>
</tr>
<tr><td>deviceState</td>
<td><p>设备状态定义如下：</p>
<div><ul>
<li>DEVICE_STATE_ACTIVE(1)：设备被启用</li>
<li>DEVICE_STATE_DISABLED(2)：设备被禁用</li>
<li>DEVICE_STATE_NOT_PRESENT(4)：无法找到设备</li>
<li>DEVICE_STATE_UNPLUGGED(8)：设备被拔开</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


#### 网络质量报告回调 (onLastmileQuality)

```
virtual void onLastmileQuality(int quality);
```

报告本地用户的网络质量。当你调用 enableLastmileTest 之后，该回调函数每 2 秒触发一次。 不在通话中时，如果启用了网络质量测试功能（enableLastmileTest），该回调方法会被不定期触发，向应用程序上报当前网络连接质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>quality</td>
<td><p>网络质量：</p>
<div><ul>
<li>QUALITY_UNKNOWN(0)：网络质量未知</li>
<li>QUALITY_EXCELLENT(1)：网络质量极好</li>
<li>QUALITY_GOOD(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>QUALITY_POOR(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>QUALITY_BAD(4)：勉强能沟通但不顺畅</li>
<li>QUALITY_VBAD(5)：网络质量非常差，基本不能沟通</li>
<li>QUALITY_DOWN(6)：完全无法沟通</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### 频道内网络质量报告回调 (onNetworkQuality)

```
virtual void onNetworkQuality(uid_t uid, int txQuality, int rxQuality);
```

该回调每 2 秒触发，向APP报告频道内所有用户当前的上行、下行网络质量。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID。表示该回调报告的是持有该 ID 的用户的网络质量。当 uid 为 0 时，返回的是本地用户的网络质量</td>
</tr>
<tr><td>txQuality</td>
<td><p>该用户的上行网络质量：</p>
<ul>
<li>QUALITY_UNKNOWN(0)：网络质量未知</li>
<li>QUALITY_EXCELLENT(1)：网络质量极好</li>
<li>QUALITY_GOOD(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>QUALITY_POOR(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>QUALITY_BAD(4)：勉强能沟通但不顺畅</li>
<li>QUALITY_VBAD(5)：网络质量非常差，基本不能沟通</li>
<li>QUALITY_DOWN(6)：完全无法沟通</li>
</ul>
</td>
</tr>
<tr><td>rxQuality</td>
<td><p>该用户的下行网络质量：</p>
<ul>
<li>QUALITY_UNKNOWN(0)：网络质量未知</li>
<li>QUALITY_EXCELLENT(1)：网络质量极好</li>
<li>QUALITY_GOOD(2)：用户主观感觉和 excellent 差不多，但码率可能略低于 excellent</li>
<li>QUALITY_POOR(3)：用户主观感受有瑕疵但不影响沟通</li>
<li>QUALITY_BAD(4)：勉强能沟通但不顺畅</li>
<li>QUALITY_VBAD(5)：网络质量非常差，基本不能沟通</li>
<li>QUALITY_DOWN(6)：完全无法沟通</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### 用户静音回调 (onUserMuteAudio)

```
virtual void onUserMuteAudio(uid_t uid, bool muted);
```

提示有其他用户已将其音频流静音（或取消静音）。


> 现阶段，当频道内主播数超过 20 人，该回调会失效。后续会改进。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>True: 该用户已将音频静音</li>
<li>False: 该用户取消了音频静音</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### 网络中断回调 (onConnectionInterrupted)

```
virtual void onConnectionInterrupted();
```

该回调方法表示 SDK 和服务器失去了网络连接。

与 onConnectionLost 回调的区别是: onConnectionInterrupted 回调在 SDK 刚失去和服务器连接时触发，onConnectionLost 在失去连接且尝试自动重连失败后才触发。 失去连接后，除非 APP 主动调用 leaveChannel() ，不然 SDK 会一直自动重连。

#### 连接丢失回调 (onConnectionLost)

```
virtual void onConnectionLost();
```

在 SDK 和服务器失去了网络连接后，会触发 onConnectionInterrupted 回调，并自动重连。如果重连一段时间（默认 10 秒）后仍未连上，会触发该回调。 如果 SDK 在调用 joinChannel 后 10 秒内没有成功加入频道，也会触发该回调。 该回调触发后，SDK 仍然会尝试重连，重连成功后会触发 onRejoinChannelSuccess 回调。

#### 连接已被禁止回调 (onConnectionBanned)

```
virtual void onConnectionBanned();
```

当你被服务端禁掉连接的权限时，会触发该回调。

#### 接收到对方数据流消息的回调 (onStreamMessage)

```
virtual void onStreamMessage(uid_t uid, int streamId, const char* data, size_t length);
```

该回调提示本地用户在 5 秒内收到了对方发送的数据包。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>streamId</td>
<td>数据流 ID</td>
</tr>
<tr><td>data</td>
<td>接收到的数据</td>
</tr>
<tr><td>length</td>
<td>数据长度</td>
</tr>
</tbody>
</table>



#### 接收对方数据流消息错误的回调 (onStreamMessageError)

```
virtual void onStreamMessageError(uid_t uid, int streamId, int code, int missed, int cached);
```

该回调提示了本地用户没有在 5 秒内收到对方发送的数据包。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>用户 ID</td>
</tr>
<tr><td>streamId</td>
<td>数据流 ID</td>
</tr>
<tr><td>code</td>
<td><ul>
<li>ERR_OK = 0, 没有错误</li>
<li>ERR_NOT_IN_CHANNEL=113，用户不在频道内</li>
<li>ERR_BITRATE_LIMIT=115, 码率受限</li>
</ul>
</td>
</tr>
<tr><td>missed</td>
<td>丢失的消息数量</td>
</tr>
<tr><td>cached</td>
<td>数据流中断时，后面缓存的消息数量</td>
</tr>
</tbody>
</table>



#### Token 过期回调 (onRequestToken)

```
virtual void onRequestToken();
```

在调用 joinChannel 时如果指定了 Token，由于 Token 具有一定的时效，在通话过程中 SDK 可能由于网络原因和服务器失去连接，重连时可能需要新的 Token。 该回调通知 APP 需要生成新的 token，并需调用 renewToken() 为 SDK 指定新的 Token 。

之前的处理方法是在 onError() 回调报告 ERR_TOKEN_EXPIRED(109)、ERR_INVALID_TOKEN(110) 时，APP 需要生成新的 Token。 在新版本中，原来的处理仍然有效，但建议把相关逻辑放进该回调里。

#### 伴奏播放已结束回调 (onAudioMixingFinished)

```
virtual void onAudioMixingFinished()
```

当调用 startAudioMixing 播放伴奏音乐结束后，会触发该回调。如果调用 startAudioMixing 失败，会在 onError 回调里，返回错误码 WARN_AUDIO_MIXING_OPEN_ERROR 。

#### 音效播放已结束回调 (onAudioEffectFinished)

```
virtual void onAudioEffectFinished(int soundId)
```

当指定的音效播放结束后，会触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>名称</td>
<td>描述</td>
</tr>
<tr><td>soundId</td>
<td>指定音效的 ID。每个音效均有唯一的 ID</td>
</tr>
</tbody>
</table>



#### 监测到活跃用户回调 (onActiveSpeaker)

```
virtual void onActiveSpeaker(uid_t uid);
```

如果用户开启了 `enableAudioVolumeIndication` 功能，则当音量检测模块监测到频道内有新的活跃用户说话时，会通过本回调返回该用户的 uid。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>uid</td>
<td>当前时间段声音最大的用户的 uid。如果返回的 uid 为 0，则默认为本地用户</td>
</tr>
</tbody>
</table>


> -   你需要开启 `enableAudioVolumeIndication` 方法才能收到该回调。
> -   uid 返回的是当前时间段内声音最大的用户 uid，而不是瞬时声音最大的用户 uid。


#### 上下麦回调 (onClientRoleChanged)

```
virtual void onClientRoleChanged(CLIENT_ROLE_TYPE oldRole, CLIENT_ROLE_TYPE newRole);
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
<tr><td>oldRole</td>
<td>切换前的角色</td>
</tr>
<tr><td>newRole</td>
<td>切换后的角色</td>
</tr>
</tbody>
</table>



#### 音频设备声音变更回调 (onAudioDeviceVolumeChanged)

```
virtual void onAudioDeviceVolumeChanged(MEDIA_DEVICE_TYPE deviceType, int volume, bool muted);
```

当放音设备、录音设备以及应用程序音量发生变化时将触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>deviceType</td>
<td><p>设备类型:</p>
<div><ul>
<li>AUDIO_PLAYOUT_DEVICE: 放音设备</li>
<li>AUDIO_RECORDING_DEVICE: 录音设备</li>
<li>AUDIO_APPLICATION_PLAYOUT_DEVICE: 应用程序</li>
</ul>
</div>
</td>
</tr>
<tr><td>volume</td>
<td>音量值，范围 0-255</td>
</tr>
<tr><td>muted</td>
<td>静音标识，true/false</td>
</tr>
</tbody>
</table>


### 错误代码和警告代码 - Agora Native SDK

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_native.md)。
