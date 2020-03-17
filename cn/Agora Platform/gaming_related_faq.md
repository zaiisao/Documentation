
---
title: 游戏相关
description: 
platform: All Platforms
updatedAt: Wed Aug 07 2019 08:58:35 GMT+0800 (CST)

# 游戏相关
## 游戏音效问题

### 频道之前，游戏音效是静音状态，但是指挥模式下进入语音频道之后，为什么游戏音效自动打开？

进入通话后，音效播放的音量走通话音量，而通话音量无法设置为 0。

### 打开游戏音效，设置一定的系统音量不改变，进入频道后听到的游戏音效的音量，明显比进入频道之前听到的游戏音效的音量高。

媒体音量和通话音量分别属于 2 个不同的、独立的系统，一个设置不会影响到另外一个。 进入通话后，音效的播放也是从通话音量出来。退出通话后，则走媒体音量。

### iOS 上开了游戏语音后，启动语音并立马退出语音房间，游戏背景音就变小了。 Android 上无此问题。

在 joinChannel 之前，设备使用 Unity 播放游戏背景音乐，对应的 AudioSessioCategory 为 `AVAudioSessionCategoryAmbient`，mode 为 `AVAudioSessionModeDefault`，对应的是媒体音量。

在 joinChannel 之后，SDK 将 AudioSessioCategory 更改为 `AVAudioSessionCategoryPlayAndRecord`，mode 为 `AVAudioSessionModeVoiceChat`，对应的是通话音量。

如果不想在进出房间时发生音量变化，我们建议在退出语音房间时，把 AudioSessionCategory 的 mode 设置回进房间之前的设置。

AMG SDK v2.2 及之后的版本，以及 2019 年之后发布的 SDK，会自动完成该操作。

AMG2.2 SDK 以及2019年以后的SDK，已经会负责来完成这个操作了。

### 接入 SDK 后游戏音乐音效与语音相互冲突。

在加入频道的过程中，会发生音频断一下的问题。可以通过设置只走媒体音量解决，但有可能会导致回声等问题。我们建议充分测试后进行实现。

### 麻将棋牌游戏中，4 个人的背景音乐会被相互串到通话里面 ？

如果是用户自己播放的音频，不是通过调用声网 API 播放的背景音乐，是会存在这个问题的。由于游戏的声音不通过 SDK 播放，播放出来的声音会被录进去。

我们建议在通话的时候，直接关掉音乐和音效，或者在通话连麦时，降低游戏背景音量，以避免该问题。

### 在 App 里播放背景音乐， 然后加入频道， 背景音乐音量变化；调用 leaveChannel 之后，背景音乐也没有了。

在 `joinChannel` 之前，调用：`setParameters("{\"che.audio.keep.audiosession\":true}")`;

### 播放背景音效的情况下，通信模式加入频道或者直播模式连麦后，听到的背景音效声音会变小。

你可以选择如下一种方法解决该问题：

* 确保声音都走外放。你可以调用 `setEnableSpeakerohone` 方法设置语音路由为外放。
* 在语音连麦过程中，手机系统会开启回声消除以保证人声体验，因此会压低声音，也会压低背景音效，因此 Agora 建议调用 startAudioMixing 或 playEffect 方法来播放音效文件。

### onAudioVolumeIndication 获得的音量是 0~255, 有没有什么合适的经验阈值界定说话和没说话?

40～50。考虑到不同的人对说话、没说话的定义不一，我们建议你基于该阈值稍作调整。

### AMG SDK 中媒体音量系统，在连麦后进入通话音量系统时，需要静音？

在 `joinChannel` 前设置如下接口。

#### iOS 平台

mRtcEngine.setParameters("{\"che.audio.use.remoteio\":true}");

#### Android 平台

mRtcEngine.setParameters("{\"che.audio.stream_type\":3}");
mRtcEngine.setParameters("{\"che.audio.audioMode\":0}");

#### Android 和 iOS 通用的设置

mRtcEngine.setParameters("{\"che.audio.enable.aec\":true}");
mRtcEngine.setParameters("{\"che.audio.enable.agc\":true}");
mRtcEngine.setParameters("{\"che.audio.enable.ns\":true}");

上述设置可能会造成回声问题，我们建议充分测试后进行实现。


### 蓝牙耳机为什么没有立体声 ？

因为蓝牙要开通话模式，即 SCO，才能实现播放和录音双向功能。 而在 SCO 模式下，蓝牙只能单声道播放；在 A2DP 模式下蓝牙可以走双声道，但如果蓝牙音箱只支持 A2DP 不支持 SCO，又无法进行语音通话。

* A2DP：是一种单向的高品质音频数据传输链路，通常用于播放立体声音乐。
* SCO： 则是一种双向的音频数据的传输链路，该链路只支持 8K 及 16K 单声道的音频数据，只能用于普通语音的传输。

## 设备兼容性问题

### Android 与 iPhone 耳机可以兼容吗？

不能。iPhone 耳机接口的左右声道、麦克风的排列跟 Android 手机是不一样的。

