
---
title: RESTful API
description: 
platform: All Platforms
updatedAt: Fri May 15 2020 08:23:53 GMT+0800 (CST)
---
# RESTful API
## 认证

<div class="alert info">在使用本文 RESTful API 提供的功能前，请确认你的账号已在控制台开通指定项目的相关权限。Agora 支持自定义用户角色和相应的项目权限，详见<a href="../../cn/Agora%20Platform/manage_member.md">控制台角色权限说明</a >。</div>

控制台 RESTful API 仅支持 HTTPS 协议。发送请求时，你需要提供 `api_key:api_secret` 通过 Basic HTTP 认证并填入 HTTP 请求头部的 Authorization 字段：

- `api_key`: Customer ID （客户 ID）
- `api_secret`: Customer Certificate （客户证书）

你可以在控制台的 [RESTful API](https://console.agora.io/restfulApi) 页面找到你的 Customer ID 和 Customer Certificate。具体生成 `Authorization` 字段的方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

<div class="alert note">本文 RESTful API 调用频率上限为每秒 10 次。若需提高调用频率上限，可<a href="https://agora-ticket.agora.io/">提交工单</a>联系技术支持。</div>

## 接入点

所有请求都发送给 BaseUrl：**https://api.agora.io/dev** 

-   请求：参数格式必须为 JSON ，内容类型: application/json。
-   响应：响应内容的格式为 JSON。以下为定义的响应状态：


| 状态码 | 描述 | 
| ---------------- | ---------------- | 
| 200      | 请求处理成功      |
| 400      | 输入格式错误      |
| 401      | 未经授权的（App ID/Customer Certificate匹配错误） |
| 404      | API 调用错误       |
| 429      | 请求过于频繁      |
| 500      | 服务器内部错误  |


## 项目相关 API

BaseUrl：**https://api.agora.io/dev**

下图展示了项目相关 API 的使用逻辑。

![](https://web-cdn.agora.io/docs-files/1583829129191)

### 创建项目 (POST)

创建一个 Agora 项目。

**基本信息**

| 请求基本信息 | 描述 | 
| ---------------- | ---------------- | 
| 方法      | POST      | 
| 请求 URL | BaseUrl/v1/project/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `name`      | 项目名称      |
| `enable_sign_key` | 是否启用 App 证书：<ul><li>true：启用</li><li>false：不启用</li></ul> |

**请求示例**

```json
{
  "name": "projectx",
  "enable_sign_key": true
}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `name` | 项目名称 |
| `vendor_key` | 项目的 App ID |
| `sign_key` | 项目的 App 证书 |
| `recording_server` | 录制项目服务器 IP。<ul><li>如果你使用的是本地服务端录制 SDK v1.9.0 及之前版本，请关注此字段；</li><li>如果你使用的是本地服务端录制 SDK v1.11.0 及之后版本，请忽略此字段。</li></ul> |
| `status` | 项目状态：<ul><li>1：启用</li><li>0：禁用</li></ul> |
| `created` | 项目创建时间，Unix 时间戳，单位为毫秒 |

**响应示例**

```json
{
  "project":[
    {
      "id": "xxxx",
      "name": "project1",
      "vendor_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "sign_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "recording_server": "10.2.2.8:8080",
      "status": 1,
      "created": 1464165672
    }
  ]
}
```


### 获取所有项目 (GET)

获取所有的 Agora 项目信息。

**基本信息**

| 请求基本信息 | 描述 | 
| ---------------- | ---------------- | 
| 方法      | GET      |
| 请求 URL  | BaseUrl/v1/projects/ |

**请求参数**

无

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `name` | 项目名称 |
| `vendor_key` | 项目的 App ID |
| `sign_key` | 项目的 App 证书 |
| `recording_server` | 录制项目服务器 IP。<ul><li>如果你使用的是本地服务端录制 SDK v1.9.0 及之前版本，请关注此字段；</li><li>如果你使用的是本地服务端录制 SDK v1.11.0 及之后版本，请忽略此字段。</li></ul> |
| `status` | 项目状态：<ul><li>1：启用</li><li>0：禁用</li></ul> |
| `created` | 项目创建时间，Unix 时间戳，单位为毫秒 |

**响应示例**

```json
{
  "projects":[
    {
      "id": "xxxx",
      "name": "project1",
      "vendor_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "sign_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "recording_server": "10.2.2.8:8080",
      "status": 1,
      "created": 1464165672
    }
  ]
}
```

### 获取指定项目 (GET)

获取指定的 Agora 项目信息。

**基本信息**

| 请求基本信息 | 描述 | 
| ---------------- | ---------------- |
| 方法      | GET      |
| 请求 URL | BaseUrl/v1/project/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `name` | 项目名称 |

**请求示例**

```json
{
  "id": "xxxx",
  "name": "xxxx"
}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `name` | 项目名称 |
| `vendor_key` | 项目的 App ID |
| `sign_key` | 项目的 App 证书 |
| `recording_server` | 录制项目服务器 IP。<ul><li>如果你使用的是本地服务端录制 SDK v1.9.0 及之前版本，请关注此字段；</li><li>如果你使用的是本地服务端录制 SDK v1.11.0 及之后版本，请忽略此字段。</li></ul> |
| `status` | 项目状态：<ul><li>1：启用</li><li>0：禁用</li></ul> |
| `created` | 项目创建时间，Unix 时间戳，单位为毫秒 |

**响应示例**

```json
{
  "project":[
    {
      "id": "xxxx",
      "name": "project1",
      "vendor_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "sign_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "recording_server": "10.2.2.8:8080",
      "status": 1,
      "created": 1464165672
    }
  ]
}
```

### 禁用或启用项目 (POST)

禁用或启用指定的项目。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | POST      |
| 请求 URL | BaseUrl/v1/project_status/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `status` | 想要设置的项目状态：<ul><li>0：禁用</li><li>1：启用</li></ul> |

**请求示例**

```json
{
  "id": "xxxx"
  "status": 0
}
```

**响应参数**

- 如果成功启用或禁用指定项目，则返回该项目的所有信息，并报告当前项目的状态。

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `name` | 项目名称 |
| `vendor_key` | 项目的 App ID |
| `sign_key` | 项目的 App 证书 |
| `recording_server` | 录制项目服务器 IP。<ul><li>如果你使用的是本地服务端录制 SDK v1.9.0 及之前版本，请关注此字段；</li><li>如果你使用的是本地服务端录制 SDK v1.11.0 及之后版本，请忽略此字段。</li></ul> |
| `status` | 项目状态：<ul><li>1：启用</li><li>0：禁用</li></ul> |
| `created` | 项目创建时间，Unix 时间戳，单位为毫秒 |

```json
{
  "project":[
    {
      "id": "xxxx",
      "name": "project1",
      "vendor_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "sign_key": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "recording_server": "10.2.2.8:8080",
      "status": 0,
      "created": 1464165672
    }
  ]
}
```

- 如果指定的项目被删除或不存在，则返回状态码 404。

```json
status 404
{
  "error_msg": "project not exit"
}
```

### 删除项目 (DELETE)

删除指定的 Agora 项目。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | DELETE      |
| 请求 URL | BaseUrl/v1/project/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |

**请求示例**

```json
{
  "id": "xxxx"
}
```

**响应参数**

- 如果成功删除项目：

```json
{
  "success": true
}
```

- 如果指定的项目不存在，则返回状态码 404。

```json
status 404
{
  "error_msg": "project not exit"
}
```

### 设置录制服务器 IP (POST)

设置指定项目的录制服务器的 IP 地址。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | POST      |
| 请求 URL | BaseUrl/v1/recording_config/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `recording_server` | 项目的录制服务器 IP。<ul><li>如果你使用的是本地服务端录制 SDK v1.9.0 及之前版本，请关注此字段；</li><li>如果你使用的是本地服务端录制 SDK v1.11.0 及之后版本，请忽略此字段。</li></ul> |

**请求示例**

```json
{
  "id": "xxxx",
  "recording_server": "10.12.1.5:8080"
}
```

**响应参数**

- 成功设置录制服务器的 IP：

```json
{
  "success": true
}
```

- 如果指定的项目不存在或被禁用，则返回状态码 404。

```json
status 404
{
  "error_msg": "project not exit"
}
```

### 启用 App 证书 (POST)

启用指定项目的 App 证书。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | POST      |
| 请求 URL | BaseUrl/v1/signkey/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |
| `enable` | 是否启用项目的 App 证书：<ul><li>true：启用</li><li>false：不启用</li></ul> | 

**请求示例**

```json
{
  "id": "xxxx",
  "enable": true
}
```

**响应参数**

- 成功启用项目的 App 证书：

```json
{
  "success": true
}
```

-  如果指定的项目不存在或被禁用，则返回状态码 404。

```json
status 404
{
  "error_msg": "project not exit"
}
```

### 重置 App 证书 (POST)

重置指定项目的 App 证书。如果原来的 App 证书泄漏了，你可以使用该方法对 App 证书进行重置。

如果该项目的 App 证书尚未启用，调用该方法会启用 App 证书。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | POST      |
| 请求 URL | BaseUrl/v1/reset_signkey |

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `id`      | 项目 ID      |

**请求示例**

```json
{
  "id": "xxxx"
}
```

**响应参数**

- 成功重置指定项目的 App 证书：

```json
{
  "success": true
}
```

-  如果指定的项目不存在或被禁用，则返回状态码 404。

```json
status 404
{
  "error_msg": "project not exit"
}
```

## 用量相关 API

BaseUrl: **https://api.agora.io/dev**

### 获取用量数据（GET）

Agora 提供 V1 和 V3 版本的获取用量数据 API。为获取更多用量信息，Agora 建议使用 [V3 版本](#UsageV3) API。

#### V1

<details>
	<summary><font color="#3ab7f8">获取用量数据 V1</font></summary>

下图展示了获取用量数据 V1 API 的使用逻辑。
![](https://web-cdn.agora.io/docs-files/1583830859466)

-   方法：GET
-   路径：BaseUrl/v1/usage/
-   参数 (格式为一个日期到另一个日期的YYYY-MM-DD)：

    ```
    "from_date"=YYYY-MM-DD&to_date=YYYY-MM-DD&projects=id1,id2,id3
    "from_date"=2015-01-01&to_date=2015-03-21&projects=id1,id2
    ```

<div class="alert note"><li>你可以指定项目，但如果不指定，系统将查询该账户下的全部项目。<li>你可以查询一年内的用量数据。建议查询时段不要包含当天，因为当天数据会持续变化。</li></div>


-   响应：

    -   成功：

        ```
        {
          "usages":[
        
                    { "project": "xxx",
                                "daily": [
                                      { "date": 20150101, "audio": 20, "sd": 100, "hd": 132, "hdp": 225},
                                      { "date": 20150102, "audio": 20, "sd": 100, "hd": 132, "hdp": 225},
                                  ]
                                },
        
                                { "project": "yyy",
                                  "daily": [....]
                                }
        
                  ]
        }
        ```

    -   报错: 如果指定的项目 \(projects\) 不存在，会直接被忽略。不会报错。

<div class="alert note">该响应中语音时长（audio）、SD 视频时长（sd）、HD 视频时长（hd） 及 HDP 视频时长（hdp）的单位为分钟。</div>

</details>

<a name="UsageV3"></a>
#### V3

下图展示了获取用量数据 V3 API 的使用逻辑。
![](https://web-cdn.agora.io/docs-files/1583829230163)

-   方法：GET
-   路径：BaseUrl/v3/usage/
-   参数：

 ```
// from_date: 指定查询开始日期（UTC 时间）。日期格式为 YYYY-MM-DD，例如 2020-01-01。
// to_date: 指定查询结束日期（UTC 时间）。日期格式为 YYYY-MM-DD，例如 2020-01-31。
// project_id: 指定项目 ID。
// business: 指定业务类型。可设为以下值：
// - default：默认业务，为 RTC
// - transcodeDuration: 转码
// - recording: 本地服务端录制
// - cloudRecording: 云端录制
// - miniapp: 小程序
from_date=2020-01-01&to_date=2020-01-31&project_id=id1&business=default
 ```

<div class="alert note"><li>请确保填写有效的 <tt>project_id</tt>，否则无法获取用量数据。<li>你可以查询一年内的用量数据。建议查询时段不要包含当天，因为当天数据会持续变化。</li></div>

-   响应：

 ```json
 {
    "meta": {
        "durationAudioAll": {
            "cn": "总音频时长",
            "en": "Total Audio Duration",
            "unit": "second"
        },
        "durationVideoHd": {
            "cn": "HD视频时长（含本地服务端录制）",
            "en": "HD Video Duration（including On-premise Recording）",
            "unit": "second"
        },
        "durationVideoHdp": {
            "cn": "HDP视频时长（含本地服务端录制）",
            "en": "HDP Video Duration（including On-premise Recording)",
            "unit": "second"
        }
    },
    "usages": [
        {
            // 查询的时间，使用 UTC 时间和 Unix 时间戳。
            "date": "2020-03-01T00:00:00.000Z",
            "usage": {
                // 总音频时长。详细描述见 meta 对象中的 durationAudioAll。
                "durationAudioAll": 0,
                // HD视频时长（含本地服务端录制）。详细描述见 meta 对象中的 durationVideoHd。
                "durationVideoHd": 0,
                 // HDP 视频时长（含本地服务端录制）。详细描述见 meta 对象中的 durationVideoHdp。
                "durationVideoHdp": 0
            }
        }
    ]
}
 ```

## 服务端踢人 API

BaseUrl: **https://api.agora.io/dev**

下图展示了服务器踢人相关 API 的使用逻辑。
![](https://web-cdn.agora.io/docs-files/1583829281301)

<div class="alert note">本组 API 调用频率上限为每秒 10 次。</div>

用户被踢出频道后，会收到网络连接已被服务器禁止回调：
- Android: [`onConnectionStateChanged`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) 回调报告 `CONNECTION_CHANGED_BANNED_BY_SERVER(3)`
- iOS/macOS: [`connectionChangedToState`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调报告 `AgoraConnectionChangedBannedByServer(3)`
- Web: [`onclient-banned`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#on)
- Windows: [`onConnectionStateChanged`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) 回调报告 `CONNECTION_CHANGED_BANNED_BY_SERVER(3)`
- Electron: [`AgoraRtcEngine.on("connectionStateChanged")`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/electron/classes/agorartcengine.html#on) 回调报告 `3`
- Unity: [`OnConnectionStateChangedHandler`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/unity/namespaceagora__gaming__rtc.html#adae7694cb602375ccbc14be3062a230c) 回调报告 `CONNECTION_CHANGED_BANNED_BY_SERVER(3)`

### 创建规则 (POST)

创建服务端踢人规则。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | POST      |
| 请求 URL | BaseUrl/v1/kicking-rule/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `appid`      | 控制台中项目的 App ID      |
| `cname`    | （可选）频道名称。不能设为空字符串"" |
| `uid`           | （可选）用户 ID，可以直接使用 SDK 的方法获取。不能设为 0 |
| `ip`             | （可选）想要封的用户 IP 地址。不能设为 0 |
| `time`        | 封人时间，单位为分钟，取值范围为 [1, 1440]，默认值为 60。设置 0 到 1 之间的，服务端自动改设为 1，大于 1440 的自动改设为 1440。设为 0 时，表示不封禁。服务端会对频道内符合设定规则的用户进行下线一次的操作。用户可以重新登录进入频道。 |
| `privileges` | 默认参数，表示用户的权限，设为 `["join_channel"]` |

踢人规则通过 `cname`、`uid` 和 `ip` 三个字段实现过滤，规则如下：
* 如果填写 `ip`，不填写 `cname` 或 `uid`，则该 IP 无法登录 app 中的任何频道。
* 如果填写 `cname`，不填写 `uid` 或 `ip`，则任何人都无法登录 app 中该 `cname` 对应的频道。
* 如果填写 `cname` 和 `uid`，不填写 `ip`，则该 UID 无法登录 app 中该 `cname` 对应的频道。
* 如果填写 `uid`，不填写 `cname` 或 `ip`，则该 UID 无法登录 app 中的任何频道。

**请求示例**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
  "cname": "",
  "uid":"",
  "ip":"",
  "time":60,
  "privilege":["join_channel"]
}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `status`      | 本次请求的状态      |
| `id`              | 规则 ID。当更新踢人规则时，需要使用该规则 ID | 

**响应示例**

```json
{
  "status": "success",
  "id": 1953
}
```

### 获取规则列表 (GET)

获取服务端的踢人规则列表。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | GET      |
| 请求 URL | BaseUrl/v1/kicking-rule/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `appid`      | 控制台中项目的 App ID      |

**请求示例**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2"
}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `status`      | 本次请求的状态      |
| `rules`        | 踢人规则列表，包含如下字段：<ul><li>`id`：规则 ID</li><li>`appid`：控制台中项目的 App ID</li><li>`uid`：用户 ID</li><li>`opid`：操作 ID。该操作 ID 用于问题排查时核对操作记录</li><li>`cname`：频道名</li><li>`ip`：IP 地址</li><li>`ts`：该规则生效的截止时间戳</li><li>`createAt`：创建该规则的时间戳</li><li>`updateAt`：更新该规则的时间戳</li></ul> |

**响应示例**

```json
{
  "status": "success",
  "rules": [
    {
      "id": 1953,
      "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
      "uid": 1,
      "opid": 1406,
      "cname": "11",
      "ip": null,
      "ts": "2018-01-09T07:23:06.000Z",
      "createAt": "2018-01-09T06:23:06.000Z",
      "updateAt": "2018-01-09T14:23:06.000Z"
    }
  ]
}
```


### 更新规则时间 (PUT)

更新服务端踢人规则的生效时间。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- | 
| 方法      | PUT      |
| 请求 URL  | BaseUrl/v1/kicking-rule/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 | 
| ---------------- | ---------------- |
| `appid`      | 控制台中项目的 App ID      |
| `id`             | 想要更新的规则的 ID |
| `time`         | 想要更新的封人时间，单位为分钟，取值范围为 [1, 1440]，默认值为 60。小于 1 的设置自动改设为 1，大于 1440 的自动改设为 1440。设为 0 时，表示不封禁。服务端会对频道内符合设定规则的用户进行下线一次的操作。用户可以重新登录进入频道。 |

**请求示例**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
  "id": 1953,
  "time": 60,
}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `status`     | 本次请求的状态 |
| `result`      | 本次规则更新的结果，包含如下字段：<ul><li>`id`：规则 ID</li><li>`ts`：规则生效的截止时间</li></ul>      |

**响应示例**

```json
{
  "status": "success",
  "result": {
    "id": 1953,
    "ts": "2018-01-09T08:45:54.545Z"
  }
}
```


### 删除规则 (DELETE)

删除服务端踢人规则。

**基本信息**

| 请求基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | DELETE      |
| 请求 URL |  BaseUrl/v1/kicking-rule/ |

**请求参数**

#### 包体参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `appid`      | 控制台中项目的 App ID     |
| `id`             | 想要删除的规则 ID |

**请求示例**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
  "id": 1953
}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `status`      | 本次请求的状态     | 
| `id`             | 删除的规则 ID |

**响应示例**

```json
{
  "status": "success",
  "id": 1953
}
```

<a name="query"></a>
## 查询在线频道信息 API

BaseUrl：**https://api.agora.io/dev**

下图展示了查询频道信息相关 API 的使用逻辑。

![](https://web-cdn.agora.io/docs-files/1583829296222)

### 查询用户状态 (GET)

该方法查询某个用户是否在指定频道中，以及该用户在该频道中的角色等状态。

- 主播：可以发布和接收音视频流。
- 观众：只能接收音视频流，无法发布。

<div class="alert note">若需查询指定频道中所有用户的角色，详见<a href="#userList">获取用户列表 (GET)</a >。</div>

**基本信息**

| 基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | GET      |
| 请求  URL  | BaseUrl/v1/channel/user/property/ |

**请求参数**

#### URL 参数

| 参数名 | 描述 |
| ---------------- | ---------------- | 
| `appid`      | 控制台中项目的 App ID      | 
| `uid`          | 用户 ID，可以通过 SDK 获取到 |
| `cname`   | 频道名 |

**请求示例**

```
BaseUrl/v1/channel/user/property/{appid}/{uid}/{channelName}
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `success`  | 本次请求的状态：<ul><li>true：请求成功</li><li>false：请求不成功</li></ul> |
| `data`  | 指定用户在频道中的状态数据，包含如下字段：<ul><li>`join`：该用户加入频道的时间戳</li><li>`in_channel`：该用户是否在频道内：<ul><li>true：该用户在频道内</li><li>false：该用户不在频道内</li></ul><li>`role`：该用户在频道内的角色：<ul><li>0：未知</li><li>1：通信场景的用户</li><li>2：直播场景的主播</li><li>3：直播场景的观众</li></ul></li></ul> |

**响应示例**

```json
{
  "success": true,
  "data": {
    "join": 1549073054,
    "in_channel": true,
    "role": 2
  }
}
```

<a name="userList"></a>
### 获取用户列表 (GET)

该方法获取指定频道内的用户列表：
- 通信场景下，返回频道内的用户列表。
- 直播场景下，返回频道内的主播列表和观众列表。

**基本信息**

| 基本信息 | 描述 |
| ---------------- | ---------------- |
| 方法      | GET      |
| 请求 URL  | BaseUrl/v1/channel/user/ |

**请求参数**

#### URL 参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `appid`      | 控制台中项目的 App ID      |
| `cname`    | 频道名 |

**请求示例**

```
BaseUrl/v1/channel/user/{appid}/{channelName}
```

**响应参数**

不同的频道场景下，该方法返回的响应内容不同。

- 通信场景：

| 参数 | 描述 |
| ---------------- | ---------------- |
| `sucess`      | 本次请求是否成功：<ul><li>true：请求成功</li><li>false：请求不成功</li></ul>      |
| `data` | 用户信息数据，包含如下字段：<ul><li>`channel_exist`：指定查询的频道是否存在<ul><li>true：存在</li><li>false：不存在</li></ul><li>`mode`：频道场景：<ul><li>1：通信场景</li><li>2：直播场景</li></ul><li>`total`：频道内的用户总人数</li><li>`users`：频道内所有用户的 ID 列表。</li></ul> |

```json
{
  "success": true,
  "data": {
    "channel_exist": true,
    "mode": 1,
    "total": 1,
    "users": [<uid>]
  }
}
```

- 直播场景：

| 参数 | 描述 |
| ---------------- | ---------------- |
| `sucess`      | 本次请求是否成功：<ul><li>true：请求成功</li><li>false：请求不成功</li></ul>      |
| `data` | 用户信息数据，包含如下字段：<ul><li>`channel_exist`：指定查询的频道是否存在<ul><li>true：存在</li><li>false：不存在</li></ul><li>`mode`：频道场景：<ul><li>1：通信场景</li><li>2：直播场景</li></ul><li>`broadcasters`：频道内所有主播的用户 ID 列表</li><li>`audience`：频道内观众的 uid 列表，最多包含前 10000 名观众。</li><li>`audience_total`：频道内的观众总人数。</li></ul> |

```json
{
  "success": true,
  "data": {
    "channel_exist": true,
    "mode": 2,
    "broadcasters": [<uid>],
    "audience": [<uid>],
    "audience_total": <count>
  }
}
```
		
### 分页查询厂商频道列表 (GET)

该方法查询指定厂商的频道列表。

**基本信息**

| 请求基本参数 | 描述 |
| ---------------- | ---------------- |
| 方法      | GET      |
| 请求 URL | BaseUrl/v1/channel/appid/ |

**请求参数**

#### 查询参数

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `page_no`      | 厂商频道列表的起始页面，默认值为 0      |
| `page_size`   | 每个页面显示的列表数量，默认值为 100，最大值为 500 |

**请求示例**

```
// 不带查询参数
BaseUrl/v1/channel/{appid}

// 带查询参数
BaseUrl/v1/channel/{appid}?page_no=0&page_size=100
```

**响应参数**

| 参数名 | 描述 |
| ---------------- | ---------------- |
| `success`     | 本次请求状态：<ul><li>true：请求成功</li><li>false：请求不成功</li></ul>      |
| `data` | 响应数据，包含以下字段：<ul><li>`channels`：频道列表，包含如下内容：<ul><li>`channel_name`：频道名</li><li>`user_count`：该频道内的用户人数</li></ul><li>`total_size`：频道总数</li></ul> |

**响应示例**

```json
{
  "success": true,
  "data": {
    "channels": [
      {
        "channel_name": "lkj144",
        "user_count": 3
      }
    ],
    "total_size": 3
  }
}
```

