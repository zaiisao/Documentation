
---
title: RTM Limitations
description: 
platform: Linux CPP
updatedAt: Wed Sep 30 2020 15:32:45 GMT+0800 (CST)
---
# RTM Limitations

This page provides a brief overview of the limitations of the Agora RTM SDK for Linux C++, including API call limit, string size, encoding, and more.


## Call limit

The call limit is for one <code>IRtmService</code> instance. If an operation corresponds to multiple methods, the number of the method calls of an operation equals the sum of the method calls of all corresponding methods in a specific time frame.

<div class="alert note">You can increase the call limit of an API by creating multiple <code>IRtmService</code> instances.</div>

| Operation                                                    | Method                                                       | Call limit                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in to the RTM system                                | [`login`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2433a0babbed76ab87084d131227346b) | Two calls per second         |
| Retrieve member count of specified channels | [`getChannelMemberCount`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a41dee47c6201acb2f29371b6e30249a5) | One call per second |
| Join a different channel each time | [`join`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a6a54cdd8e5db526514e0ca84aa9cba4c) | 50 calls every three seconds |
| Join the same channel each time | [`join`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a6a54cdd8e5db526514e0ca84aa9cba4c) | Two calls every five seconds |
| Send messages | <li>[`sendMessageToPeer`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#afec5391fa9c4ec2bfe9ac4e684705600)</li> <li>[`sendMessageToPeer`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a08c1b3d444af5a2778ede48e4c677a52)</li> <li>[`sendMessage`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a4ae01f44d49f334f7c2950d95f327d30)</li> | 1,500 calls every three seconds          |
| Retrieve a member list of the channel          | [`getMembers`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a3f9c943059ac48a568c81798da38c3cb) | Five calls every two seconds |
| Renew the token| [`renewToken`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2c33be67bfec02d69041f1e8978f4559) | Two calls per second |
| Query the online status of the specified users | [`queryPeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3add0055c4455dc8d04bfc37edfd8e94) | 100 calls every five seconds |
| Set user attributes                               | <li>[`setLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a86dcbfc38c665be8565f06c534338d33)</li><li>[`addOrUpdateLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a0a63923bd1e81e60d6ca54213a329747)</li><li>[`deleteLocalUserAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#acb669f6c4c28e08cdf889df11e1ddeb3)</li><li>[`clearLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#acc5eee875f4166fe455cde7aff1ad738)</li> | 100 calls every five seconds   |
| Get user attributes                               | <li>[`getUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a14cac887f9adb390621dd0427092a65b)</li><li>[`getUserAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#af011235917c291df5581f92afa35532f)</li> | 400 calls every five seconds   |
| Set channel attributes | <li>[`setChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#aa229a7207062b510799166c1239412fa)</li><li>[`addOrUpdateChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#ae4068ff21c8e20e8eeb45ba21959c368)</li><li>[`deleteChannelAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a1a448f33be57b31f9952822426e5c4bd)</li><li>[`clearChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#aff6cff676e3fc3150ef5f27845c9a3d3)</li> | 100 calls every five seconds         |
| Get channel attributes | <li>[`getChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3dc8409ed82d8f95a0839d5e9e7da564)</li><li> [`getChannelAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#ac97f24f9d78e885e494a22be95db8d33)</li> | 400 calls every five seconds          |
| Subscribe to the online status of the specified users | [`subscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a3a0e2d4d79ac85e23eae0dcb114ba9f0) | 10 calls every five seconds |
| Unsubscribe from the online status of the specified users | [`unsubscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a027574f04151a9fded678fadba47441e) | 10 calls every five seconds |
| Get a list of the peers whose status you have subscribed to | [`queryPeersBySubscriptionOption`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a063bd3db39660a7a3513378ce03f4456) | 10 calls every five seconds |


	
## String size

- The maximum size of a peer-to-peer or channel message is 32 KB. See [`IMessage.setText`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_message.html#a2e93098d5a3819e9d4cf8d42641474ae).
- The maximum size of the content in a call invitation is 8 KB. See [`ILocalCallInvitation.setContent`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_local_call_invitation.html#aaf4b356b7602cad2bdb998250617c920).
- The maximum size of the response in a call invitation is 8 KB. See [`IRemoteCallInvitation.setResponse`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_remote_call_invitation.html#a77c8a0902f9dce36525a66575a6ae8a5).

## Encoding

- Channel and peer-to-peer messages, the invitation content, and the invitation response must be in UTF-8 format.
- The `filePath` parameter of the following methods must be in UTF-8 format:
  - [`createFileMessageByUploading`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a99f2137ec43be135b369b7d6927b6138)
  - [`createImageMessageByUploading`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a7192d93f365c28e2d0b91547716fb5a9)
  - [`downloadMediaToFile`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a70584eb57e97476b1da072f737d88c95)


## Miscellaneous

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. Agora recommends that you call the [Gets channel events RESTful API](../../en/Real-time-Messaging/rtm_get_event.md)) from your server to get the notifications of channel events. 
- The current version supports querying the online status of up to 256 users.
- You can subscribe to the online status of up to 512 users in one method call, and you can subscribe to the online status of up to 512 users. 
- Attribute settings in one user attribute operation must not exceed 16 KB in size. Attribute settings in one channel attribute operation must not exceed 32 KB in size. Each attribute (key/value pair) must not exceed 8 KB in size. You must not set over 32 key/value pairs for one attribute operation.
- Each file or image you upload to the Agora server stays for seven days. The corresponding media ID also stays valid for seven days.
- Each file or image to upload must not exceed 30 MB in size.
- Each client instance can only support up to nine upload and download processes at the same time.
