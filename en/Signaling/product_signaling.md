
---
title: Agora Signaling Overview
description: 
platform: All Platforms
updatedAt: Mon Jul 06 2020 06:42:55 GMT+0800 (CST)
---
# Agora Signaling Overview
The Agora Signaling SDK is based on the TCP protocol and provides a stable messaging channel for you to implement real-time communication scenarios.

> Agora has launched the Agora RTM (Real-time Messaging) SDK to provide more reliable, scalable, and global real-time messaging services. It is designed as a substitute for the Agora Signaling SDK. For more design information of the Agora RTM SDK, click [here](https://docs.agora.io/en/Real-time-Messaging/product_rtm?platform=All%20Platforms). To migrate from the legacy Signaling SDK to the Agora RTM SDK, see [Signaling vs. Agora RTM SDK](https://docs.agora.io/en/Real-time-Messaging/rtm_signaling_android?platform=Android).


## Functions

The Agora Signaling SDK enables the following functions:

-   1-on-1 messages.
-   Channel messages.
-   Gets the user attributes
-   Gets the channel attributes
-   Gets the user list of the channel



## Applications

The Agora Signaling SDK can be used in the following scenarios:

<table>
  <tr>
    <th>Industry</th>
    <th>Application</th>
  </tr>
  <tr>
    <td>Live interactive streaming</td>
    <td><li>Comment streams<br><li>Chatroom<br><li>Send gifts<br><li>Likes<br><li>Maintenance of the live room status<br><li>Channel lists<br><li>Access control</td>
  </tr>
  <tr>
    <td>Social network</td>
    <td><li>Private chat messages<br><li>Group messages<br><li>Voice/Video call invitation instructions</td>
  </tr>
  <tr>
    <td>Education</td>
    <td><li>Class group message<br><li>Private chat message<br><li>Whiteboard<br><li>Authority management (reward, raising a hand, like)</td>
  </tr>
  <tr>
    <td>Game</td>
    <td>Gaming synchronization</td>
  </tr>
  <tr>
    <td>IoT</td>
    <td>Control messages</td>
  </tr>
</table>



## References

-   [Quickstart Guides](../../en/Quickstart%20Guide/signal_android-1.md) describe how to integrate the Agora Signaling SDK and provide short code snippets of common functions, such as sending point-to-point and channel messages.

-   [API Reference](../../en/API%20Reference/signal_android.md) lists the core methods and callbacks of the Agora Signaling SDK.

## Firewall ports and whitelist domains

Before accessing Agora’s signaling services, ensure that you open the local firewall ports and whitelist the domains specified below.

### Firewall ports

| Port type | Whitelist ports                                       |
| -------- | ----------------- |
| TCP ports | 1080; 8001 to 8199; 10000 to 10010; 10100 to 10110 |
| UDP ports | 8180 to 8199       |

### Whitelist domains

```
 .agora.io
 qoslbs.agoralab.co
 qos.agoralab.co
```



