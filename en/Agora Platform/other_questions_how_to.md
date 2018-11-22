
---
title: Other Questions
description: 
platform: Other Questions
updatedAt: Thu Nov 22 2018 08:54:25 GMT+0000 (UTC)
---
# Other Questions
### Which callback functions are important to apps?

Apps need information about user status-related callbacks, such as userJoined and userOffline. All user-related information and remote user callbacks are transmitted over the UDP，which is not reliable.

### What error codes are important and what are not?

All error codes are important, and warning codes can be ignored.

### What error codes mean that I should end the app call?

When any error code is received, you should end the app call.

### Which callbacks inform users that the server is disconnected?

rtcEngineConnectionDidLost or connectionLostBlock notifies users that the server is disconnected. When a server is disconnected, the SDK will reconnect it automatically.

### What happens after calling initWithAppId multiple times?

This may result in abnormal camera behavior and other problems.

### How much impact does encryption and decryption have on the system?

Encryption and decryption may affect CPU performance and network latency.

This requires testing in actual situations.

### Why does the phone screen rotate together with the phone in a call?

If the screen rotation function is enabled in your phone settings, the screen will rotate with the phone.

### Why does the screen display the other user after the network is reconnected?

It takes some time for the video communication to recover after the network connection is lost. Latency is a common problem for all video communications and is affected by network conditions.

### What should I do when the user ID used is a string?

The user ID in the SDK only accepts 32-bit unsigned integers. It is recommended to map the strings into integers on your client server.

### How do I retrieve a successful volume callback message?

Call RtcEngineParameters:: enableAudioVolumeIndication(int interval, int smooth). The volume indicator is off by default. Call the API to enable the callback before or after joining a channel.

### Why did the initialization fail on Windows XP?

Starting from v1.1, the Agora SDK uses Visual C++ 2013. Since Windows XP does not include the Visual C++ Redistributable Packages by default, download and install them from the Microsoft websit)：https://www.microsoft.com/en-us/download/details.aspx?id=40784

### When setting the video profile by calling setVideoProfile, why do some profiles have the same frame rate but different bitrates?

Different users set different video parameters:

<table>
  <tr>
    <th>Video Profile</th>
    <th>Enumeration Value</th>
    <th>Resolution (Width * Height)</th>
    <th>Frame Rate</th>
    <th>Bitrate (kbps)</th>
  </tr>
  <tr>
    <td>360P</td>
    <td>30</td>
    <td>640 x 360</td>
    <td>15</td>
    <td>400</td>
  </tr>
  <tr>
    <td>360P_9</td>
    <td>38</td>
    <td>640 x 360</td>
    <td>15</td>
    <td>800</td>
  </tr>
</table>

Users who set the bitrate as 800 kbps prefer a higher image quality.

### What is a callback?

There are two programming categories:

* System Programming: Developing libraries related to the system.
* Application Programming: Developing applications with functions that use the libraries related to the system.

A system programmer develops interfaces (Application Programming Interfaces) for the application programmers to use.

When an application is running, it calls functions included in the APIs. Callback functions require the application to send them a function to call first in order to execute the task.

The following figure shows how callback functions work:

![](https://web-cdn.agora.io/docs-files/1539331143464)

### When debugging in Windows, it says that agorartc.pdb is required. Where can I get it?

You can ignore message this as this does not impact the usage.
