
---
title: Demo User Guide
description: 
platform: Java
updatedAt: Mon Apr 29 2019 02:58:58 GMT+0800 (CST)
---
# Demo User Guide
This page demonstrates how to record by running the cloud recording demo. You can also record calls by calling the API methods. For details, see [Record by API](../../en/cloud-recording/cloud_recording_quickstart_java.md). The command line and API methods implement the same recording functions.

## Prerequisites

- Operating system (physical or virtual):
	- Ubuntu 14.04+ x64
	- CenOS 6.5+ (7.0 recommended) x64
- Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable the Agora Cloud Recording service.
- Set up the Java environment and install OpenJDK 1.6 or later.
- Deploy a third-party cloud storage. Currently, Agora Cloud Recording supports Qiniu Cloud, Aliyun and Amazon S3.

## Run the demo

Open the Cloud Recording SDK package, go to the **demo** folder, and run the following command in a command-line tool:
 
```bash
java -Xbootclasspath/a:../lib/agora-cloud-recording-sdk.jar: -jar -Dsun.boot.library.path=../lib target/cloud-recording-client-demo-1.0-SNAPSHOT.jar
```

You can see the recording parameters and options as follows:

```java
Usage: Cloud Recording Java Demo
 -a,--appId <arg>                  app id
 -A,--audioProfile <arg>           audio profile, (Optional: 0 for single
                                   channel mono, 1 for single channel
                                   music, 2 for multi channel music.
                                   default 0)
 -AK,--accessKey <arg>             cloud storage access key
 -b,--bitrate <arg>                transcoding bitrate(Optional, 500 by
                                   default
 -B,--bucket <arg>                 cloud storage bucket
 -c,--channelType <arg>            channel type (Optional 0 for
                                   communication, 1 for live, default 0
 -d,--decryption_mode <arg>        decryption mode(Optional:, 0 for none,
                                   1 for aes-128-xts, 2 for aes-128-ecb, 3
                                   for aes-256-xts, no decryption by
                                   default.
 -e,--secret <arg>                 secret(Optional, empty by default
 -f,--fps <arg>                    transcoding fps(Optional, default 15)
 -h,--help                         help
 -H,--mixHeight <arg>              transcoding mixing height (Optional,
                                   default 360)
 -i,--maxIdleTime <arg>            max idle time(Optional, 30 by default)
 -l,--mixedVideoLayoutType <arg>   mixed video layout mode(Optional:, 0
                                   for float, 1 for BestFit, 2 for
                                   vertical, 0 by defualt)
 -n,--channelName <arg>            channel name
 -r,--recordingStreamType <arg>    stream types (Optional: 0 for audio
                                   only,  1 for video only, 2 for audio
                                   and video, default 2)
 -R,--region <arg>                 cloud storage region
 -SK,--secretKey <arg>             cloud storage secret key
 -t,--token <arg>                  channel token/Optional
 -u,--uid <arg>                    User id
 -U,--maxResolutionUid <arg>       transcoding max resolution
                                   uid(Optional)
 -v,--videoStreamType <arg>        video stream type(Optional: 0 for high
                                   video, 1 for low video, default 0. Only
                                   works when recordingStreamType is not
                                   0.)
 -V,--vendor <arg>                 cloud storage vendor(Optional, 0 for
                                   Qiniu, 1 for AWS, 2 for Aliyun
 -W,--mixWidth <arg>               transcoding mixing width (Optional,
                                   defualt 640)
```

## Start cloud recording

Set the following parameters to start recording.

Command line example:
```bash
java -Xbootclasspath/a:../lib/agora-cloud-recording-sdk.jar: -jar -Dsun.boot.library.path=../lib target/cloud-recording-client-demo-1.0-SNAPSHOT.jar -a <your App ID> -n <channel name> -u <uid>  -V <vendor> -R <region> -B <bucket> -AK <access key> -SK <secret key> -c 0
```

> The `appId`, `uid`, `channelName`, `vendor`, `region`, `bucket`, `accessKey` and `secretKey` parameters are mandatory to start a recording. Other parameters can be set as needed.

If all the parameters are filled in correctly, the demo triggers the `onRecordingStarted` callback to indicate that the recording starts.

