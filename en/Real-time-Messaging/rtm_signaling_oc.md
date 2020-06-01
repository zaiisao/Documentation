
---
title: Signaling vs. Agora RTM SDK
description: 
platform: iOS,macOS
updatedAt: Mon Dec 23 2019 03:30:15 GMT+0800 (CST)
---
# Signaling vs. Agora RTM SDK
This page compares the legacy Agora Signaling APIs with the Agora Real-time Messaging APIs. 

## Login & Logout

| Method                 | Signaling                              | Real-time Messaging                |
| ---------------------- | -------------------------------------- | ---------------------------------- |
| Creates an instance.   | `getInstance`/`createAgoraSDKInstance` | [initWithAppId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:)<sup>1</sup>        |
| Login                  | `login`/`login2`                       | [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:)<sup>2</sup>                |
| Logout                 | `logout`                               | [logoutWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/logoutWithCompletion:)                           |
| Gets the login status. | `getStatus`                            | N/A. See [connectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:connectionStateChanged:reason:). |
| Destroys the instance. | `destroy`                              | N/A                                |

| Event                     | Signaling             | Real-time Messaging      |
| ------------------------- | --------------------- | ------------------------ |
| Login succeeds.           | `onLoginSuccess`      | [AgoraRtmLoginBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLoginBlock.html)     |
| Login Fails.              | `onLoginFailed`       | [AgoraRtmLoginBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLoginBlock.html)     |
| Logout results.           | `onLogout`            | [AgoraRtmLogoutBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLogoutBlock.html)    |
| Connection state changes. | N/A. See `getStatus`. | [connectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:connectionStateChanged:reason:) |

> - Unless otherwise specified, most of the core APIs of the Agora RTM SDK should only be called after the [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) method call succeeds and after you receive the `AgoraRtmLoginErrorOk` error code.
> - <sup>1</sup> You can create multiple AgoraRtmKit instances with the [initWithAppId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:) method. The Agora RTM SDK does not put a limit to the number of AgoraRtmKit instances you can create, but it only allows you to join a maximum of 20 AgoraRtmChannels at the same time. 
> - <sup>2</sup> The generation of the token you use to log in the Agora RTM system differs from the generation of the signalingToken you use to log in the Agora Signaling system. Make sure you use the right token. See [Token Security](../../en/Real-time-Messaging/rtm_token.md) for more information.
> - <sup>2</sup> The token debugging mechanism, "\_no\_need\_token" for example, of the Agora Signaling SDK does not apply to the Agora RTM SDK. 
> - <sup>2</sup> The way that the Agora RTM SDK connects or reconnects to the Agora RTM system is completely different either. For more information, see [Manage Connection States](../../en/Real-time-Messaging/reconnecting_oc.md) for more information. 

## Sending a peer-to-peer message

| Method                        | Signaling            | Real-time Messaging                                 |
| ----------------------------- | -------------------- | --------------------------------------------------- |
| Creates a message instance.   | N/A                  | [initWithText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/initWithText:)<sup>1</sup>                          |
| Sends a peer-to-peer message. | `messageInstantSend` | [sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) |

| Event                                  | Signaling                 | Real-time Messaging                        |
| -------------------------------------- | ------------------------- | ------------------------------------------ |
| Peer-to-peer message sending succeeds. | `onMessageSendSuccess`    | [AgoraRtmSendPeerMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendPeerMessageBlock.html)<sup>2</sup> |
| Peer-to-peer message sending fails.    | `onMessageSendError`      | [AgoraRtmSendPeerMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendPeerMessageBlock.html)             |
| Receives a peer-to-peer message        | `onMessageInstantReceive` | [rtmKit:messageReceived:fromPeer:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:messageReceived:fromPeer:)         |

