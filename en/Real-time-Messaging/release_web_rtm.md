
---
title: Release Notes
description: 
platform: Web
updatedAt: Fri Aug 07 2020 02:07:04 GMT+0800 (CST)
---
# Release Notes
  ## Overview

Designed as a replacement for the legacy Agora Signaling SDK, the Agora Real-time Messaging (RTM) SDK provides a more streamlined and stable messaging mechanism for you to quickly implement real-time messaging for various scenarios.

## v1.2.2

v1.2.2 was released on February 21, 2020.

**Compatibility Changes**


| Timeout | Before v1.2.2 | v1.2.2 |
| ---------------- | ---------------- | ---------------- |
| Send a channel message.     | 5 s    | 10 s     |



**Issues fixed**

The SDK occasionally returns `LOGIN_ERR_UNKNOWN` when logging in from a recent version of Chrome on a Windows platform. 

## v1.2.1

v1.2.1 was released on December 17, 2019.  

**New Feature**

*Compatible with the endCall method of the Agora Signaling SDK*

If you use the `sendMessageToPeer` method to send a <i>text</i> message in the format of AgoraRTMLegacyEndcallCompatibleMessagePrefix\_\<channelId\>\_\<your additional information\>, then this method is compatible with the endCall method of the legacy Agora Signaling SDK. Replace \<channelId\> with the channel ID from which you want to leave (end call), and replace \<your additional information\> with any additional information. Note that you must not put any "_" (underscore) in your additional information but you can set \<your additional information\> as empty "".

**Issues Fixed**


- If a channel member reconnects to the Agora RTM server after being interrupted, chances are the rest members of the channel can receive `MemberJoined` twice. 

## v1.2.0 

v1.2.0 was released on November 15, 2019. 

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
 
**Issues fixed**
 
The SDK is occasionally kicked by the server: When the issue occurs, the Client instance receives the `ConnectionStateChange` callback, which indicates the connection state is `ABORTED` and the reason for the connection state change `INTERRUPTED`. The log file shows the error code from the server is 10001. 
 

## v1.1.0

v1.1.0 was released on September 18, 2019. It adds the following features: 

- [Gets the member count of specified channel(s).](#getcount)
- [Automatically returns the latest numer of members in the current channel](#oncount)
- [Channel attribute operations](#channelattributes)



**Compatibility Changes**

1. The [getServerReceivedTs](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a7994de6da26269c3137e93ddf7a2c2be) method of the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) object supports both peer-to-peer and channel messages. 
2. Timeout for sending a peer-to-peer message is 10 seconds from this release, compared to 5 seconds in previous versions. See [PEER_MESSAGE_ERR_TIMEOUT ](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_peer_message_error.html#a9aaaa5b9fa46cc15327abd6c2825bc4d).

**New Features**

<a name="getcount"></a>
#### 1. Gets the member count of specified channel(s).

You can now get the member count of specified channel(s) without the need to join, by calling the `getChannelMemberCount` method. You can get the member counts of a maximum of 32 channels in one method call. 

<a name="oncount"></a>
#### 2. Automatically returns the latest numer of members in the current channel 

If you are already in a channel, you do not have to call the `getChannelMemberCount` method to get the member count of the current channel. We also do not recommend using `onMemberJoined` and `onMemberLeft` to keep track of the member counts. As of this release, the SDK returns to the channel members `MemberCountUpdated` the latest channel member count when the number of channel members changes. Note that:

- When the number of channel members ≤ 512, the SDK returns this callback when the number changes and at a MAXIMUM speed of once per second.
- When the number of channel members exceeds 512, the SDK returns this callback when the number changes and at a MAXIMUM speed of once every three seconds.

<a name="channelattributes"></a>
#### 3. Channel attribute operations

Supports setting or getting the attribute(s) of a specified channel. You can use this feature to create group anouncement.

Each channel attribute comes as a key-value pair. See [RtmChannelAttribute](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel_attribute.html) for more information. Where: 

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

When updating attributes of a channel, you can use the  `enableNotificationToChannelMembers` flag to decide whether or not to notify all members of the channel about this attribute change.  

**Improvements**

#### Resends peer-to-peer messages

This release improves the resending mechanism of peer-to-peer messages, and extends the timeout for sending a peer-to-peer message from five to 10 seconds, greatly improving the success rate of peer-to-peer message sending under weak network conditions. 

#### Caches channel messages

The Agora RTM system will resend a maximum of 32 channel messages of up to 30 seconds to channel members, when they manage to reconnect to the system from poor network conditions. This greatly improves the overall arrival rate of channel messages under weak network conditions. 


**API Changes**

#### Added Methods

- [setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes): Sets the attributes of a specified channel with new ones.
- [addOrUpdaeChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/classes/rtmclient.html#addorupdatechannelattributes): Adds or updates the attribute(s) of a specified channel.
- [deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/classes/rtmclient.html#deletechannelattributesbykeys): Deletes the attributes of a specified channel by attribute keys.
- [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/classes/rtmclient.html#clearchannelattributes): Clears all attributes of a specified channel.
- [getChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/classes/rtmclient.html#getchannelattributes): Gets all attributes of a specified channel.
- [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/classes/rtmclient.html#getchannelattributesbykeys): Gets the attributes of a specified channel by attribute keys.
- [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/classes/rtmclient.html#getchannelmembercount): Gets the member count of specified channel(s).

#### Added Callbacks

- [AttributesUpdated](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2904a1f1f78c497b9176fffb853be96f): Returns all attributes of the channel when the channel attributes are updated. 
- [MemberCountUpdated](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#ad778e702e026a79460f45a992bb8576d): Occurs when the number of the channel members changes, and returns the new number.

#### Added Error Codes 

- [GetChannelMemberCountErrCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/enums/rtmerrorcode.getchannelmembercounterrcode.html): Error codes related to retrieving the channel member count of specified channel(s).
- [JOIN_CHANNEL_ERR_JOIN_SAME_CHANNEL_TOO_OFTEN](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/v1.1.0/enums/rtmerrorcode.joinchannelerror.html#join_channel_err_join_same_channel_too_often): The frequency of joining the same channel exceeds two times every five seconds.
- [JOIN_CHANNEL_ERR_ALREADY_JOINED_CHANNEL_OF_SAME_ID](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmerrorcode.joinchannelerror.html#join_channel_err_already_joined_channel_of_same_id): You have already joined another channel instance of the same channel ID. 







## v1.0.1

v1.0.1 was released on September 5, 2019. 

**Issues Fixed**

- Peer-to-peer messages have a chance to become offline messages even if [enableOfflineMessaging](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/sendmessageoptions.html) is not set. 
- The local user has a chance of not being able to retrieve the latest user attributes of a remote user, if the remote user updates his/her attributes immediately after logging in the Agora RTM system. 

**Improvements**

- Caches up to six seconds of peer-to-peer messages sent when the network is interrupted, and resends the cached messages immediately after the SDK reconnects to the Agora RTM system. 
- Timeout for sending a peer-to-peer message is reset to 10 seconds. 

## v1.0.0

v1.0.0 was released on August 5th, 2019.

**New Features**

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

v0.9.3 was released on July 24th, 2019.

**New Features**

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

**API Changes**

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

v0.9.1 was released on May 20th, 2019.

**New Features**

This release adds the call invitation feature, allowing you to create, send, cancel, accept, and decline a call invitation in a one-to-one or one-to-many voice/video call. 



**API Changes**

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

v0.9.0 was released on February 14th, 2019.

Initial version. 

**Key features**

- Sends or receives peer-to-peer messages.
- Joins or leaves a channel.
- Sends or receives channel messages.

