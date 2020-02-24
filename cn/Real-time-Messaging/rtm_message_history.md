
---
title: 历史消息 RESTful API
description: 
platform: RESTful
updatedAt: Mon Feb 24 2020 07:39:02 GMT+0800 (CST)
---
# 历史消息 RESTful API
实时消息 RESTful API 目前支持查询点对点或频道历史消息。如需将某条点对点或频道消息存为历史消息，你需要在发送消息时将 `sendMessageOptions` 中的 `enableHistoricalMessaging` 设为 `true`，之后可以通过 RESTful API 查询到这条历史消息。

<div class="alert note"><li>当前版本的 RESTful API 仅支持文本消息，不支持自定义二进制消息。</li><li>历史消息 RESTful API 的 qps 限制为：单厂商每秒 100 次。</div>

## <a name="auth"></a>认证

实时消息 RESTful API 仅支持 HTTPS 协议。你可以通过以下任意一种认证方式：

- [Basic Authentication](#basicauth)
- [Token Authentication](#tokenauth)

### <a name="basicauth"></a>Basic Authentication

发送请求时，你需要提供 `api_key:api_secret` 通过 Basic HTTP 认证并填入 HTTP 请求头部的 Authorization 字段：

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

你可以在控制台的 [RESTful API](https://console.agora.io/restful) 页面找到你的 Customer ID 和 Customer Certificate。具体生成 `Authorization` 字段的方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

### <a name="tokenauth"></a>Token Authentication

如果你已经在服务端生成了 RTM Token，你也可以选用 Token Authentication 认证。你需要再发送 HTTP 请求时在 HTTP 请求头部的 x-agora-token 字段和 x-agora-uid 字段分别填入：

- 服务端生成的 RTM Token
- 生成 RTM Token 时使用的 uid。

**示例代码**

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
- Authorization： 详见：[认证](#auth)。
- 请求：请求的格式详见下面各个 API 中的示例。
- 响应：响应内容的格式为 JSON。

> 所有的请求 URL 和请求包体内容均区分大小写。

## 创建历史消息查询资源 API

该方法向 Agora RTM 服务器申请历史消息查询资源。若请求成功，你可以通过 GET 方法从服务器返回的 `location` 获取查询到的历史消息。

- 方法：POST
- 接入点：~/rtm/message/history/query
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query
```

### `query` 请求包体示例

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
|  `order`       | string | （可选）排序方法 <ul><li> 按时间顺序（默认）： asc </li><li>按时间倒序： desc </li></ul>                 |

`filter` 中需要填写的内容如下：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `source`        | string | （视情况）消息发送方 <sup>1</sup>       |
| `destination`        | string | （视情况）消息接收方。<sup>1</sup> |
| `start_time`         | string   | （必选）历史消息查询起始时间。使用 [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) 标准为 UTC 时间。 |
|  `end_time`       | string | （必选）历史消息查询结束时间。使用 [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) 标准为 UTC 时间。 |

> <sup>1</sup> `source` 和 `destination` 的匹配原则详见下表：

| <a name="rule"></a>source | destination | 说明                                |
| -------- | ----------- | ------------------------------------- |
| null     | null        | `source` 和 `destination` 不得同时为空。   |
| null     | UserA       | UserA 在指定时间内收到的所有消息。           |
| null     | ChannelA    | ChannelA 在指定时间段内收到的所有频道消息。   |
| UserA    | null        | UserA 在指定时间段内发送的所有消息。         |
| channelA | 任意字符串    | 返回空值。                               |
| UserA    | UserB       | UserA 在指定时间段内发送给 UserB 的所有点对点消息。|
| UserA    | ChannelA    | UserA 在指定时间段内发送到 ChannelA 的所有频道消息。|

###<a name="queryresponse"></a>`query` 响应包体示例

```json
{
    "result": "success",
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
    "location" : "~/rtm/message/history/query/123456123456"
}
```

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `result`   | string | 请求结果                                                     |
| `offset`   | int    | 当前时间段内的消息偏移量。                                   |
| `limit`    | int    | 单页历史消息条数。                                           |
| `location` | string | 历史消息资源地址。你可以从这个 URL 调用 [GET  方法](#get) 获取查询结果。 |

### 错误码

| 错误码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| 400    | 请求的语法错误（如参数错误）。请根据返回提示检查。           |
| 408    | 成功请求并创建了新的资源。                                   |

## <a name="get"></a>获取历史消息 API

该方法从 Agora RTM 服务器指定的地址获取历史消息。

- 方法：GET
- 接入点：~/rtm/message/history/query/$handle
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query/$handle
```

> - 请求 URL 由服务器在 `query` [响应包体](#queryresponse)中返回。

### `get` 响应包体示例
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
    "src": "src",
    "dst" : "dst",
    "payload" : "123",
    "ts" : 123
}
```

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `result`        | string | 本次请求结果。                |
| `code`        | string | 资源准备情况：<ul><li> 资源准备完成： "ok" </li><li>资源还未准备完成，请稍后重试： "in progress" <sup>2</sup></li></ul> |
| `message`         | JSON   | 历史消息列表。 |

> <sup>2</sup>  当服务器返回 "in progress" 时，请间隔 2 秒再发起 GET 请求。

每个 `message` 包含以下参数：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `src`        | string | 消息发送方。                |
| `dest`        | string | 消息接收方。 |
| `payload`         | string   | 消息体。当前版本仅支持文本消息。 |
|  `ts`       | string | 服务器接收时间。使用 [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) 标准为 UTC 时间。 |

### 错误码

| 错误码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| 400    | 请求的语法错误（如参数错误）。请根据返回提示检查。           |
| 408    | 成功请求并创建了新的资源。                                   |
| 410    | 资源已经失效，请重新调用 POST 方法创建新的查询资源。 |

## 获取历史消息数目 API

- 方法：GET
- 接入点：~/rtm/message/history/count
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count
```

> - 请求 URL 由服务器在 `query` [响应包体](#queryresponse)中返回。

### `count` 请求示例

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?source="src"&destination="dst"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```



| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `source`        | string | （视情况）消息发送方 <sup>1</sup>       |
| `destination`        | string | （视情况）消息接收方。<sup>1</sup> |
| `start_time`         | string   | （必选）历史消息查询起始时间。使用 [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) 标准为 UTC 时间。 |
|  `end_time`       | string | （必选）历史消息查询结束时间。使用 [ISO8601](http://en.wikipedia.org/wiki/ISO_8601) 标准为 UTC 时间。 |

> <sup>3</sup> `source` 和 `destination` 的匹配原则详见[匹配关系表](#rule)。

### `count` 响应包体示例

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
| `code`        | string | 资源准备情况：<ul><li> 资源准备完成： "ok" </li><li>资源还未准备完成，请稍后重试： "in progress" <sup>4</sup></li></ul> |
| `count`         | int   | 历史消息数目。 |

> <sup>4</sup>  当服务器返回 "in progress" 时，请间隔 2 秒再发起 GET 请求。

### 错误码

| 错误码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| 400    | 请求的语法错误（如参数错误）。请根据返回提示检查。           |
| 408    | 成功请求并创建了新的资源。                                   |



