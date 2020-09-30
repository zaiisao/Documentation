
---
title: RTM Limitations
description: 
platform: Android
updatedAt: Wed Sep 30 2020 15:30:51 GMT+0800 (CST)
---
# RTM Limitations


This page provides a brief overview of the limitations of the Agora RTM SDK for Android, including API call limit, string size, encoding, and more.


## Call limit

The call limit is for one <code>RtmClient</code> instance. If an operation corresponds to multiple methods, the number of the method calls of an operation equals the sum of the method calls of all corresponding methods in a specific time frame.

<div class="alert note">You can increase the call limit of an API by creating multiple <code>RtmClient</code> instances.</div>

| Operation                                                    | Method                                                       | Call limit                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in the RTM system                                | [`login`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) | 2 calls per second         |
| Retrieve member count of specified channel(s) | [`getChannelMemberCount`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aff0384f2a004ed75498e20e1917352e4) | 1 call per second |
| Join a different channel each time | [`join`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) | 50 calls every 3 seconds |
| Join the same channel each time | [`join`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) | 2 calls every 5 seconds |
| Send messages | <li>[`sendMessageToPeer`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a)<li> [`sendMessageToPeer`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9) <li> [`sendMessage`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0)  | 180 calls every three seconds          |
| Retrieve a member list of the channel          | [`getMembers`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) | 5 calls every 2 seconds |
| Renew the token| [`RtmClient.renewToken`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a9a6d33282509384165709107d7a89353) | 2 calls per second |
| Query the online status of the specified user(s) | [`queryPeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ac711f981405648ed5ef1cb07436125f3) | 10 calls every 5 seconds |
| Set user attributes                               | <li>[`setLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a339b7b2371ff2b86137b6db6c1c66294)<li>[`addOrUpdateLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a765b186d62ed3ef6d67a5e875b040875)<li>[`deleteLocalUserAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a2477533989c1bb9ced831af210f1dba4)<li>[`clearLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ae0c6c5c5bae6020e69009441d8a41785)  | 10 calls every 5 seconds   |
| Get user attributes                               | <li>[`getUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#aee9a6c027f35b652781f654a89433755)<li> [`getUserAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a3b927c35cca5ebd31afb976d60e99193)  | 40 calls every 5 seconds   |
| Set channel attributes | <li>[`setChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#ad25f51a3671db50e348ec6c170044ec6)<li>[`addOrUpdateChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a997a31e6bfe1edc9b6ef58a931ef3f23)<li>[`deleteChannelAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a4cbf3329abda4940b73a75455cd1dc06)<li>[`clearChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6ed0ef4baacda8fa00eda5373d17f59f)  | 10 calls every 5 seconds         |
| Get channel attributes | <li>[`getChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a81f14a747a4012815ab4ba8d9e480fb6)<li> [`getChannelAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a358b47f4b42d678fafa76f3f30290e5e)  | 10 calls every 5 seconds          |
| Subscribes to the online status of the specified user(s) | [`subscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3a0e2d4d79ac85e23eae0dcb114ba9f0) | 10 calls every 5 seconds |
| Unsubscribes from the online status of the specified user(s) | [`unsubscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#unsubscribepeersonlinestatus) | 10 calls every 5 seconds |
| Gets a list of the peers, to whose specific status you have subscribed. | [`queryPeersBySubscriptionOption`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersbysubscriptionoption) | 10 calls every 5 seconds |
| Subscribes to the online status of the specified user(s) | [`subscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a7a9ec7398c013ed35e17bc5d93e71420) | 10 calls every 5 seconds |
| Unsubscribes from the online status of the specified user(s) | [`unsubscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#acf3ab093be17a0752d8aff094e3aabc4) | 10 calls every 5 seconds |
| Gets a list of the peers, to whose specific status you have subscribed. | [`queryPeersBySubscriptionOption`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a971c357f7d0c27d122ff877389314ccc) | 10 calls every 5 seconds |



## String size

- The maximum size of a peer-to-peer or channel message is 32 KB. See [`RtmMessage.setText`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2).
- The maximum size of the content in a call invitation is 8 KB. See [`LocalInvitation.setContent`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_local_invitation.html#a4cec28ff6d356242329b1034c7531445).
- The maximum size of the response in a call invitation is 8 KB. See [`RemoteInvitation.setResponse`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_remote_invitation.html#a229b8cf773eaa0e79b0d67815fd6b6f1).

## Encoding

- Channel and peer-to-peer messages, invitation content, and invitation response must be in UTF-8 format.
- The `filePath` parameter of the following methods must be in UTF-8 format:
  - [`createFileMessageByUploading`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a1b08207278d611e5e4b87e6d9712e0c7)
  - [`createImageMessageByUploading`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#afc93fad7700593a803ddbc87482c0ac0)
  - [`downloadMediaToFile`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a34e0bd19fb0bbd1d91dec0a1af100038)


## Miscellaneous

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. Agora recommends that you call the [Gets channel events RESTful API](../../en/Real-time-Messaging/rtm_get_event.md) from your server to get the notifications of channel events. 
- The current version supports querying the online status of up to 256 users.
- You can subscribe to the online status of up to 512 users in one method call, and you can subscribe to the online status of up to 512 users.
- Attribute settings in one user attribute operation must not exceed 16 KB in size. Attribute settings in one channel attribute operation must not exceed 32 KB in size. Each attribute (key/value pair) must not exceed 8 KB in size. The number of key/value pairs you set in one attribute operation must not exceed 32.
- Each file or image you upload to the Agora server stays for 7 days. The corresponding media ID also stays valid for seven days.
- Each file or image to upload must not exceed 30 MB.
- Each client instance can only support up to 9 upload and download processes at the same time.
