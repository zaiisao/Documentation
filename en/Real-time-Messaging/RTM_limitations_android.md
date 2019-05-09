
---
title: RTM Limitations
description: 
platform: Android
updatedAt: Thu May 09 2019 08:34:16 GMT+0800 (CST)
---
# RTM Limitations
This page provides information about the limitations of the Agora RTM Java SDK for Android v0.9.1

## Multiple Instances

Supports a maximum of 20 `RtmChannel` instances at the same time. If you call `createChannel()` after the number of `RtmChannel` instances reaches 20, the SDK throws exceptions. As a good practice, we recommend calling the `RtmChannel.release()` method to release all resources of unused `RtmChannel` instances.

## Call Frequencies

| Function                                                    | Method                                                       | Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Logs in the Agora RTM system                                | [RtmClient.login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) | Two queries per second         |
| Sends messages (peer-to-peer and channel messages combined) | [RtmClient.sendMessageToPeer()](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a) and [RtmChannel.SendMessage()](https://docs.agora.io/en/Real-time-Messaging/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0) combined | 60 queries per second          |
| Retrieves a member list of the channel                      | [RtmChannel.getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) | Five queries every two seconds |
| Renews the token| [RtmClient.renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) | Two queries per second |
| Queries the online status of the specified user(s) | [RtmClient.queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) | Ten queries every five seconds. |

## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [RtmMessage.setText()](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_javaclassio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2).
- The maximum length of the content in a call invitation is 8 KB. See [LocalInvitation.setContent()](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445)
- The maximum length of the response in a call invitation is 8 KB. See [RemoteInvitation.setResponse()](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1)

## Encoding 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. If you have special requirements, contact sales-us@agora.io.
- v0.9.2 supports querying the online status of a maximum of 256 users.
