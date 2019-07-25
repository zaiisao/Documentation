
---
title: Release Notes
description: 
platform: Web
updatedAt: Thu Jul 25 2019 03:23:32 GMT+0800 (CST)
---
# Release Notes
## Overview

Designed as a substitute for the legacy Agora Signaling SDK, the Agora Real-time Messaging SDK provides a more streamlined implementation and more stable messaging mechanism for you to quickly implement real-time messaging scenarios.

> For more information about the SDK features and applications, see [Agora RTM Overview](../../en/Real-time-Messaging/RTM_product.md).

## v0.9.1

v0.9.1 is released on May 20th, 2019.

### New Features

This release adds the call invitation feature, allowing you to create, send, cancel, accept, and decline a call invitation in a one-to-one or one-to-many voice/video call. 



### API Changes

#### Adds

- [createLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createlocalinvitation): Creates a call invitation. 
- [LocalInvitation.send](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send): Allows the caller to send a call invitation to a specified user (callee). 
- [LocalInvitation.cancel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send)Allows the caller to cancel a sent call invitation. 
- [LocalInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/localinvitationstate.html): **RETURNED TO THE CALLER** Call invitation status codes. 
- [LocalInvitationFailureReason](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/localinvitationfailurereason.html): **RETURNED TO THE CALLER** Reason for failure of the outgoing call invitation. 
- [RemoteInvitationReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmclientevents.html#remoteinvitationreceived): Occurs when the callee receives a call invitation. 
- [RemoteInvitation.accept](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept): Allows the callee to accept an incoming call invitation. 
- [RemoteInvitation.refuse](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#refuse): Allows the callee to declien an incoming call invitation. 
- [RemoteInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/remoteinvitationstate.html): **RETURNED TO THE CALLEE**: Call invitation status codes. 
- [RemoteInvitationFailureReason](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/remoteinvitationfailurereason.html): **RETURNED TO THE CALLEE**: Reason for the failure of the incoming call invitation.

#### Renames

- `RtmClient.ConnectionStateChange` to [ConnectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmclientevents.html#connectionstatechanged) .

#### Deletes

- The `RtmChannel.getId()` method. Uses the [channelId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#channelid) property instead.

## v0.9.0

v0.9.0 is released on February 14th, 2019.

Initial version. 

Key features:

- Sends or receives peer-to-peer messages.
- Joins or leaves a channel.
- Sends or receives channel messages.

