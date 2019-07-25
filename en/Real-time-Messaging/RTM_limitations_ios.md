
---
title: RTM Limitations
description: 
platform: iOS,macOS
updatedAt: Thu Jul 25 2019 03:32:37 GMT+0800 (CST)
---
# RTM Limitations
This page provides information about the limitations of the Agora RTM SDK. 


## Multiple Instances

Supports a maximum of 20 `AgoraRtmChannel` instances at the same time. If you call `createChannelWithId:delegate` after the number of `AgoraRtmChannel` instances reaches the limit of 20, the SDK returns `nil`. 

## Call frequencies

| Function                                                    | Method                                                       | Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Logs in the Agora RTM system                                | [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | Two queries per second         |
| Sends messages (peer-to-peer and channel messages combined) | [sendMessage:toPeer:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) and [sendMessage:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:) combined | 60 queries per second          |
| Retrieves a member list of the channel                      | [getMembersWithCompletion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | Five queries every two seconds |
| Renews the token | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) | Two queries per second |
| Queries the online status of the specified users. | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) | Ten queries every five seconds |


## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html).
- The maximum length of the content in a call invitation is 8 KB. See [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html).
- The maximum length of the response in a call invitation is 8 KB. See [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html).

## Encoding 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. If you have special requirements, contact sales-us@agora.io.
- v0.9.2 supports querying the online status of a maximum of 256 users.
