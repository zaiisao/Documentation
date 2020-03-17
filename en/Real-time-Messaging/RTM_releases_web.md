
---
title: Release Notes
description: 
platform: Web
updatedAt: Thu Sep 05 2019 04:01:39 GMT+0800 (CST)
---
# Release Notes
## Overview

Designed as a substitute for the legacy Agora Signaling SDK, the Agora Real-time Messaging SDK provides a more streamlined implementation and more stable messaging mechanism for you to quickly implement real-time messaging scenarios.

> For more information about the SDK features and applications, see [Agora RTM Overview](../../en/Real-time-Messaging/RTM_product.md).


## v1.0.1

v1.0.1 is released on September 5, 2019. 

### Issues Fixed

- Peer-to-peer messages have a chance to become offline messages even if [enableOfflineMessaging](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/sendmessageoptions.html) is not set. 
- The local user has a chance of not being able to retrieve the latest user attributes of a remote user, if the remote user updates his/her attributes immediately after logging in the Agora RTM system. 

### Improvements

- Caches up to six seconds of peer-to-peer messages sent when the network is interrupted, and resends the cached messages immediately after the SDK reconnects to the Agora RTM system. 
- Timeout for sending a peer-to-peer message is reset to 10 seconds. 

## v1.0.0

v1.0.0 is released on August 5th, 2019.

### New Features

#### Interconnects with the legacy Agora Signaling SDK

v1.0.0 implements the `LocalInvitation.channelId` and `LocalInvitation.channelId` property. 

> - To intercommunicate with the legacy Agora Signaling SDK, you MUST set the channel ID. However, even if the callee successfully accepts the call invitation, the Agora RTM SDK does not join the channel of the specified channel ID.
> - If your App does not involve the legacy Agora Signaling SDK, we recommend using the `LocalInvitation.content` or the `RemoteInvitation.response` property to set customized contents. 


#### Sets the output log level of the SDK

Supports setting the output log level of the SDK using the `logFilter` parameter.  The log level follows the sequence of OFF, ERROR, WARNING, and INFO. Choose a level to see the logs preceding that level. If, for example, you set the log level to WARNING, you see the logs within levels ERROR and WARNING. 


### API Changes

#### Adds

- [logFilter](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmparameters.html#logfilter)
- [setParameters](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setparameters)



## v0.9.3

v0.9.3 is released on July 24th, 2019.

### New Features

#### Sends an (offline) peer-to-peer message to a specified user (receiver)

This version allows you to send a message to a specified user when he/she is offline. If you set a message as an offline message and the specified user is offline when you send it, the RTM server caches it. Please note that for now we only cache 200 offline messages for up to seven days for each receiver. When the number of the cached messages reaches this limit, the newest message overrides the oldest one.


#### User attribute-related operations

This version allows you to set or update a user's attributes. You can:

- Substitutes the local user's attributes with new ones.
- Adds or updates the local user's attribute(s).
- Deletes the local user's attributes using attribute keys.
- Clears all attributes of the local user.
- Gets all attributes of a specified user.
- Gets the attributes of a specified user using attribute keys.

> The attributes you set will be cleard when you log out of the RTM system.


#### Queries the Online Status of the Specified Users

This release introduces a new concept: online and offline. 

- Online: The user has logged in the Agora RTM system. 
- Offline: The user has logged out of the Agora RTM system. 

This release adds the function of querying the online status of the specified users. After logging in the Agora RTM system, you can get the online status of a maximum of 256 specified users. 


#### Renews the Token

This release allows you to renew a token.

### API Changes

#### Adds

- [sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer)
- [setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)
- [addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)
- [deleteLocalUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)
- [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes)
- [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)
- [getUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys)
- [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus)
- [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken)

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

