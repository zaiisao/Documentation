
---
title: 实现码流传输
description: v1.1.1
platform: iOS,macOS
updatedAt: Thu Aug 15 2019 03:55:02 GMT+0800 (CST)
---
# 实现码流传输
## 初始化和监听事件
首先，调用 `+initWithAppId:uid:events:logDirectory:` 方法初始化 RTSA service。

在该方法中，你需要：
- 设置 `appId` 参数，传入你获取到的 App ID。了解[获取 App ID](../../cn/RTSA/demo_guide_ios.md)。
- 设置 `uid` 参数传入用户 ID。为 32 位整型，取值范围为 1 到 2<sup>32</sup> - 1。0 是无效的 `uid`，如果将 `uid` 设为 0，系统将自动分配一个 UID。
- 设置 `events` 参数，设置事件回调 `AgoraRtcServiceEvents`，用以通知 SDK 在运行过程中发生的事件。
- 设置 `logDirectory` 参数，用于存放 SDK 日志。如填写 nil，则使用默认目录，即当前应用程序的 documents 目录。

示例代码如下，仅供参考：
~~~objective-c
[AgoraRtcService initWithAppId:[KeyCenter appId] uid:0 events:self logDirectory:nil];

[AgoraRtcService setLogLevel:6];

[AgoraRtcService configLogWithPerFileSize:100000 rollCount:10];
~~~

## 加入频道

调用 `+joinChannel:tokenBytes:` 方法加入频道。

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
- 设置 `tokenBytes` 参数，传入能标识用户角色和权限的 Token。这就好比加入房间所需要的钥匙或门卡。Agora 提供两种鉴权机制：App ID 和 token。详见[校验用户权限](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms)。如果安全要求不高，使用 App ID 作为鉴权机制，可将 `tokenBytes` 设为 nil。

成功加入频道后，SDK 会触发 `-rtcServiceDidJoinChannel:elapsed:` 回调。

示例代码如下，仅供参考：
~~~objective-c
[AgoraRtcService joinChannel:@"my_first_channel" tokenBytes:nil];
~~~

## 发送/获取数据流
成功加入频道后，你可以：
* 监听`-rtcServiceDidReceiveAudioDataInChannel:uid:timestamp:codec:audioData:` 回调接收所有你已加入的频道内的音频数据流。
* 监听 `-rtcServiceDidReceiveVideoDataInChannel:uid:timestamp:codec:streamId:isKeyFrame:videoData:` 回调接收所有你已加入的频道内的视频数据流。
* 调用 `+sendAudioDataToChannel:codec:audioData:` 方法向指定或所有你已加入的频道发送音频数据流。
* 调用 `+sendVideoDataToChannel:codec:streamId:isKeyFrame:videoData:` 方法向指定或所有你已加入的频道发送视频数据流。

示例代码如下，仅供参考：
~~~objective-c
[AgoraRtcService sendAudioDataToChannel:CHANNEL_NAME codec:AUDIO_CODEC audioData:data];

[AgoraRtcService sendVideoDataToChannel:CHANNEL_NAME codec:VIDEO_CODEC  streamId:VIDEO_STREAM_ID isKeyFrame:shouldBeKey videoData:data];

- (void)rtcServiceDidReceiveAudioDataInChannel:(NSString *)channelName uid:(uint32_t)uid timestamp:(uint16_t)timestamp codec:(uint8_t)codec audioData:(NSData *)audioData {
	NSLog(@"didReceiveAudioData");
}

- (void)rtcServiceDidReceiveVideoDataInChannel:(NSString *)channelName uid:(uint32_t)uid timestamp:(uint16_t)timestamp codec:(uint8_t)codec streamId:(uint8_t)streamId isKeyFrame:(BOOL)isKeyFrame videoData:(NSData *)videoData {
	NSLog(@"didReceiveVideoData");
}

~~~

## 离开频道
调用 `+leaveChannel:`  方法离开指定频道，结束在该频道的数据传输。

示例代码如下，仅供参考：
~~~objective-c
[AgoraRtcService leaveChannel:@"my_first_channel"];
~~~




