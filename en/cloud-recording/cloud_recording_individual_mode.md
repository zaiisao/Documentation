
---
title: Individual Recording
description: 
platform: All Platforms
updatedAt: Thu Jul 09 2020 02:55:40 GMT+0800 (CST)
---
# Individual Recording
## Overview

Agora Cloud Recording supports two recording modes:

- Individual recording mode: Records the audio and video as separate files for each UID in a channel.
- Composite recording mode: Generates a single mixed audio and video file for all UIDs in a channel.

This article explains how to record a call in individual recording mode by using the RESTful API.

Before proceeding, ensure that you know how to use Agora Cloud Recording by using the RESTful API. For more information, see [Agora Cloud Recording RESTful API Quickstart](../../en/cloud-recording/cloud_recording_rest.md). You must select either individual recording mode or composite recording mode when you start the recording. You cannot switch between the two modes after a recording starts. 

See [Differences between individual recording mode and composite recording mode](https://docs.agora.io/en/faq/recording_mode) to decide which mode you should use.

> We assume that each UID in the channel sends audio and video streams. If a UID does not send any audio or video stream, such as the audience in a live interactive streaming, no file is generated, except for Web users.

## Implementation

To enable individual recording mode, set `mode` to `individual` when calling [`start`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/start).

In individual recording mode, the audio and video profiles of the recording file are as follows:

-  Audio profile: The sample rate of the recording file is 48 kHz, and the bitrate and number of audio channels of the recording file are the same as those of the original audio stream.
-  Video profile: The video profile of the recording file is the same as that of the original video stream.

Based on the setting of `streamTypes`, the recorded files are as follows:

| Settings                                   | Recorded content      | Recorded files                                               |
| :----------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| Set `streamTypes` as `0`                   | Audio only            | One M3U8 file and several TS/WebM files for each UID. The TS/WebM files store only audio. |
| Set `streamTypes` as `1`                   | Video only (no audio) | One M3U8 file and several TS/WebM files for each UID. The TS/WebM files store only video (no audio). |
| Set `streamTypes` as `2` (default setting) | Audio and video       | Two M3U8 files and several TS/WebM files for each UID. The TS/WebM files store audio and video separately. |

> When you use Agora Cloud Recording to record a Web call, and if you choose the VP8 codec and individual recording mode, Agora Cloud Recording generates WebM files instead of TS files. Agora Cloud Recording generates TS files in all other circumstances.

For detailed information about the naming convention of the recorded files, see [Manage Recorded Files](../../en/cloud-recording/cloud_recording_manage_files.md).

The following diagram illustrates the recorded files generated in individual recording mode when there are two users in the channel, and when you record both audio and video:

![](https://web-cdn.agora.io/docs-files/1575010094214)

The cloud recording service generates four M3U8 files, two for each UID, and several TS/WebM files, which store audio and video separately. After merging the files using the [Audio & Video File Merging script](https://docs.agora.io/en/cloud-recording/cloud_recording_merge_files?platform=All%20Platforms), you get two MP4 files.

### An HTTP request example of `start`

- To enable individual recording mode, the request URL should be: 

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/individual/start
```

- `Content-type` is `application/json;charset=utf-8.`

- `Authorization` is Basic authorization. For more information, see [How to pass the basic HTTP authentication](https://docs.agora.io/en/faq/restful_authentication).

- The request body is:

```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "channelType": 0, 
            "videoStreamType": 1, 
            "subscribeVideoUids": ["123","456"], 
            "subscribeAudioUids": ["123","456"],
            "subscribeUidGroup": 0
        }, 
        "storageConfig": {
            "accessKey": "xxxxxxf",
            "region": 3,
            "bucket": "xxxxx",
            "secretKey": "xxxxx",
            "vendor": 2,
            "fileNamePrefix": ["directory1","directory2"]
       }
   }
}
```

## Considerations

- If you want to merge each UID's audio and video files into one file, you can use our [Merge Audio and Video Files](../../en/cloud-recording/cloud_recording_merge_files.md).

- If you record only video (no audio) or both video and audio, Agora Cloud Recording generates a black video file for a Web user who does not send any video stream.
