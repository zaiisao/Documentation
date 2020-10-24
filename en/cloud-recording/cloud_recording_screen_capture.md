
---
title: Capture Screenshots
description: snapshot
platform: All Platforms
updatedAt: Fri Oct 23 2020 08:40:53 GMT+0800 (CST)
---
# Capture Screenshots
## Overview

This article describes how to use Cloud Recording RESTful APIs to take video screenshots and upload the screenshots to your third-party cloud storage.

<div class="alert note">You cannot record a channel and take screenshots simultaneously in one session. To both record a channel and take screenshots, call <a href="https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/acquire">acquire</a> twice to get two resource IDs, one for recording and the other for capturing screenshots.</div>

Ensure you understand how to use Cloud Recording RESTful APIs for recording before reading this article. See [Cloud Recording RESTful API Quickstart](https://docs.agora.io/en/cloud-recording/cloud_recording_rest) for details.



## Implementation

To take screenshots of the video streams, configure `snapshotConfig` when calling [`start`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/start). Cloud Recording generates screenshots in the specified format at a set interval, and then uploads the screenshots to your third-party cloud storage.

### Example

```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "channelType": 1,
            "subscribeUidGroup": 0
        }, 
        "snapshotConfig": {
            "captureInterval": 5,
            "fileType": ["jpg"]
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

## Naming convention

The screenshot naming convention is `<sid>_<cname>__uid_s_<uid>__uid_e_video_<utc>.jpg`. 

Where:

- `<sid>`: Recording ID
- `<cname>`: Channel name
- `<uid>`: User ID
- `<utc>`: The UTC time when the screenshot was generated. The time zone is UTC+0, and the timestamp consists of the year, month, day, hour, minute, second, and millisecond. For example, if `utc` is `20190611073246073`, the screenshot was generated on June 11, 2019, at 07:32:46.073 a.m.

The screenshot file format is determined by `fileType` you set in `start`. It currently supports JPG only.

When [a cloud recording server is disconnected or the process killed](https://docs.agora.io/en/faq/high-availability), the cloud recording service enables the high availability mechanism, where the fault processing center automatically switches to a new server within 90 seconds to resume the service. When the service enables the mechanism, the screenshot filenames are prepended with `bak<n>`, where `n` stands for the number of times the mechanism is enabled in a recording and starts off with `0`. Taking the file name of `bak0_sid_channel1__uid_s_123__uid_e_video_20190611073246073.jpg` as an example, `bak0` indicates that this file was generated after the service enabled the high availability mechanism for the first time.


## Considerations

Pay attention to the following parameters as incorrect settings result in errors and failure to capture screenshots:

- In the URL of your request, `mode` must be `individual`.
- You cannot set both `recordingFileConfig` and `snapshotConfig` at the same time.
- `streamTypes` must be `1` or `2`.
- If you have set `subscribeAudioUid`, you must also set `subscribeVideoUids`.

If a user stops sending the video stream during a session, the Cloud Recording service ceases taking screenshots until the stream resumes. The [Agora message notification service](../../en/cloud-recording/cloud_recording_callback_rest.md) sends you a callback notifying you of the event. The event does not affect other users in the channel.
