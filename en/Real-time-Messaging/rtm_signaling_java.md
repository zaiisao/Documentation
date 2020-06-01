
---
title: Signaling vs. Agora RTM SDK
description: 
platform: Linux Java
updatedAt: Mon Jun 01 2020 02:32:17 GMT+0800 (CST)
---
# Signaling vs. Agora RTM SDK
This page juxtaposes the legacy Agora Signaling APIs with the Agora Real-time Messaging APIs. 

## Login & Logout

| Method                 | Signaling                              | Real-time Messaging                  |
| ---------------------- | -------------------------------------- | ------------------------------------ |
| Creates an instance.   | `getInstance`/`createAgoraSDKInstance` | [createInstance](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a0bd5585641a955cbb54f279a1dae55df)<sup>1</sup>         |
| Sets a callback.       | `callbackSet`                          | N/A                                  |
| Login                  | `login`/`login2`                       | [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a)<sup>2</sup>                  |
| Logout                 | `logout`                               | [logout](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6f5695854e251ddd4ba05547ab47b317)                             |
| Gets the login status. | `getStatus`                            | N/A. See [onConnectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#a9b6f86cb2d7d5ec4adf0b6d645c16bf9). |
| Destroys the instance. | `destroy`                              | [release](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a5147d00d14afeebf0926b0d2f01079f5)                            |

| Event                     | Signaling             | Real-time Messaging        |
| ------------------------- | --------------------- | -------------------------- |
| Login succeeds.           | `onLoginSuccess`      | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)                |
| Login Fails.              | `onLoginFailed`       | [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)               |
| Logout results.           | `onLogout`            | [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)                |
| Connection state changes. | N/A. See `getStatus`. | [onConnectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#a9b6f86cb2d7d5ec4adf0b6d645c16bf9) |

> - Unless otherwise specified, most of the core APIs of the Agora RTM Android SDK should only be called after the [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) method call succeeds and after you receive the [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) callback.
> - <sup>1</sup> You can create multiple RtmClient instances with the [createInstance](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a0bd5585641a955cbb54f279a1dae55df) method. The Agora RTM SDK does not put a limit to the number of RtmClient instances you can create, but it only allows you to join a maximum of 20 RtmChannels at the same time. 
> - <sup>2</sup> The generation of the token you use to log in the Agora RTM system differs from the generation of the signalingToken you use to log in the Agora Signaling system. Make sure you use the right token. See [Token Security](../../en/Real-time-Messaging/rtm_token.md) for more information.
> - <sup>2</sup> The token debugging mechanism, "\_no\_need\_token" for example, of the Agora Signaling SDK does not apply to the Agora RTM SDK. 
> - <sup>2</sup> The way that the Agora RTM SDK connects or reconnects to the Agora RTM system is completely different either. For more information, see [Manage Connection States](../../en/Real-time-Messaging/reconnecting_java.md) for more information. 

## Sending a peer-to-peer message

| Method                        | Signaling            | Real-time Messaging         |
| ----------------------------- | -------------------- | --------------------------- |
| Creates a message instance.   | N/A                  | [createMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3)<sup>1</sup> |
| Sends a peer-to-peer message. | `messageInstantSend` | [sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9)         |

| Event                                  | Signaling                 | Real-time Messaging     |
| -------------------------------------- | ------------------------- | ----------------------- |
| Peer-to-peer message sending succeeds. | `onMessageSendSuccess`    | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)<sup>2</sup> |
| Peer-to-peer message sending fails.    | `onMessageSendError`      | [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)             |
| Receives a peer-to-peer message        | `onMessageInstantReceive` | [onMessageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#af760814981718fb31d88acb8251d19b6)     |

> <sup>1</sup> With the Agora RTM SDK, you must create a message instance before sending it. A message instance can be used either for a peer-to-peer or for a channel message. As of v0.9.3, the Agora RTM SDK allows you to send an offline message by configuring [sendMessageOptions](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_send_message_options.html).
>
> <sup>2</sup> The Agora Signaling SDK returns the `onMessageSendSuccess` callback when the server receives the peer-to-peer message; the Agora RTM SDK returns the [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5) callback when the specified user receives the message. 

## Querying the online status of specified user(s)

| Method                                         | Signaling            | Real-time Messaging      |
| ---------------------------------------------- | -------------------- | ------------------------ |
| Queries the online status of a specified user. | `queryuserStatus`    | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) |



