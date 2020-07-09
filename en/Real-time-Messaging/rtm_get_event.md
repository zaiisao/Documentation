
---
title: User and Channel Events RESTful API
description: 
platform: All Platforms
updatedAt: Thu Jul 09 2020 08:34:20 GMT+0800 (CST)
---
# User and Channel Events RESTful API
> Besides this document, you can also read our interactive document [RTM RESTful API](https://docs.agora.io/en/Real-time-Messaging/restfulapi/)![](https://web-cdn.agora.io/docs-files/1583736328279). By clicking the **Example Value** or **Schema** tab, you can view example codes or parameter definitions for request/response bodies.

The Real-time Messaging RESTful API currently supports user events and channel events.

<div class="alert note">For the user events and channel events RESTful API, the number of requests per second for each App ID must not exceed 10.</div>

## <a name="auth"></a>Authentication

Agora RTM RESTful API supports HTTPS only. You can use either of the following methods to authenticate your HTTP request: 

- [Basic authentication](#basicauth)
- [Token authentication](#tokenauth)

### <a name="basicauth"></a>Basic authentication

You need to pass the basic HTTP authentication and put `api_key:api_secret` in the `Authorization` field of the HTTP header: 

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

You can get your Customer ID and Customer Certificate on the [RESTful API](https://console.agora.io/restful) page of the Agora Console. For more information on how to generate the `Authorization` filed, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication).

### <a name="tokenauth"></a>Token authentication

If you have already generated an RTM Token on your server, you can use token authentication. You need to put the following information to the `x-agora-token` field `x-agora-uid` field when sending your HTTP request: 

- The RTM Token generated at your server. 
- The uid you use to generate the RTM Token. 

**Sample code**

The following sample code shows how to implement token authentication in Java:

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

- Content-type: `application/json;charset=utf-8`
- Authorization: See [Authentication](#auth). 
- Request: See the examples in the following APIs. 
- Response: The response content is in JSON format. 

> All the request URLs and request bodies are case sensitive. 

## API sequence

- Get user events: Call `user_events`.
- Get channel events: Call `channel_events`.

## Status code

The following table contains the most common HTTP status codes for Real-time Messaging.

| Status code | Description                                                  |
| :---------- | :----------------------------------------------------------- |
|200         | The request succeeds. |
|400         | Incorrect request syntax. |
|408         | The server request times out or the server fails to respond. |

## <a name="get"></a>Gets user events

This method gets the user events from the address specified by the Agora RTM server. 

> - The number of requests per second for each App ID must not exceed 10.
> - The RTM backend stores a maximum of 2,000 events.
> - The backend returns a maximum of 1,000 events each time.
> - Agora does not guarantee the time sequence of events across geographical regions (countries or continents), because Agora stores events by geographical regions.
> - If you have pulled events from one geographical region, you may get the same events when you pull from a different geographical region. This is because Agora only synchronize events within a geographical region and does not synchronize events across geographical regions.

- Method: GET
- Endpoint: ~/rtm/vendor/user_events
- Request URL：

```
https://api.agora.io/dev/v2/project/<appid>/rtm/vendor/user_events
```

### A response example of `get`

```json
{
    "result": "success",
    "request_id" : "10116762670167749259",
    "events" : [event]
}
```

| Parameter    | Type   | Description                    |
| :----------- | :----- | :----------------------------- |
| `result`     | string | The result of this request.    |
| `request_id` | string | The unique ID of this request. |
| `events`     | JSON   | A login or logout event.       |

### An example of `event`

```json
{
    "user_id": "rtmtest_RTM_4852_4857w7%",
    "type" : "Login",
    "ts" : 1578027420761
}
```

| Parameter     | Type   | Description   |
| :------- | :----- | :-------------------- |
| `user_id` | string | The user ID involved.   |
| `type`   | string | Event type: <li><code>Login</code>: A user has logged in the Agora RTM system;</li><li><code>Logout</code>: A user has logged out of the Agora RTM ststem.</li> |
| `ts`  | int    | Timestamp in milliseconds.      |


## Gets channel events

This method gets the channel events from the address specified by the Agora RTM server. 

> - The number of requests per second for each App ID must not exceed 10.
> - The RTM backend stores a maximum of 2,000 events.
> - The backend returns a maximum of 1,000 events each time.
> - Agora does not guarantee the time sequence of events across geographical regions (countries or continents), because Agora stores events by geographical regions.
> - If you have pulled events from one geographical region, you may get the same events when you pull from a different geographical region. This is because Agora only synchronize events within a geographical region and does not synchronize events across geographical regions.

- Method: GET
- Endpoint: ~/rtm/vendor/channel_events
- Request URL:

```
https://api.agora.io/dev/v2/project/<appid>/rtm/hook/channel_events
```


### A response example of `get`

```json
{
    "result": "success",
    "request_id" : "10116762670167749259",
    "events" : [event]
}
```

| Parameter    | Type   | Description                    |
| :----------- | :----- | :----------------------------- |
| `result`     | string | The result of this request.    |
| `request_id` | string | The unique ID of this request. |
| `events`     | JSON   | A join or leave event.         |

### An example of `event`

```json
{
    "channel_id": "example_channel_id",
    "user_id": "rtmtest_RTM_4852_4857w7%",
    "type" : "Join",
    "ts" : 1578027418981
}
```

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `channel_id` | string | The channel ID involved.                                        |
| `user_id` | string | The user ID involved.                                        |
| `type`    | string | The event type: <li><code>Join</code>: A user has joined the channel;</li><li><code>Leave</code>: A user has left the channel. </li> |
| `ts`      | int    | Timestamp in milliseconds.                                   |
