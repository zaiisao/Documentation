
---
title: 更改音视频流传输状态
description: v1.1.1
platform: Android
updatedAt: Thu Aug 15 2019 03:55:18 GMT+0800 (CST)
---
# 更改音视频流传输状态
## 功能描述
用户成功加入频道并开始音视频流传输后，可以暂停向指定频道或所有频道发送流，也可以暂停接收指定频道或所有频道的流，从而灵活调整音视频流传输状态。

## 实现方法
### 发送端

**暂停/恢复发送本地音视频流**

可调用以下方法暂停或恢复发送本地音视频流：

- `muteLocalAudio`：暂停发送本地**音频**流。
- `muteLocalVideo`：暂停发送本地**视频**流。

>你既可以通过传入频道名暂停或恢复向指定频道发送数据，也可以将 `channelName` 设置为 null 暂停或恢复向所有频道发送数据。

示例代码如下，仅供参考：
~~~java
AgoraRtcService.muteLocalAudio("my_first_channel", true); // Stop sending audio
AgoraRtcService.muteLocalVideo("my_first_channel", true); // Stop sending video
AgoraRtcService.muteLocalAudio("my_first_channel", false); // Start sending audio
AgoraRtcService.muteLocalVideo("my_first_channel", false); // Start sending video
~~~

### 接收端

**提示频道内远端用户音视频流发送状态**

发送端调用 `muteLocalAudio` 或 `muteLocalVideo` 方法更改本地音视频流传输状态后， SDK 会相应地触发以下回调，提示接收端频道内远端用户是否暂停发送音视频流：

* `onUserMuteAudio`：提示本地用户频道内远端用户是否暂停发送**音频**流。
* `onUserMuteVideo`：提示本地用户频道内远端用户是否暂停发送**视频**流。

示例代码如下，仅供参考：
~~~java
private final AgoraRtcEvents event_listener = new AgoraRtcEvents() {
	@Override
	public void onUserMuteAudio(String channel, int uid, boolean is_muted) {
		Log.i("Foo", "onUserMuteAudio");
	}

	@Override
	public void onUserMuteVideo(String channel, int uid, boolean is_muted) {
		Log.i("Foo", "onUserMuteVideo");
	}

	// ...
}
~~~

**暂停/恢复接收远端音视频流**
可调用以下方法暂停或恢复接收指定频道内指定用户的音视频流：

* `muteRemoteAudio`：暂停接收远端**音频**流。
* `muteRemoteVideo`：暂停接收远端**视频**流。

>你既可以通过填写频道名暂停接收指定频道的数据，也可以将 `channelName` 设置为 null 暂停接收所有频道的数据。

~~~java
final int remote_uid = 666;

// Stop receiving audio from a remote user
AgoraRtcService.muteRemoteAudio("my_first_channel", remote_uid, true);

// Stop receiving video from a remote user
AgoraRtcService.muteRemoteVideo("my_first_channel", remote_uid, true);

// Start receiving audio from a remote user
AgoraRtcService.muteRemoteAudio("my_first_channel", remote_uid, false);

// Start receiving video from a remote user
AgoraRtcService.muteRemoteVideo("my_first_channel", remote_uid, false);
~~~

## API 参考
- [`muteLocalAudio`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_java/classio_1_1agora_1_1rtc_1_1_agora_rtc_service.html#a9725096cd62d3f37c6d89318483a4605)
- [`muteLocalVideo`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_java/classio_1_1agora_1_1rtc_1_1_agora_rtc_service.html#a4804522ecdc4eb7c8758f9f8fff7ccc9)
- [`muteRemoteAudio`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_java/classio_1_1agora_1_1rtc_1_1_agora_rtc_service.html#a287f5728e3423461471efc0efca64957)
- [`muteRemoteVideo`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_java/classio_1_1agora_1_1rtc_1_1_agora_rtc_service.html#a99c6a45f812dd83d465c475bc2730d51)
- [`onUserMuteAudio`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_java/interfaceio_1_1agora_1_1rtc_1_1_agora_rtc_events.html#ae96b0fec26509af3c1ef11ce6be750e4)
- [`onUserMuteVideo`](https://docs.agora.io/cn/RTSA/RTSA/API%20Reference/rtsa_java/interfaceio_1_1agora_1_1rtc_1_1_agora_rtc_events.html#a25bac59ed64457d701ebabd279727b8c)
