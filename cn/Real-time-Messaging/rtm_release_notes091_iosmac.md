
---
title: Agora RTM SDK v0.9.1 iOS/macOS 平台版本更新信息
description: 
platform: iOS,macOS
updatedAt: Fri Apr 12 2019 12:46:43 GMT+0800 (CST)
---
# Agora RTM SDK v0.9.1 iOS/macOS 平台版本更新信息
## v0.9.1 发版说明

本文列举Agora RTM iOS SDK v0.9.1 版本较 v0.9.0 版本的差异

> macOS 平台暂不支持呼叫邀请功能

1. `AgoraRtmSendMessageErrorCode` 枚举型拆分为 `AgoraPeerMessageError` 和 `AgoraChannelMessageError` 。

| v0.9 AgoraRtmSendMessageErrorCode                            | v0.9.1 AgoraRtmSendPeerMessageErrorCode                      |v0.9.1 AgoraRtmSendChannelMessageErrorCode    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |-----------------------------------|
| AgoraRtmSendMessageErrorOkReceivedByPeer = 0, <br>AgoraRtmSendMessageErrorOkReceivedByServer = 1, <br>AgoraRtmSendMessageErrorFailure = 2, AgoraRtmSendMessageErrorTimeout = 3, | AgoraRtmSendPeerMessageErrorOk = 0, <br>AgoraRtmSendPeerMessageErrorFailure = 1, <br>AgoraRtmSendPeerMessageErrorTimeout = 2, <br>AgoraRtmSendPeerMessageErrorPeerUnreachable = 3, |AgoraRtmSendChannelMessageErrorOk = 0,<br>AgoraRtmSendChannelMessageErrorFailure = 1,<br>AgoraRtmSendChannelMessageErrorTimeout = 2,

2. `AgoraRtmConnectionChangeReason` 枚举型中增加连接状态改变原因。

| v0.9                           |v0.9.1                |
|--------------------------------|----------------------|
|N/A                     |AgoraRtmConnectionChangeReasonRemoteLogin = 8,|

3. `setParameters` 方法参数命名改动，由 `options` 改为 `parameters`。

\- (int)setParameters:(NSString * _Nonnull)parameters;

4. 所有 RtmChannel 相关的回调中删除 `rtmKit`，并允许在 channel 内开放引用 rtmKit。

5. 开放 `channelDelegate` 为 `property` 属性，客户可以随时更新 delegate。

6. 增加了呼叫邀请相关方法、回调及枚举类型。详情请见 API 参考。  


