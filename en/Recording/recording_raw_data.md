
---
title: Get the Raw Data
description: draft
platform: Linux
updatedAt: Wed Mar 11 2020 02:00:40 GMT+0800 (CST)
---
# Get the Raw Data
The Agora On-premise Recording SDK supports raw audio and video data in individual recording mode, and only raw audio data in composite recording mode.

For details about configuring an on-premise recording in these modes, see [Individual Recording](../../en/Recording/recording_individual_mode.md) and [Composite Recording](../../en/Recording/recording_composite_mode.md).

## Get raw audio and video data from an individual recording session

Depending on whether you record by command line or by APIs, set the parameter according to the following table to get the raw audio and video data from an individual recording.

| Data type      | Command line                                                 | API settings                                                 |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Raw audio data | <li>`--getAudioFrame 1`: AAC file<li>`--getAudioFrame 2`: PCM file | <li>`decodeAudio = 1`: AAC file<li>`decodeAudio = 2`: PCM file |
| Raw video data | <li>`--getVideoFrame 1`: H.264 file<li>`--getVideoFrame 2`: YUV frame file<li>`--getVideoFrame 3`: JPG frame file<li>`--getVideoFrame 4`: JPG file<li>`--getVideoFrame 5`: JPG file + MPEG-4 video file | <li>`decodeVideo = 1`: H.264 file<li>`decodeVideo = 2`: YUV frame file<li>`decodeVideo = 3`: JPG frame file<li>`decodeVideo = 4`: JPG file<li>`decodeVideo = 5`: JPG file + MP4 video file |

> The Agora RTC SDK for Web supports raw data in YUV format only, not in H.264 format.

You can get the raw video data from the `videoFrameReceived` callback, and the raw audio data from `audioFrameReceived`.

## Get raw audio and video data from a composite recording session

The Agora On-premise Recording SDK supports only raw audio data in composite recording mode. Depending on whether you record by command line or by APIs, set the parameter according to the following table to get the raw audio data in composite recording mode, storing it in a PCM file.

| Data type      | Command line                   | API settings                |
| :------------- | :----------------------------- | :-------------------------- |
| Raw audio data | `-- getAudioFrame 3`: PCM file | `decodeAudio = 3`：PCM file |

You can get the raw audio data from the `audioFrameReceived` callback.
