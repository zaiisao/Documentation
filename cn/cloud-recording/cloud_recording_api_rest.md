
---
title: 云端录制 RESTful API
description: Cloud recording restful api reference
platform: All Platforms
updatedAt: Wed Aug 21 2019 04:07:02 GMT+0800 (CST)
---
# 云端录制 RESTful API
阅读本文前请确保你已经了解如何使用 [RESTful API 录制](../../cn/cloud-recording/cloud_recording_rest.md)。

## <a name="auth"></a>认证

云端录制 RESTful API 仅支持 HTTPS 协议。发送请求时，你需要提供 `api_key:api_secret` 通过 Basic HTTP 认证并填入 HTTP 请求头部的 Authorization 字段：

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

你可以在 Dashboard 的 [RESTful API](https://dashboard.agora.io/restful) 页面找到你的 Customer ID 和 Customer Certificate。具体生成 `Authorization` 字段的方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

## 数据格式

所有的请求都发送给域名：`api.agora.io`。

- 请求：请求的格式详见下面各个 API 中的示例
- 响应：响应内容的格式为 JSON

> 所有的请求 URL 和请求包体内容都是区分大小写的。

## 调用步骤

一般进行云端录制的步骤如下：

1. 通过 [`acquire`](#acquire) 请求获取一个 resource ID 用于开启云端录制。
2. 获取 resource ID 后在 5 分钟内调用 [`start`](#start) 开始云端录制。
3. 录制完成后调用 [`stop`](#stop) 停止录制。

在整个过程中可以通过 [`query`](#query) 请求查询云端录制的状态。完整示例代码请参考[云端录制快速开始示例代码](../../cn/cloud-recording/cloud_recording_rest.md)。

![](https://web-cdn.agora.io/docs-files/1559640254333)

## <a name="acquire"></a>获取云端录制资源的 API

在开始云端录制之前，你需要调用该方法获取一个 resource ID。

>  一个 resource ID 只能用于一次云端录制服务。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/acquire

> 请求数限制为每秒钟 10 次。

调用该方法成功后，你可以从 HTTP 响应包体中的 `resourceId` 字段得到一个 resource ID。这个 resource ID 的时效为 5 分钟，你需要在 5 分钟内用这个 resource ID 去调用[开始云端录制的 API](#start)。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数    | 类型   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| `appid` | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待录制的频道名。                                             |
| `uid`           | String | 字符串内容为云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。例如`"527841"`。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该方法无需填入任何内容，为一个空的 JSON。 |

### `acquire` 请求示例

- 请求 URL：`https://api.agora.io/v1/apps/<yourappid>/cloud_recording/acquire`
- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：

 ```json
{
    "cname": "httpClient463224",
    "uid": "527841",
    "clientRequest":{
    }
}
 ```

### `acquire` 响应示例

```json
"Code": 200,
"Body":
{
 "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制资源 resource ID，使用这个 resource ID 可以开始一段云端录制。这个 resource ID 的有效期为 5 分钟，超时需要重新请求。

## <a name="start"></a>开始云端录制的 API

获取 resource ID 后，调用该方法开始云端录制。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/mode/\<mode\>/start

> 请求数限制为每秒钟 10 次。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数         | 类型   | 描述                                                         |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |
| `resourceid` | String | 通过 [`acquire`](#acquire) 请求获取的 resource ID。          |
| `mode`       | String | 录制模式，目前只支持合流模式 `mix`。                         |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待录制的频道名。                                             |
| `uid`           | String | 字符串内容为云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。例如`"527841"`。云端录制不支持 String 用户名（User Account），请确保该字段引号内为整型 UID，且频道内所有用户均使用整型 UID。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该请求请参考下面的列表设置。         |

`clientRequest` 中需要填写的内容如下：

- `token`：（选填） String 类型，用于鉴权的[动态密钥](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-namekeya动态密钥)。如果待录制的频道使用了动态密钥，请务必在该参数中传入一个动态密钥，详见[校验用户权限](https://docs.agora.io/cn/cloud-recording/token?platform=All%20Platforms)。
- `recordingConfig`：（选填）JSON 类型，录制的详细设置。
  - `streamTypes`：（选填）Number 类型，录制的媒体流类型。
    - `0`：仅录制音频
    - `1`：仅录制视频
    - `2`：（默认）录制音频和视频
  - `decryptionMode`：（选填）Number 类型，解密方案。如果频道设置了加密，该参数必须设置。解密方式必须与频道设置的加密方式一致。
    - `0`：无（默认）
    - `1`：设置 AES128XTS 解密方案
    - `2`：设置 AES128ECB 解密方案
    - `3`：设置 AES256XTS 解密方案
  - `secret`：（选填）String 类型。启用解密模式后，设置的解密密码。
  - `channelType`：（选填）Number 类型，频道模式。频道模式必须与 Agora Native/Web SDK 的设置一致，否则可能导致问题。
	  - `0`：通信模式（默认）
	  - `1`：直播模式
  - `audioProfile`：（选填）设置录制文件的音频采样率，码率，编码模式和声道数。
    - `0`：（默认）48 KHz 采样率，音乐编码，单声道，编码码率约 48 Kbps
    - `1`：48 KHz 采样率，音乐编码, 单声道，编码码率约 128 Kbps
    - `2`：48 KHz 采样率，音乐编码, 双声道，编码码率约 192 Kbps
  - `videoStreamType`：（选填）Number 类型，设置录制的视频流类型。如果频道中有用户开启了双流模式，你可以选择录制视频大流或者小流。
    - `0`：视频大流（默认），即高分辨率高码率的视频流
    - `1`：视频小流，即低分辨率低码率的视频流
  - `maxIdleTime`：（选填）Number 类型，最长空闲频道时间。默认值为 30 秒，该值需大于等于 5。如果频道内无用户的状态持续超过该时间，录制程序会自动退出。
  - `transcodingConfig`：（选填）JSON 类型，视频转码的详细设置。如果不设置将使用默认值。如果设置该参数，请务必填入 `width`、`height`、`fps` 和 `bitrate` 字段。
    - `width`：（必填）Number 类型，录制视频的宽度，单位为像素，默认值 360。支持的最大分辨率为 1080p，超过该分辨率会报错。
    - `height`：（必填）Number 类型，录制视频的高度，单位为像素，默认值 640。支持的最大分辨率为 1080p，超过该分辨率会报错。
    - `fps`：（必填）Number 类型，录制视频的帧率，单位 fps，默认值 15。
    - `bitrate`：（必填）Number 类型，录制视频的码率，单位 Kbps，默认值 500。
    - `maxResolutionUid`：（选填）String 类型，如果视频合流布局设为垂直布局，用该参数指定显示大视窗画面的用户 ID。
    - `mixedVideoLayout`：（选填）Number 类型，设置视频合流布局，0、1、2 为[预设的合流布局](https://docs.agora.io/cn/cloud-recording/cloud_layout_guide?platform=Linux)，3 为自定义合流布局。该参数设为 3 时必须设置 `layoutConfig` 参数。
      - `0`：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。
      - `1`：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。
      - `2`：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。
      - `3`：自定义布局。设置 `layoutConfig` 参数自定义合流布局。
    - `backgroundColor`：（选填）String 类型。屏幕（画布）的背景颜色。支持 RGB 颜色表，字符串格式为 # 号后 6 个十六进制数。默认值 `"#000000"` 黑色。
    - `layoutConfig`：（选填）JSONArray 类型。由每个用户对应的布局画面设置组成的数组，支持最多 17 个用户画面。当 mixedVideoLayout 设为 3 时，可以通过该参数自定义合流布局。一个用户画面设置包括以下参数：
      - `uid`：（选填）String 类型。字符串内容为待显示在该区域的用户的 UID，32 位无符号整数。如果不指定 UID，会按照用户加入频道的顺序自动匹配 `layoutConfig` 中的画面设置。
      - `x_axis`：（必填）Float 类型。屏幕里该画面左上角的横坐标的相对值，范围是  [0.0,1.0]。从左到右布局，0.0 在最左端，1.0 在最右端。
      - `y_axis`：（必填）Float 类型。屏幕里该画面左上角的纵坐标的相对值，范围是  [0.0,1.0]。从上到下布局，0.0 在最上端，1.0 在最下端。
      - `width`：（必填）Float 类型。该画面宽度的相对值，取值范围是 [0.0,1.0]。
      - `height`：（必填）Float 类型。该画面高度的相对值，取值范围是 [0.0,1.0]。
      - `alpha`：（选填）Float 类型。图像的透明度。取值范围是 [0.0,1.0] 。默认值 1.0。0.0 表示图像为透明的，1.0 表示图像为完全不透明的。
      - `render_mode`：（选填）Number 类型。画面显示模式：
        - `0`：（默认）裁剪模式。优先保证画面被填满。视频尺寸等比缩放，直至整个画面被视频填满。如果视频长宽与显示窗口不同，则视频流会按照画面设置的比例进行周边裁剪或图像拉伸后填满画面。
        - `1`：缩放模式。优先保证视频内容全部显示。视频尺寸等比缩放，直至视频窗口的一边与画面边框对齐。如果视频尺寸与画面尺寸不一致，在保持长宽比的前提下，将视频进行缩放后填满画面，缩放后的视频四周会有一圈黑边。
- `storageConfig`：JSON 类型，第三方云存储的详细设置。
  
  - `vendor`：Number 类型，第三方云存储供应商。    
    - `0`：七牛云
    - `1`：Amazon S3
    - `2`：阿里云
  - `region`：Number 类型，第三方云存储指定的地区信息。
    当 `vendor` = 0，即第三方云存储为七牛云时：  
    - `0`：Huadong 
    - `1`：Huabei 
    - `2`：Huanan 
  - `3`：Beimei  
  
    当 `vendor` = 1，即第三方云存储为 Amazon S3 时：
    - `0`：US_EAST_1 
    - `1`：US_EAST_2 
    - `2`：US_WEST_1 
    - `3`：US_WEST_2 
    - `4`：EU_WEST_1 
    - `5`：EU_WEST_2 
    - `6`：EU_WEST_3 
    - `7`：EU_CENTRAL_1 
    - `8`：AP_SOUTHEAST_1 
    - `9`：AP_SOUTHEAST_2 
    - `10`：AP_NORTHEAST_1 
    - `11`：AP_NORTHEAST_2 
    - `12`：SA_EAST_1 
    - `13`：CA_CENTRAL_1 
    - `14`：AP_SOUTH_1 
    - `15`：CN_NORTH_1 
    - `16`：CN_NORTHWEST_1 
  - `17`：US_GOV_WEST_1 
  
    当 `vendor` = 2，即第三方云存储为阿里云时： 
    - `0`：CN_Hangzhou 
    - `1`：CN_Shanghai 
    - `2`：CN_Qingdao 
    - `3`：CN_Beijin 
    - `4`：CN_Zhangjiakou 
    - `5`：CN_Huhehaote 
    - `6`：CN_Shenzhen 
    - `7`：CN_Hongkong 
    - `8`：US_West_1 
    - `9`：US_East_1 
    - `10`：AP_Southeast_1 
    - `11`：AP_Southeast_2 
    - `12`：AP_Southeast_3 
    - `13`：AP_Southeast_5 
    - `14`：AP_Northeast_1 
    - `15`：AP_South_1 
    - `16`：EU_Central_1 
    - `17`：EU_West_1 
    - `18`：EU_East_1
  
  - `bucket`：String 类型，第三方云存储的 bucket。
  - `accessKey`：String 类型，第三方云存储的 access key。
  - `secretKey`：String 类型，第三方云存储的 secret key。

### `start` 请求示例

- 请求 URL：`https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/mix/start`
- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：

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

### `start` 响应示例

```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。

## <a name="update"></a>更新合流布局的 API

在录制过程中，可以随时调用该方法更新合流布局的设置。

每次调用该方法都会覆盖原来的布局设置，例如在开始录制时设置了 `backgroundColor` 为 `"#FF0000"`（红色），调用该方法更新合流布局时如果不设置 `backgroundColor` 参数，背景色会变为默认值黑色。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/updateLayout

> 请求数限制为每秒钟 10 次。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| appid      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id) |
| resourceid | String | 通过 [`acquire`](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#acquire) 请求获取的 resource ID。 |
| sid        | String | 通过 [`start`](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#start) 请求获取的录制 ID。 |
| mode       | String | 录制模式，目前只支持合流模式 `mix`。                         |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待录制的频道名。                                             |
| `uid`           | String | 字符串内容为云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。例如 `"527841"`。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该请求请参考下面的列表设置。         |

clientRequest 中需要填写的内容如下：

- `maxResolutionUid`：（选填）String 类型，如果 `layoutType` 设为 2（垂直布局），用该参数指定显示大视窗画面的用户 ID。
- `mixedVideoLayout`：（选填）Number 类型，设置视频合流布局，0、1、2 为[预设的合流布局](https://docs.agora.io/cn/cloud-recording/cloud_layout_guide?platform=Linux)，3 为自定义合流布局。该参数设为 3 时必须设置 `layoutConfig` 参数。
  - `0`：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。
  - `1`：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。
  - `2`：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。
  - `3`：自定义布局。设置 `layoutConfig` 参数自定义合流布局。
- `backgroundColor`：（选填）String 类型。屏幕（画布）的背景颜色。支持 RGB 颜色表，字符串格式为 # 号后 6 个十六进制数。默认值 `"#000000"` 黑色。
- `layoutConfig`：（选填）JSONArray 类型。由每个用户对应的布局画面设置组成的数组，支持最多 17 个用户画面。当 `layoutType` 设为 3 时，可以通过该参数自定义合流布局。一个用户画面设置包括以下参数：
  - `uid`：（选填）String 类型。字符串内容为待显示在该画面的用户的 UID，32 位无符号整数。如果不指定 UID，会按照用户加入频道的顺序自动匹配 `layoutConfig` 中的画面设置。
  - `x_axis`：（必填）Float 类型。屏幕里该画面左上角的横坐标的相对值，范围是  [0.0,1.0]。从左到右布局，0.0 在最左端，1.0 在最右端。
  - `y_axis`：（必填）Float 类型。屏幕里该画面左上角的纵坐标的相对值，范围是  [0.0,1.0]。从上到下布局，0.0 在最上端，1.0 在最下端。
  - `width`：（必填）Float 类型。该画面宽度的相对值，取值范围是 [0.0,1.0]。
  - `height`：（必填）Float 类型。该画面高度的相对值，取值范围是 [0.0,1.0]。
  - `alpha`：（选填）Float 类型。图像的透明度。取值范围是 [0.0,1.0] 。默认值 1.0。0.0 表示图像为透明的，1.0 表示图像为完全不透明的。
  - `render_mode`：（选填）Number 类型。画面显示模式：
    - 0：（默认）裁剪模式。优先保证画面被填满。视频尺寸等比缩放，直至整个画面被视频填满。如果视频长宽与显示窗口不同，则视频流会按照画面设置的比例进行周边裁剪或图像拉伸后填满画面。
    - 1：缩放模式。优先保证视频内容全部显示。视频尺寸等比缩放，直至视频窗口的一边与画面边框对齐。如果视频尺寸与画面尺寸不一致，在保持长宽比的前提下，将视频进行缩放后填满画面，缩放后的视频四周会有一圈黑边。

### updateLayout 请求示例

- 请求 URL：`https://api.agora.io/v1/apps/<appid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/updateLayout`
- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：

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

### updateLayout 响应示例

```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number 类型，[响应状态码](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。

## <a name="query"></a>查询云端录制状态的 API

- 方法：GET
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/query

> 请求数限制为每秒钟 10 次。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| appid      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |
| resourceid | String | 通过 [`acquire`](#acquire) 请求获取的 resource ID。          |
| sid        | String | 通过 [`start`](#start) 请求获取的录制 ID。                   |
| mode       | String | 录制模式，目前只支持合流模式 `mix`。                         |

### `query` 请求示例

- 请求 URL：`https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/mix/query`
- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

### `query` 响应示例

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

- `code`：Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。
- `serverResponse`：JSON 类型，录制的具体信息。
  - `fileList`：String 类型，录制生成的文件索引。每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。
  - `status`：Number 类型，当前录制的状态。
    - `0`：没有开始云端录制。
    - `1`：云端录制初始化完成。
    - `2`：录制组件开始启动。
    - `3`：上传组件启动完成。
    - `4`：录制组件启动完成。
    - `5`：已成功上传第一个文件。第一个文件上传完成后，录制在进行中均会返回此状态。
    - `6`：已经停止录制。
    - `7`：云端录制服务全部停止。
    - `8`：云端录制准备退出。
    - `20`：云端录制异常退出。
  - `sliceStartTime`: String 类型，录制开始的时间，Unix 时间戳，单位为毫秒。

## <a name="stop"></a>停止云端录制的 API

录制完成后，调用该方法离开频道，停止录制。录制停止后如需再次录制，必须再调用  [`acquire`](#acquire) 方法请求一个新的 resource ID。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/stop

> - 请求数限制为每秒钟 10 次。
> - 当频道空闲（无用户）超过预设时间（默认为 30 秒） 后，云端录制也会自动退出频道停止录制。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数         | 类型   | 描述                                                         |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |
| `resourceid` | String | 通过 [`acquire`](#acquire) 请求获取的 resource ID。          |
| `sid`        | String | 通过 [`start`](#start) 请求获取的录制 ID。                   |
| `mode`       | String | 录制模式，目前只支持合流模式 `mix`。                         |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待录制的频道名。                                             |
| `uid`           | String | 字符串内容为云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。例如`"527841"`。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该方法无需填入任何内容，为一个空的 JSON。 |

### `stop` 请求示例

- 请求 URL：`https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/mix/stop`
- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：
 ```json
{
    "cname": "httpClient463224",
    "uid": "527841",  
    "clientRequest":{
    }
}
 ```

### `stop` 响应示例

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

- `code`: Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。
- `serverResponse`: JSON 类型，录制的具体信息。
  - `fileList`：String 类型，录制生成的文件索引。每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。
  - `uploadingStatus`：String 类型，当前录制上传的状态。
    - `"uploaded"`：本次录制的文件已经全部上传至客户指定的第三方云存储。
    - `"backuped"`：本次录制的文件已经全部上传完成，但是至少有一个 TS 文件上传到了 Agora 云备份。Agora 服务器会自动将这部分文件继续上传至指定的第三方云存储。
    - `"unknown"`：未知状态。

## <a name="status"></a>响应状态码

| 状态码 | 描述                                               |
| :----- | :------------------------------------------------- |
| 200    | 请求成功。                                         |
| 201    | 成功请求并创建了新的资源。                         |
| 206    | 服务器成功处理了部分 GET 请求。                    |
| 400    | 请求的语法错误（如参数错误），服务器无法理解。     |
| 404    | 服务器无法根据请求找到资源（网页）。               |
| 500    | 服务器内部错误，无法完成请求。                     |
| 504    | 充当网关或代理的服务器未及时从远端服务器获取请求。 |

## 常见错误

下面仅列出使用云端录制 RESTful API 过程中常见的错误码或错误信息，如果遇到其他错误，请联系 Agora 技术支持。

- `2`：参数不合法，请确保参数类型正确、大小写正确、必填的参数均已填写。
- `8`：HTTP 请求头部字段错误，有以下几种情况：
	- `Content-type` 错误，请确保 `Content-type` 为 `application/json;charset=utf-8`。
	- 请求 URL 中缺少 `cloud_recording` 字段。
	- 使用了错误的 HTTP 方法。
- `433`：resource ID 过期。获得 resource ID 后必须在 5 分钟内开始云端录制。请重新调用 [`acquire`](#acquire) 获取新的 resource ID。
- `501`：录制服务正在退出。该错误可能在调用了 [`stop`](#stop) 方法后再调用 [`query`](#query) 时发生。
- `1001`：resource ID 解密失败。请重新调用 [`acquire`](#acquire) 获取新的 resource ID。
- `1003`：App ID 或者录制 ID（sid）与 resource ID 不匹配。请确保在一个录制周期内 resource ID、App ID 和录制 ID 一一对应。
- `1004`：录制已经在进行中，请勿重复 [`start`](#start) 请求。
- `1005`：resource ID 已被使用。一个 resource ID 只能用于一次云端录制。
- `1013`：频道名不合法。频道名必须为长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）：
  - 26 个小写英文字母 a-z
  - 26 个大写英文字母 A-Z
  - 10 个数字 0-9
  - 空格
  - "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","

- `1028`：[`updateLayout`](#update) 方法的请求包体中参数错误。
- `"invalid appid"`：无效的 App ID。请确保 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-nameappidaapp-id) 填写正确。如果检查了 App ID 没有问题仍遇到此错误，请联系 Agora 技术支持。
