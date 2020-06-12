
---
title: RTM Limitations
description: 
platform: Android
updatedAt: Fri Jun 12 2020 06:23:43 GMT+0800 (CST)
---
# RTM Limitations

This page provides a brief overview of the limitations of the Agora RTM SDK for Android, including API call limit, string size, encoding, and more.


## Maximum Call Frequency

The call limit is for one <code>RtmClient</code> instance. If an operation corresponds to multiple methods, the number of the method calls of an operation equals the sum of the method calls of all corresponding methods in a specific time frame.

<div class="alert note">You can increase the call limit of an API by creating multiple <code>RtmClient</code> instances.</div>

| Function                                                    | Method                                                       | Maximum Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in the RTM system                                | [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) | Two queries per second         |
| Retrieve member count of specified channel(s) | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4) | one query per second |
| Join a channel<sup>1</sup> | [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) | 50 queries every three seconds |
| Send messages | <li>[sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a)<li> [sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9) <li> And [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0) taken together | 60 queries per second          |
| Retrieve a member list of the channel          | [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) | Five queries every two seconds |
| Renew the token| [RtmClient.renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) | Two queries per second |
| Query the online status of the specified user(s) | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) | Ten queries every five seconds. |
| Set user attributes                               | <li>[setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a339b7b2371ff2b86137b6db6c1c66294)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)<li>And [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785) taken together | 10 queries every five seconds   |
| Get user attributes                               | <li>[getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)<li> And [getUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193) taken together | 40 queries every five seconds   |
| Set channel attributes | <li>[setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ad25f51a3671db50e348ec6c170044ec6)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a4cbf3329abda4940b73a75455cd1dc06)<li>And [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6ed0ef4baacda8fa00eda5373d17f59f) taken together | 10 queries every five seconds         |
| Get channel attributes | <li>[getChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6)<li> And [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a358b47f4b42d678fafa76f3f30290e5e) taken together | 10 queries every five seconds          |
| Subscribes to the online status of the specified user(s) | [subscribePeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3a0e2d4d79ac85e23eae0dcb114ba9f0) | Ten queries every five seconds. |
| Unsubscribes from the online status of the specified user(s) | [unsubscribePeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#unsubscribepeersonlinestatus) | Ten queries every five seconds. |
| Gets a list of the peers, to whose specific status you have subscribed. | [queryPeersBySubscriptionOption](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersbysubscriptionoption) | Ten queries every five seconds. |
| Subscribes to the online status of the specified user(s) | [subscribePeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a7a9ec7398c013ed35e17bc5d93e71420) | Ten queries every five seconds. |
| Unsubscribes from the online status of the specified user(s) | [unsubscribePeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#acf3ab093be17a0752d8aff094e3aabc4) | Ten queries every five seconds. |
| Gets a list of the peers, to whose specific status you have subscribed. | [queryPeersBySubscriptionOption](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a971c357f7d0c27d122ff877389314ccc) | Ten queries every five seconds. |

<div class="alert note"><sup>1</sup> The maximum call frequency limit for joining the same channel is two queries every five seconds.</div>

## Operation timeout

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| Operation | Timeout (s) | 
| ---------------- | ---------------- | 
| Log in the RTM system   | 6   | 
| Send peer messages  | 10     | 
| Query the online status of specified users  | 10     | 
| Subscribe to the online status of specified users  | 10     | 
| Unsubscribe from the online status of specified users  | 10     | 
| Query peers by subscription option  | 5     | 
| User attribute or channel attribute operations  | 5     | 
| Retrieve member count of specified channels  | 5    | 
| Join a channel  | 5    | 
| Send a channel message| 10    | 
| Get a member list of the channel  | 5   | 




## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [RtmMessage.setText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2).
- The maximum length of the content in a call invitation is 8 KB. See [LocalInvitation.setContent](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445)
- The maximum length of the response in a call invitation is 8 KB. See [RemoteInvitation.setResponse](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1)

## Unicode support 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512.
- The current version supports querying the online status of a maximum of 256 users.
- You can subscribe to the online status of a maximxim of 512 users in one method call, and you can subscribe to the online status of at most 512 users. 
- Attribute settings in one user attribute operation should not exceed 16 KB in size; attribute settings in one channel attribute operation should not exceed 32 KB in size; each attribute (key/value pair) should not excced 8 KB in size; the number of key/value pairs you set in one attribute operation should not exceed 32. 
