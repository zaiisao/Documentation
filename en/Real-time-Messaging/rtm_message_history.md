
---
title: Message history APIs
description: 
platform: RESTful
updatedAt: Mon Feb 24 2020 07:37:34 GMT+0800 (CST)
---
# Message history APIs
You can now query a peer-to-peer or channel message history with the Agora RTM RESTful APIs. A message is not automatically stored in the message history. To store a message in the message history, you must set `enableHistoricalMessaging` (under `AgoraRtmSendMessageOptions` for iOS/macOS or `SendMessageOptions` for Android, Linux Java, Linux C++, Web, and Windows C++) as `true` when calling the `sendMessageToPeer` or `sendMessage` method. 

<div class="alert note"><li>The current version of Agora Message History RESTful APIs works only for the text messages, not for the raw messages. </li><li> qps of the Message History RESTful APIs: 10 (for each vendor).</div>

## <a name="auth"></a>Authentication

Agora RTM RESTful API supports HTTPS only. You can use either of the following to authenticate your HTTP request: 

- [Basic Authentication](#basicauth)
- [Token Authentication](#tokenauth)

### <a name="basicauth"></a>Basic Authentication

You need to pass the basic HTTP authentication and put `api_key:api_secret` into the `Authorization` field of the HTTP header: 

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

You can get your Customer ID and Customer Certificate on the [RESTful API](https://console.agora.io/restful) page of the Agora Console. For more information on how to generate the `Authorization` filed, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication).

### <a name="tokenauth"></a>Token Authentication

If you have already generated an RTM Token on your server, you can use Token Authentication. You need to put the following information to the `x-agora-token` field `x-agora-uid` field when sending your HTTP request: 

- The RTM Token generated at your server. 
- The uid you use to generate the RTM Token. 

**Sample code**

```java
  Request request = new Request.Builder()
  ...
  // Puts your RTM Token to the `x-agora-token` field of the HTTP request. 
  .addHeader("x-agora-token", "<Your RTM Token>")
  // Puts the uid, which generates the RTM token, to the `x-agora-uid` field of the HTTP request.
  .addHeader("x-agora-uid", "<Your uid used to generate the RTM Token>")
  ...
```

> For more information about how to generate an RTM Token, see [Token Security](https://docs.agora.io/en/Real-time-Messaging/rtm_token?platform=All%20Platforms).

## Data format

All requests are sent to the host: `api.agora.io`.

- Base URL: 
```
https://api.agora.io/dev/v2/project/<appid>/
```
- Content-type： `application/json;charset=utf-8`
- Authorization： `Basic Authorization`. See [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details. 
- Request: See the examples in the following APIs. 
- Response: The response content is in JSON format. 

> All the request URLs and request bodies are case sensitive. 

## Creates a query resource

This method requests Agora RTM server for a historical message resource. If the method call succeeds, you can use the  GET method to get the historical messages from  `location` that the receiver returns. 

- Method: POST
- Endpoint: 
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query
```

### A request example of `query`

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

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `filter`  | JSON   | The filtering condition.                                     |
| `offset`  | int    | (Optional) Offset of the messages within the specified time frame. |
| `limit`   | int    | (Optional) The number of historical messages on a single page. Optional values: <ul><li>20</li><li>50</li><li>100</li></ul> |
| `order`   | string | (Optional) The sorting order. <ul><li> Ascending order: asc </li><li>Descending order (default): desc </li></ul> |

`filter` has the following parameters:

| Parameter     | Type   | Description                                                  |
| :------------ | :----- | :----------------------------------------------------------- |
| `source`      | string | (Conditional) The message sender. <sup>1</sup>               |
| `destination` | string | (Conditional) The message receiver. <sup>1</sup>             |
| `start_time`  | string | (Mandatory) The starting time (UTC) of the historical message query. |
| `end_time`    | string | (Mandatory) The ending time (UTC) of the historical message query. |

> <sup>1</sup> See the following table for the relation between `source` and `destination`: 

| <a name="rule"></a>source | destination | Description                                                  |
| ------------------------- | ----------- | ------------------------------------------------------------ |
| null                      | null        | `source` and `destination` cannot be `null` at the same time. |
| null                      | UserA       | All the messages that UserA received within the specified time frame. |
| null                      | ChannelA    | All the channel messages that ChannelA received within the specified time frame. |
| UserA                     | null        | All the messages that UserA sent within the specified time frame. |
| channelA                  | Any string  | Returns `null`.                                              |
| UserA                     | UserB       | All the peer-to-peer messages that UserA sent to UserB within the specified time frame. |
| UserA                     | ChannelA    | All the channel messages that UserA sent to ChannelA within the specified time frame. |

### <a name="queryresponse"></a>A response example of `query`

```json
{
    "result": "success",
    "offset" : 100,
    "limit" : 20,
    "order" : "asc"
    "location" : "~/rtm/message/history/query/123456123456"
}
```

| Parameter  | Type   | Description                                                  |
| :--------- | :----- | :----------------------------------------------------------- |
| `result`   | String | The result of the request.                                   |
| `offset`   | int    | Offset of the messages within the specified time frame.                                |
| `limit`    | int    | Number of historical messages on a single page.              |
| `location` | string | The URL address of the historical message resource. You can use this URL to get the query result of the [GET ](#get) method call. |

### Error codes

| Error codes | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| 400         | The request has a syntax error, e.g., an invalid parameter exists. Please check according to the returned error message. |
| 408         | The request succeeds and creates a new resource.             |

## <a name="get"></a>Gets the message history

This method gets the message history from the URL address specified by the Agora RTM server. 

- Method: GET
- Endpoint: 
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/query/$handle
```

> - The server returns the request URL in the [query response](#queryresponse).

### A response example of `get`

```json
{
    "result": "success",
    "code" : "ok",
    "messages" : [Message]
}
```

A single `message` JSON response example:

```json
{
    "src": "src",
    "dst" : "dst",
    "payload" : "123",
    "ts" : 123
}
```

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `result`  | string | The result of this request.                                  |
| `code`    | string | Whether the resource is ready: <ul><li> A resource is ready: "ok" </li><li>A resource is not yet ready, please try later: "in progress" <sup>2</sup></li></ul> |
| `message` | JSON   | A list of historical messages. <ul><li>20</li><li>50</li><li>100</li></ul> |

> <sup>2</sup>  Please wait at least two seconds to send a second `GET` request, after the server returns the "in progress" message. 

Each `message` body includes the following parameters: 

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `src`     | string | The message sender.                                          |
| `dest`    | string | The message receiver.                                        |
| `payload` | string | The message body. The current version supports text message only. |
| `ts`      | string | Timestamp of when the receiver receives the message.         |

### Error codes

| Error codes | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| 400         | The request has a syntax error, e.g., an invalid parameter exists. Please check according to the returned error message. |
| 408         | The request succeeds and creates a new resource.             |
| 410         | The resource fails. Please call the POST method again to a new query resource. |

## Gets the number of historical messages

- Method: GET
- Endpoint: 
```
https://api.agora.io/dev/v2/project/<appid>/rtm/message/history/count
```

> - The server returns the request URL in the [query response](#queryresponse).

### An HTTP request example of `count`

```json
https://api.agora.io/dev/v2/project/\<appid\>/rtm/message/history/count?source="src"&destination="dst"&start_time=2019-08-01T01:22:10Z&end_time=2019-08-01T01:24:10Z
```



| Parameter     | Type   | Description                                                  |
| :------------ | :----- | :----------------------------------------------------------- |
| `source`      | string | (Conditional) The message sender. <sup>3</sup>               |
| `destination` | string | (Conditional) The message receiver. <sup>3</sup>             |
| `start_time`  | string | (Mandatory) The starting time (UTC) of the historical message query. |
| `end_time`    | string | (Mandatory) The ending time (UTC) of the historical message query. |

> <sup>3</sup> See [Matching principle](#rule) for a matching relation between `source` and `destination`. 

###  A response example of `count`

```json
{
    "result": "success",
    "code" : "ok",
    "count" : 123
}
```

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `result`  | string | The result of this request.                                  |
| `code`    | string | Whether the resource is ready: <ul><li> A resource is ready: "ok" </li><li>A resource is not yet ready, please try later: "in progress" <sup>4</sup></li></ul> |
| `count`   | int    | The number of the historical messages.                       |

> <sup>4</sup>  Please wait at least two seconds to send the next `GET` request, after the server returns the "in progress" message. 

### Error codes

| Error codes | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| 400         | The request has a syntax error, e.g., an invalid parameter exists. Please check according to the returned error message. |
| 408         | The request succeeds and creates a new historical message resource. |


