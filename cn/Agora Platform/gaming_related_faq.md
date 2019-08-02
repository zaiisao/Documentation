
---
title: 游戏相关
description: 
platform: All Platforms
updatedAt: Fri Aug 02 2019 09:51:58 GMT+0800 (CST)
---
# 游戏相关
## 游戏音效问题

### 进入指挥模式语音频道之前，通过系统的静音键将游戏的音效静音了，但是进入频道之后，为什么游戏的音效就被自动打开了？

通话过程中，通话音量都无法降为 0。 所有类似软件都有相同的现象，包括微信等等。进入通话后，音效播放的也是从通话音量出来的。

### 打开游戏音效，设置一定的系统音量不改变，进入频道后听到的游戏音效的音量，明显比进入频道之前听到的游戏音效的音量高。

媒体音量 + 通话音量分别属于 2 个不同的、独立的系统，一个设置不会影响到另外一个。 进入通话后，音效播放的也是从通话音量出来的。退出通话后，走的媒体音量。 例如，王者荣耀的连麦、断麦也是这样。

### iOS 上开了游戏、游戏语音后，启动语音并立马退出语音房间，然后游戏背景音就变小了。 Android上 是 OK 的。

退出语音房间时，把 AudioSession Category 和 Mode 设置回进房间之前的设置。

在 join 之前，使用 unity 播放游戏背景音乐，对应的 audio session category 为 `AVAudioSessionCategoryAmbient`，mode 为 `AVAudioSessionModeDefault`，对应一套 volume（媒体音量）。

在 join之后，agora sdk 将 audio session 的 category 更改为 `AVAudioSessionCategoryPlayAndRecord`，mode 为 `AVAudioSessionModeVoiceChat`，对应另一套 volume（通话音量）。

### 接入 SDK 后游戏音乐音效与语音相互冲突。

必然有的问题。 加频道的一个过程，会断一下。可以设置为只走媒体音量，但是有回声等问题。

### 麻将棋牌游戏中，4 个人的背景音乐会被相互串到通话里面 ？

如果是客户自己播放的音频，不是经过声网的 API 播放的背景音乐，是会存在这个问题的。开通话的时候，直接关掉音乐和音效,就没有了。

### 在 app 里播放背景音乐， 然后开加入频道， 这个时候背景音乐音量变化， 当调用了 leaveChannel 之后 背景音乐也没有了。

在 `joinChannel` 之前，加入：`setParameters("{\"che.audio.keep.audiosession\":true}")`;

### 在播放背景音乐的情况下，join channel 后，听到的背景音乐声音会变小。

在 `join channel` 之后背景游戏音乐播放仍留在 earpiece 没有切到正确设备（扬声器），造成声音听起来较小。app 调用 `setEnablespeaker` 确保背景音乐和通话音的 output route 一致。

### onAudioVolumeIndication 获得的音量是 0~255, 有没有什么合适的经验阈值界定说话和没说话?

40～50。客户在此基础上调整一下，不同的人对说话、没说话的定义不一样。

### AMG SDK中 媒体音量系统，在连麦后进入通话音量系统时，原来媒体音量系统播放的背景音效不能调节 ？

在 `joinChannel` 前设置一下接口。

#### iOS 平台

mRtcEngine.setParameters("{\"che.audio.use.remoteio\":true}");

#### Android 平台

mRtcEngine.setParameters("{\"che.audio.stream_type\":3}");
mRtcEngine.setParameters("{\"che.audio.audioMode\":0}");

#### Android 和 iOS 通用的设置

mRtcEngine.setParameters("{\"che.audio.enable.aec\":true}");
mRtcEngine.setParameters("{\"che.audio.enable.agc\":true}");
mRtcEngine.setParameters("{\"che.audio.enable.ns\":true}");

### 蓝牙耳机为什么没有立体声 ？

因为蓝牙要开通话模式，即 SCO，才能实现双向（播放、录音）。 蓝牙在 SCO 模式下只能单声道播放。蓝牙在 A2DP 模式下可能是双声道的。如果蓝牙音箱只支持 A2DP 不支持 SCO 的话，是无法用来进行语音通话的。

* A2DP：是一种单向的高品质音频数据传输链路，通常用于播放立体声音乐。
* SCO： 则是一种双向的音频数据的传输链路，该链路只支持 8K 及 16K 单声道的音频数据，只能用于普通语音的传输。

## 设备兼容性问题

### Android 与 iPhone 耳机可以兼容吗？

不能。iPhone 耳机接口的左右声道、mic 的排列跟 Android 手机是不一样的。


