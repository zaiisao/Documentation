
---
title: Agora Cloud Recording RESTful API
description: Cloud recording restful api reference
platform: All Platforms
updatedAt: Tue Sep 17 2019 07:24:57 GMT+0800 (CST)
---
# Agora Cloud Recording RESTful API
Ensure that you know how to [record with the RESTful API](../../en/cloud-recording/cloud_recording_rest.md) before reading this document.

## <a name="auth"></a>Authentication

The RESTful API only supports HTTPS. Before sending HTTP requests, you must pass the basic Agora HTTP authentication (the `Authorization` parameter in the HTTP request head) with `api_key:api_secret`:

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

You can find your Customer ID and Customer Certificate on the [RESTful API](https://dashboard.agora.io/restful) page in Dashboard. See [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.

## Data format

All requests are sent to the host: `api.agora.io`.

- Request: See the examples in the following APIs.
- Response: The response content is in JSON format.

> All the request URLs and request bodies are case sensitive.

## Call sequence

Use the RESTful API in the following steps:

1. Call the [`acquire`](#acquire) method to request a resource ID for cloud recording.
2. Call the  [`start`](#start) method within five minutes after getting the resource ID to start the recording.
3. Call the [`stop`](#stop) method to stop the recording.

During the recording, you can call the [`query`](#query) method to check the recording status.

See [Sample code](../../en/cloud-recording/cloud_recording_rest.md) for an example using the RESTful API.

![](https://web-cdn.agora.io/docs-files/1559641367749)

## <a name="acquire"></a>Gets the recording resource

Before starting the cloud recording, you need to call this method to get a resource ID.

> One resource ID can only be used for one recording session.

- Method:POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/acquire

> The request frequency limit is 10 times per second.

If this method call succeeds, you get a resource ID (`resourceId`) from the HTTP response body. The resource ID is valid for five minutes, so you need to [start recording](#start) with this resource ID within five minutes.

### Parameters

The following parameter is required in the URL.

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `appid`   | String | The [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) used in the channel to be recorded. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | Name of the channel to be recorded.                          |
| `uid`           | String | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel, for example `"527841"`. Do not set it as `"0"`. |
| `clientRequest` | JSON   | A specific client request that is empty for this method.     |

### An HTTP request example of `acquire`

- The request URL is `https://api.agora.io/v1/apps/<yourappid>/cloud_recording/acquire`.
- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body:

```json
{
    "cname": "httpClient463224",
    "uid": "527841",
    "clientRequest":{
    }
}
```

### A response example of `aquire`

```json
"Code": 200,
"Body":
{
 "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.

## <a name="start"></a>Starts cloud recording

After getting a resource ID, call this method to start cloud recording.

- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/mode/\<mode\>/start

> The request frequency limit is 10 times per second.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) used in the channel to be recorded. |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `mode`       | String | The recording mode. Only `mix` (composite mode) is supported. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | Name of the channel to be recorded.                          |
| `uid`           | String | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel, for example `"527841"`. Do not set it as `"0"`. |
| `clientRequest` | JSON   | A specific client request. See the following list for details. |

`clientRequest` requires the following parameters:

- `token`: (Optional) String. The [dynamic key](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-namekeyadynamic-key) used in the channel to be recorded. Ensure you set this parameter if the recording channel uses a token.

- `recordingConfig`: (Optional) JSON. The recording configuration. 

  - `streamTypes`: (Optional) Number. The type of the recorded media stream.
    - `0`: Records audio only.
    - `1`: Records video only.
    - `2`: (Default) Records audio and video.
  - `decryptionMode`: (Optional) Number. When the channel is encrypted, Agora Cloud Recording uses `decryptionMode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Agora Native/Web SDK. 
    - `0`: (Default) None.
    - `1`: AES-128, XTS mode.
    - `2`: AES-128, ECB mode.
    - `3`: AES-256, XTS mode.
  - `secret`: (Optional) String. The decryption password when decryption mode is enabled. 
  - `channelType`: (Optional) Number. Channel profile. Agora Cloud Recording must use the same channel profile as the Agora Native/Web SDK. Otherwise, issues may occur.
    - `0`: (Default) Communication profile. 
    - `1`: Live broadcast profile.
  - `audioProfile`: (Optional) Number. Sets the sample rate, bitrate, encoding mode, and the number of channels:
    - `0`: (Default) Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 48 Kbps.
    - `1`: Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps.
    - `2`: Sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps.
  - `videoStreamType`: (Optional) Number. Sets the type of the video stream. 
    - `0`: (Default) Records the high-stream video.
    - `1`: Records the low-stream video.
  - `maxIdleTime`: (Optional) Number. Agora Cloud Recording automatically stops recording and leaves the channel when there is no user in the recording channel after a time period (30 seconds by default) set by this parameter. The minimum value is 5.
  - `transcodingConfig`: (Optional) JSON. The transcoding configuration. If you omit this parameter, the recording uses the default values. If you set this parameter, ensure that you set `width`, `height`, `fps`, and `bitrate`.
    - `width`: (Mandatory) Number. The width of the mixed video (pixels). The default value is 360. The maximum resolution is 1080p and an error occurs if the maximum resolution is exceeded.
    - `height`: (Mandatory) Number. The height of the mixed video (pixels). The default value is 640. The maximum resolution is 1080p and an error occurs if the maximum resolution is exceeded.
    - `fps`: (Mandatory) Number. The video frame rate (fps). The default value is 15.
    - `bitrate`: (Mandatory) Number. The video bitrate (Kbps). The default value is 500.
    - `maxResolutionUid`: (Optional) String. When `mixedVideoLayout` is set as `2` (vertical layout), you can specify the UID of the large video window by this parameter.
    - `backgroundColor`: (Optional) String. The background color of the canvas (the display window or screen) in RGB hex value. The string starts with a "#". The default value is `"#000000"`, the black color.
    - `mixedVideoLayout`: (Optional) Number. Sets the video mixing layout. 0, 1, and 2 are the [predefined layouts](../../en/cloud-recording/cloud_layout_guide.md). If you set this parameter as 3, you need to set the layout by the `layoutConfig` parameter.
      - `0`: (Default) Floating layout. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.
      - `1`: Best fit layout. This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.
      - `2`: Vertical layout. One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users.
      - `3`: Customized layout. Set the `layoutConfig` parameter to customize the layout.
    - `layoutConfig`: (Optional) JSONArray. An array of the configuration of each user's region. Supports 17 users at most. Each user's region configuration is a JSON object with the following parameters:
      - `uid`: (Optional) String. The string contains the UID of the user displaying the video in the region. If this parameter is not specified, the configurations apply in the order of the users joining the channel.
      - `x_axis`: (Mandatory) Float. Relative horizontal position of the top-left corner of the region. The value is between 0.0 (leftmost) and 1.0 (rightmost). 
      - `y_axis`: (Mandatory) Float. Relative vertical position of the top-left corner of the region. The value is between 0.0 (top) and 1.0 (bottom).
      - `width`: (Mandatory) Float. Relative width of the region. The value is between 0.0 and 1.0.
      - `height`: (Mandatory) Float. Relative height of the region. The value is between 0.0 and 1.0.
      - `alpha`: (Optional) Float. The transparency of the image. The value is between 0.0 (transparent) and 1.0 (opaque). The default value is 1.0.
      - `render_mode`: (Optional) Number. The video display mode:
        - `0`: (Default) Cropped mode. Uniformly scales the video until it fills the visible boundaries (cropped). One dimension of the video may have clipped contents.
        - `1`: Fit mode. Uniformly scales the video until one of its dimension fits the boundary (zoomed to fit). Areas that are not filled due to the disparity in the aspect ratio will be filled with black.

- `storageConfig`: JSON. The third-party cloud storage configuration.

  - `vendor`: Number. The third-party cloud storage vendor.    

    - `0`: [Qiniu Cloud](https://www.qiniu.com/en/products/kodo)
    - `1`: [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls)
    - `2`: [Alibaba Cloud](https://www.alibabacloud.com/product/oss)

  - `region`: Number. The regional information specified by the third-party cloud storage:
    When the third-party cloud storage is [Qiniu Cloud](https://www.qiniu.com/en/products/kodo) (`vendor` = 0):

    - `0`: East China
    - `1`: North China 
    - `2`: South China
    - `3`: North America  

    When the third-party cloud storage is [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls) (`vendor` = 1):

    - `0`: US_EAST_1 
    - `1`: US_EAST_2 
    - `2`: US_WEST_1 
    - `3`: US_WEST_2 
    - `4`: EU_WEST_1 
    - `5`: EU_WEST_2 
    - `6`: EU_WEST_3 
    - `7`: EU_CENTRAL_1 
    - `8`: AP_SOUTHEAST_1 
    - `9`: AP_SOUTHEAST_2 
    - `10`: AP_NORTHEAST_1 
    - `11`: AP_NORTHEAST_2 
    - `12`: SA_EAST_1 
    - `13`: CA_CENTRAL_1 
    - `14`: AP_SOUTH_1 
    - `15`: CN_NORTH_1 
    - `16`: CN_NORTHWEST_1 
    - `17`: US_GOV_WEST_1 

    When the third-party cloud storage is [Alibaba Cloud](https://www.alibabacloud.com/product/oss) (`vendor` = 2):

    - `0`: CN_Hangzhou 
    - `1`: CN_Shanghai 
    - `2`: CN_Qingdao 
    - `3`: CN_Beijing
    - `4`: CN_Zhangjiakou 
    - `5`: CN_Huhehaote 
    - `6`: CN_Shenzhen 
    - `7`: CN_Hongkong 
    - `8`: US_West_1 
    - `9`: US_East_1 
    - `10`: AP_Southeast_1 
    - `11`: AP_Southeast_2 
    - `12`: AP_Southeast_3 
    - `13`: AP_Southeast_5 
    - `14`: AP_Northeast_1 
    - `15`: AP_South_1 
    - `16`: EU_Central_1 
    - `17`: EU_West_1 
    - `18`: EU_East_1

  - `bucket`: String. The bucket of the third-party cloud storage.

  - `accessKey`: String. The access key of the third-party cloud storage.

  - `secretKey`: String. The secret key of the third-party cloud storage.

### An HTTP request example of `start`

- The request URL is `https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/mix/start`.
- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body:

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
            }
        }, 
        "storageConfig": {
            "accessKey": "xxxxxxf",
            "region": 3,
            "bucket": "xxxxx",
            "secretKey": "xxxxx",
            "vendor": 2
        }
    }
}
```

### A response example of `start`

```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.
- `sid`: String. The recording ID. The unique identification of the current recording.

## <a name="update"></a>Updates the video mixing layout

During a recording, you can call this method to update the video mixing layout multiple times.

This method call overrides the existing layout configurations. For example, if you set the `backgroundColor` parameter as `"#FF0000"` (red) when starting a recording and call this method to update the layout without setting the `backgroundColor` parameter, the background color changes back to black (the default value).

- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/updateLayout

> The request frequency limit is 10 times per second.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) used in the channel to be recorded. |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `sid`        | String | The recording ID created by the [`start`](#start) method.    |
| `mode`       | String | The recording mode. Only `mix` (composite mode) is supported. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | Name of the channel to be recorded.                          |
| `uid`           | String | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel, for example `"527841"`. Do not set it as `"0"`. |
| `clientRequest` | JSON   | A specific client request. See the following list for details. |

`clientRequest` requires the following parameters:

- `maxResolutionUid`: (Optional) String. When the `layoutType` parameter is set as `2` (vertical layout), you can specify the UID of the large video window by this parameter.
- `mixedVideoLayout`: (Optional) Number. Sets the video mixing layout. 0, 1, and 2 are the [predefined layouts](../../en/cloud-recording/cloud_layout_guide.md). If you set this parameter as 3, you need to set the layout by the `layoutConfig` parameter.
  - `0`: (Default) Floating layout. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.
  - `1`: Best fit layout. This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.
  - `2`: Vertical layout. One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users.
  - `3`: Customized layout. Set the `layoutConfig` parameter to customize the layout.
- `backgroundColor`: (Optional) String. The background color of the canvas (the display window or screen) in RGB hex value. The string starts with a "#". The default value is `"#000000"`, the black color.
- `layoutConfig`: (Optional) JSONArray. An array of the configuration of each user's region. Supports 17 users at most. Each user's region configuration is a JSON object with the following parameters:
  - `uid`: (Optional) String. The string contains the UID of the user displaying the video in the region. If this parameter is not specified, the configurations apply in the order of the users joining the channel.
  - `x_axis`: (Mandatory) Float. Relative horizontal position of the top-left corner of the region. The value is between 0.0 (leftmost) and 1.0 (rightmost). 
  - `y_axis`: (Mandatory) Float. Relative vertical position of the top-left corner of the region. The value is between 0.0 (top) and 1.0 (bottom).
  - `width`: (Mandatory) Float. Relative width of the region. The value is between 0.0 and 1.0.
  - `height`: (Mandatory) Float. Relative height of the region. The value is between 0.0 and 1.0.
  - `alpha`: (Optional) Float. The transparency of the image. The value is between 0.0 (transparent) and 1.0 (opaque). The default value is 1.0.
  - `render_mode`: (Optional) Number. The video display mode:
    - `0`: (Default) Cropped mode. Uniformly scales the video until it fills the visible boundaries (cropped). One dimension of the video may have clipped contents.
    - `1`: Fit mode. Uniformly scales the video until one of its dimension fits the boundary (zoomed to fit). Areas that are not filled due to the disparity in the aspect ratio will be filled with black.

### A request example of `updateLayout `

- The request URL is `https://api.agora.io/v1/apps/<appid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/updateLayout`.
- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authorization, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body:

 ```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "mixedVideoLayout": 3,
        "backgroundColor": "#FF0000",
        "layoutConfig": [
        {
             "uid": "1",
             "x_axis": 0.1,
             "y_axis": 0.1,
             "width": 0.1,
             "height": 0.1,
             "alpha": 1.0,
             "render_mode": 1
         },
        {
             "uid": "2",
             "x_axis": 0.2,
             "y_axis": 0.2,
             "width": 0.1,
             "height": 0.1,
             "alpha": 1.0,
             "render_mode": 1
         }
         ]
     }
}
```

### A response example of `updateLayout`

```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.
- `sid`: String. The recording ID. The unique identification of the current recording.

## <a name="query"></a>Queries the recording status

- Method: GET
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/query

> The request frequency limit is 10 times per second.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) used in the channel to be recorded. |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `sid`        | String | The recording ID created by the [`start`](#start) method.    |
| `mode`       | String | The recording mode. Only `mix` (composite mode) is supported. |

### An HTTP request example of `query`

- The request URL is `https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/mix/query`.
- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.

### A response example of `query`

```json
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "fileList": "xxxxx.m3u8",
    "status": "5",
    "sliceStartTime": "1562724971626"
    }    
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.
- `sid`: String. The recording ID. The unique identification of the current recording.
- `serverResponse`: JSON. Recording status details.
  - `fileList`: String. The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files.
  - `status`: Number. The recording status.
    - `0`: The recording has not started.
    - `1`: The initialization is complete.
    - `2`: The recorder is starting.
    - `3`: The uploader is ready.
    - `4`: The recorder is ready.
    - `5`: The first recorded file is uploaded. After uploading the first file, the status is `5` when the recording is running.
    - `6`: The recording stops.
    - `7`: The Agora Cloud Recording service stops.
    - `8`: The recording is ready to exit.
    - `20`: The recording exits abnormally.
  - `sliceStartTime`: String. The time when the recording starts. Unix timestamp in ms.

## <a name="stop"></a>Stops cloud recording

When the recording finishes, call this method to leave the channel and stop recording. To use Agora Cloud Recording again, you need to call the  [`acquire`](#acquire) method for a new resource ID.

- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/stop

> - The request frequency limit is 10 times per second.
> - Agora Cloud Recording automatically leaves the channel and stops recording when no user is in the channel for more than 30 seconds by default.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) used in the channel to be recorded. |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `sid`        | String | The recording ID created by the [`start`](#start) method.    |
| `mode`       | String | The recording mode. Only `mix` (composite mode) is supported. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | Name of the channel to be recorded.                          |
| `uid`           | String | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel, for example `"527841"`. Do not set it as `"0"`. |
| `clientRequest` | JSON   | A specific client request that is empty for this method.     |

### An HTTP request example of `stop`

- The request URL is `https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/mix/stop`.

- `Content-type` is `application/json;charset=utf-8`.

- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.

- The request body:

  ```json
  {
    "cname": "httpClient463224",
    "uid": "527841",
    "clientRequest":{
    }
  }
  ```

### A response example of `stop`

```json
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "fileList": "xxxxx.m3u8",
    "uploadingStatus": "uploaded"
  }
}

```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.
- `sid`: String. The recording ID. The unique identification of the current recording.
- `serverResponse`: JSON. Recording details.
  - `fileList`: String. The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files.
  - `uploadingStatus`: String. The upload status.
    - `"uploaded"`: All the recorded files are uploaded to the third-party cloud storage.
    - `"backuped"`:  Some of the recorded files fail to upload to the third-party cloud storage and upload to Agora Cloud Backup instead. Agora Cloud Backup automatically uploads these files to your cloud storage. 
    - `"unknown"`: Unknown status. 

## <a name="status"></a>Status code

| Code | Description                                                  |
| :--- | :----------------------------------------------------------- |
| 200  | The request handle is successful.                            |
| 201  | The request is successful and new resources are created.     |
| 206  | The server is delivering only part of the resource due to a range header sent by the client. |
| 400  | The input is in the wrong format.                            |
| 404  | The requested resource could not be found.                   |
| 500  | An internal error occurs in the Agora Cloud Recording RESTful API service. |
| 504  | The server was acting as a gateway or proxy and did not receive a timely response from the upstream server. |

## Errors

This section lists the common errors in using the Agora Cloud Recording RESTful API. If you encounter other errors, contact Agora technical support.

- `2`: Invalid parameter. Possible reasons:
  - The parameter type is wrong.
  - The parameter is spelt wrong. All the parameters are case sensitive.
  - The mandatory parameters are missing.
- `8`: Errors in the HTTP request header fields. Possible reasons:
  - Content-type is wrong. Ensure that the `Content-type` field is `application/json;charset=utf-8`.
  - `cloud_recording` is missing in the request URL.
  - The HTTP method is wrong.
- `433`: The resource ID has expired. You need to start recording within five minutes after getting a resource ID. Call [acquire](#acquire) to get a new resource ID.
- `501`: The recording service is exiting. This error might occur when you call [query](#query) after [stop](#stop).
- `1001`: Fails to parse the resource ID. Call [acquire](#acquire) to get a new resource ID.
- `1003`: The App ID or recording ID (sid) does not match the resource ID. Ensure that the App ID or recording ID matches the resource ID in each recording session.
- `1004`: The recording is already running. Do not repeat the [start](#start) request.
- `1005`: The resource ID is already used. One resource ID can only be used in one recording session.
- `1013`: The channel name is invalid. The string length must be less than 64 bytes. Supported character scopes are:
  - The 26 lowercase English letters: a to z.
  - The 26 uppercase English letters: A to Z.
  - The 10 numbers: 0 to 9.
  - The space.
  - "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",".
- `1028`: Invalid parameters in the request body of the [`updateLayout`](#update) method.
- `"invalid appid"`: Invalid App ID. Ensure that the [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) is correct. If the App ID is correct and you still get this error message, contact Agora technical support.
