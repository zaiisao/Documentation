
---
title: 事件与历史消息查询 RESTful API
description: 
platform: All Platforms
updatedAt: Mon Aug 17 2020 06:06:52 GMT+0800 (CST)
---
# 事件与历史消息查询 RESTful API
事件与历史消息查询 RESTful API 目前支持以下功能：
- 查询用户事件和频道事件。
- 查询历史消息。

## 认证

### Basic HTTP 认证

实时消息 RESTful API 仅支持 HTTPS 协议。发送请求时，你需要提供 `api_key:api_secret` 通过 basic HTTP 认证并填入 HTTP 请求头部的 `Authorization` 字段。具体生成 `Authorization` 字段的方法请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

### Token 认证

如果你已经在服务端生成了 RTM Token，你也可以选用 token 认证。你需要在发送 HTTP 请求时在 HTTP 请求头部的 `x-agora-token` 字段和 `x-agora-uid` 字段分别填入：

- 服务端生成的 RTM Token。
- 生成 RTM Token 时使用的 uid。

#### 示例代码

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

<div class="alert note">关于如何生成 RTM Token，详见<a href="https://docs.agora.io/cn/Real-time-Messaging/rtm_token?platform=All20%Platforms">校验用户权限</a>。</div>


## API 调用步骤

### 查询用户事件和频道事件

