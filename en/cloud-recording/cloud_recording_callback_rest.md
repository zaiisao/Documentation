
---
title: Agora Cloud Recording RESTful API Callback Service
description: Cloud recording restful api callback
platform: All Platforms
updatedAt: Wed Sep 04 2019 09:42:49 GMT+0800 (CST)
---
# Agora Cloud Recording RESTful API Callback Service
The Agora Cloud Recording RESTful API provides the callback service. You can set up an HTTP/HTTPS server to receive the event notifications of Agora Cloud Recording. When an event occurs, the Agora Cloud Recording service notifies the Agora notification center, and then the notification center notifies your server through an HTTP/HTTPS request.

## Enable the callback service

Contact [sales-us@agora.io](http://sales-us@agora.io/) to enable the callback service with the following information:

- The URL address of your HTTP/HTTPS server used to receive the callbacks
- Your App ID
- The event types for which you need to receive notifications

> You can set one URL to receive the callbacks for one App ID.

After configuring the message notification service, we will automatically generate a `secret`. The `secret` is a string type key that can be used to generate a signature. You can save the `secret` to generate the signature and verify with the `"Agora-Signature"` parameter in the HTTP requests. See [Signature verification](#signature) for details. The signature verification is optional.

## Data format

### Notification format 

Notifications are sent to your server through HTTP/HTTPS POST requests in JSON format.

The Content-type header in the POST request is application/json.

### Response format

After receiving an event notification, your server needs to respond to the Agora notification center in the application/json format.

The Agora notification center assumes that sending an event notification fails in the following situations:

- Your server does not respond within 20 seconds.
- The HTTP response status code is not 200.

In these situations, the Agora notification center resends the event notification up to four times.

## Common callback information 

Each type of event notification contains the following properties:

- `notificationId`: String. The unique identifier of the notification.
- `eventType`: Number. The event type. For details, see [callback event](../../en/cloud-recording/cloud_recording_callback_rest.md).
- `eventMs`: Number. The time (ms) when Agora Cloud Recording notifies the Agora notification center (Unix timestamp).
- `notifyMs`: Number. The time (ms) when the Agora notification center sends the HTTP/HTTPS request (Unix timestamp).  When the notification center resends the request, `notifyMs` is updated.

- `payload`: JSON. The main body of the notification. `payload` in each type of event notification contains the following properties:
  - `cname`: String. The name of the channel to be recorded.
  - `uid`: String. The UID of the recording client.
  - `sid`: String. The recording ID. The unique identifier of each recording.
  - `sequence`: String. The serial number of the notifications, starting from 0. You can use this parameter to identify notifications that are random, lost, or resent.
  - `sendts`: Number. The time (UTC) when the event happens.
  - `serviceType`: Number. The type of the Agora callback service. 0 indicates cloud recording.
  - `noticeLevel`: Number. The notification level. The higher the number, the higher the level of attention.
    - `1`: Debug 
    - `2`: Info 
    - `3`: Notice 
    - `4`: Important 
    - `5`: Critical 
  - `details`: JSON. The details of the callback events are described as follows.

## Callback events

The types (`eventType`) of the Agora Cloud Recording callback events are listed as follows: 

- [`1`](#1): An error occurs during the recording.
- [`2`](#2): A warning occurs during the recording.
- [`3`](#3): The status of the Agora Cloud Recording service changes.
-  [`4`](#4): The M3U8 playlist file is generated.
- [`30`](#30): The upload service starts.
- [`31`](#31): All the recorded files are uploaded to the specified third-party cloud storage.
- [`32`](#32): All the recorded files are uploaded, but at least one file is uploaded to Agora Cloud Backup. 
- [`33`](#33): The current upload progress.
- [`40`](#40): The recording starts.
- [`41`](#41): The recording exits.
- [`42`](#42): The recording service starts slicing the first recording file.

###  <a name="1"></a>cloud_recording_error

`eventType` 1 indicates that an error occurs during the recording. `details` contains the following fields:

- `msgName`: String. The name of the message, `"cloud_recording_error"`.
- `module`: Number. The module in which the error occurs.
  - `0`: The recorder
  - `1`: The uploader
  - `2`: The Agora Cloud Recording Service
- `errorLevel`: Number. The error level.
  - `1`: Debug
  - `2`: Minor
  - `3`: Medium
  - `4`: Major
  - `5`: Fatal. A fatal error may cause the recording to exit. If you receive a message of this level, call the `query` API to check the current status and process the error according to the error notifications.
- `errorCode`: Number. The error code. 
  - If the error occurs in the recorder, see [error code and warning code](https://docs.agora.io/cn/Recording/the_error_native).
  - If the error occurs in the uploader, see [upload error code](#uploaderr). 
  - If the error occurs in the Agora Cloud Recording Service, see [cloud recording platform error code](#clouderr).
  - If you do not find any error code in the above-mentioned pages, contact our technical support.

- `stat`: Number. The event status. 0 indicates normal status; other values indicate abnormal status.
- `errorMsg`: String. The detailed description of the error.

###  <a name="2"></a>cloud_recording_warning

`eventType` 2 indicates that a warning occurs during the recording. `details` contains the following fields:

- `msgName`: String. The message name, `cloud_recording_warning`.
- `module`: Number. The name of the module where the warning occurs.
  - `0`: The recorder.
  - `1`: The uploader.
- `warnCode`: Number. The warning code. 
  - If the warning occurs in the recorder, see [error code and warning code](https://docs.agora.io/cn/Recording/the_error_native).
  - If the warning occurs in the uploader, see [upload warning code](#uploadwarn).

###  <a name="3"></a>cloud_recording_status_update

`eventType` 3 indicates that the status of the Agora Cloud Recording service changes. `details` contains the following fields:

- `msgName`: String. The message name, `cloud_recording_status_update`.
- `status`: Number. The current status of the cloud recording. See [cloud recording service status code](#state).
- `recordingMode`: Number. The recording mode. Only supports the composite recording mode. The value is 0.
- `fileList`: String. The name of the M3U8 playlist file generated by the recording.

###  <a name="4"></a>cloud_recording_file_infos

`eventType` 4 indicates that an M3U8 playlist file is generated and uploaded. Each recording instance has an M3U8 file as the playlist pointing to all the recorded TS files. You can play and manage your recordings with the M3U8 file.

`details` contains the following fields:

- `msgName`: String. The message name, `cloud_recording_file_infos`.
- `fileList`: String. The name of the M3U8 file.

###  <a name="30"></a>uploader_started

`eventType` 30 indicates that the upload service starts, and `details` contains the following fields:

- `msgName`: String. The message name, `uploader_started`.
- `status`: Number. The event status. 0 indicates normal status; other values indicate abnormal status.

###  <a name="31"></a>uploaded

`eventType` 31 indicates that all the recorded files are uploaded to the specified third-party cloud storage, and `details` contains the following fields:

- `msgName`: String. The message name, `uploaded`.
- `status`: Number. The event status. 0 indicates normal status; other values indicate abnormal status.

###  <a name="32"></a>backuped

`eventType` 32 indicates that all the recorded files are uploaded, but at least one of them is uploaded to Agora Cloud Backup. Agora Cloud Backup automatically uploads the files to the specified third-party cloud storage. `details` contains the following fields:

- `msgName`: String. The message name, `backuped`.
- `status`: Number. The event status. 0 indicates normal status; other values indicate abnormal status.

###  <a name="33"></a>uploading_progress

`eventType` 33 indicates the current upload progress. The Agora server notifies you of the upload progress once every minute after the recording starts. `details` contains the following fields:

- `msgName`: String. The message name, `uploading_progress`.
- `progress`: Number. A constantly changing number between 0 and 10000, which equals to the ratio of the uploaded file to the recorded file multiplied by 10000. After the recording exits, 10000 indicates that the upload is complete.

###  <a name="40"></a>recorder_started

`eventType` 40 indicates that the recording service starts, and `details` contains the following fields:

- `msgName`: String. The message name, `recorder_started`.
- `status`: Number. The event status. 0 indicates normal status; other values indicate abnormal status.

###  <a name="41"></a>recorder_leave

`eventType` 41 indicates that the recording service leaves the channel, and `details` contains the following fields:

- `msgName`: String. The message name, `recorder_leave`.
- `leaveCode`: Number. The code that indicates why the recording service leaves the channel.
  - `0`: The initialization fails.
  - `1`: The AgoraCoreService process receives the SIGINT signal. 
  - `2`: The Agora Clouding Recording Service automatically leaves the channel and stops recording as there is no user in the channel.
  - `4`: The AgoraCoreService process receives the SIGTERM signal.
  - `8`: The recording application calls the stop cloud recording method to leave the channel.

### <a name="42"></a>recorder_slice_start

`eventType` 42 indicates that the recording service starts slicing the first recording file, and `details` contains the following fields:

- `msgName`: String. The message name, `recorder_slice_start`.
- `startUtcMs`：Number. The time (ms) in UTC when the recording starts (the starting time of the first slice file).
- `discontinueUtcMs`：Number. In most cases, this field is the same as `startUtcMs`. When the recording is interrupted, the Agora Cloud Recording service automatically resumes the recording and  triggers this event. In this case, the value of this field is the time (ms) in UTC when the last slice file stops.

For example, you start a recording session and get this event notification in which `startUtcMs` is the starting time of slice file 1. Then, the recording session continues and records slice file 2 to slice file N. If an error occurs when recording slice file N + 1, you lose this file and the recording session stops. Agora Cloud Recording automatically resumes the recording and triggers this event again. In the event notification, `startUtcMs` is the time when slice file N + 2 starts, and `discontinueUtcMs` is the time when slice file N stops.


## Reference

###  <a name="uploaderr"></a>Error codes for uploading

| Error code | Description                                                  |
| :----------- | :----------------------------------------------------------- |
| 32           | An error occurs in the configuration of the third-party cloud storage. |
| 47           | The recording file upload fails.                             |
| 51           | An error occurs when the uploader processes the recorded files. |

### <a name="uploadwarn"></a>Warning codes for uploading

| Warning code | Description                                |
| :----------- | :----------------------------------------- |
| 31           | Re-uploads to the specified cloud storage. |
| 32           | Re-uploads to Agora Cloud Backup.          |

### <a name="clouderr"></a>Error codes for the Agora Cloud Recording platform

| Error code | Description                                               |
| :----------- | :-------------------------------------------------------- |
| 50           | A timeout occurs in uploading the recorded files.         |
| 52           | A timeout occurs in starting the Cloud Recording Service. |

### <a name="state"></a>Status codes for the Agora Cloud Recording service

| Status code | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| 0           | The recording has not started.                               |
| 1           | The initialization is complete.                              |
| 2           | The recorder is starting.                                    |
| 3           | The uploader is ready.                                       |
| 4           | The recorder is ready.                                       |
| 5           | The first recorded file is uploaded. After uploading the first file, the status is `5` when the recording is running. |
| 6           | The recording stops.                                         |
| 7           | The Agora Cloud Recording service stops.                     |
| 8           | The recording is ready to exit.                              |
| 20          | The recording exits abnormally.                              |

### <a name="signature"></a>Signature verification

We generate the `key` directly. After using the HMAC/SHA1 algorithm, we generate the `Agora-Signature`. You can use the saved `key` to verify the signature with the following code: 

- If you use Python

```python
# !-*- coding: utf-8 -*-
Import hashlib
Import hmac
 
# Get the request body of the notification callback, note that this is a string, not a JSON object
Request_body = '{"eventMs":1560408533119,"eventType":10,"noticeId":"4eb720f0-8da7-11e9-a43e-53f411c2761f","notifyMs":1560408533119,"payload":{"a":"1" , "b": 2}, "productId": 1}'
 
secret = 'secret'
 
Signature = hmac.new(secret, request_body, hashlib.sha1).hexdigest()
 
Print(signature) # 033c62f40f687675f17f0f41f91a40c71c0f134c
```

- If you use Node.js

```javascript
Const crypto = require('crypto')
 
// Get the request body of the notification callback, note that this is a string, not a JSON object
Const requestBody = '{"eventMs":1560408533119,"eventType":10,"noticeId":"4eb720f0-8da7-11e9-a43e-53f411c2761f","notifyMs":1560408533119,"payload":{"a":"1 ","b":2},"productId":1}'
 
Const secret = 'secret'
 
Const signature = crypto.createHmac('sha1', secret).update(requestBody, 'utf8').digest('hex')
 
Console.log(signature) // 033c62f40f687675f17f0f41f91a40c71c0f134c
```
