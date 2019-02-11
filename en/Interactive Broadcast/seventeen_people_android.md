
---
title: Video Conference of 7+ Users
description: 
platform: Android
updatedAt: Mon Feb 11 2019 11:54:30 GMT+0000 (UTC)
---
# Video Conference of 7+ Users
A video conference with too many hosts may cause network latency and packet loss. 

If we set the subscribing stream to the **1-N** Mode (set one stream as high and the rest as low), then a maximum of 17 users can join as hosts in an interactive broadcast without any network latency.

This page describes the scenarios and considerations for implementing a video conference of 7+ users using the Interactive Broadcast APIs.

## Scenario

A typical scenario for a video conference of 7+ users is in a small online class scenario that includes a teacher and a small number of students, where:

-   The teacher and students enter the class as hosts from either a PC or a mobile device. Upon joining the channel, the teacher and students need to call `enableDualStreamMode` to enable the dual-stream function.

-   The teacher can specify any student to present in class through Agora's signaling services. Agora supports at most five students presenting at the same time. Now everyone in the class can see the video of the specified students from their devices.

-   Signaling services notify everyone in the channel of who are the presenters. The teacher and students can choose which student they would like to see and call `setRemoteVideoStreamType` to set the stream type to high or low and subscribe to that particular student. PCs support a maximum of one high stream and 16 low streams, while mobile devices support a maximum of one high stream and six low streams.


## 1. User Role

In a 17-way live video broadcast, each client is set as a host \(broadcaster\). The video interaction is a process of joining hosts together.

## 2. Parameter Settings

Enable the following mode before calling the `joinChannel` API  method.

-   Enable the multi-party video mode: `setParameters("{\"che.audio.live_for_comm\":true}");`



## 3. Stream Type Settings

### 3.1 Enable Dual-stream Mode

Whether or not to enable dual-stream mode depends on the application scenario. If your app uses dual windows (one big and one small), then Agora recommends enabling dual-stream mode. Use the `enableDualStreamMode` method to enable dual-stream mode:

### 3.2 Supported High-stream Parameters

A 17-way live video broadcast supports all the video profiles supported in the Interactive Broadcast API. However, Agora does not recommend using video profiles that exceed either a resolution of 640 x 480 or a frame rate of 30 fps.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Resolution</strong></td>
<td><strong>Frame Rate</strong></td>
<td><strong>Bitrate</strong></td>
</tr>
<tr><td>640 x 480</td>
<td>15 fps</td>
<td>500 Kbps</td>
</tr>
<tr><td>640 x 360</td>
<td>15 fps</td>
<td>400 Kbps</td>
</tr>
<tr><td>640 x 360</td>
<td>24 fps</td>
<td>800 Kbps</td>
</tr>
</tbody>
</table>


### 3.3 Customize Low-stream Video Parameters

If you enable dual-stream mode, you can customize the default low-stream video parameters at the app level. For example, setting the video profile to 320 x 180, 15 fps, and 140 Kbps:

```
setParameters("{\"che.video.lowBitRateStreamParameter\":{\"width\":320,\"height\":180,\"frameRate\":15,\"bitRate\":140}}"
```

### 3.4 Comply with the Aspect Ratio

The aspect ratio of the low-stream video profile should be identical to that of the preset video profile. For example:

-   If a high-stream video profile \(on a PC\) is set to a resolution of 640 x 480, then the low-stream video profile must be set to a resolution with the same aspect ratio (4:3).

-   If a high-stream video profile \(on iOS\) is set to a resolution of 360 x 640, then the low-stream video profile must be set to a resolution with the same aspect ratio (9:16).


### 3.5 User Interface Layout

Agora recommends using a layout with one big window and multiple small windows.

-   Use a high-stream layout for the big window.
-   Use a low-stream layout for small windows.

## 4. Release Memory Resources

When a user is offline, the app releases all memory resources of the view used by that user. The recommended procedure is:

1.  When receiving the callback indicating that a `uid` is offline, call the `setupremotevideo` method.
2.  Set the view of the `remote` parameter as NULL.



