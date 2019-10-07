
---
title: Agora Real-time Messaging SDK Overview
description: 
platform: All Platforms
updatedAt: Mon Oct 07 2019 03:15:37 GMT+0800 (CST)
---
# Agora Real-time Messaging SDK Overview
You can use the Agora RTM (Real-time Messaging) SDK to create a stable messaging mechanism for real-time messaging scenarios that require low latency and high concurrency for a global audience. 

## Functions

The Agora RTM SDK enables the following functions:

-   Send and receive (offline) peer-to-peer messages.
-   Send and receive channel messages.
-   Get the member list of the channel.
-   Create, send, cancel, accept, or decline a call invitation. 
-   Set, update, or get a user's attributes. 
-   Set, update, or get attributes of a specified channel.
-   Get the latest member count of specified channel(s). 
-   Interconnect with the legacy Agora Signaling SDK.


## Applications

You can use the Agora RTM SDK for the following scenarios:

<table>
  <tr>
    <th>Industry</th>
    <th>Application</th>
  </tr>
  <tr>
    <td>Live Broadcast</td>
    <td><li>Commentaries<br><li>Chatrooms<br><li>Send gifts<br><li>Likes<br><li>Maintenance of the chat room status (e.g., number of the channel members)<br><li>Privilege management (e.g., remove or mute a specified user)<br></td>
  </tr>
  <tr>
    <td>Social Network</td>
    <td><li>Private chat messages<br><li>Group messages<br><li>Voice/Video call invitation commands<br></td>
  </tr>
  <tr>
    <td>Education</td>
    <td><li>Class group messages<br><li>Private chat messages<br><li>Whiteboard<br><li>Privilege management (e.g., awards, presenting, hands up or likes)<br></td>
  </tr>
  <tr>
    <td>IoT</td>
    <td>Control messages</td>
  </tr>
</table>

## Features

The Agora RTM SDK provides the following features:

<table border="1" width="100%">
  <tr>
    <th width="20%">Feature </th>
    <th width="50%">Description</th>
  </tr>
  <tr>
    <td>High concurrency</td>
    <td>Supports sending up to a million  channel messages simultaneously. Can cope with the high concurrency scenarios, such as in an online quiz. <br></td>
  </tr>
  <tr>
    <td>High reliability</td>
    <td>Service availability at 99.999%</td>
  </tr>
	  <tr>
    <td>Low latency</td>
    <td>We have data centers distributed worldwide. <li>The average inter-continental latency is less than 200 ms.<br><li>The average intra-continental latency is less than 100 ms.<br></td>
  </tr>
	  <tr>
    <td>Compatibility</td>
    <td>Supports the following platforms:<li>iOS, Android (arm64, armv7, x86), macOS, Windows (coming soon), and Linux<br><li> Web: Chrome 58+, Firefox 56+, Safari 11+<br><li>Java server and C++ server<br></td>
  </tr>
</table>

## Real-time Messaging vs. Signaling

The Agora Real-time Messaging SDK (RTM) is designed to replace the legacy Agora Signaling SDK with expanded capabilities. 

> Maintenance of the legacy Agora Signaling SDK ends Q4 2019. 

