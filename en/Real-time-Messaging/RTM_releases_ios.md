
---
title: Release Notes
description: migration information
platform: iOS,macOS
updatedAt: Mon May 06 2019 07:14:59 GMT+0800 (CST)
---
# Release Notes
## Overview

Designed as a substitute for the legacy Agora Signaling SDK, the Agora Real-Time-Messaging SDK provides a more streamlined implementation and more stable messaging mechanism for you to quickly implement real-time messaging scenarios.

> Fore more information about the SDK features and applications, see [Product Overview](../../en/Real-time-Messaging/RTM_releases_android.md).

## v0.9.2 

v0.9.2 is released on May 5th, 2019.

### New Features

#### Queries the Online Status of the Specified User(s)

This release introduces a new concept: online and offline. 

- Online: The user has logged in the Agora RTM system. 
- Offline: The user has logged out of the Agora RTM system. 

This release adds the function of querying the online status of the specified users. After logging in the Agora RTM system you can get the online status of a maximum of 256 specified users.  See: [queryPeersOnlineStatus:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:).

> - The sequence of the returned user IDs is identical to the input sequence. 
> - The call frequency of this method is 10 queries every 5 seconds.  See: [Limitations](../../en/Real-time-Messaging/RTM_limitations_ios.md) .


#### Renews the Token

In the production environment, you need to put a token to [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) the Agora RTM system. Each token expires 24 hours after it is created. This release allows you to [renew token](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) .

- If you are logging in the Agora RTM system and if your token has expired, the SDK returns the [AgoraRtmLoginErrorTokenExpired](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLoginErrorCode.html) error code. 
- if you has logged in the Agora RTM system, you will not be kicked immediately when your token expires. But you need to renew your token the next time you log in the Agora RTM system. Therefore, we still recommend that you renew your token when you receive the [rtmKitTokenDidExpire:](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:) callback.


> - renewToken must be called before [creating an RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:) .
> - The call frequency of the `RenewToken` method  is 2 queries every second. See: [Limitations](../../en/Real-time-Messaging/RTM_limitations_ios.md) .

### Improvements

-  Supports a `userId` that starts with a space.


### API Changes

#### Queries the Online Status of the Specified User(s)

##### Adds

- Method:  [queryPeersOnlineStatus:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:).
- Error Codes: [QueryPeersOnlineStatusError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmQueryPeersOnlineErrorCode.html)

#### Renews the Token

##### Adds

- Method:  [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:)
- Callback: [onTokenExpired](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:)
- Error Codes:  [AgoraRtmRenewTokenErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRenewTokenErrorCode.html)

#### Call Invitation

Adds the following the error code to cover the situation where a user sends a call invitation without logging in the Agora RTM system. 

- [AgoraRtmLocalInvitationErrorNotLoggedIn](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html)

## v0.9.1

v0.9.1 is released on April 4th, 2019.

### New Features

This release adds the call invitation feature, allowing you to create, send, cancel, accept, and decline a call invitation in a one-to-one or one-to-many voice/video call. 

> This version does not come with the `setLogFile` or `setLogFilter` method. All log information is kept at **~/Library/caches/agorasdk.log** by default. 

### Improvements

- Optimizes the object relations to facilitate understanding.
- Simplifies the process of sending a channel or peer-to-peer message. 

### API Changes

#### Adds

- [initWithCalleeId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html#//api/name/initWithCalleeId:): allows the caller to create an AgoraRtmLocalInvitation instance.
- [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html): the caller's call invitation object.
- [localInvitationReceivedByPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationReceivedByPeer:): occurs when the callee receives the call invitation.
- [localInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationAccepted:withResponse:): occurs when the callee accepts the call invitation.
- [localInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationRefused:withResponse:): occurs when the callee declines the call invitation.
- [localInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationCanceled:): occurs when the caller cancels the call invitation.
- [localInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:): occurs when the life cycle of the outgoing call invitation ends in failure.
- [remoteInvitationReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationReceived:): occurs when the callee receives a call invitation.
- [remoteInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationAccepted:): occurs when the callee accepts a call invitation.
- [remoteInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationRefused:): occurs when the callee declines a call invitation.
- [remoteInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationCanceled:): occurs when the caller cancels the call invitation.
- [remoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:): occurs when the life cyle of the incoming call invitation ends in failure.
- [AgoraRtmCallDelegate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html): enables Agora RTM call callback event notifications to your app.
- [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:): sends a call invitation to a specified user.
- [acceptRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:): accepts an incoming call invitation.
- [refuseRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:): declines an incoming call invitation.
- [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:): allows the caller to cancel an outgoing call invitation.
- [AgoraRtmLocalInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationState.html): states of an outgoing call invitation.
- [AgoraRtmRemoteInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationState.html): states of an incoming call invitation.
- [AgoraRtmLocalInvitationErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html): error codes of an outgoing call invitation.
- [AgoraRtmRemoteInvitationErrorCode](https://docs.agora.io/en/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationErrorCode.html): error codes of an incoming call invitation.
- [AgoraRtmInvitationApiCallErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmInvitationApiCallErrorCode.html): error codes of the invitation-specific API calls.
- [AgoraRtmConnectionChangeReasonRemoteLogin](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmConnectionChangeReason.html): another instance has logged in the Agora RTM system with the same user ID.
- [AgoraRtmSendPeerMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html): error codes of sending a peer-to-peer message. 
- [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html): error codes of sending a channel message. 
- [AgoraRtmConnectionChangeReasonRemoteLogin](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmConnectionChangeReason.html): error codes of the connection state change. 



#### Deletes

- Deletes the `AgoraRtmSendMessageErrorCode` constants, uses [AgoraRtmSendPeerMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html)  and [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html) instead.

#### Modifies

- Sets `channelDelegate` as a property so that developers can freely update it. 
- Removes rtmKit from the callbacks under `AgoraRtmChannelDelegate`.
- Changes the parameter of the [setParameters](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setParameters:) method, which configures the SDK with JSON options. 
- Deletes AgoraRtmSendChannelMessageStateReceivedByServer from [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html).
- Deletes the AgoraRtmSendPeerMessageState interface, uses [AgoraRtmSendPeerMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html) instead.
- Deletes the AgoraRtmSendChannelMessageState interface, uses [AgoraRtmSendChannelMessageErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendChannelMessageErrorCode.html) instead.

## v0.9.0

v0.9.0 is released on February 4th, 2019.

Initial version. 

Key features:

- Sends or receives peer-to-peer messages.
- Joins or leaves a channel.
- Sends or receives channel messages.