- 查询用户上线或下线事件：直接调用[获取用户上线或下线事件 API](#get_user)。
- 查询用户加入或离开频道事件：直接调用[获取用户加入或离开频道事件 API](#get_channel)。

### 查询历史消息

- 查询历史消息：先调用[创建历史消息查询资源 API](#create_history_res)，再调用[获取历史消息 API](#get_history_message)。
- 查询历史消息数目：直接调用[获取历史消息数目 API](#get_history_message_count)。

## 数据格式

所有的请求都发送给域名：`api.agora.io`。

- 请求：请求的格式详见下面各个 API 中的示例
- 响应：响应内容的格式为 JSON
- 基本 URL：`https://api.agora.io/dev/v2/project/<appid>`

<div class="alert note"><code>&lt;appid&gt;</code> 是你的 RTM 项目使用的 <a href="https://docs.agora.io/cn/Real-time-Messaging/rtm_token?platform=All%20Platforms#a-name--appida使用-app-id-鉴权">App ID</a>。所有的请求 URL 和请求包体内容都是区分大小写的。</div>

## 用户与频道事件 API

### <a name="get_user"></a>获取用户上线或下线事件 API（GET）

该方法从 Agora RTM 服务器指定的地址获取用户上线或下线事件。已经获取的事件会从 Agora RTM 服务器中移除，你无法再次获取。

>  - 每个 App ID 每秒钟的请求数不能超过 10 次。
>  - RTM 后台最多存储 2000 条事件。如果事件超过 2000 条，最老的事件会被最新的事件替换。
>  - 单次返回最多 1000 条事件。
>  - Agora 对事件按地理区域缓存，因此不保证来自不同地理区域的事件顺序的正确性。
>  - Agora 只在同一地理区域内同步事件，不在地理区域间同步。所以，你从某地理区域发起请求拉取了事件后，如果你从另一个区域再次发起请求可能会得到相同的事件。

- 方法：GET
- 接入点：`/rtm/vendor/user_events`
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/vendor/user_events
```

#### 响应示例
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

`[event]` 包含以下内容：

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
| `ts`  | int    | 从1970 年 1 月 1 日（UTC）到服务器接受请求的时间（UTC）的毫秒数。     |


### <a name="get_channel"></a>获取用户加入或离开频道事件 API（GET）

该方法从 Agora RTM 服务器指定的地址获取用户加入或离开频道事件。已经获取的事件会从 Agora RTM 服务器中移除，你无法再次获取。

>  - 每个 App ID 每秒钟的请求数不能超过 10 次。
>  - RTM 系统最多存储 2000 条事件。如果事件超过 2000 条，最老的事件会被最新的事件替换。
>  - 单次返回最多 1000 条事件。
>  - Agora 对事件按地理区域缓存，因此不保证来自不同地理区域的事件顺序的正确性。
>  - Agora 只在同一地理区域内同步事件，不在地理区域间同步。所以，你从某地理区域发起请求拉取了事件后，如果你从另一个区域再次发起请求可能会得到相同的事件。

- 方法：GET
- 接入点：`/rtm/vendor/channel_events`
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/vendor/channel_events
```


#### 响应示例
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

`[event]` 包含以下内容：

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
| `ts`  | int    | 从1970 年 1 月 1 日（UTC）到服务器接受请求的时间（UTC）的毫秒数。 |

### 响应状态码

| 错误码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
|200	|请求成功。|
|400	|请求的语法错误。|
|408	|服务器请求超时或服务器无响应。|

## 历史消息 API

### <a name="create_history_res"></a>创建历史消息查询资源 API（POST）

该方法向 Agora RTM 服务器申请历史消息查询资源。若请求成功，你可以通过 GET 方法从服务器返回的 `location` 获取查询到的历史消息。

<div class="alert note">如果需要将某条点对点或频道消息存为历史消息，你必须在调用 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a6e16eb0e062953980a92e10b0baec235"><code>sendMessage</code></a> 或 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9"><code>sendMessageToPeer</code></a> 时将 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_send_message_options.html"><code>sendMessageOptions</code></a> 类中的 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_send_message_options.html#a924ef8c28e7a1d41a215a7331e284330"><code>enableHistoricalMessaging</code></a> 成员变量设为 <code>true</code>。否则你无法通过 RESTful API 查询到这条历史消息。</div>

> - 历史消息默认保留七天，单个 App ID 的默认存储空间限制为 2 GB，一般足够支持数万日活应用的消息量。如果需要延长保留时间或提高存储空间，请[提交工单](https://agora-ticket.agora.io/?_ga=2.80217495.108219700.1587866859-1583961819.1580439641)联系技术支持。
> - 当前版本仅支持文本消息，不支持自定义二进制消息。
> - 对于每个 App ID，每秒请求数不能超过 100 次。

- 方法：POST
- 接入点：`/rtm/message/history/query`
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query
```

请求包体的参数如下：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `filter`        | JSON | 筛选条件。                |
| `offset`        | int | （可选）当前时间段内的消息偏移量。 |
| `limit`         | int   | （可选）单页历史消息条数。可选值：<ul><li>20</li><li>50</li><li>100</li></ul> |
| `order`       | string | （可选）排序方法 <ul><li> `asc`（默认）：按时间顺序。 </li><li> `desc`：按时间倒序。 </li></ul>                 |

`filter` 中需要填写的内容如下：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `source`        | string | （可选）消息发送方，必须是用户。 如果此参数不赋值，则 API 会筛选出消息接收方在指定时间段内收到的所有消息。     |
| `destination`        | string | （可选）消息接收方，可以是用户或频道。如果此参数不赋值，则 API 会筛选出消息发送方在指定时间段内发送的所有消息。 |
| `start_time`         | string   | 历史消息查询起始时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|
|  `end_time`       | string | 历史消息查询结束时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|

> 频道只能作为 `destination`。API 的筛选规则与 `source` 和 `destination` 的关系详见下表：

| <a name="rule"></a>`source` | `destination` | 筛选规则                                |
| -------- | ----------- | ------------------------------------- |
| 不赋值     | userA       | API 会筛选出 userA 在指定时间段内收到的所有消息，包括点对点消息和频道消息。           |
| 不赋值     | channelA    | API 会筛选出 channelA 在指定时间段内收到的所有频道消息。   |
| userA    | 不赋值      | API 会筛选出 userA 在指定时间段内发送的所有消息，包括点对点消息和频道消息。         |
| userA    | userB       | API 会筛选出 userA 在指定时间段内发送给 userB 的所有点对点消息。|
| userA    | channelA    | API 会筛选出 userA 在指定时间段内发送到 channelA 的所有频道消息。|

> `start_time` 和 `end_time` 仅支持 UTC 时间，不支持时区和夏令时。如果你的本地时间不是 UTC 时间，你需要先将本地时间转换为 UTC 时间。例如，如果你的本地时间是 `2019-08-01T09:24:10`，时区为东八区（UTC/GMT+08:00），则 UTC 时间应为 `2019-08-01T01:24:10Z`。

#### 请求示例

##### userA 在指定时间段内收到的所有消息

```json
{
    "filter": {
        "destination": "userA",
        "start_time" : "2019-08-01T01:22:10Z",
        "end_time" : "2019-08-01T01:32:10Z"
    },
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
}
```

##### channelA 在指定时间段内收到的所有消息

```json
{
    "filter": {
        "destination": "channelA",
        "start_time" : "2019-08-01T01:22:10Z",
        "end_time" : "2019-08-01T01:32:10Z"
    },
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
}
```

##### userA 在指定时间段内发送的所有消息

```json
{
    "filter": {
        "source": "userA",
        "start_time" : "2019-08-01T01:22:10Z",
        "end_time" : "2019-08-01T01:32:10Z"
    },
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
}
```

##### userA 在指定时间段内发送给 userB 的所有点对点消息

```json
{
    "filter": {
        "source": "userA",
        "destination": "userB",
        "start_time" : "2019-08-01T01:22:10Z",
        "end_time" : "2019-08-01T01:32:10Z"
    },
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
}
```

##### userA 在指定时间段内发送到 channelA 的所有频道消息

```json
{
    "filter": {
        "source": "userA",
        "destination": "channelA",
        "start_time" : "2019-08-01T01:22:10Z",
        "end_time" : "2019-08-01T01:32:10Z"
    },
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
}
```



#### <a name="queryresponse"></a>响应示例

```json
{
    "result": "success",
    "offset" : 100,
    "limit" : 20,
    "order" : "asc",
    "location" : "~/rtm/message/history/query/123456123456"
}
```

响应包体的参数如下：

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `result`   | string | 请求结果。                                                     |
| `offset`   | int    | 当前时间段内的消息偏移量。                                   |
| `limit`    | int    | 单页历史消息条数。                                           |
| `location` | string | 历史消息资源地址。你可以从这个 URL 调用[获取历史消息 API](#get_history_message) 获取查询结果。 |


### <a name="get_history_message"></a>获取历史消息 API（GET）

该方法从 Agora RTM 服务器指定的地址获取历史消息。

<div class="alert note">如果需要将某条点对点或频道消息存为历史消息，你必须在调用 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a6e16eb0e062953980a92e10b0baec235"><code>sendMessage</code></a> 或 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9"><code>sendMessageToPeer</code></a> 时将 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_send_message_options.html"><code>sendMessageOptions</code></a> 类中的 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_send_message_options.html#a924ef8c28e7a1d41a215a7331e284330"><code>enableHistoricalMessaging</code></a> 成员变量设为 <code>true</code>。否则你无法通过 RESTful API 查询到这条历史消息。</div>


> - 历史消息默认保留七天，单个 App ID 的默认存储空间限制为 2 GB，一般足够支持数万日活应用的消息量。如果需要延长保留时间或提高存储空间，请[提交工单](https://agora-ticket.agora.io/?_ga=2.80217495.108219700.1587866859-1583961819.1580439641)联系技术支持。
> - 当前版本仅支持文本消息，不支持自定义二进制消息。
> - 对于每个 App ID，每秒请求数不能超过 100 次。

- 方法：GET
- 接入点：`/rtm/message/history/query/$handle`
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query/$handle
```

> - `$handle`  是[创建历史消息查询资源 API](#create_history_res) 返回的 `location` 字段中 `~/rtm/message/history/query/` 后面的部分。例如，如果` location` 返回 `"~/rtm/message/history/query/123456123456"`，则 `$handle` 为 `123456123456`。

#### 响应示例

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

响应包体的参数如下：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `result`        | string | 本次请求结果。                |
| `code`        | string | 资源准备情况：<ul><li> `ok`：资源准备完成。</li><li>`in progress`：资源还未准备完成，请稍后重试。当服务器返回 `in progress` 时，请等待 2 秒再发起请求。</li></ul> |
| `message`         | JSON   | 历史消息列表。 |


每个 `message` 包含以下参数：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `src`        | string | 消息发送方。 |
| `dst`        | string | 消息接收方。 |
| `message_type`  | string  |   消息类型。可以是 `peer_message` 或 `channel_message`。    |
| `payload` | string   | 消息体。当前版本仅支持文本消息。 |
|  `ms`       | int |  从1970 年 1 月 1 日（UTC）到服务器接受请求的时间（UTC）的毫秒数。|


### <a name="get_history_message_count"></a>获取历史消息数目 API（GET）

该方法从 Agora RTM 服务器指定的地址获取历史消息数目。

<div class="alert note">如果需要将某条点对点或频道消息存为历史消息，你必须在调用 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a6e16eb0e062953980a92e10b0baec235"><code>sendMessage</code></a> 或 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a729079805644b3307297fb2e902ab4c9"><code>sendMessageToPeer</code></a> 时将 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_send_message_options.html"><code>sendMessageOptions</code></a> 类中的 <a href="https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_send_message_options.html#a924ef8c28e7a1d41a215a7331e284330"><code>enableHistoricalMessaging</code></a> 成员变量设为 <code>true</code>。否则你无法通过 RESTful API 查询到这条历史消息。</div>


> - 历史消息默认保留七天，单个项目的默认存储空间限制为 2 GB，一般足够支持数万日活应用的消息量。如果需要延长保留时间或提高存储空间，请[提交工单](https://agora-ticket.agora.io/?_ga=2.80217495.108219700.1587866859-1583961819.1580439641)联系技术支持。
> - 当前版本仅支持文本消息，不支持自定义二进制消息。
> - 对于每个 App ID，每秒请求数不能超过 100 次。

- 方法：GET
- 接入点：`/rtm/message/history/count`
- 请求 URL：
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count
```

请求的查询参数如下：

| 参数            | 类型   | 描述                          |
| :------------- | :----- | :--------------------------- |
| `source`        | string | （可选）消息发送方，必须是用户。 如果此参数不赋值，则 API 会筛选出消息接收方在指定时间段内收到的所有消息。     |
| `destination`        | string | （可选）消息接收方，可以是用户或频道。如果此参数不赋值，则 API 会筛选出消息发送方在指定时间段内发送的所有消息。 |
| `start_time`         | string   | 历史消息查询起始时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|
|  `end_time`       | string | 历史消息查询结束时间。时间为 UTC 时间，使用 ISO8601 标准。时间的格式为 `yyyy-mm-ddThh:mm:ssZ` ，例如 `2019-08-01T01:24:10Z`。`Z` 代表时间偏移量为 0，即为 UTC 时间。|

> 频道只能作为 `destination`。API 的筛选规则与 `source` 和 `destination` 的关系详见下表：

| <a name="rule"></a>`source` | `destination` | 筛选规则                                |
| -------- | ----------- | ------------------------------------- |
| 不赋值     | userA       | API 会筛选出 userA 在指定时间段内收到的所有消息，包括点对点消息和频道消息。           |
| 不赋值     | channelA    | API 会筛选出 channelA 在指定时间段内收到的所有频道消息。   |
| userA    | 不赋值      | API 会筛选出 userA 在指定时间段内发送的所有消息，包括点对点消息和频道消息。         |
| userA    | userB       | API 会筛选出 userA 在指定时间段内发送给 userB 的所有点对点消息。|
| userA    | channelA    | API 会筛选出 userA 在指定时间段内发送到 channelA 的所有频道消息。|

> `start_time` 和 `end_time` 仅支持 UTC 时间，不支持时区和夏令时。如果你的本地时间不是 UTC 时间，你需要先将本地时间转换为 UTC 时间。例如，如果你的本地时间是 `2019-08-01T09:24:10`，时区为东八区（UTC/GMT+08:00），则 UTC 时间应为 `2019-08-01T01:24:10Z`。


#### 请求示例

##### userA 在指定时间段内收到的所有消息

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?&destination="userA"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```

##### channelA 在指定时间段内收到的所有消息

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?&destination="channelA"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```

##### userA 在指定时间段内发送的所有消息

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?source="userA"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```

##### userA 在指定时间段内发送给 userB 的所有点对点消息

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?source="userA"&destination="userB"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```

##### userA 在指定时间段内发送到 channelA 的所有频道消息

```json
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count?source="userA"&destination="channelA"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```


#### 响应示例

响应包体参数如下：

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
| `code`        | string | 资源准备情况：<ul><li> `ok`：资源准备完成。</li><li>`in progress`：资源还未准备完成，请稍后重试。当服务器返回 `in progress` 时，请等待 2 秒再发起请求。</li></ul> |
| `count`         | int   | 历史消息数目。 |

### 响应状态码

| 错误码 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
|200	|请求成功。|
|400	|请求的语法错误。|
|408	|服务器请求超时或服务器无响应。|
