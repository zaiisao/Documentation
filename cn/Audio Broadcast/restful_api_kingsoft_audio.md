
---
title: 金山语音智能审核 RESTful API
description: 
platform: All Platforms
updatedAt: Fri Oct 30 2020 06:17:25 GMT+0800 (CST)
---
# 金山语音智能审核 RESTful API
该文提供金山智能语音审核 RESTful API 的详细信息。

## 认证

金山智能语音审核 RESTful API 仅支持 HTTPS 协议。使用 RESTful API 前，你需要通过 [HTTP 基本认证](https://docs.agora.io/cn/faq/restful_authentication)。

## 数据格式

所有的请求都发送给域名：`api.agora.io`。

- 请求：请求的格式详见下面各个 API 中的示例
- 响应：响应内容的格式为 JSON

> 所有的请求 URL 和请求包体内容都是区分大小写的。

## 调用步骤

一般进行审核的步骤如下：

1. 通过 `acquire` 请求获取一个 resource ID 用于开启金山智能语音审核。
2. 获取 resource ID 后在 5 分钟内调用 `start` 开始审核。
3. 审核完成后调用 `stop` 停止审核。

在整个过程中可以通过 `query` 请求查询审核的状态。

## 获取审核资源的 API

在开始审核之前，你需要调用该方法获取一个 resource ID，用于开启金山智能语音审核。

> 一个 resource ID 只能用于一次审核。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/acquire

> 每个 App ID 每秒钟的请求数限制为 10 次。如需提高此限制，请[提交工单](https://agora-ticket.agora.io/)联系技术支持。

调用该方法成功后，你可以从 HTTP 响应包体中的 `resourceId` 字段得到一个 resource ID。这个 resource ID 的时效为 5 分钟，你需要在 5 分钟内用该 resource ID 调用 `start` 方法，超时需重新调用 `acquire` 申请。在审核中该资源一直有效，直到审核结束。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数    | 类型   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| `appid` | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待审核的频道相同的 App ID。 |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待审核的频道名。                                             |
| `uid`           | String | 字符串内容为金山智能语音审核使用的 UID，用于标识该审核服务，例如`"527841"`。需满足以下条件：<ul><li>取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0。</li><li>*不能与当前频道内的任何 UID 重复。*</li><li>金山智能语音审核不支持 String 用户 ID（User Account），请确保该字段引号内为整型 UID，且频道内所有用户均使用整型 UID。</li></ul> |
| `clientRequest` | JSON   | 客户请求参数，包含 `resourceExpiredHour` 字段。`resourceExpiredHour` 为 Number 类型，单位为小时，用于设置金山智能语音审核 RESTful API 的调用时效，从成功开启审核并获得 `sid` （审核 ID）后开始计算。超时后，你将无法调用 `query` 和 `stop` 方法。`resourceExpiredHour` 需大于等于 `1`， 且小于等于 `720`，默认值为 `72`。 |

### `acquire` 请求示例

- 请求 URL：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/acquire
```

- `Content-type` 为 `application/json;charset=utf-8`
- `Authorization` 为 Basic authorization，生成方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。
- 请求包体内容：

```json
{
  "cname": "httpClient463224",
  "uid": "527841",
  "clientRequest":{
    "resourceExpiredHour":  24
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

- `code`：Number 类型，[响应状态码](#response_status)。
- `resourceId`：String 类型，金山智能语音审核的 resource ID，使用这个 resource ID 可以开始一段审核。

## 开始审核的 API

获取 resource ID 后，调用该方法开始审核。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/mode/\<mode\>/start

> 每个 APP ID 每秒钟的请求数限制为 10 次。如需提高此限制，请[提交工单](https://agora-ticket.agora.io/)联系技术支持。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数         | 类型   | 描述                                                         |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待审核的频道相同的 App ID。 |
| `resourceid` | String | 通过 `acquire` 请求获取的 resource ID。                      |
| `mode`       | String | 审核模式，目前只支持合流模式 (`mix`)。合流模式下，金山智能语音审核会将频道内所有（或指定） UID 的音频混合成一路流后进行审核。 |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待审核的频道名。                                             |
| `uid`           | String | 字符串内容为金山智能语音审核使用的 UID，用于标识该审核服务，需要和你在 `acquire` 请求中输入的 `uid` 相同。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该请求包含以下参数：<ul><li>`token：`String 类型，用于鉴权的[动态密钥](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#token)。如果你的项目已启用 App 证书，则务必在该参数中传入你项目的动态秘钥。详见[校验用户权限](https://docs.agora.io/cn/cloud-recording/token?platform=All%20Platforms)。</li><li>`recordingConfig`：JSON 类型，订阅的详细设置。审核服务会对你订阅的内容进行审核。</li><li>`extensionServiceConfig`：JSON 类型，扩展服务的设置，包括金山智能语音审核的详细设置。</li></ul> |

#### 订阅设置

`recordingConfig` 是一个用于设置订阅选项的 JSON Object。金山智能语音审核会对你设置的订阅内容进行审核。`recordingConfig` 包含以下字段：

- `channelType`：Number 类型，频道场景。频道场景必须与 Agora RTC SDK 的设置一致，否则可能导致问题。

  - `0`：通信场景（默认）
  - `1`：直播场景

- `streamTypes`：Number 类型，订阅的媒体流类型。使用金山智能语音审核时，你需要将其设置为 `0`，即仅订阅音频。

  - `0`：仅订阅音频
  - `1`：仅订阅视频
  - `2`：（默认）订阅音频和视频

- `decryptionMode`：（选填）Number 类型，解密方案。如果频道设置了加密，该参数必须设置。解密方式必须与频道设置的加密方式一致。

  - `0`：无（默认）
  - `1`：设置 AES128XTS 解密方案
  - `2`：设置 AES128ECB 解密方案
  - `3`：设置 AES256XTS 解密方案

- `secret`：（选填）String 类型，启用解密模式后，设置的解密密码。

- `audioProfile`：（选填）Number 类型，设置输出音频的采样率，码率，编码模式和声道数。

  - `0`：（默认）48 kHz 采样率，音乐编码，单声道，编码码率约 48 Kbps
  - `1`：48 kHz 采样率，音乐编码，单声道，编码码率约 128 Kbps
  - `2`：48 kHz 采样率，音乐编码，双声道，编码码率约 192 Kbps

- `maxIdleTime`：（选填）Number 类型，最长空闲频道时间。默认值为 30 秒，该值需大于等于 5，且小于等于 (2<sup>32</sup>-1)。如果频道内无用户的状态持续超过该时间，审核服务会自动退出。

  <div class="alert note"><ul><li>通信场景下，如果频道内有用户，但用户没有发流，不算作无用户状态。</li><li>直播场景下，如果频道内有观众但无主播，一旦无主播的状态超过 <code>maxIdleTime</code>，审核服务会自动退出。</li></div>

- `subscribeAudioUids`：（选填）JSONArray 类型，由 UID 组成的数组，指定订阅哪几个 UID 的音频流。数组长度不得超过 32，不推荐使用空数组。`subscribeAudioUids` 和 `unSubscribeAudioUids` 不能同时设置。

- `unSubscribeAudioUids`: （选填）JSONArray 类型，由 UID 组成的数组，指定不订阅哪几个 UID 的音频流。审核服务会订阅频道内除指定 UID 外所有 UID 的音频流。数组长度不得超过 32，不推荐使用空数组。`subscribeAudioUids` 和 `unSubscribeAudioUids` 不能同时设置。

- `subscribeUidGroup`: （选填）Number 类型，预估的订阅人数峰值。举例来说，如果 `subscribeAudioUids` 为 `["100","101","102"]`，则订阅人数为 3 人，该参数需要设置为 `1`。

  - `0`: 1 到 2 人
  - `1`: 3 到 7 人
  - `2`: 8 到 12 人
  - `3`: 13 到 17 人

#### 扩展服务设置

`extensionServiceConfig` 是一个用于设置扩展服务的 JSON Object。扩展服务是 Agora 提供的处理音视频流的一系列云端应用，如语音审核服务。`extensionServiceConfig` 包含以下字段：

- `errorHandlePolicy：`（选填）String 类型，错误处理策略。目前仅可设置为 `"error_abort"`。
  - `"error_abort"`：（默认）当扩展服务发生错误后，订阅等其他服务均停止。
  - `"error_ignore"`：当扩展服务发生错误后，扩展服务停止，订阅等其他服务继续。
- `apiVersion`：（选填）String 类型，金山智能语音审核 RESTful API 的版本号，默认为 `"v1"`。
- `extensionServices`：JSONArray 类型，由每个扩展服务的设置组成的数组。本文介绍使用金山智能语音审核时需要设置的参数。
  - `serviceName`：String 类型，扩展服务的名称。要使用金山智能语音审核，你需要将其设置为 `"kingsoft_voice_scan"`。
  - `errorHandlePolicy`：（选填）String 类型。错误处理策略。目前仅可设置为 `"error_abort"`。
    - `"error_abort"`：（默认）当该扩展服务发生错误后，其他扩展服务均停止。
    - `"error_ignore"`：当该扩展服务发生错误后，该扩展服务停止，其他扩展服务继续。
  - `streamTypes`：Number 类型，要审核的媒体流类型。使用金山智能语音审核时，你需要将其设置为 `0`，即仅审核音频。
    - `0`：仅订阅音频
    - `1`：仅订阅视频
    - `2`：（默认）订阅音频和视频
  - `serviceParam`：JSON 类型，扩展服务的具体参数设置。当使用金山智能语音审核时，你需要设置以下参数：
    - `callbackAddr`：String 类型，接收[审核结果](#moderation_result)的 HTTP 服务器地址。

### `start` 请求示例

- 请求 URL：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/<mode>/start
```

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
      "streamTypes":0
    },
    "extensionServiceConfig": {
      "extensionServices": [{
        "serviceParam": {
          "callbackAddr": "http://xxxxx"
        },
        "streamTypes": 0,
        "serviceName": "kingsoft_voice_scan"
      }]
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

- `code`：Number 类型，[响应状态码](#response_status)。
- `resourceId`：String 类型，金山智能语音审核使用的 resource ID。
- `sid`：String 类型，审核 ID。成功开始金山智能语音审核后，你会得到一个 sid （审核 ID)。该 ID 是一次审核的唯一标识。

## 查询审核状态的 API

开始审核后，你可以调用 `query` 查询审核状态。

<div class="alert note"><code>query</code> 请求仅在会话内有效。如果审核启动错误，或审核已结束，调用 <code>query</code> 将返回 404。</div>

- 方法：GET
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/query

> 每个 App ID 每秒钟的请求数限制为 10 次。如需提高此限制，请[提交工单](https://agora-ticket.agora.io/)联系技术支持。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| appid      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待审核的频道相同的 App ID。 |
| resourceid | String | 通过 `acquire` 请求获取的 resource ID。                      |
| sid        | String | 通过 `start` 请求获取的审核 ID。                             |
| mode       | String | 审核模式，目前只支持合流模式（`mix`）。                      |

### `query` 请求示例

- 请求 URL：

```json
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/query
```

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
    "subServiceStatus": {
      "recordingService": "serviceInProgress",
      "mediaDistributeService": "serviceInProgress"
    },
    "extensionServiceState": [{
      "serviceName": "kingsoft_voice_scan",
      "streamState":[{
        "status": "inProgress",
        "streamType": "audio",
        "uid": "0"
      }]
    }] 
  }       
}
```

- `code`：Number 类型，[响应状态码](#response_status)。
- `resourceId`：String 类型，金山智能语音审核的 resource ID。
- `sid`：String 类型，通过 `start` 请求获取的审核 ID。
- `serverResponse`：JSON 类型，服务器返回的具体信息。
  - `subServiceStatus`：JSON 类型，子模块的服务状态。
    - `recordingService`：String 类型，订阅模块的状态，具体取值详见[服务状态](#status)。
    - `mediaDistributeService`：String 类型，媒体分发模块的状态，具体取值详见[服务状态](#status)。该模块用于管理扩展服务，控制媒体流向各扩展服务的分发。
  - `extensionServiceState`：JSONArray 类型，由各个扩展服务的当前状态组成的数组。
    - `serviceName`：String 类型，扩展服务的名称。`"kingsoft_voice_scan"` 表示当前使用的扩展服务为金山智能语音审核。
    - `streamState`：JSONArray 类型，该扩展服务的状态。当选择金山智能语音审核时，返回以下参数：
      - `uid`：Number 类型，用户 UID，表示审核的是哪个用户的媒体流。合流模式下，`uid` 总为 `0` 。
      - `streamType`：String 类型，审核的内容类型。`"audio"` 指纯音频。
      - `status`：审核的状态。
        - `"inProgress"`：审核正在进行中。
        - `"failed"`：审核发生错误。

## 停止审核的 API

审核完成后，调用该方法离开频道，停止审核。审核停止后如需再次审核，必须再调用 `acquire` 方法请求一个新的 resource ID。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/stop

> - 每个 App ID 每秒钟的请求数限制为 10 次。如需提高此限制，请[提交工单](https://agora-ticket.agora.io/)联系技术支持。
> - 当频道空闲（无用户）超过预设时间（默认为 30 秒） 后，金山智能语音审核也会自动退出频道停止审核。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数         | 类型   | 描述                                                         |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待审核的频道相同的 App ID。 |
| `resourceid` | String | 通过 `acquire` 请求获取的 resource ID。                      |
| `sid`        | String | 通过 `start` 请求获取的审核 ID。                             |
| `mode`       | String | 审核模式，目前只支持合流模式（`mix`）。                      |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待审核的频道名。                                             |
| `uid`           | String | 字符串内容为金山智能语音审核使用的 UID，需要和你在 `acquire` 请求中输入的 `uid` 相同。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该方法无需填入任何内容，为一个空的 JSON。 |

### `stop` 请求示例

- 请求 URL：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/stop
```

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
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`：Number 类型，[响应状态码](#response_status)。
- `resourceId`：String 类型，金山智能语音审核使用的 resource ID。
- `sid`：String 类型，审核 ID。成功开始金山智能语音审核后，你会得到一个 sid （审核 ID)。该 ID 是一次审核的唯一标识。

## <a name="moderation_result"></a>审核结果的回调

金山智能语音审核的审核结果，会以 HTTP 请求的形式直接发送到你在 `extensionServiceConfig` 中的 `callbackAddr` 设置的地址。回调内容详见金山云文档中[音频流检测断流接口](https://docs.ksyun.com/documents/6215#音频流检测断流接口)的回调字段说明。除此之外，该回调还会包含 `clientInfo` 字段，包含以下信息：

- `cname`：String 类型，此次审核的频道名。
- `uid`：String 类型，字符串内容为金山智能语音审核在频道内使用的用户 ID，和你在 `acquire` 请求中输入的 `uid` 相同。

## <a name="status"></a>服务状态

| 状态                      | 描述                                                         |
| :------------------------ | :----------------------------------------------------------- |
| "serviceIdle"             | 子模块服务未开始。                                           |
| "serviceStarted"          | 子模块服务已开始。                                           |
| "serviceReady"            | 子模块服务已就绪。                                           |
| "serviceInProgress"       | 子模块服务正在进行中。                                       |
| "serviceCompleted"        | 审核内容已全部上传至金山智能语音审核。                       |
| "servicePartialCompleted" | 审核内容部分上传至金山智能语音审核。                         |
| "serviceValidationFailed" | 金山智能语音审核验证失败。例如 `extensionServiceConfig` 中 `apiData` 填写错误。 |
| "serviceAbnormal"         | 子模块状态异常。                                             |

## <a name="response_status"></a>响应状态码

| 状态码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| 200    | 请求成功。                                                   |
| 201    | 成功请求并创建了新的资源。                                   |
| 206    | 整个审核过程中没有用户发流。                                 |
| 400    | 请求的语法错误（如参数错误）。如果你填入的 App ID 没有开通金山智能语音审核服务，也会返回 400。 |
| 401    | 未经授权的（客户 ID/客户密钥匹配错误）。                     |
| 404    | 服务器无法根据请求找到资源（网页）。                         |
| 500    | 服务器内部错误，无法完成请求。                               |
| 504    | 服务器内部错误。充当网关或代理的服务器未从远端服务器获取请求。 |

## <a name="errors"></a>常见错误

下面仅列出使用金山智能语音审核 RESTful API 过程中常见的错误码或错误信息，如果遇到其他错误，请联系 Agora 技术支持。

- `2`：参数不合法，请确保参数类型正确、大小写正确、必填的参数均已填写。
- `7`：审核已经在进行中 ，请勿用同一个 resource ID 重复 `start` 请求。
- `8`：HTTP 请求头部字段错误，有以下几种情况：
  - `Content-type` 错误，请确保 `Content-type` 为 `application/json;charset=utf-8`。
  - 请求 URL 中缺少 `cloud_recording` 字段。
  - 使用了错误的 HTTP 方法。
  - 请求包体不是合法的 JSON 格式。
- `49`：使用同一个 resource ID 和审核 ID（`sid`）重复 `stop` 请求。
- `53`：审核已经在进行中。当采用相同参数再次调用 `acquire` 获得新的 resource ID，并用于 `start` 请求时，会发生该错误。如需发起多路审核，需要在 `acquire` 方法中填入不同的 UID。
- `62`：调用 `acquire` 请求时，如果出现该错误，表示你填入的 App ID 没有开通金山智能语音审核。
- `65`：多为网络抖动引起。使用相同 resource ID 重试即可。
- `432`：请求参数错误。请求参数不合法。
- `433`：resource ID 过期。获得 resource ID 后必须在 5 分钟内开始审核。请重新调用 `acquire` 获取新的 resource ID。
- `435`：频道内没有用户加入，无审核对象。
- `501`：审核正在退出。该错误可能在调用了 `stop` 方法后再调用 `query` 时发生。
- `1001`：resource ID 解密失败。请重新调用 `acquire` 获取新的 resource ID。
- `1003`：App ID 或者审核 ID（sid）与 resource ID 不匹配。请确保在一个审核周期内 resource ID、App ID 和审核 ID 一一对应。
- `1013`：频道名不合法。频道名必须为长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）：
  - 26 个小写英文字母 a-z
  - 26 个大写英文字母 A-Z
  - 10 个数字 0-9
  - 空格
  - "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","
- `"invalid appid"`：无效的 App ID。请确保 App ID 填写正确。如果你已经确认 App ID 填写正确，但仍出现该错误，请[提交工单](https://agora-ticket.agora.io/)。
- `"no Route matched with those values`": 该错误可能由 HTTP 方法填写错误导致，例如将 GET 方法填写为 POST；也可能由请求 URL 填写错误导致。
- `"Invalid authentication credentials"`: 该错误可能由以下原因导致。 如果你已经排除以下原因，但仍出现该错误，请联系 [support@agora.io](mailto:support@agora.io)。
  - Customer ID 或 Customer Certificate 填写错误。
  - App ID 没有开通金山智能语音审核服务。
  - HTTP 请求头的认证信息有误，如 `Authorization` 字段的值 `Basic <Authorization>` 缺少 `Basic`。
  - HTTP 请求头的格式不正确，如 `Content-type` 字段的值 `application/json;charset=utf-8` 大小写不正确或包含空格。