> <sup>1</sup> With the Agora RTM SDK, you must create a message instance before sending it. A message instance can be used either for a peer-to-peer or for a channel message. As of v0.9.3, the Agora RTM SDK allows you to send an offline message by configuring [AgoraRtmSendMessageOptions](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmSendMessageOptions.html).
>
> <sup>2</sup> The Agora Signaling SDK returns the `onMessageSendSuccess` callback when the server receives the peer-to-peer message; the Agora RTM SDK returns the `AgoraRtmSendPeerMessageErrorOk` error code when the specified user receives the message. 

## Querying the online status of specified user(s)

| Method                                         | Signaling            | Real-time Messaging      |
| ---------------------------------------------- | -------------------- | ------------------------ |
| Queries the online status of a specified user. | `queryuserStatus`    | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) |



| Event                            | Signaling                 | Real-time Messaging             |
| -------------------------------- | ------------------------- | ------------------------------- |
| Returns the result of the query. | `OnQueryUserStatusResult` | [AgoraRtmQueryPeersOnlineBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmQueryPeersOnlineBlock.html) |

> With the Agora RTM SDK,  you can query the online status of a list of peer users, not of just one peer user.

## User attribute operations

| Method                                              | Signaling        | Real-time Messaging                   |
| --------------------------------------------------- | ---------------- | ------------------------------------- |
| Sets the local user's attribute                     | `setAttr`        | [addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)      |
| Gets an attribute of the local user.                | `getAttr`        | [getUserAttributesBykeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:)<sup>1</sup> |
| Gets all attributes of the local user.              | `getAttrAll`     | [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)<sup>2</sup>       |
| Gets all attributes of the specified user.          | `getUserAttrAll` | [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)                   |
| Replaces the local user's attributes with new ones. | N/A              | [setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)              |
| Deletes the specified attributes of the local user. | N/A              | [deleteLocaluserAttributeByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)      |
| Clears the local user's attributes                  | N/A              | [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:)            |
|                                                     |                  |                                       |

| Event                                                 | Signaling               | Real-time Messaging                                          |
| ----------------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| Returns the result of the user-attribute method call. | `onUserAttributeResult` | <li>[AgoraRtmSetLocalUserAttributesBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSetLocalUserAttributesBlock.html) <li>[AgoraRtmAddOrUpdateLocalUserAttributesBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmAddOrUpdateLocalUserAttributesBlock.html) <li>[AgoraRtmDeleteLocalUserAttributesBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmDeleteLocalUserAttributesBlock.html) <li> [AgoraRtmClearLocalUserAttributesBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmClearLocalUserAttributesBlock.html) <li> [AgoraRtmGetuserAttributesBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmGetUserAttributesBlock.html) |

> - <sup>1</sup> The [getuserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:) method allows you to retrieve a list of attributes from the local user.
> - <sup>2</sup> The [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:) method allows you to retrieve the attributes from either the local user or a specified peer user. 

## Joining or Leaving a Channel

| Method                      | Signaling      | Real-time Messaging               |
| --------------------------- | -------------- | --------------------------------- |
| Creates a channel instance. | N/A            | [createChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:)<sup>1</sup> |
| Joins a specified channel.  | `channelJoin`  | [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:)<sup>2</sup>  |
| Leaves a channel.           | `channelLeave` | [leaveWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/leaveWithCompletion:)             |

| Event                                                   | Signaling             | Real-time Messaging         |
| ------------------------------------------------------- | --------------------- | --------------------------- |
| Successfully joins the specified channel.               | `onChannelJoined`     | [AgoraRtmJoinChannelBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmJoinChannelBlock.html)  |
| Fails to join the specified channel                     | `onChannelJoinFailed` | [AgoraRtmJoinChannelBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmJoinChannelBlock.html)  |
| A remote user joins the current channel.                | `onChannelUserJoined` | [memberJoined](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberJoined:)               |
| Successfully leaves the current channel.                | `onChannelLeaved`     | [AgoraRtmLeaveChannelBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLeaveChannelBlock.html) |
| A remote channel member leaves the current channel.     | `onChannelUserLeaved` | [memberLeft](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberLeft:)                |
| Returns a channel member list when joining the channel. | `onChannelUserList`   | N/A<sup>3</sup>             |
|                                                         |                       |                             |

