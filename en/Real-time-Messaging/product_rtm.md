
---
title: Agora Real-time Messaging (Beta) Overview
description: 
platform: All Platforms
updatedAt: Mon Nov 02 2020 09:42:34 GMT+0800 (CST)
---
# Agora Real-time Messaging (Beta) Overview


The Agora RTM (Real-time Messaging) SDK provides global messaging cloud service with high reliability, low latency, and high concurrency. You can use the RTM SDK to quickly implement real-time messaging scenarios.

## Functions

The RTM SDK supports the following functions:

- **Peer-to-peer and channel messaging**: Send and receive messages with text, image, file, expression, and custom format. You can perform one-to-one chat or group chat. You can also perform status synchronization. 
- **Offline messages**: Cache offline peer-to-peer messages when the receiver is offline. The receiver will get the message when back online.
- **User and channel attributes**: Add, delete, update, and get user and channel attributes.
- **Channel member count and member list**: Get the member count of one or multiple channels. Get the member list of a channel.
- **User online status**: Get or subscribe to the online status of a specified user.
- **Call invitation**: Send and receive call invitation.


## Applications

The RTM SDK can help implement the following scenarios:

### Voice chat

- **Online chat**: One-to-one chat, group chat, gifts, likes, expressions, images, custom messages.
- **Host seat management**: Invite a member to a host seat or remove a host.
- **Room management**: Room member count, member list, and notification of entering and leaving the room.

### Video chat

- **Call invitation**: Send and receive call invitation.
- **User management**: User online status and information.
- **One-to-one chat**: Gifts, likes, expressions, images, and custom messages.

### Live streaming 

- **Online chat**: One-to-one chat, group chat, gifts, likes, expressions, image, and custom messages.
- **Room management**: Room member count, member list, and notification of entering and leaving the room. 
- **Interactive control**: Send and receive questions. Request for host seat.

### Online education

- **Whiteboard**: Dot trail of pencils.
- **Online chat**: One-to-one chat, group chat, gifts, likes, expressions, image, and custom messages.
- **Classroom management**: Student list, classroom notice board, and request to speak in class.
- **Status control**: Slide control and muting.

### Internet of Things (IoT)

- Smart home remote control.
- In-vehicle remote control.
- Messaging for smart watch.
- VR/AR real-time tagging.

## Features

The RTM SDK has the following features:

<table>
  <tr>
    <th>Feature</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Global deployment</td>
    <td>The real-time network service covers more than 200 countries and regions across the globe.</td>
  </tr>
	  <tr>
    <td>Low latency	</td>
    <td>The global average latency is less than 200 ms. The average latency within the same region is less than 100 ms.</td>
  </tr>
  <tr>
    <td>High concurrency	</td>
    <td>There is no limit to the number of concurrent online users. The RTM SDK supports billion-scale concurrency at the system level and million-scale concurrency at the channel level.</td>
  </tr>
  <tr>
    <td>High reliability	</td>
    <td>Reliable keepalive mechanism with distributed servers. The message delivery rate is 100% when the packet loss rate reaches 70%.</td>
  </tr>
  <tr>
    <td>Multi-platform support</td>
    <td>Supports the following platforms:<ul> <li>iOS</li> <li>Android (arm64, armv7, x86)</li> <li>macOS</li> <li>Windows</li> <li>Linux</li> <li>Web (Chrome 49+, Firefox 52+, Safari 9+, Internet Explorer 11+)</li> <li>RESTful API</li></ul></td>
  </tr>
</table>	


## Reference

When integrating the Agora RTM SDK, you can also refer to the following articles:
- [Does the Agora RTM SDK have a limit on the number of concurrent online users?](https://docs.agora.io/en/faq/rtm_concurrency)
- [How do I enable virtual gift sending in live streaming using the Agora RTM SDK?](https://docs.agora.io/en/faq/rtm_gift_sending)
- [How can I implement call notification in a call application?](https://docs.agora.io/en/faq/call_invite_notification)


