
---
title: Cloud Recording API
description: 
platform: Java
updatedAt: Mon Apr 29 2019 02:59:17 GMT+0800 (CST)
---
# Cloud Recording API
| **Interface Class**                                          | **Description**                       |
| ------------------------------------------------------------ | ------------------------------------- |
| <a href="#CloudRecorder">CloudRecorder</a>                   | Provides methods invoked by your app. |
| <a href="#ICloudRecordingEventHandler">ICloudRecordingEventHandler</a> | Enables callbacks to your app.        |

<a name = "CloudRecorder"></a>

## CloudRecorder class

The CloudRecorder class provides the main methods that can be invoked by your app and consists of the following methods:

- [`createCloudRecorderInstance`](#createCloudRecorderInstance)
- [`getRecordingId`](#getRecordingId)
- [`startCloudRecording`](#startCloudRecording)
- [`stopCloudRecording`](#stopCloudRecording)
- [`release`](#release)

<a name = "createCloudRecorderInstance"></a>

### createCloudRecorderInstance

```java
public static CloudRecorder createCloudRecorderInstance(ICloudRecordingEventHandler handler)
```

Creates an Agora Cloud Recording instance. Direct instantiation is not supported.

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `handler` | The Agora Cloud Recording SDK notifies the app of the triggered events by callbacks in the <a href="#ICloudRecorderObserver">ICloudRecordingEventHandler</a> class. |

#### Returns

- `CloudRecorder`: Returns an Agora Cloud Recording instance.

<a name = "getRecordingId"></a>

### getRecordingId

```java
public String getRecordingId()
```

Gets a Cloud Recording ID. Each recording instance has a unique recording ID.

#### Returns

- `String`: Returns a recording ID. The unique identification of the current recording.

<a name = "startCloudRecording"></a>

### startCloudRecording

```java
public RecordingErrorCode startCloudRecording(
    String appId,
    String channelName,
    String token,
    int uid,
    RecordingConfig config,
    CloudStorageConfig storageConfig)
```

Allows the Agora Cloud Recording SDK to join a channel and start recording.

| Parameter       | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `appId`         | The app ID of the channel to be recorded. See <a href="../../en/cloud-recording/token.md">Getting an App ID</a>. |
| `channelName`   | The name of the channel to be recorded.                      |
| `token`         | The Token used in the channel to be recorded. See <a href="../../en/cloud-recording/token.md">Use Security Keys</a>. |
| `uid`           | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel. |
| `config`        | The recording configurations. See <a href="#RecordingConfig">RecordingConfig</a>. |
| `storageConfig` | The Third-party cloud storage configurations. See <a href="#CloudStorageConfig">CloudStorageConfig</a>. |

#### Returns

- `RecordingErrorCode.RecordingErrorOk`: Success.
- ≠ `RecordingErrorCode.RecordingErrorOk`: Failure. See <a href="#RecordingErrorCode">RecordingErrorCode</a>.

<a name = "RecordingConfig"></a>

#### RecordingConfig

```java
RecordingConfig:
public attributes:
  RecordingStreamType recordingStreamType
  DecryptionMode decryptionMode
  String secret
  ChannelType channelType
  AudioProfile audioProfile
  VideoStreamType videoStreamType
  int maxIdleTime
  TranscodingConfig transcodingConfig

public member functions:
  void setTranscodingWidth(int width)
  void setTranscodingHeight(int height)
  void setTranscodingFps(int fps)
  void setTrancodingBitrate(int bitrate)
  void setTranscodingMixedVideoLayout(MixedVideoLayoutType layout, int maxResolutionUid)
```

| Parameter             | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `recordingStreamType` | The type of the media stream:<li>`RecordingStreamType.StreamTypeAudioVideo`: (Default) Records audio and video.<li>`RecordingStreamType.StreamTypeAudioOnly`: Records audio only.<li>`RecordingStreamType.StreamTypeVideoOnly`: Records video only. |
| `decryptionMode`      | hen the channel is encrypted, the Agora Cloud Recording SDK uses `decryption_mode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Agora Native/Web SDK. The following decryption methods are supported:<li>`DecryptionMode.DecryptionModeNone`: (Default) None.<li>`DecryptionMode.DecryptionModeAes128Xts`: AES-128, XTS mode.<li>`DecryptionMode.DecryptionModeAes128Ecb`: AES-128, ECB mode.<li>`DecryptionMode.DecryptionModeAes256Xts`: AES-256, XTS mode. |
| `secret`              | The decryption password when decryption mode is enabled. The default value is NULL. |
| `channelType`         | Channel profile: <li>`ChannelType.ChannelTypeCommunication`: (Default) Communication profile. This profile is used in one-on-one or group calls, where all users in the channel can talk freely. <li>`ChannelType.ChannelTypeLive`: Live broadcast. This profile includes two roles, host and audience. The host sends and receives audio/video, while the audience only receives audio/video.<p>**The Agora Cloud Recording SDK must use the same channel profile as the Agora Native/Web SDK. Otherwise, issues may occur.** |
| `audioProfile`        | Sets the sample rate, bitrate, encoding mode, and the number of channels.<li>`AudioProfile.AudioProfileMusicStandard`: (Default) Sample rate of 48 kHz, music encoding, mono, and bitrate of up to 48 Kbps.<li>`AudioProfile.AudioProfileMusicHighQuality`: Sample rate of 48 kHz, music encoding, mono, and bitrate of up to 128 Kbps.<li>`AudioProfile.AudioProfileMusicHighQualityStereo`: Sample rate of 48 kHz, music encoding, stereo, and bitrate of up to 192 Kbps. |
| `videoStreamType`     | Sets the type of the video stream.<li>`VideoStreamType.VideoStreamTypeHigh`: (Default) Records the high-stream video.<li>`VideoStreamType.VideoStreamTypeLow`: Records the low-stream video. |
| `maxIdleTime`         | The Agora Cloud Recording SDK automatically stops recording when there is no user in the recording channel after a time period (30 seconds by default) set by this parameter. |
| `transcodingConfig`   | The transcoding configurations. See <a href="#TranscodingConfig">TranscodingConfig</a>。 |

<a name = "TranscodingConfig"></a>

#### TranscodingConfig

```java
TranscodingConfig:
public attributes:
  int width
  int height
  int fps
  int bitrate
  int maxResolutionUid
  MixedVideoLayoutType layout
```

| Parameter          | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `width`            | The width of the mixed video (pixels). The default value is 360. The maximum resolution is 1080p and an error occurs if the maximum resolution is exceeded. |
| `height`           | The height of the mixed video (pixels). The default value is 640. The maximum resolution is 1080p and an error occurs if the maximum resolution is exceeded. |
| `fps`              | The video frame rate (fps). The default value is 15.         |
| `bitrate`          | The video bitrate (Kbps). The default value is 500.          |
| `maxResolutionUid` | When `layout` is set as `kMixedVideolayoutTypeVertical` (vertical layout), you can specify the uid of the large video window by this parameter. |
| `layout`           | Sets the video mixing layout.<li>`MixedVideoLayoutType.MixedVideoLayoutTypeFloat`: Default layout. The first user joining the channel displays a large video window on the top of the screen, while video streams from other users are arranged horizontally at the bottom in small video windows. A maximum of two rows with eight windows in each row is supported.<p>![](https://web-cdn.agora.io/docs-files/1550656826359)<li>`MixedVideoLayoutType.MixedVideoLayoutTypeBestFit`: Best fit. This layout automatically scales according to the number of video streams. The number of columns and rows varies depending on the number of video streams. Supports up to 17 video streams.<p>![](https://web-cdn.agora.io/docs-files/1547544528138)<li>`MixedVideoLayoutType.MixedVideoLayoutTypeVertical`: Vertical layout. A specified uid displays a large video window on the left of the screen, while video streams from other users are arranged vertically on the right in small video windows. A maximum of two columns with eight windows in each column is supported.<p>![](https://web-cdn.agora.io/docs-files/1547544539933) |

<a name = "resolution_table"></a>

For detailed video profile, see **Resolution, Frame Rate and Bitrate Table**.

| Resolution | Frame rate (fps) | Base Bitrate (Kbps, for Communication) | Live Bitrate (Kbps, for Live Broadcast) |
| ---------- | ---------------- | -------------------------------------- | --------------------------------------- |
| 160 × 120  | 15               | 65                                     | 130                                     |
| 120 × 120  | 15               | 50                                     | 100                                     |
| 320 × 180  | 15               | 140                                    | 280                                     |
| 180 × 180  | 15               | 100                                    | 200                                     |
| 240 × 180  | 15               | 120                                    | 240                                     |
| 320 × 240  | 15               | 200                                    | 400                                     |
| 240 × 240  | 15               | 140                                    | 280                                     |
| 424 × 240  | 15               | 220                                    | 440                                     |
| 640 × 360  | 15               | 400                                    | 800                                     |
| 360 × 360  | 15               | 260                                    | 520                                     |
| 640 × 360  | 30               | 600                                    | 1200                                    |
| 360 × 360  | 30               | 400                                    | 800                                     |
| 480 × 360  | 15               | 320                                    | 640                                     |
| 480 × 360  | 30               | 490                                    | 980                                     |
| 640 × 480  | 15               | 500                                    | 1000                                    |
| 480 × 480  | 15               | 400                                    | 800                                     |
| 640 × 480  | 30               | 750                                    | 1500                                    |
| 480 × 480  | 30               | 600                                    | 1200                                    |
| 848 × 480  | 15               | 610                                    | 1220                                    |
| 848 × 480  | 30               | 930                                    | 1860                                    |
| 640 × 480  | 10               | 400                                    | 800                                     |
| 1280 × 720 | 15               | 1130                                   | 2260                                    |
| 1280 × 720 | 30               | 1710                                   | 3420                                    |
| 960 × 720  | 15               | 910                                    | 1820                                    |
| 960 × 720  | 30               | 1380                                   | 2760                                    |

<a name = "CloudStorageConfig"></a>

#### CloudStorageConfig

```java
CloudStorageConfig:
public attributes:
  CloudStorageVendor vendor
  int region
  String bucket
  String accessKey
  String secretKey
```

| Parameter   | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| `vendor`    | The third-party cloud storage.<li>`CloudStorageVendor.CloudStorageVendorQiniu`: Qiniu Cloud.<li>`CloudStorageVendor.CloudStorageVendorAws`: Amazon S3. <li>`CloudStorageVendor.CloudStorageVendorAliyun`: Aliyun. |
| `region`    | The regional information specified by the third-party cloud storage.<br>When the third-party cloud storage is Qiniu Cloud ( `vendor` = `CloudStorageVendor.CloudStorageVendorQiniu` ): <li>0：Huadong <li>1：Huabei <li>2：Huanan <li>3：Beimei  <br>When the third-party cloud storage is Amazon S3 ( `vendor` = `CloudStorageVendor.CloudStorageVendorAws` ): <li>0：US_EAST_1 <li>1：US_EAST_2 <li>2：US_WEST_1 <li>3：US_WEST_2  <li>4：EU_WEST_1 <li> 5：EU_WEST_2  <li>6：EU_WEST_3 <li>7：EU_CENTRAL_1 <li>8：AP_SOUTHEAST_1 <li>9：AP_SOUTHEAST_2 <li>10：AP_NORTHEAST_1 <li>11：AP_NORTHEAST_2 <li>12：SA_EAST_1 <li>13：CA_CENTRAL_1 <li>14：AP_SOUTH_1 <li>15：CN_NORTH_1 <li>16：CN_NORTHWEST_1 <li>17：US_GOV_WEST_1 <br>When the third-party cloud storage is Aliyun  (`vendor` = `CloudStorageVendor.CloudStorageVendorAliyun`):<li>0：CN_Hangzhou <li>1：CN_Shanghai <li>2：CN_Qingdao <li>3：CN_Beijin  <li>4：CN_Zhangjiakou <li> 5：CN_Huhehaote  <li>6：CN_Shenzhen <li>7：CN_Hongkong <li>8：US_West_1 <li>9：US_East_1 <li>10：AP_Southeast_1 <li>11：AP_Southeast_2 <li>12：AP_Southeast_3 <li>13：AP_Southeast_5 <li>14：AP_Northeast_1 <li>15：AP_South_1 <li>16：EU_Central_1 <li>17：EU_West_1 <li>18：EU_East_1|
| `bucket`    | The bucket of the third-party cloud storage.                 |
| `accessKey` | The access key of the third-party cloud storage.             |
| `secretKey` | The secret key of the third-party cloud storage.             |

<a name = "stopCloudRecording"></a>

### stopCloudRecording

```java
public RecordingErrorCode stopCloudRecording()
```

Allows the Cloud Recording SDK to leave the channel and stop recording. If the `stopCloudRecording` method is not called, the SDK automatically leaves the channel and stops recording when no user is in the channel for more than 30s by default, and triggers the `RecordingErrorRecorderExit` callback to inform the app of the exception.
	
After the recording stops, you need to call `release` to destroy the recording instance.

#### Returns

- `RecordingErrorCode.RecordingErrorOk`: Success.
- ≠ `RecordingErrorCode.RecordingErrorOk`: Failure. See <a href="#RecordingErrorCode">RecordingErrorCode</a>。

<a name = "release"></a>

### release

```java
public RecordingErrorCode release(boolean cancelCloudRecording)
```

Destroys the `CloudRecorder` instance and releases all recording resources. To use Agora Cloud Recording again, you need to call the `createCloudRecorderInstance` method to recreate a new instance.


| Parameter                   | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| `cancelCloudRecording` | Sets whether or not to record in the background: <li>`true`: The recording and uploading processes stop.<li>`false`: (Default) The recording and uploading processes continue in the background on the Agora server. |
	
<a name = "ICloudRecordingEventHandler"></a>

## ICloudRecordingEventHandler class

The `ICloudRecordingEventHandler` class provides callbacks to your app and consists of the following callbacks:

- [`onRecordingConnecting`](#onRecordingConnecting)
- [`onRecordingStarted`](#onRecordingStarted)
- [`onRecordingStopped`](#onRecordingStopped)
- [`onRecordingUploaded`](#onRecordingUploaded)
- [`onRecordingBackedUp`](#onRecordingBackedUp)
- [`onRecordingUploadingProgress`](#onRecordingUploadingProgress)
- [`onRecorderFailure`](#onRecorderFailure)
- [`onUploaderFailure`](#onUploaderFailure)
- [`onRecordingFatalError`](#onRecordingFatalError)

<a name = "onRecordingConnecting"></a>

### onRecordingConnecting

```java
public void onRecordingConnecting(String recording_id)
```

Occurs when the app is connecting to the Agora Cloud Recording server.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "onRecordingStarted"></a>

### onRecordingStarted

```java
public void onRecordingStarted(String recording_id)
```

Occurs when the Agora Cloud Recording SDK starts recording.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "onRecordingStopped"></a>

### onRecordingStopped

```java
public void onRecordingStopped(String recording_id)
```

Occurs when the Agora Cloud Recording SDK stops recording.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "onRecordingUploaded"></a>

### onRecordingUploaded

```java
public void onRecordingBackedUp(String recording_id, String file_name)
```

Occurs when all the recorded files are uploaded to the third-party cloud storage.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `file_name`    | The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files.   |

<a name = "onRecordingBackedUp"></a>

### onRecordingBackedUp

```java
public void onRecordingBackedUp(String recording_id, String file_name)
```

Occurs when the recorded file is uploaded to Agora Cloud Backup. 
	
If any of the files fail uploading to the third-party cloud storage, the Agora server automatically uploads these files to Agora Cloud Backup and triggers the `onRecordingBackedUp` callback after the recording stops.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `file_name`    | The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files.       |

<a name = "onRecordingUploadingProgress"></a>

### onRecordingUploadingProgress

```java
public void onRecordingUploadingProgress(
    String recording_id,
    int progress,
    String recording_playlist_filename)
```

Occurs when the recorded file is uploading to the third-party cloud storage. The SDK triggers this callback once every minute to inform the app of the uploading progress.

| Parameter                     | Description                                                  |
| ----------------------------- | ------------------------------------------------------------ |
| `recording_id`                | The recording ID. The unique identification of the current recording. |
| `progress`                    | The uploading progress. Shows the percentage of the uploaded files in the recorded files. |
| `recording_playlist_filename` | The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files.    |

<a name = "onRecorderFailure"></a>

### onRecorderFailure

```java
public void onRecorderFailure(
    String recording_id,
    RecordingErrorCode code,
    String msg)
```

Occurs when an error occurs in the recorder component. This callback is only used to notify the state of the recorder component, and you do not need to do anything with this.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `code`         | Error code.                                                  |
| `msg`          | Error message.                                               |

<a name = "onUploaderFailure"></a>

### onUploaderFailure

```java
public void onUploaderFaliure(
    String recording_id,
    RecordingErrorCode code,
    String msg)
```

Occurs when an error occurs in the uploader component. This callback is only used to notify the state of the uploader component, and you do not need to do anything with this.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `code`         | Error code.                                                  |
| `msg`          | Error message.                                               |

<a name = "onRecordingFatalError"></a>

### onRecordingFatalError

```java
public void onRecordingFatalError(
    String recording_id,
    RecordingErrorCode code)
```

Occurs when the Agora Cloud Recording SDK encounters an error that cannot be recovered automatically. You need to call the `release` method to release all resources and check the status of the Agora Cloud Recording server according to the returned error code.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `code`         | Error code.                                                  |


<a name = "RecordingErrorCode"></a>

## Error codes

| Error Codes                         | Description                                                  | Solution                                                     |
| ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `RecordingErrorOk`                 | No error.                                                    | None.                                                        |
| `RecordingErrorConnectError`       | Fails to connect to the Agora Cloud Recording server.        | Check the permission of appid and the network connection status. |
| `RecordingErrorDisconnected`       | The connection to the Agora Cloud Recording server is interrupted. | Check the network connection status.                         |
| `RecordingErrorInvalidParameter`   | Invalid parameters.                                          | Check the parameters.                                        |
| `RecordingErrorInvalidOperation`   | Invalid operation.                                           | Check the operations.                                        |
| `RecordingErrorNoUsers`            | No user is in the channel.                                   | Call the Release (false) method to release resources.        |
| `RecordingErrorNoRecordedData`     | No data is generated.                                        | Call the Release (false) method to release resources.        |
| `RecordingErrorRecorderInit`       | An error occurs in initializing the recorder component.      | Call the Release (true) method to restart recording.          |
| `RecordingErrorRecorderFailed`     | An error occurs in the recorder component.                   | Call the Release (true) method to restart recording.          |
| `RecordingErrorUploaderInit`       | An error occurs in initializing the uploader component.      | Call the Release (true) method to restart recording.          |
| `RecordingErrorUploaderFailed`     | An error occurs in the uploader component.                   | Call the Release (true) method to restart recording.          |
| `RecordingErrorBackupFailed`       | An error occurs in the cloud backup.                         | Call the Release (false) method to release resources.        |
| `RecordingErrorRecorderExpireExit` | Abnormal exit.                                               | Call the Release (true) method to restart recording.          |
| `RecordingErrorGeneralError`       | Other errors.                                                | Call the Release (true) method to restart recording.          |
