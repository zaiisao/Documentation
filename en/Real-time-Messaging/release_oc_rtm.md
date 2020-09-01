
---
title: Release Notes
description: migration information
platform: iOS,macOS
updatedAt: Tue Sep 01 2020 09:52:41 GMT+0800 (CST)
---
# Release Notes
  ## Overview

Designed as a replacement for the legacy Agora Signaling SDK, the Agora Real-time Messaging (RTM) SDK provides a more streamlined and stable messaging mechanism for you to quickly implement real-time messaging for various scenarios. See [product overview](../../en/Real-time-Messaging/product_rtm.md) for more information.

## v1.2.2

v1.2.2 was released on December 13, 2019.



**Issues Fixed**

Occasionally fails to receive any callback after a channel attribute operation. 

## v1.2.1

v1.2.1 was released on November 29, 2019.  

**New Feature**

*Compatible with the endCall method of the Agora Signaling SDK*

If you use the `sendMessageToPeer` method to send a <i>text</i> message in the format of AgoraRTMLegacyEndcallCompatibleMessagePrefix\_\<channelId\>\_\<your additional information\>, then this method is compatible with the endCall method of the legacy Agora Signaling SDK. Replace \<channelId\> with the channel ID from which you want to leave (end call), and replace \<your additional information\> with any additional information. Note that you must not put any "_" (underscore) in your additional information but you can set \<your additional information\> as empty "".

**Issues Fixed**


- The SDK fails to reconnect to the Agora RTM system if the user disables VPN. 
- If a channel member reconnects to the Agora RTM server after being interrupted, chances are the rest members of the channel can receive `MemberJoined` twice. 

## v1.2.0 

v1.2.0 was released on November 6, 2019. 

**Compatibility Changes**

Deprecated the `isOnline` property of the `AgoraRtmPeerOnlineStatus`. Use `state` instead. 


**New Features**

#### Subscribe to the online status of the specified user(s)

When the method call succeeds, the SDK returns the PeersOnlineStatusChanged callback to report the online status of peers, to whom you subscribe.
When the online status of the peers, to whom you subscribe, changes, the SDK returns the PeersOnlineStatusChanged callback to report whose online status has changed.
If the online status of the peers, to whom you subscribe, changes when the SDK is reconnecting to the server, the SDK returns the PeersOnlineStatusChanged callback to report whose online status has changed when successfully reconnecting to the server.

#### Unsubscribe from the online status of the specified user(s)

Allows you to unsubscribe from the online status of the specified user(s).

#### Get a list of the peers, to whose specific status you have subscribed

Allows you to get a list of the peers, to whose specific status you have subscribed.

#### Create a raw message

Creates and initializes a raw message to be sent.

 If you set a text description, ensure that the size of the raw message and the description combined does not exceed 32 KB.

## v1.1.0

v1.1.0 is released on September 18, 2019. It adds the following features: 

