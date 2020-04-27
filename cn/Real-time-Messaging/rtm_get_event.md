
---
title: 实时消息 RESTful API
description: 
platform: RESTful
updatedAt: Mon Apr 27 2020 07:05:33 GMT+0800 (CST)
---
# 实时消息 RESTful API
> 除本文外，你也可以查看我们全新的交互式 API 文档交互式 API 文档
[实时消息 RESTful API](https://docs.agora.io/cn/Real-time-Messaging/restfulapi/)![](https://web-cdn.agora.io/docs-files/1583736328279)。你可以通过切换 **Example Value** 和 **Schema** 标签页查看各 API 请求和响应包体的示例代码和参数说明。目前，交互式 API 文档只包括查询用户事件和频道事件 RESTful API。

实时消息 RESTful API 目前支持以下功能：
- 查询用户事件和频道事件。
- 查询历史消息。

## <a name="auth"></a>认证

实时消息 RESTful API 仅支持 HTTPS 协议。你可以使用以下任意一种认证方式：

- [Basic HTTP 认证](#basicauth)
- [Token 认证](#tokenauth)

### <a name="basicauth"></a>Basic HTTP 认证

发送请求时，你需要提供 `api_key:api_secret` 通过 Basic HTTP 认证并填入 HTTP 请求头部的 Authorization 字段：

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

你可以在控制台的 [RESTful API](https://console.agora.io/restful) 页面找到你的 Customer ID 和 Customer Certificate。具体生成 `Authorization` 字段的方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

### <a name="tokenauth"></a>Token 认证

如果你已经在服务端生成了 RTM Token，你也可以选用 token 认证。你需要在发送 HTTP 请求时在 HTTP 请求头部的 `x-agora-token` 字段和 `x-agora-uid` 字段分别填入：

- 服务端生成的 RTM Token
- 生成 RTM Token 时使用的 uid。

**示例代码**

下面的 Java 示例代码展示了如何实现 token 认证。

```java
  Request request = new Request.Builder()
  ...
  // 在 HTTP 请求头部的 x-agora-token 字段填入获取的 RTM Token
  .addHeader("x-agora-token", "<Your RTM Token>")
  // 在 HTTP 请求头部的 x-agora-uid 字段填入用于生成该 RTM Token 的 uid
  .addHeader("x-agora-uid", "<Your uid used to generate the RTM Token>")
  ...
```

> 关于如何生成 RTM Token，详见 [校验用户权限](https://docs.agora.io/cn/Real-time-Messaging/rtm_token?platform=All%20Platforms)。

## 数据格式

所有的请求都发送给域名：`api.agora.io`。

- Base URL: 
```
https://api.agora.io/dev/v2/project/<appid>/
```
- Content-type： `application/json;charset=utf-8`
- Authorization： 详见[认证](#auth)。
- 请求：请求的格式详见下面各个 API 中的示例。
- 响应：响应内容的格式为 JSON。

> 所有的请求 URL 和请求包体内容均区分大小写。

## API 调用步骤

### 查询用户事件和频道事件

- 查询用户上线或下线事件：直接调用 `user_events` 方法。
- 查询用户加入或离开频道事件：直接调用 `channel_events` 方法。

### 查询历史消息

- 查询历史消息：先调用 `query` 方法，再调用 `query/$handle` 方法。
- 查询历史消息数目：先调用 `query` 方法，再调用 `count` 方法。

## 响应状态码

| 错误码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
|200	|请求成功。|
|400	|请求的语法错误。|
|408	|服务器请求超时或服务器无响应。|

## 用户与频道事件 API

### <a name="get"></a>获取用户上线或下线事件 API

该方法从 Agora RTM 服务器指定的地址获取用户上线或下线事件。

>  - 每个 App ID 每秒钟的请求数不能超过 10 次。
>  - RTM 后台最多存储 2000 条事件。
>  - 单次返回最多 1000 条事件。
>  - Agora 对事件按地理区域缓存，因此不保证来自不同地理区域（跨国或者跨洲）的事件顺序的正确性。
>  - Agora 只在同一地理区域内同步事件，不在地理区域间同步。所以，你从某地理区域拉取了事件后，如果你从另一个区域再次拉取可能会得到相同的事件。

- 方法：GET
- 接入点：/rtm/vendor/user_events
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/vendor/user_events
```

#### `get` 响应包体示例
```json
{
    "result": "success",
    "request_id" : "10116762670167749259",
    "events" : [event]
}
```

| 参数     | 类型   | 描述        |
| :------- | :----- | :-------------------- |
| `result` | string | 本次请求结果。   |
| `request_id`   | string | 本次请求唯一的 ID。 |
| `events`  | JSON    | 用户上线下线事件。      |

#### `event` 事件包体示例
```json
{
    "user_id": "rtmtest_RTM_4852_4857w7%",
    "type" : "Login",
    "ts" : 1578027420761
}
```

| 参数     | 类型   | 描述   |
| :------- | :----- | :-------------------- |
| `user_id` | string | 本次事件对应的用户名。   |
| `type`   | string | 事件类型：<li>`Login`: 用户登录（上线）事件。</li><li>`Logout`: 用户登出（下线）事件。</li> |
| `ts`  | int    | 时间戳（ms）。      |


### 获取用户加入或离开频道事件 API

该方法从 Agora RTM 服务器指定的地址获取用户加入或离开频道事件。

>  - 每个 App ID 每秒钟的请求数不能超过 10 次。
>  - RTM 后台最多存储 2000 条事件。
>  - 单次返回最多 1000 条事件。
>  - Agora 对事件按地理区域缓存，因此不保证来自不同地理区域（跨国或者跨洲）的事件顺序的正确性。
>  - Agora 只在同一地理区域内同步事件，不在地理区域间同步。所以，你从某地理区域拉取了事件后，如果你从另一个区域再次拉取可能会得到相同的事件。

- 方法：GET
- 接入点：/rtm/vendor/channel_events
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/vendor/channel_events
```


#### `get` 响应包体示例
```json
{
    "result": "success",
    "request_id" : "10116762670167749259",
    "events" : [event]
}
```

| 参数     | 类型   | 描述        |
| :------- | :----- | :-------------------- |
| `result` | string | 本次请求结果。   |
| `request_id`   | string | 本次请求唯一的 ID。 |
| `events`  | JSON   | 加入退出频道事件。      |

#### `event` 事件包体示例
```json
{
    "channel_id": "example_channel_id"
    "user_id": "rtmtest_RTM_4852_4857w7%",
    "type" : "Join",
    "ts" : 1578027418981
}
```

| 参数     | 类型   | 描述   |
| :------- | :----- | :-------------------- |
| `channel_id` | string | 本次事件对应的频道名。   |
| `user_id` | string | 本次事件对应的用户名。   |
| `type`   | string | 事件类型：<li>`Join`: 用户加入频道事件。</li><li>`Leave`: 用户离开频道事件。</li> |
| `ts`  | int    | 时间戳（ms）。 |


## 历史消息 API

### 创建历史消息查询资源 API

该方法向 Agora RTM 服务器申请历史消息查询资源。若请求成功，你可以通过 GET 方法从服务器返回的 `location` 获取查询到的历史消息。

<div class="alert note">如果需要将某条点对点或频道消息存为历史消息，你必须在发送消息时将 sendMessageOptions 方法中的 enableHistoricalMessaging 参数设为 true。否则你无法通过 RESTful API 查询到这条历史消息。</div>

> - 历史消息默认保留 7 天，默认存储空间限制为 2 GB，一般足够支持数万日活 App 的消息量。如果需要提高存储空间，请联系 Agora 技术支持。
> - 当前版本仅支持文本消息，不支持自定义二进制消息。
> - 对于每个 App ID，每秒请求数不能超过 100 次。

- 方法：POST
- 接入点：/rtm/message/history/query
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query
```

#### `query` 请求包体示例

```json
{
    "filter": {
        "source": "src_account",
        "destination": "dst_account",
        "start_time" : "2019-08-01T01:22:10Z",
        "end_time" : "2019-08-01T01:32:10Z"
    },
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
}
```
| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `filter`        | JSON | 筛选条件                |
| `offset`        | int | （可选）当前时间段内的消息偏移量。 |
| `limit`         | int   | （可选）单页历史消息条数。可选值：<ul><li>20</li><li>50</li><li>100</li></ul> |
|  `order`       | string | （可选）排序方法 <ul><li> `asc`：按时间顺序（默认）。 </li><li> `desc`：按时间倒序。 </li></ul>                 |

`filter` 中需要填写的内容如下：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `source`        | string | 消息发送方 <sup>1</sup>       |
| `destination`        | string | 消息接收方。<sup>1</sup> |
| `start_time`         | string   | 历史消息查询起始时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|
|  `end_time`       | string | 历史消息查询结束时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|

> `start_time` 和 `end_time` 仅支持 UTC 时间，不支持时区和夏令时。如果你的本地时间不是 UTC 时间，你需要先将本地时间转换为 UTC 时间。例如，如果你的本地时间是 `2019-08-01T09:24:10`，时区为东八区（UTC/GMT+08:00），则 UTC 时间应为 `2019-08-01T01:24:10Z`。

> <sup>1</sup> `source` 和 `destination` 的匹配原则详见下表：

| <a name="rule"></a>source | destination | 说明                                |
| -------- | ----------- | ------------------------------------- |
| null     | null        | `source` 和 `destination` 不得同时为空。   |
| null     | UserA       | UserA 在指定时间内收到的所有消息。           |
| null     | ChannelA    | ChannelA 在指定时间段内收到的所有频道消息。   |
| UserA    | null        | UserA 在指定时间段内发送的所有消息。         |
| ChannelA | 任意字符串    | 无效的参数组合，返回空值。                               |
| UserA    | UserB       | UserA 在指定时间段内发送给 UserB 的所有点对点消息。|
| UserA    | ChannelA    | UserA 在指定时间段内发送到 ChannelA 的所有频道消息。|

#### <a name="queryresponse"></a>`query` 响应包体示例

```json
{
    "result": "success",
    "offset" : 100,
    "limit" : 20,
    "order" : "asc",
    "location" : "~/rtm/message/history/query/123456123456"
}
```

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `result`   | string | 请求结果。                                                     |
| `offset`   | int    | 当前时间段内的消息偏移量。                                   |
| `limit`    | int    | 单页历史消息条数。                                           |
| `location` | string | 历史消息资源地址。你可以从这个 URL 调用 [GET  方法](#get) 获取查询结果。 |


### <a name="get"></a>获取历史消息 API

该方法从 Agora RTM 服务器指定的地址获取历史消息。

<div class="alert note">如果需要将某条点对点或频道消息存为历史消息，你必须在发送消息时将 sendMessageOptions 方法中的 enableHistoricalMessaging 参数设为 true。否则你无法通过 RESTful API 查询到这条历史消息。</div>

> - 历史消息默认保留 7 天，默认存储空间限制为 2 GB，一般足够支持数万日活 App 的消息量。如果需要提高存储空间，请联系 Agora 技术支持。
> - 当前版本仅支持文本消息，不支持自定义二进制消息。
> - 对于每个 App ID，每秒请求数不能超过 100 次。

- 方法：GET
- 接入点：/rtm/message/history/query/$handle
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query/$handle
```

> - `$handle`  由服务器在 `query` [响应包体](#queryresponse)中的 `location` 返回。

#### `get` 响应包体示例

```json
{
    "result": "success",
    "code" : "ok",
    "messages" : [Message]
}
```
单个 `message` JSON 响应示例：

```json
{
    "dst": "dst",
	"message_type": "peer_message",
	"ms": 1587009745719,
    "payload": "123",
	"src": "src"
```

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `result`        | string | 本次请求结果。                |
| `code`        | string | 资源准备情况：<ul><li> `ok`：资源准备完成。</li><li>`in progress`：资源还未准备完成，请稍后重试。当服务器返回 `in progress` 时，请等待 2 秒再发起 GET 请求。</li></ul> |
| `message`         | JSON   | 历史消息列表。 |


每个 `message` 包含以下参数：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `src`        | string | 消息发送方。 |
| `dst`        | string | 消息接收方。 |
| `message_type`  | string  |   消息类型。可以是 `peer_message` 或 `channel_message`。    |
| `payload` | string   | 消息体。当前版本仅支持文本消息。 |
|  `ms`       | int |  返回从1970 年 1 月 1 日（UTC）到服务器接受请求的时间（UTC）的秒数。|


### 获取历史消息数目 API

该方法从 Agora RTM 服务器指定的地址获取历史消息数目。

<div class="alert note">如果需要将某条点对点或频道消息存为历史消息，你必须在发送消息时将 sendMessageOptions 方法中的 enableHistoricalMessaging 参数设为 true。否则你无法通过 RESTful API 查询到这条历史消息。</div>

> - 历史消息默认保留 7 天，默认存储空间限制为 2 GB，一般足够支持数万日活 App 的消息量。如果需要提高存储空间，请联系 Agora 技术支持。
> - 当前版本仅支持文本消息，不支持自定义二进制消息。
> - 对于每个 App ID，每秒请求数不能超过 100 次。

- 方法：GET
- 接入点：/rtm/message/history/count
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count
```


#### `count` 请求示例

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?source="src"&destination="dst"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```



| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `source`        | string |  消息发送方 <sup>1</sup>       |
| `destination`        | string | 消息接收方。<sup>1</sup> |
| `start_time`         | string   | 历史消息查询起始时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|
|  `end_time`       | string | 历史消息查询结束时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|

> `start_time` 和 `end_time` 仅支持 UTC 时间，不支持时区和夏令时。如果你的本地时间不是 UTC 时间，你需要先将本地时间转换为 UTC 时间。例如，如果你的本地时间是 `2019-08-01T09:24:10`，时区为东八区（UTC/GMT+08:00），则 UTC 时间应为 `2019-08-01T01:24:10Z`。

> <sup>3</sup> `source` 和 `destination` 的匹配原则详见[匹配关系表](#rule)。

#### `count` 响应包体示例

```json
{
    "result": "success",
    "code" : "ok",
    "count" : 123
}
```
| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `result`        | string | 本次请求结果。                |
| `code`        | string | 资源准备情况：<ul><li> `ok`：资源准备完成。</li><li>`in progress`：资源还未准备完成，请稍后重试。当服务器返回 `in progress` 时，请等待 2 秒再发起 GET 请求。</li></ul> |
| `count`         | int   | 历史消息数目。 |

