
---
title: Signaling vs. Agora RTM SDK
description: 
platform: Web
updatedAt: Thu Jan 02 2020 12:45:18 GMT+0800 (CST)
---
# Signaling vs. Agora RTM SDK
This page compares the legacy Agora Signaling APIs with the Agora Real-time Messaging APIs. 

## Login & Logout

| Method                 | Signaling                              | Real-time Messaging                                          |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------ |
| Creates an instance.   | `Signal` |  [createInstance](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/modules/agorartm.html#createinstance)<sup>1</sup> |
| Login                  | `login`/`login2`                       | [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login)<sup>2</sup> |
| Logout                 | `logout`                               | [logout](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#logout) |
| Gets the login status. | `getStatus`                            | N/A. See [ConnectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#connectionstatechanged). |

| Event                     | Signaling             | Real-time Messaging                                          |
| ------------------------- | --------------------- | ------------------------------------------------------------ |
| Login succeeds.           | `onLoginSuccess`      | The Promise resolves after the user logs in the Agora RTM system successfully. |
| Login Fails.              | `onLoginFailed`       | Use the `catch` method of the Promise for catching and handling exceptions. |
| Logout results.           | `onLogout`            | The Promises resolves after the user logs out of the Agora RTM system and disconnects from WebSocket. |
| Connection state changes. | N/A. See `getStatus`. | [ConnectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#connectionstatechanged) |

> - Unless otherwise specified, most of the core APIs of the Agora RTM SDK should only be called after the [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) method call succeeds.
> - <sup>1</sup> You can create multiple RtmClient instances with the [createInstance](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/modules/agorartm.html#createinstance) method. The Agora RTM SDK does not put a limit to the number of RtmClient instances you can create, but it only allows you to join a maximum of 20 RtmChannels at the same time. 
> - <sup>2</sup> The generation of the token you use to log in the Agora RTM system differs from the generation of the signalingToken you use to log in the Agora Signaling system. Ensure that you use the right token. See [Token Security](../../en/Real-time-Messaging/rtm_token.md) for more information.
> - <sup>2</sup> The token debugging mechanism, "\_no\_need\_token" for example, of the Agora Signaling SDK does not apply to the Agora RTM SDK. 
> - <sup>2</sup> The way that the Agora RTM SDK connects or reconnects to the Agora RTM system is completely different either. For more information, see [Manage Connection States](../../en/Real-time-Messaging/reconnecting_oc.md) for more information. 

## Sending a peer-to-peer message

| Method                        | Signaling            | Real-time Messaging                                          |
| ----------------------------- | -------------------- | ------------------------------------------------------------ |
| Creates a message instance.   | N/A                  | N/A                                                          |
| Sends a peer-to-peer message. | `messageInstantSend` | [sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer)<sup>1</sup> |

| Event                                  | Signaling                 | Real-time Messaging                                          |
| -------------------------------------- | ------------------------- | ------------------------------------------------------------ |
| Peer-to-peer message sending succeeds. | See the `cb` parameter that `messageInstantSend` takes.   | Promise<sup>2</sup>                                          |
| Peer-to-peer message sending fails.    | See the `cb` parameter that `messageInstantSend` takes.      | Promise                                                      |
| Receives a peer-to-peer message        | `onMessageInstantReceive` | [MessageFromPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#messagefrompeer) |

> <sup>1</sup>As of v0.9.3, the Agora RTM SDK allows you to send an offline message by configuring [SendMessageOptions](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/sendmessageoptions.html).
>
> <sup>2</sup> The Agora Signaling SDK returns the `onMessageSendSuccess` callback when the server receives the peer-to-peer message; the Agora RTM SDK returns the `resolve` state of a Promise when the specified user receives the message. 

## Querying the online status of specified users

| Method                                         | Signaling         | Real-time Messaging                                          |
| ---------------------------------------------- | ----------------- | ------------------------------------------------------------ |
| Queries the online status of a specified user. | `invoke` | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) |



| Event                            | Signaling                 | Real-time Messaging |
| -------------------------------- | ------------------------- | ------------------- |
| Returns the result of the query. | `OnInvokeRet` | Promise             |

> With the Agora RTM SDK,  you can query the online status of a list of peer users, not of just one peer user.

## User attribute operations

| Method                                              | Signaling        | Real-time Messaging                                          |
| --------------------------------------------------- | ---------------- | ------------------------------------------------------------ |
| Sets the local user's attribute                     | `invoke`        | [addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes) |
| Gets an attribute of the local user.                | `invoke`        | [getUserAttributesBykeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys)<sup>1</sup> |
| Gets all attributes of the local user.              | `invoke`     | [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<sup>2</sup> |
| Gets all attributes of the specified user.          | `invoke` | [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes) |
| Replaces the local user's attributes with new ones. | N/A              | [setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes) |
| Deletes the specified attributes of the local user. | N/A              | [deleteLocaluserAttributeByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys) |
| Clears the local user's attributes                  | N/A              | [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes) |
|                                                     |                  |                                                              |

| Event                                                 | Signaling               | Real-time Messaging |
| ----------------------------------------------------- | ----------------------- | ------------------- |
| Returns the result of the user-attribute method call. | `OnInvokeRet` | Promise             |

> - <sup>1</sup> The [getuserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys) method allows you to retrieve a list of attributes from the local user.
> - <sup>2</sup> The [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes) method allows you to retrieve the attributes from either the local user or a specified peer user. 

## Joining or Leaving a Channel

| Method                      | Signaling      | Real-time Messaging                                          |
| --------------------------- | -------------- | ------------------------------------------------------------ |
| Creates a channel instance. | N/A            | [createChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createchannel)<sup>1</sup> |
| Joins a specified channel.  | `channelJoin`  | [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join)<sup>2</sup> |
| Leaves a channel.           | `channelLeave` | [leave](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#leave) |

| Event                                                   | Signaling             | Real-time Messaging                                          |
| ------------------------------------------------------- | --------------------- | ------------------------------------------------------------ |
| Successfully joins the specified channel.               | `onChannelJoined`     | Promise                                                      |
| Fails to join the specified channel                     | `onChannelJoinFailed` | Promise                                                      |
| A remote user joins the current channel.                | `onChannelUserJoined` | [MemberJoined](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#memberjoined) |
| Successfully leaves the current channel.                | `onChannelLeaved`     | Promise                                                      |
| A remote channel member leaves the current channel.     | `onChannelUserLeaved` | [MemberLeft](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#memberleft) |
| Returns a channel member list when joining the channel. | `onChannelUserList`   | N/A<sup>3</sup>                                              |


> - <sup>1</sup> The Agora RTM SDK requires you to create a channel instance before joining it. 
> - <sup>2</sup> The Agora RTM SDK allows you to join a maximum of 20 channels at the same time. 
> - <sup>3</sup> The Agora RTM SDK does not return to the user a member list of the channel when he/she successfully joins it.
> - The Agora RTM SDK does not support a special channel as the Agora Signaling SDK does. 

## Sending a channel message

| Method                                          | Signaling                 | Real-time Messaging                                          |
| ----------------------------------------------- | ------------------------- | ------------------------------------------------------------ |
| Creates a message instance.                     | N/A                       | N/A                                                          |
| Sends a channel message from within a channel.  | `messageChannelSend`      | [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage)<sup>1</sup> |



| Event                                     | Signaling                 | Real-time Messaging                                          |
| ----------------------------------------- | ------------------------- | ------------------------------------------------------------ |
| Successfully sends out a channel message. | See the `cb` parameter that `messageChannelSend` takes.    | Promise                                                      |
| Fails to send out a channel message.      | See the `cb` parameter that `messageChannelSend` takes.      | Promise                                                      |
| Receives a channel message.               | `onMessageChannelReceive` | [ChannelMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#channelmessage)<sup>2</sup> |



> - <sup>1</sup> The Agora RTM SDK does not support sending channel messages from outside a channel. That said, you must join a channel before being able to send out a channel message. 
> - <sup>2</sup> The [ChannelMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#channelmessage) callback is not returned to the message sender. 

## Channel attribute operations

| Method                                                       | Signaling          | Real-time Messaging                                          |
| ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| Sets a channel attribute.                                    | `channelSetAttr`   | [addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatechannelattributes)<sup>1</sup> |
| Deletes a channel attribute.                                 | `channelDelAttr`   | [deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys)<sup>1</sup> |
| Deletes all attributes of a channel.                         | `channelClearAttr` | [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearchannelattributes)<sup>1</sup> |
| Substitutes the attributes of a specified channel with new ones | N/A                | [setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes)<sup>1</sup> |
| Gets all attributes of a specified channel                   | N/A                | [getChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes) |
| Gets the attributes of a specified channel by attribute keys | N/A                | [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys) |

> <sup>1</sup> With the Agora RTM SDK, you can update the attributes of a channel without the need to join it. 

| Event                           | Signaling              | Real-time Messaging                                          |
| ------------------------------- | ---------------------- | ------------------------------------------------------------ |
| A channel attribute is updated. | See the `cb` parameter that the corresponding method takes. | [AttributesUpdated](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmchannelevents.html#attributesupdated)<sup>2</sup> |

> <sup>2</sup>  This callback is disabled by default. It is enabled only when the user, who updates the attributes of the channel, sets [enableNotificationToChannelMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/channelattributeoptions.html#enablenotificationtochannelmembers) as `true`.

## Retrieving a member list of the current channel

| Method                                                 | Signaling | Real-time Messaging                                          |
| ------------------------------------------------------ | --------- | ------------------------------------------------------------ |
| Retrieves the latest user list of a specified channel. | `invoke`  | [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers)<sup>1</sup> |



| Event                                         | Signaling     | Real-time Messaging |
| --------------------------------------------- | ------------- | ------------------- |
| Returns the user list in a specified channel. | `onInvokeRet` | Promise             |

> <sup>1</sup> You must join an RtmChannel before retrieving a member list of it. When the number of the channel members exceeds 512, the Agora RTM SDK only returns a list of randomly selected 512 channel members. 



## Retrieving the Number of Users of a Specified Channel

| Method                                                | Signaling             | Real-time Messaging                                          |
| ----------------------------------------------------- | --------------------- | ------------------------------------------------------------ |
| Retrieves the number of users in a specified channel. | N/A | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelmembercount)<sup>1</sup> |

> <sup>1</sup>  The Agora RTM SDK supports retrieving the member count of up to 32 channels. 

| Event                                               | Signaling                     | Real-time Messaging |
| --------------------------------------------------- | ----------------------------- | ------------------- |
| Returns the number of users in a specified channel. | N/A | Promise             |

## Call Invitation



| Method                                                       | Signaling                                | Real-time Messaging                                          |
| ------------------------------------------------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| Allows the caller to create a `LocalInvitation.`             | N/A                                      | [createLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#createlocalinvitation)<sup>1</sup> |
| Allows the caller to send a call invite to a specified user (callee). | `channelInviteUser2` | [send](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send)<sup>2</sup> |
| Allows the caller to send a call invite to land-line user.   | `channelInviteDTMF`                      | N/A                                                          |
| Allows the caller to cancel a sent call invite.              | `channelInviteEnd`                       | [cancel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#cancel)<sup>3</sup> |
| Allows the callee to accept an incoming call invite.         | `channelInviteAccept`                    | [accept](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept) |
| Allows the callee to decline an incoming call invite.        | `channelInviteRefuse`                    | [refuse](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept) |
|                                                              |                                          |                                                              |

> - <sup>1</sup> The Agora RTM SDK introduces the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) and [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) objects. The former is created by the caller using the `createLocalInvitation` method, the latter is created automatically by the SDK when the callee receives the call invitation from the caller. You can take the two objects as the same call invitation taking in two different forms. The caller uses the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) object to specify the callee, set the content, or check the [LocalInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.localinvitationstate.html); the callee uses the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) object to set a response, check the caller ID, or check the [RemoteInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.remoteinvitationstate.html). 
> - <sup>2</sup> The [send](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send) method does not take an `extra` argument as the `channelInviteUser2` method does. 
> - <sup>3</sup> The `channelInviteEnd` method can end a call invite any time, whilst the [cancel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#cancel) method can only cancel a sent and ongoing call invite. 
> - To intercommunicate with the Agora Signaling SDK, you must upgrade your Agora RTM SDK to v1.0+ and set the channel ID.  Please also note that even if the callee accepts the call invite, the Agora RTM SDK does not add either the caller or the callee to the specified channel. 

| Synchronous Callback      | Signaling | Real-time Messaging                                          |
| ------------------------- | --------- | ------------------------------------------------------------ |
| The method call succeeds. | N/A       | <li>[LocalInvitationEvents](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html)<li> [RemoteInvitationEvents](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html) |
| The method call fails.    | N/A       | <li>[LocalInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationfailure)<li> [RemoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html). See [InvitationApiCallError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmerrorcode.invitationapicallerror.html) for the error codes.<sup>5</sup> |

> <sup>5</sup> If a user calls [send](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#send), [cancel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#cancel), [accept](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept) or [refuse](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#accept) before the life cycle of a [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) starts or after the life cycle of a [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) ends (either in failure or in success), the SDK returns the [InvitationApiCallError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmerrorcode.invitationapicallerror.html) error code. 



| Event                                                        | Signaling                | Real-time Messaging                                          |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| Returns to the caller: the callee receives the call invite.  | `oninviteReceivedByPeer` | [LocalInvitationReceivedByPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationreceivedbypeer) |
| Returns to the caller: the call invite successfully cancelled. | `onInviteEndByMyself`    | [LocalInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationcanceled) |
| Returns to the caller: call invite accepted.                 | `onInviteAcceptedByPeer` | [LocalInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationaccepted) |
| Returns to the caller: call invite declined.                 | `onInviteRefusedByPeer`  | [LocalInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationrefused) |
| Returns to the caller: the outgoing call invite ends in failure. | `onInviteFailed`         | [LocalInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationfailure). See [LocalInvitationFailureReason](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.localinvitationfailurereason.html) for the error codes. <sup>6</sup> |
|                                                              |                          |                                                              |

> <sup>6</sup>: The SDK returns the [LocalInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationfailure) to the caller if the call invitation process has started but ends in failure. Scenarios include: the callee is offline, the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) object times out, the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html) object expires, and the callee receives the call invitation but fails to respond in time. 

| Event                                                   | Signaling           | Real-time Messaging                                          |
| ------------------------------------------------------- | ------------------- | ------------------------------------------------------------ |
| Returns to the land-line user: receives a call invite.  | `onInviteMsg`       | N/A                                                          |
| Returns to the callee: receives a call invite.          | `oninviteReceived`  | [RemoteInvitationReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#remoteinvitationreceived) |
| Returns to the callee: the caller cancels the invite.   | `onInviteEndByPeer` | [RemoteInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationcanceled) |
| Returns to the callee: call invite accepted.            | N/A                 | [RemoteInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationaccepted) |
| Returns to the callee: call invite declined.            | N/A                 | [RemoteInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.localinvitationevents.html#localinvitationrefused) |
| Returns to the callee: the call invite ends in failure. | N/A                 | [RemoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationfailure). See [RemoteInvitationFailureReason](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/enums/rtmstatuscode.remoteinvitationfailurereason.html) for the error codes. <sup>7</sup> |
|                                                         |                     |                                                              |

> <sup>7</sup> The SDK returns the [RemoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.remoteinvitationevents.html#remoteinvitationfailure) to the callee if the call invitation process has started but ends in failure. Scenarios include: the caller is offline, the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) object times out, and the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html) ojbect expires. 

## Renewing a Token



| Method                    | Signaling | Real-time Messaging                                          |
| ------------------------- | --------- | ------------------------------------------------------------ |
| Renews the current Token. | N/A       | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken) |



| Event                                  | Signaling | Real-time Messaging |
| -------------------------------------- | --------- | ------------------- |
| Returns the result of the method call. | N/A       | Promise             |
| The token has expired.                 | N/A       | [TokenExpired](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmevents.rtmclientevents.html#tokenexpired)     |
