
---
title: RTM Limitations
description: 
platform: Web
updatedAt: Sun Jul 28 2019 14:36:44 GMT+0800 (CST)
---
# RTM Limitations
This page provides information about the limitations of the Agora RTM Web SDK. 

## Call Frequency

| Function                                                    | Method                                                       | Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Logs in the Agora RTM system                                | [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) | Two queries per second         |
| Sends messages | <li>[sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer) <li> And [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage) taken together | 60 queries per second          |
| Retrieves a member list of the channel                      | [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers) | Five queries every two seconds |
| Sets user attributes | <li>[setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)<li>And [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes) taken together | 10 queries every five seconds          |
| Gets user attributes | <li>[getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<li>And [getUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys) taken together | 40 queries every five seconds          |

## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [RtmMessage.text](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmmessage.html#text).
- The maximum length of the content in a call invitation is 8 KB. See [LocalInvitation.content](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#content)
- The maximum length of the response in a call invitation is 8 KB. See [RemoteInvitation.response](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#response)

## Encoding 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512. If you have special requirements, contact sales-us@agora.io.
- The current version supports querying the online status of a maximum of 256 users.
- Attribute settings in one attribute operation should not exceed 32 KB in size, and the number of key/value pairs you set in one attribute operation should not exceed 32. 
