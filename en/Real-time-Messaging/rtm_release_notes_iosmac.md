
---
title: Agora RTM v0.9.1 Migration Information
description: migration information
platform: iOS
updatedAt: Thu Apr 18 2019 02:21:38 GMT+0800 (CST)
---
# Agora RTM v0.9.1 Migration Information
This page describes the differences between v0.9.1 and v0.9.0 of the Agora RTM Android SDK.

v0.9.1:

1. Splited the AgoraRtmSendMessageErrorCode enum into the AgoraPeerMessageError and AgoraChannelMessageError enums.

| v0.9 AgoraRtmSendMessageErrorCode                            | v0.9.1 AgoraRtmSendPeerMessageErrorCode                      |v0.9.1 AgoraRtmSendChannelMessageErrorCode    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |-----------------------------------|
| AgoraRtmSendMessageErrorOkReceivedByPeer = 0, <br>AgoraRtmSendMessageErrorOkReceivedByServer = 1, <br>AgoraRtmSendMessageErrorFailure = 2, AgoraRtmSendMessageErrorTimeout = 3, | AgoraRtmSendPeerMessageErrorOk = 0, <br>AgoraRtmSendPeerMessageErrorFailure = 1, <br>AgoraRtmSendPeerMessageErrorTimeout = 2, <br>AgoraRtmSendPeerMessageErrorPeerUnreachable = 3, |AgoraRtmSendChannelMessageErrorOk = 0,<br>AgoraRtmSendChannelMessageErrorFailure = 1,<br>AgoraRtmSendChannelMessageErrorTimeout = 2,

2. Added a connection change reason to the AgoraRtmConnectionChangeReason enum. 

| v0.9                           |v0.9.1                |
|--------------------------------|----------------------|
|N/A                     |AgoraRtmConnectionChangeReasonRemoteLogin = 8,|

3. Changed the parameter of the `setParameters` method.

\- (int)setParameters:(NSString * _Nonnull)parameters;

4. Removed rtmKit from all RtmChannel-related callbacks. 

5. Set the `channelDelegate` as property so that developers can freely update the delegate. 

6. Added methods and enums for the call invitation function. See the API reference for more information.  

