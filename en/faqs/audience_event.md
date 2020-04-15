
---
title: How can I listen for an audience joining or leaving a live broadcast channel?
description: 
platform: All Platforms
updatedAt: Wed Mar 11 2020 12:14:13 GMT+0800 (CST)
---
# How can I listen for an audience joining or leaving a live broadcast channel?
The Agora RTC SDK does not provide any callback events that listen for an audience joining or leaving a live broadcast channel. However, you can listen for these events using either of the following two approaches:

- By using the event notifications provided by Agora's message notification service.
- By using the signaling service provided by the Agora RTM SDK.

## Using the message notification service

The message notification service enables you to listen for various service events, which are sent to your Server in the form of HTTP/HTTPS requests.

### Implementation

1. Follow the steps in [User configuration](https://docs-preview.agoralab.co/en/Agora%20Platform/ncs#user-configuration) to enable the message notification service.
2. Once the service is enabled, the message notification service sends messages to your Server as HTTP/HTTPS POST requests. Listen for the following events of the **Real-Time Communication** service to detect whether an audience joins or leaves a live broadcast channel:

| event_type | event_name | Description | Field name of payload |
| ---------------- | ---------------- | ---------------- | ----------------- |
| 105      | `audience join channel`      | In the Live-broadcast profile, an audience joins the channel.      |<ul><li>channelName</li><li>uid</li><li>platform</li><li>ts</li></ul> |
| 106      | `audience leave channel`   | In the Live-broadcast profile, an audience leaves the channel      |<ul><li>channelName</li><li>uid</li><li>platform</li><li>reason</li><li>ts</li></ul> |

### Reference

Refer to the following documents for more detailed steps and descriptions:

- [Message Notification Service](https://docs-preview.agoralab.co/en/Agora%20Platform/ncs?platform=All%20Platforms)
- [RTC event type](https://docs-preview.agoralab.co/en/Agora%20Platform/rtc_eventtype?platform=All%20Platforms)

## Using the signaling service

The Agora RTM SDK aims at providing stable signaling services, such as maintaining the channel information and sending real-time messages, for various social and education scenarios.

### Implementation

By integrating both the Agora RTC SDK and the Agora RTM SDK, you can listen for the callback events of the RTM SDK to detect the states of the audience in the RTC channel:

- Let a user join both an RTC and an RTM channel. Ensure that the RTC channel and the RTM channel share the same channel name.
- Bind the actions of joining both channels in your code logic.
- As a result, the RTM SDK reporting that a user has joined the RTM channel means that the user has joined the RTC channel as well. The same applies to a user leaving the channel.

### Reference

Refer to the following documents for detailed integration guide and API reference:

- [RTM quickstart](https://docs.agora.io/en/Real-time-Messaging/messaging_android?platform=Android)
- [RTM APIs for joining or leaving a channel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/index.html#joinorleavechannel)
	
	
