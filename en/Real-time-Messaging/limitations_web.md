
---
title: RTM Limitations
description: 
platform: Web
updatedAt: Fri Feb 21 2020 06:34:55 GMT+0800 (CST)
---
# RTM Limitations

This page provides a brief overview of the limitations of the Agora RTM Web SDK, including the maximum method call frequencies, maximum string length, unicode support and more.

## Call Frequency

When mentioning a qps limitation, we are referring to the qps of an API in the context of one single RtmClient instance, not in the context of one Agora RTM SDK.

<div class="alert note">We <b>do not recommend</b> increasing the qps limitation by creating multiple RtmClient instances.</div>

| Function                                                    | Method                                                       | Call Frequency                 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Log in the Agora RTM system                                | [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#login) | Two queries per second         |
| Retrieve member count of specified channel(s) | [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelmembercount) | one query per second |
| Join a channel<sup>1</sup> | [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#join) | 50 queries every three seconds |
| Send messages | <li>[sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#sendmessagetopeer) <li> And [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#sendmessage) taken together | 60 queries per second          |
| Retrieve a member list of the channel                      | [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmchannel.html#getmembers) | Five queries every two seconds |
| Renew the Token        | [renewToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#renewtoken) | Two queries per second         |
| Query the online status of the specified users            | [queryPeersOnlineStatus](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#querypeersonlinestatus) | Ten queries every five seconds        |
| Set user attributes | <li>[setLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setlocaluserattributes)<li>[addOrUpdateLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatelocaluserattributes)<li>[deleteLocalUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletelocaluserattributesbykeys)<li>And [clearLocalUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearlocaluserattributes) taken together | 10 queries every five seconds          |
| Get user attributes | <li>[getUserAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributes)<li>And [getUserAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getuserattributesbykeys) taken together | 40 queries every five seconds          |
| Set channel attributes | <li>[setChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#setchannelattributes)<li>[addOrUpdateChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#addorupdatechannelattributes)<li>[deleteChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#deletechannelattributesbykeys)<li>And [clearChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#clearchannelattributes) taken together | 10 queries every five seconds          |
| Get channel attributes | <li>[getChannelAttributes](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributes)<li>And [getChannelAttributesByKeys](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/rtmclient.html#getchannelattributesbykeys) taken together | 10 queries every five seconds          |

<div class="alert note"><sup>1</sup> The maximum call frequency limit for joining the same channel is two queries every five seconds.</div>

## Timeout settings

<style> table th:first-of-type {     width: 300px; } th:third-of-type {     width: 100px; }</style>

| Function | Timeout settings (s) | 
| ---------------- | ---------------- | 
| Log in the RTM system   | 6     | 
| Send peer messages  | 10     | 
| Query the online status of specified users  | 10     | 
| Subscribe to the online status of specified users  | 10     | 
| Unsubscribe from the online status of specified users  | 10     | 
| Query peers by subscription option  | 5     | 
| user attribute or channel attribute operations  | 5     | 
| Retrieve member count of specified channels  | 5    | 
| Join a channel  | 5    | 
| Send a channel message| 10    | 
| Gets a member list of the channel  | 5   | 


 
<div class="alert note">As of v1.2.2: <li>The timeout setting for logging in to the Agora RTM system is prolonged from 6 s to 10 s. <li>The timeout setting for sending a peer-to-peer message on the Web platform is prolonged from 5 s to 10 s. </div>
 

## String Length

- The maximum length of a peer-to-peer or channel message is 32 KB. See [RtmMessage.text](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/interfaces/rtmtextmessage.html#text).
- The maximum length of the content in a call invitation is 8 KB. See [LocalInvitation.content](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/localinvitation.html#content)
- The maximum length of the response in a call invitation is 8 KB. See [RemoteInvitation.response](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_web/classes/remoteinvitation.html#response)
	
## Supported Browsers
	
See [Supported Browsers](https://docs.agora.io/en/Real-time-Messaging/messaging_web?platform=Web#prerequisites).

## Unicode support 

Supports channel and peer-to-peer messages, invitation content, and invitation response in UTF-8 only. 


## Miscellaneous 

- Notifications of a member joining or leaving the channel are automatically disabled when the number of channel members exceeds 512..
- The current version supports querying the online status of a maximum of 256 users.
- Attribute settings in one user attribute operation should not exceed 16 KB in size; attribute settings in one channel attribute operation should not exceed 32 KB in size; each attribute (key/value pair) should not excced 8 KB in size; the number of key/value pairs you set in one attribute operation should not exceed 32. 
