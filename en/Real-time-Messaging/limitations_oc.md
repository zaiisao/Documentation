
---
title: RTM Limitations
description: 
platform: iOS,macOS
updatedAt: Mon Nov 25 2019 10:09:07 GMT+0800 (CST)
---
# RTM Limitations

This page provides a brief overview of the limitations of the Agora RTM Objective-C SDK for iOS or Agora RTM Objective-C SDK for macOS, including the maximum method call frequencies, maximum string length, unicode support and more.



## Maximum Call Frequency

| Function                                                    | Method                                                       | Maximum Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in the Agora RTM system                                | [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | Two queries per second         |
| Retrieve member count of specified channel(s) | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelMemberCount:completion:) | one query per second |
| Join a channel<sup>1</sup> | [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) | 50 queries every three seconds |
| Send messages| <li>[sendMessage:toPeer:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) <li>[sendMessage:toPeer:sendMessageOptions:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:) <li>And [sendMessage:completion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:) taken together | 60 queries per second          |
| Retrieve a member list of the channel                      | [getMembersWithCompletion:](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | Five queries every two seconds |
| Renew the token | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) | Two queries per second |
| Query the online status of the specified users. | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) | Ten queries every five seconds |
| Set user attributes | <li>[setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)<li>And [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:) taken together | 10 queries every five seconds          |
| Get user attributes | <li>[getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)<li>And [getUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:) taken together | 40 queries every five seconds         |
| Set channel attributes | <li>[setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setChannel:Attributes:Options:completion:)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateChannel:Attributes:Options:completion:)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteChannel:AttributesByKeys:Options:completion:)<li>And [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearChannel:Options:AttributesWithCompletion:) taken together | 10 queries every five seconds          |
| Get channel attributes | <li>[getChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:)<li>And [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAttributes:ByKeys:completion:) taken together | 10 queries every five seconds         |
| Subscribes to the online status of the specified user(s) | [subscribePeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/subscribePeersOnlineStatus:completion:) | Ten queries every five seconds. |
| Unsubscribes from the online status of the specified user(s) | [unsubscribePeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/unsubscribePeersOnlineStatus:completion:) | Ten queries every five seconds. |
| Gets a list of the peers, to whose specific status you have subscribed. | [queryPeersBySubscriptionOption](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersBySubscriptionOption:completion:) | Ten queries every five seconds. |

> <sup>1</sup> The maximum call frequency limit for joining the same channel is two queries every five seconds. 

## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html).
- The maximum length of the content in a call invitation is 8 KB. See [AgoraRtmLocalInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html).
- The maximum length of the response in a call invitation is 8 KB. See [AgoraRtmRemoteInvitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html).

## Unicode support 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. If you have special requirements, contact sales-us@agora.io.
- The current version supports querying the online status of a maximum of 256 users.
- Attribute settings in one user attribute operation should not exceed 16 KB in size; attribute settings in one channel attribute operation should not exceed 32 KB in size; each attribute (key/value pair) should not excced 8 KB in size; the number of key/value pairs you set in one attribute operation should not exceed 32. 
