
---
title: Demo User Guide
description: 
platform: Linux CPP
updatedAt: Tue Oct 22 2019 01:47:45 GMT+0800 (CST)
---
# Demo User Guide
<div class="alert note">The service for the Cloud Recording SDK will be phased out according to the following schedule:<li>From November 15, the service for the Cloud Recording SDK will no longer be maintained, but you can still use the Cloud Recording SDK before the service termination.</li><li>From Decemeber 15, the service for the Cloud Recording SDK will be terminated. After the service termination, you can no longer use the Cloud Recording SDK.</li></div>

This page shows how to record by running the cloud recording demo. You can also record calls by calling the API methods. For details, see [Quickstart for Cloud Recording](../../en/cloud-recording/cloud_recording_quickstart.md). The command line and API methods implement the same recording functions.

## Prerequisites

Ensure that you meet the following requirements:

- Operating system (physical or virtual):
	- Ubuntu 14.04+ x64
	- CenOS 6.5+ (7.0 recommended) x64
- Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable the Agora Cloud Recording service and get the **cloud_recording_demo.bin** file.
- Deploy a third-party cloud storage. Agora Cloud Recording supports [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls), [Alibaba Cloud](https://www.alibabacloud.com/product/oss), and [Qiniu Cloud](https://www.qiniu.com/en/products/kodo).

## Run the demo

1. Download the [Agora Cloud Recording SDK](http://download.agora.io/acrsdk/release/Agora_Cloud_Recording_SDK_for_Linux_v1_0_0_FULL.tar.gz) package.
2. Open a command-line tool and run the `./cloud_recording_demo.bin` command under the directory of the **cloud_recording_demo.bin** file. You can see the recording parameters and options as follows:

 ```c++
Usage: 
  ./cloud_recording_demo.bin --appId STRING --channelName STRING --uid UINTEGER32 --token STRING --recordingStreamType UINTEGER32 --videoStreamType UINTEGER32 --decryption_mode UINTEGER32 --secret STRING --channelType UINTEGER32 --audioProfile UINTEGER32 --mixWidth UINTEGER32 --mixHeight UINTEGER32 --fps UINTEGER32 --bitrate UINTEGER32 --maxResolutionUid UINTEGER32 --mixedVideoLayoutType UINTEGER32 --maxIdleTime UINTEGER32 --vendor UINTEGER32 --region UINTEGER32 --bucket STRING --accessKey STRING --secretKey STRING 
                             --appId     (app id)
                             --channelName     (channel name)
                             --uid     (User id)
                             --token     (channel token/Optional)
                             --recordingStreamType     (stream types (Optional: 0 for audio only, 1 for video only, 2 for audio and video, default 2))
                             --videoStreamType     (video stream type (Optional: 0 for high video, 1 for low video, default 0. Only works when recordingStreamType is not 0.))
                             --decryption_mode     (decryption mode (Optional: 0 for none, 1 for aes-128-xts, 2 for aes-128-ecb, 3 for aes-256-xts, no decryption by default.))
                             --secret     (decryption key (Optional, empty by default))
                             --channelType     (channel type (Optional: 0 for communication, 1 for live, default 0))
                             —audioProfile     (audio profile (Optional: 0 for single-channel mono, 1 for single-channel music, 2 for multi-channel music. default 0))
                             —mixWidth     (transcoding mixing width (Optional, default 640))
                             —mixHeight     (transcoding mixing height (Optional, default 360))
                             —fps     (transcoding fps (Optional, default 15))
                             —bitrate     (transcoding bitrate (Optional, 500 by default))
                             —maxResolutionUid     (transcoding max resolution uid (Optional))
                             —mixedVideoLayoutType     (mixed video layout mode (Optional: 0 for float, 1 for BestFit, 2 for vertical, 0 by default))
                             —maxIdleTime     (max idle time (Optional, 30 by default))
                             —vendor     (cloud storage vendor (0 for Qiniu, 1 for [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls), 2 for [Alibaba Cloud](https://www.alibabacloud.com/product/oss)))
                             —region     (cloud storage region)
                             —bucket     (cloud storage bucket)
                             —accessKey     (cloud storage access key)
                             —secretKey     (cloud storage encryption key)
```

## Start cloud recording

Set the following parameters to start recording.

Command line example:
```bash
$ ./cloud_recording_demo.bin --appId <Your app ID> --channelName <channle name> --uid <uid> --vendor 0 --region 0 --bucket <your bucket name> --accessKey <access key> --secretKey <encryption key> --channelType 1
```

> - Do not set `uid` as 0.
> - The `appId`, `uid`, `channelName`, `vendor`,  `region`, `bucket`, `accessKey`, and `secretKey` parameters are mandatory to start a recording. Other parameters can be set as needed.
> - If the Agora Native/Web SDK uses Token, `token` must be set.

| **Parameters**       | **Description**                                              |
| -------------------- | ------------------------------------------------------------ |
| appId                | The app ID of the channel to be recorded. The App ID must be the same as the one used in the Agora Native/Web SDK. See <a href="../../en/cloud-recording/token.md">Getting an App ID</a>. |
| channelName          | The name of the channel to be recorded.                      |
| uid                  | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel. Do not set it as 0. **Agora Cloud Recording does not support string user accounts. Ensure that the recording channel uses integer UIDs.** |
| token                | The token used in the channel to be recorded. See <a href="../../en/cloud-recording/token.md">Use Security Keys</a>. |
| recordingStreamType  | The type of the media stream:<li>0: Records audio only.<li>1: Records video only.<li>2: (Default) Records audio and video. |
| videoStreamType      | Sets the type of the video stream:<li>0: (Default) Records the high-stream video.<li>1: Records the low-stream video. |
| decryption_mode      | When the channel is encrypted, the Agora Cloud Recording SDK uses `decryption_mode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Agora Native/Web SDK. <br>The following decryption modes are supported: <li>0: (Default) None.<li>1: AES-128, XTS mode.<li>2: AES-128, ECB mode. <li>3: AES-256, XTS mode. |
| secret               | The decryption password when decryption mode is enabled. The default value is empty. |
| channelType          | Channel profile: <li>0: (Default) Communication profile. This profile is used in one-on-one or group calls, where all users in the channel can talk freely.<li>1: Live broadcast. This profile includes two roles, host and audience. The host sends and receives audio/video, while the audience only receives audio/video. <p>**The Agora Cloud Recording SDK must use the same channel profile as the Agora Native/Web SDK. Otherwise, issues may occur.** |
| audioProfile         | Sets the sample rate, bitrate, encoding mode, and the number of channels: <li>0: (Default) Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 48 Kbps.<li>1: Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps.<li>2: Sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps. |
| mixWidth             | The width of the mixed video (pixels). The default value is 360. For more information, see the <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate, and Bitrate Table</a>. |
| mixHeight            | The height of the mixed video (pixels). The default value is 640. For more information, see the <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate, and Bitrate Table</a>. |
| fps                  | The video frame rate (fps). The default value is 15. For more information, see the <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate, and Bitrate Table</a>. |
| bitrate              | The video bitrate (Kbps). The default value is 500. For more information, see the <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate, and Bitrate Table</a>. |
| maxResolutionUid     | When `mixedVideoLayoutType` is set as vertical layout, you can specify the uid of the large video window by this parameter. |
| mixedVideoLayoutType | Sets the video mixing layout, see [Set Video Layout](../../en/cloud-recording/cloud_layout_guide.md) for details. <li>0: (Default) Floating layout. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.<li>1: Best fit layout. This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.<li>2: Vertical layout. One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users. |
| maxIdleTime          | Agora Cloud Recording automatically stops recording and leaves the channel when there is no user in the recording channel after a time period (30 seconds by default) set by this parameter. The minimum value is 5. |
| vendor     | The third-party cloud storage: <li>0: [Qiniu Cloud](https://www.qiniu.com/en/products/kodo).<li>1: [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls).<li>2: [Alibaba Cloud](https://www.alibabacloud.com/product/oss). |
| region     | The regional information specified by the third-party cloud storage: <ul><li>When the third-party cloud storage is [Qiniu Cloud](https://www.qiniu.com/en/products/kodo) (`vendor` = 0):<ul><li>0: East China <li>1: North China <li>2: South China <li>3: North America</ul>  <li>When the third-party cloud storage is [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls) (`vendor` = 1): <ul><li>0: US_EAST_1 <li>1: US_EAST_2 <li>2: US_WEST_1 <li>3: US_WEST_2  <li>4: EU_WEST_1 <li> 5: EU_WEST_2  <li>6: EU_WEST_3 <li>7: EU_CENTRAL_1 <li>8: AP_SOUTHEAST_1 <li>9: AP_SOUTHEAST_2 <li>10: AP_NORTHEAST_1 <li>11: AP_NORTHEAST_2 <li>12: SA_EAST_1 <li>13: CA_CENTRAL_1 <li>14: AP_SOUTH_1 <li>15: CN_NORTH_1 <li>16: CN_NORTHWEST_1 <li>17: US_GOV_WEST_1</ul> <li>When the third-party cloud storage is [Alibaba Cloud](https://www.alibabacloud.com/product/oss)  (`vendor` = 2): <ul><li>0: CN_Hangzhou <li>1: CN_Shanghai <li>2: CN_Qingdao <li>3: CN_Beijing  <li>4: CN_Zhangjiakou <li> 5: CN_Huhehaote  <li>6: CN_Shenzhen <li>7: CN_Hongkong <li>8: US_West_1 <li>9: US_East_1 <li>10: AP_Southeast_1 <li>11: AP_Southeast_2 <li>12: AP_Southeast_3 <li>13: AP_Southeast_5 <li>14: AP_Northeast_1 <li>15: AP_South_1 <li>16: EU_Central_1 <li>17: EU_West_1 <li>18: EU_East_1|
| bucket               | The bucket of the third-party cloud storage.                 |
| accessKey            | The access key of the third-party cloud storage.             |
| secretKey            | The secret key of the third-party cloud storage.             |
	
## Stop cloud recording

The demo automatically leaves the channel and stops recording when no user is in the channel for more than 30 seconds by default, and triggers the [OnRecorderFailure](../../en/cloud-recording/cloud_recording_api.md) callback.
	
## Upload and manage the recorded files

After the recording starts, the Agora server automatically splits the recorded content into multiple TS files and keeps uploading them to the third-party cloud storage until the recording stops.

### Recording ID

The recording ID is the unique identification of a recording. Each recording session has a unique recording ID.

After the recording starts, all the callbacks provide the recording ID and the channel name, separated by `_`, see the following figure as an example:
	
![](https://web-cdn.agora.io/docs-files/1556079007410)

### Playlist of the recorded files

Each recording session has an M3U8 file, which is a playlist pointing to all the split TS files of the recording. You can use the M3U8 file to play and manage the recorded files.

The name of the M3U8 file consists of the recording ID and the channel name. For example `recording_id_channelName.m3u8`.

### Upload the recorded files

The Agora server automatically uploads the recorded files. Pay attention to the following callbacks.

During the recording, the SDK triggers the following callback once every minute:

- [OnRecordingUploadingProgress](../../en/cloud-recording/cloud_recording_api.md): Reports the upload progress (%) of the recorded files.

After the recording stops, the SDK triggers one of the following callbacks:

- [OnRecordingUploaded](../../en/cloud-recording/cloud_recording_api.md): Occurs when all the recorded files are uploaded to the third-party cloud storage.
- [OnRecordingBackedUp](../../en/cloud-recording/cloud_recording_api.md): Occurs when some of the recorded files fail to upload to the third-party cloud storage and uploads to Agora Cloud Backup instead.

You can get the M3U8 filename in any of these callbacks.

When all the recorded files are uploaded, you can view them in your cloud storage. See the following figure as an example:

![](https://web-cdn.agora.io/docs-files/1556078908213)


## View the log

After recording by running the cloud recording demo, a log file named by the recording ID is generated in the **$(pwd)/cloud_recording_log** folder. 

## Status information

When running the cloud recording demo, the command-line tool prints the following information to show the current status.

### Normal status

| Status information    | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| OnRecordingConnecting        | The app connects to the Agora Cloud Recording server.        |
| OnRecordingStarted           | The Agora Cloud Recording SDK starts recording.              |
| OnRecordingStopped           | The Agora Cloud Recording SDK stops recording.               |
| OnRecordingUploaded          | The recorded file is uploaded to the third-party cloud storage. |
| OnRecordingBackedup          | The recorded file is uploaded to Agora Cloud Backup. If uploading to the third-party cloud storage fails, the Agora server automatically backs up the recorded files to Agora Cloud Backup. |
| OnRecordingUploadingProgress | The upload progress, including the recording ID, the upload progress, and the M3U8 playlist filename. |
| Recorder exit.Wait uploading | Recording in the channel is complete and the recorded file is waiting to be uploaded. |
| Stopped                      | The recording session is finished.                |

### Abnormal status
	
| Status information                         | Description                                                  |
| --------------------------------------------------- | ------------------------------------------------------------ |
| Cloud recording recorder init fail.                 | Fails to initialize the recorder component.                  |
| Cloud recording recorder failed.                    | An error occurs in the recorder component.                   |
| Cloud recording uploader init fail.                 | Fails to initialize the uploader component.                  |
| Cloud recording uploader failed.                    | An error occurs in the uploader component.                   |
| Cloud recording connection lost.                    | The connection to the Agora Cloud Recording server is interrupted. |
| Recording parameter is not right, please check. | Invalid parameters.                                          |
| Current operation is not supported.                 | Invalid operation.                                           |
| Can't connect to cloud recording.                   | Fails to connect to the Agora Cloud Recording server.        |
| No recorded data.                                   | No data is generated.                                        |
| Cloud recording backup failed.                      | An error occurs in the cloud backup.                         |
| No users in the channel.                                | No user is in the channel.                                   |
| Error occurs.                                        | Other errors.                                                |


