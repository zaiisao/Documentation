
---
title: RTM Limitations
description: 
platform: iOS,macOS
updatedAt: Wed Sep 30 2020 15:31:52 GMT+0800 (CST)
---
# RTM Limitations

This page provides a brief overview of the limitations of the Agora RTM Objective-C SDK for iOS or Agora RTM Objective-C SDK for macOS, including API call limit, string size, encoding, and more.



## Call limit

The call limit is for one <code>AgoraRtmKit</code> instance. If an operation corresponds to multiple methods, the number of the method calls of an operation equals the sum of the method calls of all corresponding methods in a specific time frame.

<div class="alert note">You can increase the call limit of an API by creating multiple <code>AgoraRtmKit</code> instances.</div>

| Operation                                                    | Method                                                       | Call limit                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in to the RTM system                                | [`loginByToken`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) | 2 calls per second         |
| Retrieve member count of specified channel(s) | [`getChannelMemberCount`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelMemberCount:completion:) | 1 call per second |
| Join a different channel each time | [`joinWithCompletion`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) | 50 calls every 3 seconds |
| Join the same channel each time | [`joinWithCompletion`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) | 2 calls every 5 seconds |
| Send messages| <li>[`sendMessage`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:)</li> <li>[`sendMessage`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:sendMessageOptions:completion:)</li> </li><li>[`sendMessage`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:)</li> | 180 calls every three seconds          |
| Retrieve a member list of the channel                      | [`getMembersWithCompletion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) | 5 calls every 2 seconds |
| Renew the token | [`renewToken`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/renewToken:completion:) | 2 calls per second |
| Query the online status of the specified users. | [`queryPeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersOnlineStatus:completion:) | 10 calls every 5 seconds |
| Set user attributes | <li>[`setLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setLocalUserAttributes:completion:)</li><li>[`addOrUpdateLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateLocalUserAttributes:completion:)</li><li>[`deleteLocalUserAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteLocalUserAttributesByKeys:completion:)</li><li> [`clearLocalUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearLocalUserAttributesWithCompletion:)</li> | 10 calls every 5 seconds          |
| Get user attributes | <li>[`getUserAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAllAttributes:completion:)</li><li> [`getUserAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getUserAttributes:ByKeys:completion:)</li>  | 40 calls every 5 seconds         |
| Set channel attributes | <li>[`setChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/setChannel:Attributes:Options:completion:)</li><li>[`addOrUpdateChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/addOrUpdateChannel:Attributes:Options:completion:)</li><li>[`deleteChannelAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/deleteChannel:AttributesByKeys:Options:completion:)</li><li> [`clearChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/clearChannel:Options:AttributesWithCompletion:)</li>  | 10 calls every 5 seconds          |
| Get channel attributes | <li>[`getChannelAttributes`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAllAttributes:completion:)</li><li> [`getChannelAttributesByKeys`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/getChannelAttributes:ByKeys:completion:)</li> | 10 calls every 5 seconds         |
| Subscribe to the online status of the specified user(s) | [`subscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/subscribePeersOnlineStatus:completion:) | 10 calls every 5 seconds |
| Unsubscribe from the online status of the specified user(s) | [`unsubscribePeersOnlineStatus`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/unsubscribePeersOnlineStatus:completion:) | 10 calls every 5 seconds |
| Get a list of the peers whose status you have subscribed to. | [`queryPeersBySubscriptionOption`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/queryPeersBySubscriptionOption:completion:) | 10 calls every 5 seconds |



## String size

- The maximum size of a peer-to-peer or channel message is 32 KB. See [`AgoraRtmMessage`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html).
- The maximum size of the content in a call invitation is 8 KB. See [`AgoraRtmLocalInvitation`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmLocalInvitation.html).
- The maximum size of the response in a call invitation is 8 KB. See [`AgoraRtmRemoteInvitation`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmRemoteInvitation.html).

## Encoding 

- Channel and peer-to-peer messages, the invitation content, and the invitation response must be in UTF-8 format. 
- The `filePath` parameter of the following methods must be in UTF-8 format:
    - [`createFileMessageByUploading:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createFileMessageByUploading:withRequest:completion:)
    - [`createImageMessageByUploading:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createImageMessageByUploading:withRequest:completion:)
    - [`downloadMedia:toFile:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/downloadMedia:toFile:withRequest:completion:) 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. Agora recommends that you call the [Gets channel events RESTful API](../../en/Real-time-Messaging/rtm_get_event.md) from your server to get the notifications of channel events. 
- The current version supports querying the online status of up to 256 users.
- You can subscribe to the online status of up to 512 users in one method call, and you can subscribe to the online status of up to 512 users. 
- Attribute settings in one user attribute operation must not exceed 16 KB in size. Attribute settings in one channel attribute operation must not exceed 32 KB in size. Each attribute (key/value pair) must not exceed 8 KB in size. You must not set over 32 key/value pairs for one attribute operation.
- Each file or image you upload to the Agora server stays for seven days. The corresponding media ID also stays valid for seven days.
- Each file or image to upload must not exceed 30 MB in size.
- Each client instance can only support up to nine upload and download processes at the same time.
