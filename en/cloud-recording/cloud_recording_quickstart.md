
---
title: Quickstart for Cloud Recording
description: 
platform: CPP
updatedAt: Mon Apr 29 2019 02:58:52 GMT+0800 (CST)
---
# Quickstart for Cloud Recording
This page demonstrates how to integrate the Agora Cloud Recording SDK to record a call or live broadcast.

> The Agora Cloud Recording SDK joins a channel as a dummy client. It needs to join the same channel and use the same App ID and channel profile as the Agora Native/Web SDK.

## Prerequisites

Ensure that you meet the following requirements:

- Open the following TCP ports: 4447, 8913 and 9700.
- Open the following UDP ports: 53 and 4444.
- Operating system (physical or virtual):
	- Ubuntu 14.04+ x64
	- CenOS 6.5+ (7.0 recommended) x64
- Install a compiler: g++ 4.8 or later.
- Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable the Agora Cloud Recording service.
- Deploy a third-party cloud storage. Currently, Agora Cloud Recording supports Qiniu Cloud, Aliyun (recommended), and Amazon S3.

## Integrate the SDK

The Agora Cloud Recording SDK package includes the `IAgoraCloudRecording.h` and the `libagora-cloud-recording-client.so` files. 

Integrate the SDK in the following steps:

1. Add the `libagora-cloud-recording-client.so` and `IAgoraCloudRecording.h` files in the SDK package to your project and ensure that these files are connected to your project.
2. Include the `IAgoraCloudRecording.h` file.

## Create an instance

```c++
ICloudRecorder* CreateCloudRecorderInstance(ICloudRecorderObserver *observer);
```

Call the [`CreateCloudRecorderInstance`](../../en/cloud-recording/cloud_recording_api.md) method  to create a Cloud Recording instance and connect it with your app. You can create multiple instances to record simultaneously.

>  When an instance is no longer needed, call the [Release](../../en/cloud-recording/cloud_recording_api.md) method to release all resources related to the recording.

## Start cloud recording

```c++
virtual int StartCloudRecording(
  const char* appId,
  const char* channel_name,
  const char* token,
  const uid_t uid,
  const RecordingConfig& recording_config,
  const CloudStorageConfig &storage_config) = 0;
```

After creating a Cloud Recording instance, call the [`StartCloudRecording`](../../en/cloud-recording/cloud_recording_api.md) method to start recording. In this method, fill in the following parameters:

- appId: (Mandatory) App ID of the channel to be recorded.
- channel_name: (Mandatory) Name of the channel to be recorded.
- token: (Optional) Token used in the channel to be recorded. See [Use Security Keys](../../en/cloud-recording/token.md).
- uid: (Mandatory) UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel.
- recording_config: (Optional) Recording configuration. See [`RecordingConfig`](../../en/cloud-recording/cloud_recording_api.md) for details. 
- storage_config: (Mandatory). Third-party cloud storage configuration. See  [`CloudStorageConfig`](../../en/cloud-recording/cloud_recording_api.md) for details.

> Ensure the settings of App ID, channel name, and token are exactly the same as those of the Agora Native/Web SDK for the channel to be recorded.

After calling the [`StartCloudRecording`](../../en/cloud-recording/cloud_recording_api.md) method, the SDK starts recording when detecting other users in the channel and triggers the [`OnRecordingStarted`](../../en/cloud-recording/cloud_recording_api.md) callback to inform the app.

## Stop cloud recording

```c++
virtual int StopCloudRecording() = 0;
```

Call the [`StopCloudRecording`](../../en/cloud-recording/cloud_recording_api.md) method to stop recording and the SDK triggers the [`OnRecordingStopped`](../../en/cloud-recording/cloud_recording_api.md) callback.

> To start recording again after calling `StopCloudRecording`, create another instance.

If the [`StopCloudRecording`](../../en/cloud-recording/cloud_recording_api.md) method is not called, the SDK automatically leaves the channel and stops recording when no user is in the channel for more than 30 seconds by default, and triggers the [`OnRecorderFailure`](../../en/cloud-recording/cloud_recording_api.md) callback with the error code `RecordingErrorNoUsers` to inform the app of the abnormal exit.

## Release resources

```c++
virtual void Release(bool cancelCloudRecording = false) = 0;
```

Call the [`Release`](../../en/cloud-recording/cloud_recording_api.md) method to destroy the `ICloudRecorder` instance and release all recording resources.  

In this method, the `cancelCloudRecording` parameter is `false` by default, and normally you should use the default setting. In case errors occur and you need to restart the recording service, set the parameter as `true`, see [Error codes](../../en/cloud-recording/cloud_recording_api_java.md) for details.

> To use Cloud Recording again, you must call the  [`CreateCloudRecorderInstance`](../../en/cloud-recording/cloud_recording_api.md) method to create a new instance. 

## Upload and manage the recorded files

After the recording starts, the Agora server automatically splits the recorded content into multiple TS files and keeps uploading them to the third-party cloud storage until the recording stops.

### Recording ID

The recording ID is the unique identification of a recording. Each recording instance has a unique recording ID.

After creating the recording instance, you can call [`GetRecordingId`](../../en/cloud-recording/cloud_recording_api.md) to get the recording ID. You can also get the recording ID from any of the callback.

### Playlist of the recorded files

Each recording instance has an M3U8 file, which is a playlist pointing to all the split TS files of the recording. You can use the M3U8 file to play and manage the recored files.

The name of the M3U8 file consists of the recording ID and the channel name, for example  `recording_id_channel_name.M3U8`.

### Upload the recorded files

Agora server automatically upload the recorded files for you. Pay attention to the following callbacks.

During the recording, the SDK triggers the following callback once every minute:

- [`OnRecordingUploadingProgress`](../../en/cloud-recording/cloud_recording_api.md): reports the percentage of the uploaded files in the recorded files. 

After the recording stops, the SDK triggers one of the following callbacks:

- [`OnRecordingUploaded`](../../en/cloud-recording/cloud_recording_api.md): if all the recorded files are uploaded to the third-party cloud storage. 
- [`OnRecordingBackedUp`](../../en/cloud-recording/cloud_recording_api.md): if some of the recorded files fail to upload to the third-party cloud storage and get uploaded to Agora Cloud Backup. Agora Cloud Backup will automatically upload these files to your cloud storage. If you cannot [play the recorded files](../../en/cloud-recording/cloud_recording_onlineplay.md) after five minutes, please contact Agora technical support.

You can get the M3U8 file name in any of the above callbacks.


## Error codes

The SDK might trigger the following callbacks when errors occur:

- `OnRecorderFailure`: Occurs when an error occurs in the recorder component. Usually you do not need to do anything about this.
- `OnUploaderFailure`: Occurs when an error occurs in the uploader component. Usually you do not need to do anything about this.
- `OnRecordingFatalError`: Occurs when the SDK encounters an error that cannot be recovered automatically. You need to call the `Release` method to release all resources and check the status of the Agora Cloud Recording server according to the returned error code. 

See [Error codes](../../en/cloud-recording/cloud_recording_api.md) for more information.