- [Gets the member count of specified channel(s).](#getcount)
- [Automatically returns the latest numer of members in the current channel](#oncount)
- [Channel attribute operations](#channelattributes)



**Compatibility Changes**

1. Deprecates the [sendMessage:toPeer:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) method, and uses [sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) instead
2. The [serverReceivedTs](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/serverReceivedTs) property of the [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) instance supports both peer-to-peer and channel messages.
3. Timeout for sending a peer-to-peer message is 10 seconds from this release, compared to 5 seconds in previous versions. See [AgoraRtmSendPeerMessageErrorTimeout](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmSendPeerMessageErrorCode.html)
4. Puts a limit on the frequency of [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) the same channel:  [Two times every five seconds](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmJoinChannelErrorCode.html). 

**New Features**

<a name="getcount"></a>
#### 1. Gets the member count of specified channel(s).

You can now get the member count of specified channel(s) without the need to join, by calling the [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelMemberCount:completion:) method. You can get the member counts of a maximum of 32 channels in one method call. 

<a name="oncount"></a>
#### 2. Automatically returns the latest numer of members in the current channel 

If you are already in a channel, you do not have to call the `getChannelMemberCount` method to get the member count of the current channel. We also do not recommend using `onMemberJoined` and `onMemberLeft` to keep track of the member counts. As of this release, the SDK returns to the channel members [memberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberCount:) the latest channel member count when the number of channel members changes. Note that: 

- When the number of channel members ≤ 512, the SDK returns this callback when the number changes and at a MAXIMUM speed of once per second.
- When the number of channel members exceeds 512, the SDK returns this callback when the number changes and at a MAXIMUM speed of once every three seconds.

> Please treat this callback and the [AgoraRtmGetMembersBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmGetMembersBlock.html) callback separately: 
> - The former is an automatic callback. It returns the current numer of channel members;
> - The latter is triggered by the [getMembersWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) method. It returns a member list of the current channel. If the number of channel members exceeds 512, the SDK only returns a list of 512 randomly-selected channel members. 

<a name="channelattributes"></a>
#### 3. Channel attribute operations

Supports setting or getting the attribute(s) of a specified channel. You can use this feature to create group anouncement.

Each channel attribute comes as a key-value pair. See [AgoraRtmChannelAttribute](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannelAttribute.html). Where: 

- The key of each channel attribute must be visible characters and not exceed 8 KB.
- Each channel attribute must not exceed 8 KB in length. 
- The overall size of the attributes of a channel must not exceed 32 KB. 
- The number of attributes of a channel must not exceed 32. 

Specific features: 

- Sets the attributes of a specified channel with new ones.
- Adds or updates the attribute(s) of a specified channel.
- Deletes the attributes of a specified channel by attribute keys.
- Clears all attributes of a specified channel.
- Gets all attributes of a specified channel.
- Gets the attributes of a specified channel by attribute keys.

When updating attributes of a channel, you can use the [enableNotificationToChannelMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannelAttributeOptions.html#//api/name/enableNotificationToChannelMembers) flag to decide whether or not to notify all members of the channel about this attribute change.

> The SDK caches the channel attributes. If multiple users have the privilege to update the channel attributes, then we recommend calling the [getChannelAllAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:) to update the cache before updating the channel attributes. 

**Improvements**

#### Resends peer-to-peer messages

This release improves the resending mechanism of peer-to-peer messages, and extends the timeout for sending a peer-to-peer message from five to 10 seconds, greatly improving the success rate of peer-to-peer message sending under weak network conditions. 

#### Caches channel messages

The Agora RTM system will resend a maximum of 32 channel messages of up to 30 seconds to channel members, when they manage to reconnect to the system from poor network conditions. This greatly improves the overall arrival rate of channel messages under weak network conditions. 


**API Changes**

#### Added Methods

- [setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setChannel:Attributes:Options:completion:)：Substitutes the attributes of a specified channel with new ones.
- [addOrUpdaeChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateChannel:Attributes:Options:completion:)：Adds or updates the attribute(s) of a specified channel.
- [deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteChannel:AttributesByKeys:Options:completion:)：Deletes the attributes of a specified channel by attribute keys.
- [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearChannel:Options:AttributesWithCompletion:)：Clears all attributes of a specified channel.
- [getChannelAllAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:): Gets all attributes of a specified channel.
- [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:): Gets the attributes of a specified channel by attribute keys.
- [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAttributes:ByKeys:completion:): Gets the member count of specified channel(s).

#### Added Callbacks

- [attributesUpdate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:attributeUpdate:): Returns all attributes of the channel when the channel attributes are updated. 
- [memberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberCount:): Occurs when the number of the channel members changes, and returns the new number.

#### Added Error Codes 

- [AgoraRtmChannelMemberCountErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmChannelMemberCountErrorCode.html): Error codes related to retrieving the channel member count of specified channel(s).
- [AgoraRtmJoinSameChannelErrorTooOften](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmJoinChannelErrorCode.html): The frequency of joining the same channel exceeds two times every five seconds.

#### Deprecated Methods

- [sendMessage:toPeer:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:): Replaced by the [sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) method.

#### Deprecated Error Codes

- [AgoraRtmAttributeOperationErrorNotReady](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmProcessAttributeErrorCode.html): Replaced by [AgoraRtmAttributeOperationErrorNotLoggedIn](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmProcessAttributeErrorCode.html).






## v1.0.1

v1.0.1 is released on August 1st, 2019. 

### Issues Fixed

- When the connection to the Agora RTM system is interrupted, the SDK does not return the `connectionStateChanged` callback. 
- Occasional crashes on iOS. 

## v1.0.0

v1.0.0 is released on July 24th, 2019.

### New Features

#### Interconnects with the legacy Agora Signaling SDK

v1.0.0 implements the `channelId` property in the `AgoraRtmLocalInvitation` class. 

> - To intercommunicate with the legacy Agora Signaling SDK, you MUST set the channel ID. However, even if the callee successfully accepts the call invitation, the Agora RTM SDK does not join the channel of the specified channel ID.
> - If your App does not involve the legacy Agora Signaling SDK, we recommend using the `content` property of the `AgoraRtmLocalInvitation` class or the `response` property of the `AgoraRtmRemoteInvitation` class to set customized contents. 

#### Specifies the default path to the SDK log file

Supports changing the default path to the SDK log file using the `setLogFile` method. To avoid creating an incomplete log file, we recommend calling this method once you have created and initialized an `AgoraRtmKit` instance. 



#### Sets the output log level of the SDK

Supports setting the output log level of the SDK using the `setLogFilter` method.  The log level follows the sequence of OFF, CRITICAL, ERROR, WARNING, and INFO. Choose a level to see the logs preceding that level. If, for example, you set the log level to WARNING, you see the logs within levels CRITICAL, ERROR, and WARNING. See also [AgoraRtmLogFilter](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLogFilter.html). 

> You can call this method once you have created and initializd an `AgoraRtmKit` instance. You do not have to call this method after calling the `loginByToken` method. 

#### Sets the log file size in KB

Supports setting the log file size using the `setLogFileSize` method. The log file has a default size of 512 KB. File size settings of less than 512 KB or greater than 10 MB will not take effect.


> You can call this method once you have created and initializd an `AgoraRtmKit` instance. You do not have to call this method after calling the `loginByToken` method. 

### Improvements

Adds error codes based on the following scenarios: 

- The Agora RTM service is not initialized.
- The method call frequency exceeds the limit. 
- The user does not call the `loginByToken` method or the method call of `loginByToken` does not succeed before calling any of the RTM core APIs. 

### Issues Fixed

- Occasional crashes on iOS. 
- One can log in the Agora RTM system with a static App ID and an RTM token, which is generated from a dynamic App ID. 


### API Changes

- [setLogFile](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLogFile:): Specifies the default path to the SDK log file.
- [setLogFilter](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLogFilters:): Sets the output log level of the SDK.
- [setLogFileSize](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLogFileSize:): Sets the log file size in KB.
- [getSDKVersion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getSDKVersion): Gets the version of the Agora RTM SDK.




## v0.9.3

v0.9.3 is released on June 7th, 2019.

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

> - Only after you successfully loggin in the Agora RTM system can you execute user attribute-related operations. Otherwise, the SDK triggers the `AgoraRtmAttributeOperationErrorNotReady` error code.
> - The attributes you set will be clears when you log out of the RTM system.
> - You can only set a maximum of 16 KB attributes in a single method call. Otherwise, the SDK triggers the `AgoraRtmAttributeOperationErrorSizeOverflow` error code. 

### Improvements

- Supports creating an RTM channel before logging in the Agora RTM system. 
- Supports creating multiple RTM channels. But a user can only join a maximum of 20 RTM channels at the same time. When the number of the joined channels exceeds 20, the SDK triggers the `AgoraRtmJoinChannelErrorFailure` error code. 

### Issues Fixed

- Occasional system crashes.
- A user who has logged out of the Agora RTM system appears online to the other users until 30 seconds later. 

## v0.9.2 

v0.9.2 is released on May 5th, 2019.

> This release does not support [creating an RtmChannel instance](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) before [logging in](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) the Agora RTM system

### New Features

#### Queries the Online Status of the Specified Users

This release introduces a new concept: online and offline. 

- Online: The user has logged in the Agora RTM system. 
- Offline: The user has logged out of the Agora RTM system. 

This release adds the function of querying the online status of the specified users. After logging in the Agora RTM system, you can get the online status of a maximum of 256 specified users. See [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:).

> - The sequence of the returned user IDs is identical to the input sequence. 
> - The call frequency of this method is 10 times every five seconds. See [Limitations](../../en/Real-time-Messaging/RTM_limitations_ios.md).


#### Renews the Token

In the production environment, you need to use a token to [log in](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken) the Agora RTM system. Each token expires 24 hours after it is created. This release allows you to [renew a token](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:).

- If you are logging in the Agora RTM system and if your token has expired, the SDK returns the [AgoraRtmLoginErrorTokenExpired](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLoginErrorCode.html) error code. 
- if you are logged in the Agora RTM system, you will not be kicked out immediately when your token expires. But you need to renew your token the next time you log in the Agora RTM system. Therefore, we still recommend that you renew your token when you receive the [rtmKitTokenDidExpire:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:) callback.


> - The [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) method must be called before [creating an RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:).
> - The call frequency of the [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) method is two times every second. See [Limitations](../../en/Real-time-Messaging/RTM_limitations_ios.md).

### Improvements

-  Supports a `userId` that starts with a space.


### API Changes

#### Queries the Online Status of the Specified User(s)

##### Adds

- Method: [queryPeersOnlineStatus:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:)
- Error Code: [QueryPeersOnlineStatusError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmQueryPeersOnlineErrorCode.html)

#### Renews the Token

##### Adds

- Method: [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:)
- Callback: [onTokenExpired](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:)
- Error Code: [AgoraRtmRenewTokenErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRenewTokenErrorCode.html)

#### Call Invitation

Adds the following error code for when a user sends a call invitation without logging in the Agora RTM system. 

- [AgoraRtmLocalInvitationErrorNotLoggedIn](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html)

## v0.9.1

v0.9.1 is released on April 4th, 2019.

### New Features

This release adds the call invitation feature, allowing you to create, send, cancel, accept, and decline a call invitation in a one-to-one or one-to-many voice/video call. 

> This version does not come with the `setLogFile` or `setLogFilter` method. 
> - For iOS platforms, all log information is kept at **/Library/Caches/agorartm.log** by default. 
> - For macOS platforms, all log information is kept at **~/Library/Logs/agorartm.log** by default. 

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

