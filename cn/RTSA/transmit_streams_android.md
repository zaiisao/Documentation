
---
title: 实现码流传输
description: v1.1.1
platform: Android
updatedAt: Tue Oct 29 2019 03:28:37 GMT+0800 (CST)
---
# 实现码流传输
## 初始化和监听事件
首先，调用 `AgoraRtcService.init` 方法初始化 RTSA serive。

在该方法中，你需要：

* 设置 `appId` 参数，传入你获取到的 App ID。了解[获取 App ID](../../cn/RTSA/demo_guide_android.md)。
* 设置 `uid` 参数传入用户 ID。为 32 位整型，取值范围为 1 到 2<sup>32</sup> - 1。0 是无效的 `uid`，如果将 `uid` 设为 0，系统将自动分配一个 UID。
* 设置 `events` 参数，设置事件回调 AgoraRtcEvents，用以通知 SDK 在运行过程中发生的事件。
* 设置 `sdkLogDir` 参数，用于存放 SDK 日志。如填写 `null`，则使用默认目录 /sdcard/。

示例代码如下，仅供参考：

~~~java
private final AgoraRtcEvents event_listener = new AgoraRtcEvents() {
	@Override
	public void onJoinChannelSuccess(String channel, int elapsed) {}

	@Override
	public void onConnectionLost(String channel) {}

	@Override
	public void onRejoinChannelSuccess(String channel, int elapsed) {}

	@Override
	public void onWarning(String channel, int war, String msg) {}

	@Override
	public void onError(String channel, int err, String msg) {}

	@Override
	public void onUserJoined(String channel, int uid, int elapsed) {}

	@Override
	public void onUserOffline(String channel, int uid, int elapsed) {}

	@Override
	public void onUserMuteAudio(String channel, int uid, boolean is_muted) {}

	@Override
	public void onUserMuteVideo(String channel, int uid, boolean is_muted) {}

	@Override
	public void onKeyFrameGenReq(String channel, int remote_uid, byte stream_id) {}

	@Override
	public void onAudioData(String channel, int uid, char sent_ts, byte codec, byte[] bytes) {}

	@Override
	public void onVideoData(String channel, int uid, char sent_ts, byte codec, byte stream_id, boolean is_key_frame, byte[] bytes) {}

	@Override
	public void onDecBitrate(String s, int i) {}

	@Override
	public void onIncBitrate(String s, int i) {}
};

private void initSdk() {
	if (isAllPermissionsGranted()) {
		AgoraRtcService.init(AppIdAndCert.APP_ID, 0, event_listener, null);
	}
}
~~~

## 加入频道
调用 `AgoraRtcService.joinChannel ` 方法加入频道。

我们把一个客户端比作一栋大楼的话，频道就好比大楼里面的一个房间。频道是由你调用 API 时在客户端创建，第一个用户加入时自动创建频道，最后一个用户离开时频道会自动销毁，无需维护。

用户与频道的关系如下：
* 一个频道内的用户可以互相传输数据流。
* 一个用户可以同时加入多个频道。该用户加入的所有频道都能接收到他发送的音视频数据流。

加入频道时，你需要：
- 设置 `channelName` 参数。这是频道名，是频道的唯一标识。长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）：
	- 26 个小写英文字母 a-z
	- 26 个大写英文字母 A-Z
	- 10 个数字 0-9
	- 空格
	- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", "{", "}", "|", "~", ","
- 设置 `tokenBytes` 参数，传入能标识用户角色和权限的 Token。这就好比加入房间所需要的钥匙或门卡。Agora 提供两种鉴权机制：App ID 和 token。详见[校验用户权限](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms)。如果安全要求不高，使用 App ID 作为鉴权机制，可将 `tokenBytes` 设为 null。

成功加入频道后，SDK 会触发 `AgoraRtcEvents.onJoinChannelSuccess` 回调。

示例代码如下，仅供参考：

~~~java
private void joinChannel() {
	AgoraRtcService.joinChannel("my_first_channel", null);
}
~~~

## 发送/获取数据流
成功加入频道后，你可以：

* 监听 `AgoraRtcEvents.onAudioData` 方法接收所有你已加入的频道内的音频数据流。
* 监听 `AgoraRtcEvents.onVideoData` 方法接收所有你已加入的频道内视频数据流。
* 调用 `AgoraRtcService.sendAudioData` 方法向指定或所有你已加入的频道发送音频数据流。
* 调用 `AgoraRtcService.sendVideoData` 方法向指定或所有你已加入的频道发送视频数据流。

示例代码如下，仅供参考：

~~~java
private final AgoraRtcEvents event_listener = new AgoraRtcEvents() {
   @Override
   public void onAudioData(String channel, int uid, char sent_ts, byte codec, byte[] bytes) {
      Log.i(Config.TAG, "onAudioData");
   }

   @Override
   public void onVideoData(String channel,
                           int uid,
                           char sent_ts,
                           byte codec,
                           byte stream_id,
                           boolean is_key_frame,
                           byte[] bytes) {
       Log.i(Config.TAG, "onVideoData");
    }
};

AgoraRtcService.sendVideoData(Config.CHANNEL_NAME,
                              Config.VIDEO_CODEC,
                              Config.VIDEO_STREAM_ID,
                              should_be_key,
                              videoFrame);

AgoraRtcService.sendAudioData(Config.CHANNEL_NAME,
                              Config.AUDIO_CODEC,
                              audioFrame);
~~~

## 离开频道
调用 `AgoraRtcService.leaveChannel`  方法离开指定频道，结束在该频道的数据传输。

示例代码如下，仅供参考：

~~~java
private void leaveChannel() {
	AgoraRtcService.leaveChannel("my_first_channel");
}
~~~
