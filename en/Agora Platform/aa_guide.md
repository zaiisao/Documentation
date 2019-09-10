
---
title: Agora Analytics
description: Introduction to Agora Analytics
platform: All Platforms
updatedAt: Mon Sep 09 2019 10:54:57 GMT+0800 (CST)
---
# Agora Analytics
[Agora Analytics](https://dashboard.agora.io/analytics/call/search) is a tool that tracks the QoE (Quality of Experience) of calls. The **Call Search** function allows you to see the quality of your calls in diagrams displaying data during the call process. The information includes:
 
- The device status, including the system CPU usage and the app's CPU usage.
- User events. For example, stop sending audio or start receiving video.
- Bitrates of the sent/received audio and video.
- The freeze time in rendering the audio and video.
- The local captured volume and the remote playout volume.
- The packet loss rates of the audio and video.
- The users' rating of the call.

See the use case [tutorials](../../en/Agora%20Platform/aa_tutorial.md) to learn how to analyze your calls.

## Search calls

Log in [Agora Dashboard](https://dashboard.agora.io/) and click **Call Search** under **Agora Analytics** on the left of the navigation bar.

![](https://web-cdn.agora.io/docs-files/1545893469823)

To search calls, fill in the search fields:

1. Select the project.
2. Specify the searching time frame (the default time frame is the last 14 days).
3. Choose to search by Channel Name or UID in the drop-down menu.
4. Enter a channel name or UID and click **Search**.

> By default, **Call Search** uses the UTC time zone. To use your local time zone, click the clock icon ![](https://web-cdn.agora.io/docs-files/1545893874792) on the top menu bar.

## View Quality of Experience

You can go to the QoE page by clicking **View the call** on the search results page.

The QoE page shows each user's quality of experience as the receiver in the call. You can see whether any user experiences audio/video freeze, blurry video, or no audio or video.

### Basic information

![](https://web-cdn.agora.io/docs-files/1545382838358)

The top of the QoE page shows the basic information of the call, such as the project name, channel name, starting and ending times, and duration of the call.

> The QoE page shows three hours of a call at maximum. If a call lasts more than 3 hours, the QoE page shows the last 3 hours by defualt. You can use the timeline to select the time frame you wish to view.

### User list

![](https://web-cdn.agora.io/docs-files/1545382846535)

Under basic information, the user list shows the information of the users in the call, such as the UID, location, online status, platform, and SDK version.

The **View QoE** toggle switch turns on/off displaying the QoE diagram for each user.

> When the call has many users, the user list only shows part of the users. If the user you want to view is not listed, enter the UID and click **Add a user** under the user list.

### Quality of Experience Overview

![](https://web-cdn.agora.io/docs-files/1545805725772)

The **Quality of Experience Overview** section displays each user's QoE diagram.

The user's basic information is shown above the QoE diagram, including the UID, platform, SDK version, and the user's rating of the call (when the rating function is enabled).

> See how to enable the rating function on [Android](../../en/Agora%20Platform/rate_android.md), [iOS](../../en/Agora%20Platform/rate_ios.md), [macOS](../../en/Agora%20Platform/rate_mac.md), and [Windows](../../en/Agora%20Platform/rate_windows.md).

![](https://web-cdn.agora.io/docs-files/1545382892984)

In the QoE diagram, the x-axis represents the time of the call, and the y-axis represents the bitrate of the received video and audio.

- The curve above the x-axis represents the bitrate of the received video from the sender. For multiple senders, the received video from each sender is shown by a colored curve.
- The red dotted line indicates a blurry video.
- The red glitches above the x-axis indicate video freeze.
- The red glitches below the x-axis indicate audio freeze.
- The curve below the x-axis represents the total bitrate of the received audio from all senders.

The QoE section displays data from all senders by default. You can select which senders to display by using the toolbar at the bottom-right of the page.

If you find quality issues of a specific sender, click the **End-to-End Details** button to go to the E2E (End-to-End) page.

## Analyze quality issues

![](https://web-cdn.agora.io/docs-files/1545804650508)

The **End-to-End Details** page shows the audio and video statistics of a specific sender and receiver. These statistics affect the user's quality of experience and help you understand the quality issues. Here is a brief explanation of these statistics:

### Device status

The CPU usage of the system and the SDK, indicating how busy the system is. If the system is busy (the CPU usage is high), the audio/video may freeze.

> Changes in Android 8.0 make it difficult to get accurate CPU usage statistics. Agora provides a **Task Scheduling Delay** indicator to represent how busy the system is.

### User events

User events directly affect the user experience. If a sender stops sending the video data, the receiver cannot see any image.

### Bitrate

The bitrate is the amount of data (bits) sent or received per second.

A higher audio/video bitrate means a higher audio/video quality. A low bitrate does not necessarily cause quality issues, but a very low bitrate often means poor audio/video quality.

### Packet loss

Data is transmitted in units called packets. In data transmission, some of the packets are lost. The packet loss rate is the percentage of the lost packets, which reflects the network quality:

- The packet loss rate of the sender is the packet loss rate of sending the data.
- The packet loss rate of the receiver is the packet loss rate from the sender to the receiver.

A low packet loss rate usually does not cause any quality issue. A high packet loss rate (5% or higher) means the network quality is poor and may cause audio/video freeze and blurry video.

### Frame rate

The frame rate is the frequency (rate) at which consecutive images (frames) appear on a display. 

A higher frame rate means smoother video but uses more bandwidth and CPU usage. A low frame rate may cause video freeze.

### Resolution

The resolution is the number of pixels in the width and height of an image. A higher resolution means clearer video.

> The **End-to-End Details** page shows only the resolution of the rendered video for the receiver.

## Key terms

This section describes the key terms used in **Call Search**. See [Agora Key Terms](../../en/Agora%20Platform/terms.md) for more key terms.

### Channel

Every call or live broadcast must occur in a channel. If we imagine an app being a building, a channel will be a room in the building.

### Call

Many calls can occur in a channel. When a user joins an empty channel, a call starts. When all users leave the channel, this call ends. If a user joins the channel after the call ends, then a new call starts. The period between the first user joining a channel and the last user leaving the channel is considered one call. 

### User

Each user in the call has a unique UID. Agora Analytics displays and analyzes the users' QoE from the perspective of sending and receiving data.

#### <a name="rec"></a>Receiver

The QoE page shows the QoE of each user as the receiver.

#### <a name="send"></a>Sender

A sender is a user sending data and has a speaker icon ![](https://web-cdn.agora.io/docs-files/1545894218077) in the user list.

### Duration

- A call's duration is the time from starting to ending the call, that is from the first user joining the channel to the last user leaving the channel.
- A user's duration is the time from the user joining to leaving the channel. A user may join and leave a channel multiple times in a call.

> The duration does not equal to the billing minutes. The calls are charged according to the actual minutes used in the channel.