> - <sup>1</sup> The Agora RTM SDK requires you to create a channel instance before joining it. 
> - <sup>2</sup> The Agora RTM SDK allows you to join a maximum of 20 channels at the same time. 
> - <sup>3</sup> The Agora RTM SDK does not return to the user a member list of the channel when he/she successfully joins it.
> - The Agora RTM SDK does not support a special channel as the Agora Signaling SDK does. 

## Sending a channel message

| Method                                          | Signaling                 | Real-time Messaging                   |
| ----------------------------------------------- | ------------------------- | ------------------------------------- |
| Creates a message instance.                     | N/A                       | [initWithText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/initWithText:)<sup>1</sup>            |
| Sends a channel message from within a channel.  | `messageChannelSend`      | [sendMessage:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:)<sup>2</sup> |
| Sends a channel message from outside a channel. | `messageChannelSendForce` | N/A                                   |



| Event                                     | Signaling                 | Real-time Messaging               |
| ----------------------------------------- | ------------------------- | --------------------------------- |
| Successfully sends out a channel message. | `onMessageSendSuccess`    | [AgoraRtmSendChannelMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendChannelMessageBlock.html) |
| Fails to send out a channel message.      | `onMessageSendError`      | [AgoraRtmSendChannelMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendChannelMessageBlock.html) |
| Receives a channel message.               | `onMessageChannelReceive` | [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:)<sup>3</sup>     |



> - <sup>1</sup> With the Agora RTM SDK, you must create a message instance before sending out a peer-to-peer or channel message. 
> - <sup>2</sup> The Agora RTM SDK does not support sending channel messages from outside a channel. That said, you must join a channel before being able to send out a channel message. 
> - <sup>3</sup> The [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:) callback is returned to the remote channel members, not to the message sender. 

## Channel attribute operations

| Method                               | Signaling          | Real-time Messaging |
| ------------------------------------ | ------------------ | ------------------- |
| Sets a channel attribute.            | `channelSetAttr`   | [addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateChannel:Attributes:Options:completion:)<sup>1</sup>   |
| Deletes a channel attribute.         | `channelDelAttr`   | [deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteChannel:AttributesByKeys:Options:completion:)<sup>1</sup>   |
| Deletes all attributes of a channel. | `channelClearAttr` |  [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearChannel:Options:AttributesWithCompletion:)<sup>1</sup>   |
| Substitutes the attributes of a specified channel with new ones | N/A | [setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setChannel:Attributes:Options:completion:)<sup>1</sup>   |
| Gets all attributes of a specified channel | N/A | [getChannelAllAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:) |
| Gets the attributes of a specified channel by attribute keys | N/A | [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAttributes:ByKeys:completion:) |

> <sup>1</sup> With the Agora RTM SDK, you can update the attribute(s) of a channel without the need to join it. 

| Event                           | Signaling              | Real-time Messaging |
| ------------------------------- | ---------------------- | ------------------- |
| A channel attribute is updated. | `onChannelAttrUpdated` | [attributesUpdate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:attributeUpdate:)<sup>2</sup>  |

> <sup>2</sup>  This callback is disabled by default. It is enabled only when the user, who updates the attributes of the channel, sets [enableNotificationToChannelMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannelAttributeOptions.html#//api/name/enableNotificationToChannelMembers) as YES.

## Retrieving a member list of the current channel

| Method                                                 | Signaling | Real-time Messaging      |
| ------------------------------------------------------ | --------- | ------------------------ |
| Retrieves the latest user list of a specified channel. | `invoke`  | [getMembersWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:)<sup>1</sup> |



| Event                                         | Signaling     | Real-time Messaging       |
| --------------------------------------------- | ------------- | ------------------------- |
| Returns the user list in a specified channel. | `onInvokeRet` | [AgoraRtmGetMembersBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmGetMembersBlock.html) |

> <sup>1</sup> You must join an RtmChannel before retrieving a member list of it. When the number of the channel members exceeds 512, the Agora RTM SDK only returns a list of randomly selected 512 channel members. 



## Retrieving the Number of Users of a Specified Channel

| Method                                                | Signaling             | Real-time Messaging |
| ----------------------------------------------------- | --------------------- | ------------------- |
| Retrieves the number of users in a specified channel. | `channelQueryUserNum` | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4)<sup>1</sup>   |

