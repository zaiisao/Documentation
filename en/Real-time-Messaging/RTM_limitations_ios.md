
---
title: RTM Limitations
description: 
platform: Android
updatedAt: Thu Apr 25 2019 08:06:12 GMT+0800 (CST)
---
# RTM Limitations
This page provides information about the limitations of the Agora RTM Java SDK for Android v0.9.1

## Multiple Instances

Supports a maximum of 20 `AgoraRtmChannel` instances at the same time. If you call `createChannelWithId:delegate` after the number of `AgoraRtmChannel` instances reaches the limit of 20, the SDK returns `nil`. 

## Call frequencies

| Function                                                    | Method                                                       | Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Logs in the Agora RTM system                                | [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | Two queries per second         |
| Sends messages (peer-to-peer and channel messages combined) | [sendMessage:toPeer:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) and [RtmChannel.sendMessage:completion:](https://docs.agora.io/en/Real-time-Messaging/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:) combined | 60 queries per second          |
| Retrieves a member list of the channel                      | [getMembersWithCompletion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | Five queries every two seconds |

## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html).
- The maximum length of the content in a call invitation is 8 KB. See [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445)
- The maximum length of the response in a call invitation is 8 KB. See [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1)

## Encoding 

Supports channel and peer-to-peer messages in UTF-8 only. 

## Miscellaneous 

Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. If you have special requirements, contact sales-us@agora.io.
