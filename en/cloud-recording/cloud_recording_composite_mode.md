
---
title: Composite Recording
description: 
platform: All Platforms
updatedAt: Wed Oct 16 2019 09:59:32 GMT+0800 (CST)
---
# Composite Recording
## Overview

Agora Cloud Recording supports two recording modes:

- Individual recording mode: Agora Cloud Recording generates one audio and/or video file for each UID.
- Composite recording mode: This is the default recording mode. The audio and video of all UIDs in the channel are mixed into one single file.

This article explains how to record a call in composite recording mode by using the RESTful API.

Before proceeding, make sure that you know how to use Agora Cloud Recording by using the RESTful API. For more information, see [Agora Cloud Recording RESTful API Quickstart](../../en/cloud-recording/cloud_recording_rest.md). You must select a recording mode before a recording starts. You cannot switch between the two modes after the recording starts.

> We assume that each UID in the channel sends audio and video streams. If a UID does not send any audio or video stream, such as the audience in a live broadcast, no file will be generated, except for Web users.

## Implementation

To enable composite recording mode, set `mode` as `mix` when you call the [`start`](../../en/cloud-recording/cloud_recording_api_rest.md) mode.

Based on the setting of `streamTypes`, the recorded files are as follows:

| Settings                                   | Recorded content      | Recorded files                                               |
| :----------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| Set `streamTypes` as `0`                   | Audio only            | One M3U8 file and several TS files. The TS files store only audio. |
| Set `streamTypes` as `1`                   | Video only (no audio) | One M3U8 file and several TS files. The TS files store only video (no audio). |
| Set `streamTypes` as `2` (default setting) | Audio and video       | One M3U8 file and several TS files. The TS files store video (with audio). |

The naming convention for the recorded files is as follows:

- M3U8 files: `<sid>_<cname>.m3u8`
- TS files: `<sid>_<cname>_<utc>.ts`

Where:

- `<sid>` is the recording ID;
- `<cname>` is the channel name;
- `<utc>` is the starting time (UTC) of the TS file. The time zone is UTC+0. The timestamp consists of the year, month, day, hour, minute, second, and millisecond. For example, if `<utc>` is `20190611073246073`, the starting time of the TS file is 07:32:46.073 a.m., June 11, 2019.

### An HTTP request example of `start`

- To enable composite recording mode, the request URL should be: 

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/mix/start
```
- `Content-type` is `application/json;charset=utf-8`.
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
            "audioProfile": 1,
            "channelType": 0, 
            "videoStreamType": 1, 
            "transcodingConfig": {
                "height": 640, 
                "width": 360,
                "bitrate": 500, 
                "fps": 15, 
                "mixedVideoLayout": 1,
                "backgroundColor": "#FF0000",
            "subscribeVideoUids": ["123","456"], 
            "subscribeAudioUids": ["123","456"],
            "subscribeUidGroup": 0
           }
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

If you record only video (no audio) or both video and audio, Agora Cloud Recording generates a black video file for a Web user who does not send any video stream.
