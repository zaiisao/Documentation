
---
title: RTM Limitations
description: 
platform: Web
updatedAt: Wed Sep 30 2020 15:18:12 GMT+0800 (CST)
---
# RTM Limitations

This page provides a brief overview of the limitations of the Agora RTM Web SDK, including API call limit, string size, encoding, and more.

## API call limit

The call limit is for one <code>RtmClient</code> instance. If an operation corresponds to multiple methods, the number of the method calls of an operation equals the sum of the method calls of all corresponding methods in a specific time frame.

<div class="alert note">We <b>do not recommend</b> increasing the call limit by creating multiple <code>RtmClient</code> instances.</div>

| Function                                                    | Method                                                       | Call limit                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in to the Agora RTM system                                | [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) | Two calls per second         |
| Retrieve member count of specified channels | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelmembercount) | One call per second |
| Join the same channel each time<sup>1</sup> | [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 50 calls every three seconds |
| Join a different channel each time<sup>1</sup> | [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | Two calls every five seconds |
| Send messages | <li>[sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer) <li> [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage)  | 180 calls every three seconds          |
| Retrieve a member list of the channel                      | [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers) | Five calls every two seconds |
| Renew the Token        | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken) | Two calls per second         |
| Query the online status of the specified users            | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) | 10 calls every five seconds        |
| Set user attributes | <li>[setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)<li> [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes)  | 10 calls every five seconds          |
| Get user attributes | <li>[getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<li> [getUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys)  | 40 calls every five seconds          |
| Set channel attributes | <li>[setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatechannelattributes)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys)<li> [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearchannelattributes)  | 10 calls every five seconds          |
| Get channel attributes | <li>[getChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes)<li> [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributesbykeys)  | 10 calls every five seconds          |


## String size

- The maximum size of a peer-to-peer or channel message is 32 KB. See [RtmMessage.text](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmtextmessage.html#text).
- The maximum size of the content in a call invitation is 8 KB. See [LocalInvitation.content](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#content)
- The maximum size of the response in a call invitation is 8 KB. See [RemoteInvitation.response](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#response)
	
## Supported browsers
	
See [Supported Browsers](https://docs.agora.io/en/Real-time-Messaging/messaging_web?platform=Web#prerequisites).

## Unicode support 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. Agora recommends that you call the [Gets channel events RESTful API](../../en/Real-time-Messaging/rtm_get_event.md) from your server to get the notifications of channel events. 
- The current version supports querying the online status of a maximum of 256 users.
- You can subscribe to the online status of a maximxim of 512 users in one method call, and you can subscribe to the online status of at most 512 users. 
- Attribute settings in one user attribute operation should not exceed 16 KB in size; attribute settings in one channel attribute operation should not exceed 32 KB in size; each attribute (key/value pair) should not excced 8 KB in size; the number of key/value pairs you set in one attribute operation should not exceed 32. 