| Event                            | Signaling                 | Real-time Messaging |
| -------------------------------- | ------------------------- | ------------------- |
| Returns the result of the query. | `OnQueryUserStatusResult` | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)         |

> With the Agora RTM SDK,  you can query the online status of a list of peer users, not of just one peer user.

## User attribute operations

| Method                                              | Signaling        | Real-time Messaging                   |
| --------------------------------------------------- | ---------------- | ------------------------------------- |
| Sets the local user's attribute                     | `setAttr`        | [addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)      |
| Gets an attribute of the local user.                | `getAttr`        | [getUserAttributesBykeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193)<sup>1</sup> |
| Gets all attributes of the local user.              | `getAttrAll`     | [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)<sup>2</sup>       |
| Gets all attributes of the specified user.          | `getUserAttrAll` | [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)                   |
| Replaces the local user's attributes with new ones. | N/A              | [setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a339b7b2371ff2b86137b6db6c1c66294)              |
| Deletes the specified attributes of the local user. | N/A              | [deleteLocaluserAttributeByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)      |
| Clears the local user's attributes                  | N/A              | [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785)            |
|                                                     |                  |                                       |

| Event                                                 | Signaling               | Real-time Messaging     |
| ----------------------------------------------------- | ----------------------- | ----------------------- |
| Returns the result of the user-attribute method call. | `onUserAttributeResult` | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |

> - <sup>1</sup> The [getuserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193) method allows you to retrieve a list of attributes from the local user.
> - <sup>2</sup> The [getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755) method allows you to retrieve the attributes from either the local user or a specified peer user. 
> - The Agora RTM SDK does not support special attributes for now. 

## Joining or Leaving a Channel

| Method                      | Signaling      | Real-time Messaging         |
| --------------------------- | -------------- | --------------------------- |
| Creates a channel instance. | N/A            | [createChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a)<sup>1</sup> |
| Joins a specified channel.  | `channelJoin`  | [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40)<sup>2</sup>          |
| Leaves a channel.           | `channelLeave` | [leave](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a9e0b6aad17bfceb3c9c939351a467d14)                     |

| Event                                                   | Signaling             | Real-time Messaging |
| ------------------------------------------------------- | --------------------- | ------------------- |
| Successfully joins the specified channel.               | `onChannelJoined`     | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)         |
| Fails to join the specified channel                     | `onChannelJoinFailed` | [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)         |
| A remote user joins the current channel.                | `onChannelUserJoined` | [onMemberJoined](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a9d39a66a0a17ebbb6c8e5c5e520100dc)    |
| Successfully leaves the current channel.                | `onChannelLeaved`     | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)         |
| A remote channel member leaves the current channel.     | `onChannelUserLeaved` | [onMemberLeft](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#acc14e332fefe76d16571c14665c2cb1c)      |
| Returns a channel member list when joining the channel. | `onChannelUserList`   | N/A<sup>3</sup>     |
|                                                         |                       |                     |

> - <sup>1</sup> The Agora RTM SDK requires you to create a channel instance before joining it. 
> - <sup>2</sup> The Agora RTM SDK allows you to join a maximum of 20 channels at the same time. 
> - <sup>3</sup> The Agora RTM SDK does not return to the user a member list of the channel when he/she successfully joins it.
> - The Agora RTM SDK does not support a special channel as the Agora Signaling SDK does. 

## Sending a channel message

| Method                                          | Signaling                 | Real-time Messaging         |
| ----------------------------------------------- | ------------------------- | --------------------------- |
| Creates a message instance.                     | N/A                       | [createMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3)<sup>1</sup> |
| Sends a channel message from within a channel.  | `messageChannelSend`      | [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0)<sup>2</sup>   |
| Sends a channel message from outside a channel. | `messageChannelSendForce` | N/A                         |



| Event                                     | Signaling                 | Real-time Messaging             |
| ----------------------------------------- | ------------------------- | ------------------------------- |
| Successfully sends out a channel message. | `onMessageSendSuccess`    | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)                     |
| Fails to send out a channel message.      | `onMessageSendError`      | [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)                     |
| Receives a channel message.               | `onMessageChannelReceive` | [onMessageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2d527e4040bcabf038533810adbd296c)<sup>3</sup> |



> - <sup>1</sup> With the Agora RTM SDK, you must create a message instance before sending out a peer-to-peer or channel message. 
> - <sup>2</sup> The Agora RTM SDK does not support sending channel messages from outside a channel. That said, you must join a channel before being able to send out a channel message. 
> - <sup>3</sup> The [onMessageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2d527e4040bcabf038533810adbd296c) callback is returned to the remote channel members, not to the message sender. 

