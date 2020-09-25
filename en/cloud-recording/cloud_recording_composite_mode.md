
---
title: Composite Recording
description: 
platform: All Platforms
updatedAt: Fri Sep 25 2020 04:20:28 GMT+0800 (CST)
---
# Composite Recording
## Overview

Agora Cloud Recording supports two recording modes:

- Individual recording mode: Records the audio and video as separate files for each UID in a channel.
- Composite recording mode: Generates a single mixed audio and video file for all or specified UIDs in a channel.

This article explains how to record a call in composite recording mode by using the RESTful API.

Before proceeding, ensure that you know how to use Agora Cloud Recording by using the RESTful API. For more information, see [Agora Cloud Recording RESTful API Quickstart](../../en/cloud-recording/cloud_recording_rest.md). You must select a recording mode before a recording starts. You cannot switch between the two modes after a recording starts.

See [Differences between individual recording mode and composite recording mode](https://docs.agora.io/en/faq/recording_mode) to decide which mode you should use.

> We assume that each UID in the channel sends audio and video streams. If a UID does not send any audio or video stream, such as the audience in a live interactive streaming, no file is generated, except for Web users.

## Implementation

To enable composite recording mode, set `mode` to `mix` when calling [`start`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/start).

Based on the setting of `streamTypes`, the recorded files are as follows:

| Settings                                   | Recorded content      | Recorded files                                               |
| :----------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| Set `streamTypes` as `0`                   | Audio only            | One M3U8 file and several TS files. The TS files store only audio. |
| Set `streamTypes` as `1`                   | Video only (no audio) | One M3U8 file and several TS files. The TS files store only video (no audio). |
| Set `streamTypes` as `2` (default setting) | Audio and video       | One M3U8 file and several TS files. The TS files store video (with audio). |

The following diagram illustrates the recorded files generated in composite recording mode when there are two users in the channel, and when you record both audio and video:

![](https://web-cdn.agora.io/docs-files/1575011002382)

The cloud recording service generates one M3U8 file and several TS/WebM files, which combine the audio and video of all users. After converting the files using the [Format Converter script](https://docs.agora.io/en/cloud-recording/cloud_recording_convert_format?platform=All%20Platforms), you get one MP4 file.

For detailed information about the naming convention of the recorded files, see [Manage Recorded Files](../../en/cloud-recording/cloud_recording_manage_files.md).

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
                "backgroundColor": "#FF0000"
						},
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

- If a Web user never publishes the local video, or unpublishes the video during a recording session, the recording service fills the region of that user with the color of the canvas; if the user mutes the local video, the region becomes black.
- If a native user stops sending streams during a recording session, the region of that user shows the last frame; if the user leaves the channel, the recording service fills the region of that user with the color of the canvas.
