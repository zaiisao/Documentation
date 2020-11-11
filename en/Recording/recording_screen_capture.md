
---
title: Capture Screenshots
description: 
platform: Linux
updatedAt: Wed Sep 25 2019 03:41:06 GMT+0800 (CST)
---
# Capture Screenshots

## Overview

This page shows how to capture screenshots by the command line. With screenshots, you can analyze and monitor the video contents to ensure that the contents are permissible.

Before proceeding, ensure that you have compiled the Agora Recorder Demo and know how to record a call by the command line. For more information, see [Record by Command Line](../../en/Recording/recording_cmd_cpp.md).

## Implementation

First, to capture screenshots, you must record video only (`isAudioOnly` is set as 0 and  `isVideoOnly` is set as 1).

Then, you can use the `getVideoFrame` parameter to set the format of the recording files. You can also use the `captureInterval` parameter to set the screen capture interval, which must be longer than one second and the default value is five seconds.

The following sections describe the `getVideoFrame` and `captureInterval` parameters in details.

### Set the format of the recording files

Use the `getVideoFrame` parameter to set the format of the recording files depending on your recording mode.

- #### **Individual recording mode**

In individual recording mode (`isMixingEnabled` is set as 0), you can set the `getVideoFrame` parameter as 3, 4, or 5.

<table>
  <tr>
    <th></th>
    <th>Setting</th>
    <th>Recording Files and Formats</th>
  </tr>
  <tr>
    <td rowspan="2">Only capture screenshots</td>
    <td>--getVideoFrame 3<br></td>
    <td>Video frame in JPEG format.</td>
  </tr>
  <tr>
    <td>--getVideoFrame 4<br></td>
    <td>Screenshots in JPEG format.</td>
  </tr>
  <tr>
    <td>Capture screenshots while recording</td>
    <td>--getVideoFrame 5</td>
    <td><li>One video file in MP4 format and multiple screenshots in JPEG format for each UID that uses the Agora Native SDK.</li><li>One video file in WebM or MP4 format and multiple screenshots in JPEG format for each UID that uses the Agora Web SDK.</li></td>
  </tr>
</table>

- #### **Composite recording mode**

In composite recording mode (`isMixingEnabled` is set as 1), you can only set the `getVideoFrame` parameter as 5 and get a video file in MP4 format and multiple screenshots in JPEG format.

### Set the screen capture interval

You can also use the `captureInterval` parameter to set the screen capture interval, which must be longer than one second and the default value is five seconds.

## Example

The following example shows how to capture screenshots in JPEG format in individual recording mode.

```
./recorder_local --appId <Your App ID> --channel <The name of the channel to be recorded> --uid 0 --appliteDir ~/Agora_Recording_SDK_for_Linux_FULL/bin --isVideoOnly 1 --getVideoFrame 4
```