## Channel attribute operations

| Method                               | Signaling          | Real-time Messaging |
| ------------------------------------ | ------------------ | ------------------- |
| Sets a channel attribute.            | `channelSetAttr`   | [addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23)   |
| Deletes a channel attribute.         | `channelDelAttr`   | [deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a4cbf3329abda4940b73a75455cd1dc06)   |
| Deletes all attributes of a channel. | `channelClearAttr` |  [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6ed0ef4baacda8fa00eda5373d17f59f) |


| Event                           | Signaling              | Real-time Messaging |
| ------------------------------- | ---------------------- | ------------------- |
| A channel attribute is updated. | `onChannelAttrUpdated` | [onAttributesUpdated](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html#a2904a1f1f78c497b9176fffb853be96f)   |


## Retrieving a member list of the current channel

| Method                                                 | Signaling | Real-time Messaging      |
| ------------------------------------------------------ | --------- | ------------------------ |
| Retrieves the latest user list of a specified channel. | `invoke`  | [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) |



| Event                                         | Signaling     | Real-time Messaging     |
| --------------------------------------------- | ------------- | ----------------------- |
| Returns the user list in a specified channel. | `onInvokeRet` | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |



## Retrieving the Number of Users of a Specified Channel

| Method                                                | Signaling             | Real-time Messaging |
| ----------------------------------------------------- | --------------------- | ------------------- |
| Retrieves the number of users in a specified channel. | `channelQueryUserNum` | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4)   |

| Event                                               | Signaling                     | Real-time Messaging |
| --------------------------------------------------- | ----------------------------- | ------------------- |
| Returns the number of users in a specified channel. | `onChannelQueryUserNumResult` | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396)   |

## Call Invitation




| Method                                                       | Signaling                                | Real-time Messaging                 |
| ------------------------------------------------------------ | ---------------------------------------- | ----------------------------------- |
| Gets an RTM call manager.                                    | N/A                                      | [getRtmCallManager](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ab3ae053d5617855ddbbdae9149067cfd)<sup>1</sup>     |
| Allows the caller to create and manage a `LocalInvitation.`  | N/A                                      | [createLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a1756dca077267acaa407c6901daa2248)<sup>2</sup> |
| Allows the caller to send a call invite to a specified user (callee). | `channelInviteUser`/`channelInviteUser2` | [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353)<sup>3</sup>   |
| Allows the caller to send a call invite to land-line user.   | `channelInviteDTMF`                      | N/A                                 |
| Allows the caller to cancel a sent call invite.              | `channelInviteEnd`                       | [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564)<sup>4</sup> |
| Allows the callee to accept an incoming call invite.         | `channelInviteAccept`                    | [acceptRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f6f97c84e426e2fbd8a5dda71e2fc6c)            |
| Allows the callee to decline an incoming call invite.        | `channelInviteRefuse`                    | [refuseRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a2ce4af944183976d18c055816f756bf6)            |
|                                                              |                                          |                                     |

> - <sup>1</sup> With the Agora RTM SDK, either a caller or a callee needs to get an [RtmCallManager](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ab3ae053d5617855ddbbdae9149067cfd) instance before using its object methods to send, cancel, accept, or decline a call invite. 
> - <sup>2</sup> The Agora RTM SDK introduces the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) object and the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) object. The former is created by the caller using the [createLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a1756dca077267acaa407c6901daa2248) method, the latter is created automatically by the SDK when the callee receives the call invitation from the caller. You can take the two objects as the same call invitation taking in two different forms. The caller uses the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) object to specify the callee, set the content, or check the [LocalInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_state.html); the callee uses the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) object to set a response, check the caller ID, or check the [RemoteInvitationState](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_state.html). 
> - <sup>3</sup> The [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353) method does not has an `extra` argument as the `channelInviteUser2` method does. 
> - <sup>4</sup> The `channelInviteEnd` method can end a call invite any time, whilst the [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564) method can only cancel a sent and ongoing call invite. 
> - To intercommunicate with the Agora Signaling SDK, you must upgrade your Agora RTM SDK to v1.0+ and set the channel ID.  Please also note that even if the callee accepts the call invite, the Agora RTM SDK does not add either the caller or the callee to the specified channel. 