| **Parameters**       | **Description**                                              |
| -------------------- | ------------------------------------------------------------ |
| appId                | The app ID of the channel to be recorded. The App ID must be the same as the Agora Native/Web SDK. See <a href="../../en/cloud-recording/token.md">Getting an App ID</a>. |
| channelName          | The name of the channel to be recorded.                      |
| uid                  | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel. |
| `vendor`     | The third-party cloud storage. <li>0: Qiniu Cloud.<li>1: Amazon S3.<li>2: Aliyun. |
| `region`     | The regional information specified by the third-party cloud storage. <br>When the third-party cloud storage is Qiniu Cloud (`vendor` = 0):<li>0: Huadong <li>1: Huabei <li>2: Huanan <li>3: Beimei  <br>When the third-party cloud storage is Amazon S3 (`vendor` = 1): <li>0: US_EAST_1 <li>1: US_EAST_2 <li>2: US_WEST_1 <li>3: US_WEST_2  <li>4: EU_WEST_1 <li> 5: EU_WEST_2  <li>6: EU_WEST_3 <li>7: EU_CENTRAL_1 <li>8: AP_SOUTHEAST_1 <li>9: AP_SOUTHEAST_2 <li>10: AP_NORTHEAST_1 <li>11: AP_NORTHEAST_2 <li>12: SA_EAST_1 <li>13: CA_CENTRAL_1 <li>14: AP_SOUTH_1 <li>15: CN_NORTH_1 <li>16: CN_NORTHWEST_1 <li>17: US_GOV_WEST_1 <br>When the third-party cloud storage is Aliyun  (`vendor` = 2):<li>0：CN_Hangzhou <li>1：CN_Shanghai <li>2：CN_Qingdao <li>3：CN_Beijin  <li>4：CN_Zhangjiakou <li> 5：CN_Huhehaote  <li>6：CN_Shenzhen <li>7：CN_Hongkong <li>8：US_West_1 <li>9：US_East_1 <li>10：AP_Southeast_1 <li>11：AP_Southeast_2 <li>12：AP_Southeast_3 <li>13：AP_Southeast_5 <li>14：AP_Northeast_1 <li>15：AP_South_1 <li>16：EU_Central_1 <li>17：EU_West_1 <li>18：EU_East_1|
| bucket               | The bucket of the third-party cloud storage.                 |
| accessKey            | The access key of the third-party cloud storage.             |
| secretKey            | The secret key of the third-party cloud storage.             |
| token                | The token used in the channel to be recorded. See <a href="../../en/cloud-recording/token.md">Use Security Keys</a>. |
| recordingStreamType  | Type of the media stream:<li>0: Records audio only.<li>1: Records video only.<li>2: (Default) Records audio and video. |
| videoStreamType      | Sets the type of the video stream.<li>0: (Default) Records the high-stream video.<li>1: Records the low-stream video. |
| decryption_mode      | When the channel is encrypted, the Agora Cloud Recording SDK uses `decryption_mode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Agora Native/Web SDK. The following decryption methods are supported:<li>0: (Default) None.<li>1: AES-128, XTS mode.<li>2: AES-128, ECB mode. <li>3: AES-256, XTS mode. |
| secret               | The decryption password when decryption mode is enabled. The default value is Empty. |
| channelType          | Channel profile: <li>0: (Default) Communication profile. This profile is used in one-on-one or group calls, where all users in the channel can talk freely.<li>1: Live broadcast. This profile includes two roles, host and audience. The host sends and receives audio/video, while the audience only receives audio/video. <p>**The Agora Cloud Recording SDK must use the same channel profile as the Agora Native/Web SDK. Otherwise, issues may occur.** |
| audioProfile         | Sets the sample rate, bitrate, encoding mode, and the number of channels.<li>0: (Default) Sample rate of 48 kHz, music encoding, mono, and bitrate of up to 48 Kbps.<li>1: Sample rate of 48 kHz, music encoding, mono, and bitrate of up to 128 Kbps.<li>2: Sample rate of 48 kHz, music encoding, stereo, and bitrate of up to 192 Kbps. |
| mixWidth             | The width of the mixed video (pixels). The default value is 360. For more information, see <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate and Bitrate Table</a>. |
| mixHeight            | The height of the mixed video (pixels). The default value is 640. For more information, see <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate and Bitrate Table</a>. |
| fps                  | The video frame rate (fps). The default value is 15. For more information, see <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate and Bitrate Table</a>. |
| bitrate              | The video bitrate (Kbps). The default value is 500. For more information, see <a href="../../en/cloud-recording/cloud_recording_api.md">Resolution, Frame Rate and Bitrate Table</a>. |
| maxResolutionUid     | When `mixedVideoLayoutType` is set as vertical layout, you can specify the uid of the large video window by this parameter. |
| mixedVideoLayoutType | Sets the video mixing layout. <li>0: Default layout. The first user joining the channel displays a large video window on the top of the screen, while video streams from other users are arranged horizontally at the bottom in small video windows. A maximum of two rows with eight windows in each row is supported.<p>![](https://web-cdn.agora.io/docs-files/1550657612208)<li>1: Best fit. This layout automatically scales according to the number of video streams. The number of columns and rows varies depending on the number of video streams. Supports up to 17 video streams.<p>![](https://web-cdn.agora.io/docs-files/1547544528138)<li>2: Vertical layout. A specified uid displays a large video window on the left of the screen, while video streams from other users are arranged vertically on the right in small video windows. A maximum of two columns with eight windows in each column is supported.<p> ![](https://web-cdn.agora.io/docs-files/1547544539933) |
| maxIdleTime          | The Agora Cloud Recording SDK automatically stops recording when there is no user in the recording channel after a time period (30 seconds by default) set by this parameter. |

	
## Stop cloud recording

After the recording starts, you can control this recording by the following commands as shown in the figure:
- `help`: View the commands supported currently.
- `stop`: Stop the recording.
- `quit`: Exit the demo.

![](https://web-cdn.agora.io/docs-files/1555582611511)

The demo automatically leaves the channel and stops recording when no user is in the channel for more than 30 seconds by default, and triggers the `onRecorderFailure` callback.
	
When the cloud recording stops, the SDK triggers the `onRecordingStopped` callback and the demo automatically releases the resource and exits.

## Upload and manage the recorded files

After the recording starts, the Agora server automatically splits the recorded content into multiple TS files and keeps uploading them to the third-party cloud storage until the recording stops.

### Recording ID

The recording ID is the unique identification of a recording. Each recording session has a unique recording ID.

After the recording starts, all the callbacks provide the recording ID and the channel name, seprarated by `_`, see the figure below as an example:



### Playlist of the recorded files

Each recording session has an M3U8 file, which is a playlist pointing to all the split TS files of the recording. You can use the M3U8 file to play and manage the recored files.

The name of the M3U8 file consists of the recording ID and the channel name, for example `recording_id_channelName.m3u8`.

### Upload the recorded files

Agora server automatically upload the recorded files for you. Pay attention to the following callbacks.

During the recording, the SDK triggers the following callback once every minute:

- `onRecordingUploadingProgress`: reports the percentage of the uploaded files in the recorded files.

After the recording stops, the SDK triggers one of the following callbacks:

- `onRecordingUploaded`: if all the recorded files are uploaded to the third-party cloud storage.
- `onRecordingBackedUp`: if some of the recorded files fails uploading to the third-party cloud storage and gets uploaded to Agora Cloud Backup.

You can get the M3U8 file name in any of the above callbacks.

When all the recored files are uploaded, you can view them in your cloud storage, see the figure below as an example:

![img](https://web-cdn.agora.io/docs-files/1556078908213)

## View the log

After recording by running the cloud recording demo, a log file named by the recording ID generates in the **$(pwd)/cloud_recording_log** folder. The log file can help you when issue occurs.


## Status information

When running the cloud recording demo, the command-line tool prints the following status information to show the current running status.

### Normal status

| Status information    | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| OnRecordingConnecting        | The app connects to the Agora Cloud Recording server.        |
| OnRecordingStarted           | The Agora Cloud Recording SDK starts recording.              |
| OnRecordingStopped           | The Agora Cloud Recording SDK stops recording.               |
| OnRecordingUploaded          | The recorded file is uploaded to the third-party cloud storage. |
| OnRecordingBackedup          | The recorded file is uploaded to Agora Cloud Backup. If uploading to the third-party cloud storage fails, the Agora server automatically backs up the recorded files to Agora Cloud Backup. |
| OnRecordingUploadingProgress | The uploading progress, including recording ID, upload progress and M3U8 playlist filename. |
| Recorder exit.Wait uploading | Recording in the channel is over and the recorded file is waiting for upload. |
| Stopped                      | The recording session is completely finished.                |

### Abnormal status

| Status information                         | Description                                                  |
| --------------------------------------------------- | ------------------------------------------------------------ |
| Cloud recording recorder init fail.                 | Fails to initialize the recorder component.                  |
| Cloud recording recorder failed.                    | An error occurs in the recorder component.                   |
| Cloud recording uploader init fail.                 | Fails to initialize the uploader component.                  |
| Cloud recording uploader failed.                    | An error occurs in the uploader component.                   |
| Cloud recording connection lost.                    | The connection to the Agora Cloud Recording server is interrupted. |
| Recording parameter not right, please have a check. | Invalid parameters.                                          |
| Current operation is not supported.                 | Invalid operation.                                           |
| Can't connect to cloud recording.                   | Fails to connect to the Agora Cloud Recording server.        |
| No recorded data.                                   | No data is generated.                                        |
| Cloud recording backup failed.                      | An error occurs in the cloud backup.                         |
| No users in channel.                                | No user is in the channel.                                   |
| Error occur.                                        | Other errors.                                                |
