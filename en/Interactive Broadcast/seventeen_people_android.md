
---
title: Video Conference of 7+ Users
description: 
platform: Android
updatedAt: Fri Nov 02 2018 19:53:11 GMT+0000 (UTC)
---
# Video Conference of 7+ Users
If a video conference has too many host broadcasters, latency or packet loss may occur.

Now, if we set the subscribing stream to the **1-N** Mode, that is, set 1 stream as high and the rest low, then a maximum of 17 participants can join host an interactive broadcast without experiencing latency.

This page describes the applicable scenarios and considerations for implementing a video conference of 7+ participants using the Interactive Broadcast APIs.

## Scenario Description

A typical scenario for the video conference of 7+ paticipants is Small Class, mini online courses that include a teacher and a small number of students, where:

-   Both the teacher and the students enter a class as broadcasters from either a PC or a mobile device. Upon joining the channel, both the teacher and the students need to call `enableDualStreamMode` to enable the dual stream function.

-   The teacher can specify any student to come up to the stage through signaling services. Agora supports at most 5 to come to the stage at the same time. Now everyone in the class can see the video of the specified student\(s\) from their devices.

-   Signaling services notify everyone in the channel of who is\(are\) on the stage. Both the teacher and students can choose which they would like to see better and call `setRemoteVideoStreamType` to set the stream type to high or low and subscribe to that particular student. PCs support a maximum of 1 high stream + 16 low streams, while mobile devices support a maximum of 1 high stream and 6 low streams.


## 1. User Role

In the 17-way live video broadcast, each client is set as a host \(broadcaster\). The video interaction is a process of joining hosts together.

## 2. Parameter Settings

Enable the following modes before calling the `joinChannel` API.

-   Enable the multi-party video mode: `setParameters("{\"che.audio.live_for_comm\":true}");`
-   Enable the counteracting-packet-loss mode: `setParameters("{\"che.video.moreFecSchemeEnable\":true}");`


## 3. Stream Type Settings

### 3.1 Enable Dual-stream Mode

Whether to enable dual-stream mode or not depends on the real applications. If your app uses dual windows, one big and one small, then Agora recommends enabling dual-stream mode. Use the following code snippet to enable dual-stream mode:

### 3.2 Supported High-stream Parameters

A 17-way live video broadcast theoretically supports all the video profiles supported in the Interactive Broadcast API. However, Agora does not recommend using video profiles that exceed either a resolution of 640 x 480 or a frame rate of 30 fps.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Resolution</strong></td>
<td><strong>Frame Rate</strong></td>
<td><strong>Bit Rate</strong></td>
</tr>
<tr><td>640 x 480</td>
<td>15 fps</td>
<td>500 kbps</td>
</tr>
<tr><td>640 x 360</td>
<td>15 fps</td>
<td>400 kbps</td>
</tr>
<tr><td>640 x 360</td>
<td>24 fps</td>
<td>800 kbps</td>
</tr>
</tbody>
</table>


### 3.3 Customize Low-stream Video Parameters

If you have enabled dual-stream mode, you can customize the default low-stream video parameters at the app level. For example, to set the video profile to 320 x 180, 15 fps, and 140 kbps:

```
setParameters("{\"che.video.lowBitRateStreamParameter\":{\"width\":320,\"height\":180,\"frameRate\":15,\"bitRate\":140}}"
```

### 3.4 Comply with the Resolution Ratio

The resolution ratio of the low-stream video profile should be identical to that of the preset video profile. For example:

-   If a high-stream video profile \(on a PC\) is set to a resolution of 640 x 480, then the low-stream video profile must be set to a resolution with the same length-width ratio, which is 4:3.

-   If a high-stream video profile \(on iOS\) is set to a resolution of 360 x 640, then the low-stream video profile must be set to a resolution with the same length-width ratio, which is 9:16.


### 3.5 User Interface Layout

Agora recommends using a layout with one big window and multiple small windows.

-   Use a high-stream layout for the big window.
-   Use a low-stream layout for small windows.

## 4. Release Memory Resources

When a user is offline, the app can completely release memory resources of the view used by that user. The recommended procedure is:

1.  When receiving the callback indicating that a uid is offline, call the `setupremotevideo` method.
2.  Set the view of the `remote` parameter as NULL.