| Synchronous Callback      | Signaling | Real-time Messaging                                          |
| ------------------------- | --------- | ------------------------------------------------------------ |
| The method call succeeds. | N/A       | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)                                                  |
| The method call fails.    | N/A       | [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396). See [InvitationApiCallError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_invitation_api_call_error.html) for the error codes.<sup>5</sup> |

> <sup>5</sup> If a user calls [sendLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#af899697061305ca840e829b92c78e353), [cancelLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f03bfe1cfd6987fbc7b5a4dc484f564), [acceptRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a5f6f97c84e426e2fbd8a5dda71e2fc6c) or [refuseRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_call_manager.html#a2ce4af944183976d18c055816f756bf6) before the life cycle of a [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) starts or after the life cycle of a [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) ends (either in failure or in success), the SDK returns the [onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) callback with the [InvitationApiCallError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_invitation_api_call_error.html) error code. 


| Event                                                        | Signaling                | Real-time Messaging                                          |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| Returns to the caller: the callee receives the call invite.  | `oninviteReceivedByPeer` | [onLocalInvitationReceivedByPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a24e1cb71d3e752963da49bdf91847788) |
| Returns to the caller: the call invite successfully cancelled. | `onInviteEndByMyself`    | [onLocalInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#ae3164e81772cd4d6171165b1705adcaa) |
| Returns to the caller: call invite accepted.                 | `onInviteAcceptedByPeer` | [onLocalInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a4dece02a62a187a66c2415fecf6b75dc) |
| Returns to the caller: call invite declined.                 | `onInviteRefusedByPeer`  | [onLocalInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6224643c400268d356cb5d489825bdd0) |
| Returns to the caller: the outgoing call invite ends in failure. | `onInviteFailed`         | [onLocalInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#acfefb97eaca497cbd71a0c1cbf5067b0). See [LocalInvitationError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_local_invitation_error.html) for the error codes. <sup>6</sup> |
|                                                              |                          |                                                              |

> <sup>6</sup>: The SDK returns the [onLocalInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#acfefb97eaca497cbd71a0c1cbf5067b0) to the caller if the call invitation process has started but ends in failure. Scenarios include: the callee is offline, the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) object times out, the [LocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html) object expires, and the callee receives the call invitation but fails to respond in time. 

| Event                                                   | Signaling           | Real-time Messaging                                          |
| ------------------------------------------------------- | ------------------- | ------------------------------------------------------------ |
| Returns to the land-line user: receives a call invite.  | `onInviteMsg`       | N/A                                                          |
| Returns to the callee: receives a call invite.          | `oninviteReceived`  | [onRemoteInvitationReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a8d01498a993c4016aa45ccb9bf4e9097) |
| Returns to the callee: the caller cancels the invite.   | `onInviteEndByPeer` | [onRemoteInvitationCanceled](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a9d0409c87455d4d2b1315f67a5f7aa12) |
| Returns to the callee: call invite accepted.            | N/A                 | [onRemoteInvitationAccepted](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a81d9d3de89d08c41408d8a94c8309d29) |
| Returns to the callee: call invite declined.            | N/A                 | [onRemoteInvitationRefused](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a7a21eaa9ff49bcf39e3c49b94f6e6ac7) |
| Returns to the callee: the call invite ends in failure. | N/A                 | [onRemoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6f9f2bbbfbcb0a766c6f1b2e4a8314a1). See [RemoteInvitationError](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_status_code_1_1_remote_invitation_error.html) for the error codes. <sup>7</sup> |
|                                                         |                     |                                                              |

> <sup>7</sup> The SDK returns the [onRemoteInvitationFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_call_event_listener.html#a6f9f2bbbfbcb0a766c6f1b2e4a8314a1) to the callee if the call invitation process has started but ends in failure. Scenarios include: the caller is offline, the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) object times out, and the [RemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html) ojbect expires. 

## Renewing a Token



| Method                    | Signaling | Real-time Messaging |
| ------------------------- | --------- | ------------------- |
| Renews the current Token. | N/A       | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353)         |



| Event                                  | Signaling | Real-time Messaging     |
| -------------------------------------- | --------- | ----------------------- |
| Returns the result of the method call. | N/A       | [onSuccess](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)/[onFailure](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a1f9145a3eb119e32cfc0afa938062396) |
| The token has expired.                 | N/A       | [onTokenExpired](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#aef74f37ed8797d274115d7f13785134e)        |