> <sup>1</sup>  The Agora RTM SDK supports retrieving the member count of up to 32 channels. 

| Event                                               | Signaling                     | Real-time Messaging |
| --------------------------------------------------- | ----------------------------- | ------------------- |
| Returns the number of users in a specified channel. | `onChannelQueryUserNumResult` | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)   |

## Call Invitation



| Method                                                       | Signaling                                | Real-time Messaging                 |
| ------------------------------------------------------------ | ---------------------------------------- | ----------------------------------- |
| Gets an RTM call manager.                                    | N/A                                      | [getRtmCallKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getRtmCallKit)<sup>1</sup>         |
| Allows the caller to create and manage a `LocalInvitation.`  | N/A                                      | [initWithCalleeId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html#//api/name/initWithCalleeId:)<sup>2</sup>      |
| Allows the caller to send a call invite to a specified user (callee). | `channelInviteUser`/`channelInviteUser2` | [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:)<sup>3</sup>   |
| Allows the caller to send a call invite to land-line user.   | `channelInviteDTMF`                      | N/A                                 |
| Allows the caller to cancel a sent call invite.              | `channelInviteEnd`                       | [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:)<sup>4</sup> |
| Allows the callee to accept an incoming call invite.         | `channelInviteAccept`                    | [acceptRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:)            |
| Allows the callee to decline an incoming call invite.        | `channelInviteRefuse`                    | [refuseRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:)            |
|                                                              |                                          |                                     |

> - <sup>1</sup> With the Agora RTM SDK, either a caller or a callee needs to get an [AgoraRtmCallKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getRtmCallKit) instance before using its object methods to send, cancel, accept, or decline a call invite. 
> - <sup>2</sup> The Agora RTM SDK introduces the [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) object and the [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) object. The former is created by the caller using the `initWithCalleeId` method, the latter is created automatically by the SDK when the callee receives the call invitation from the caller. You can take the two objects as the same call invitation taking in two different forms. The caller uses the [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) object to specify the callee, set the content, or check the [AgoraRtmLocalInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationState.html); the callee uses the [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) object to set a response, check the caller ID, or check the [AgoraRtmRemoteInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationState.html). 
> - <sup>3</sup> The [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:) method does not has an `extra` argument as the `channelInviteUser2` method does. 
> - <sup>4</sup> The `channelInviteEnd` method can end a call invite any time, whilst the [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:) method can only cancel a sent and ongoing call invite. 
> - To intercommunicate with the Agora Signaling SDK, you must upgrade your Agora RTM SDK to v1.0+ and set the channel ID.  Please also note that even if the callee accepts the call invite, the Agora RTM SDK does not add either the caller or the callee to the specified channel. 

| Synchronous Callback      | Signaling | Real-time Messaging                                          |
| ------------------------- | --------- | ------------------------------------------------------------ |
| The method call succeeds. | N/A       | <li> [AgoraRtmLocalinvitationSendBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationSendBlock.html) <li> [AgoraRtmLocalInvitationCancelBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationCancelBlock.html) <li> [AgoraRtmRemoteInvitationAcceptBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationAcceptBlock.html) <li> [AgoraRtmRemoteinvitationRefuseBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationRefuseBlock.html) |
| The method call fails.    | N/A       | <li> [AgoraRtmLocalinvitationSendBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationSendBlock.html) <li> [AgoraRtmLocalInvitationCancelBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLocalInvitationCancelBlock.html) <li> [AgoraRtmRemoteInvitationAcceptBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationAcceptBlock.html) <li> [AgoraRtmRemoteinvitationRefuseBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRemoteInvitationRefuseBlock.html). See [AgoraRtmInvitationApiCallError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmInvitationApiCallErrorCode.html) for the error codes.<sup>5</sup> |

> <sup>5</sup> If a user calls [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/sendLocalInvitation:completion:), [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/cancelLocalInvitation:completion:), [acceptRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/acceptRemoteInvitation:completion:) or [refuseRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmCallKit.html#//api/name/refuseRemoteInvitation:completion:) before the life cycle of a [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) starts or after the life cycle of an [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) ends (either in failure or in success), the SDK returns the [AgoraRtmInvitationApiCallError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmInvitationApiCallErrorCode.html) error code. 



| Event                                                        | Signaling                | Real-time Messaging                                          |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| Returns to the caller: the callee receives the call invite.  | `oninviteReceivedByPeer` | [localInvitationReceivedByPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationReceivedByPeer:)                              |
| Returns to the caller: the call invite successfully cancelled. | `onInviteEndByMyself`    | [localInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationCanceled:)                                    |
| Returns to the caller: call invite accepted.                 | `onInviteAcceptedByPeer` | [localInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationAccepted:withResponse:)                                    |
| Returns to the caller: call invite declined.                 | `onInviteRefusedByPeer`  | [localInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationRefused:withResponse:)                                     |
| Returns to the caller: the outgoing call invite ends in failure. | `onInviteFailed`         | [localInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:). See [AgoraRtmLocalInvitationErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmLocalInvitationErrorCode.html) for the error codes. <sup>6</sup> |
|                                                              |                          |                                                              |

> <sup>6</sup>: The SDK returns the [localInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:localInvitationFailure:errorCode:) to the caller if the call invitation process has started but ends in failure. Scenarios include: the callee is offline, the [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) object times out, the [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html) object expires, and the callee receives the call invitation but fails to respond in time. 

| Event                                                   | Signaling           | Real-time Messaging                                          |
| ------------------------------------------------------- | ------------------- | ------------------------------------------------------------ |
| Returns to the land-line user: receives a call invite.  | `onInviteMsg`       | N/A                                                          |
| Returns to the callee: receives a call invite.          | `oninviteReceived`  | [remoteInvitationReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationReceived:)                                   |
| Returns to the callee: the caller cancels the invite.   | `onInviteEndByPeer` | [remoteInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationCanceled:)                                   |
| Returns to the callee: call invite accepted.            | N/A                 | [remoteInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationAccepted:)                                   |
| Returns to the callee: call invite declined.            | N/A                 | [remoteInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationRefused:)                                    |
| Returns to the callee: the call invite ends in failure. | N/A                 | [remoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:). See [AgoraRtmRemoteInvitationErrorCode](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Constants/AgoraRtmRemoteInvitationErrorCode.html) for the error codes. <sup>7</sup> |
|                                                         |                     |                                                              |

> <sup>7</sup> The SDK returns the [remoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmCallDelegate.html#//api/name/rtmCallKit:remoteInvitationFailure:errorCode:) to the callee if the call invitation process has started but ends in failure. Scenarios include: the caller is offline, the [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) object times out, and the [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html) ojbect expires. 

## Renewing a Token



| Method                    | Signaling | Real-time Messaging |
| ------------------------- | --------- | ------------------- |
| Renews the current Token. | N/A       | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:)         |



| Event                                  | Signaling | Real-time Messaging       |
| -------------------------------------- | --------- | ------------------------- |
| Returns the result of the method call. | N/A       | [AgoraRtmRenewTokenBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmRenewTokenBlock.html) |
| The token has expired.                 | N/A       | [rtmTokenDidExpire:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKitTokenDidExpire:)      |

