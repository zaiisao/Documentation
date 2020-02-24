
---
title: Get user and channel events
description: 
platform: RESTful
updatedAt: Fri Feb 21 2020 13:11:33 GMT+0800 (CST)
---
# Get user and channel events
You can now get both user and channel events with the Agora RTM RESTful APIs. 

<div class="alert note">qps of the RESTful APIs: 100 (for each vendor).</div>


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

- Content-type: `application/json;charset=utf-8`
- Authorization: See [Authentication](#auth). 
- Request: See the examples in the following APIs. 
- Response: The response content is in JSON format. 

> All the request URLs and request bodies are case sensitive. 


## <a name="get"></a>Gets user events

This method gets the user events from the address specified by the Agora RTM server. 

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



> - The RTM backend stores a maximum of 2,000 events. 
> - The backend returns a maximum of 1,000 events each time. 
> - We do not guarantee the time sequence of events across geographies, because we store events by regions. 
> - If you have pulled events from one region, you may get the same events when you pull from a different region. This is because we only synchronize events within a region and do not synchronize events across regions. 

### Error code

| Error code | Description                                                  |
| :--------- | :----------------------------------------------------------- |
| 408        | The server request times out, or the server fails to respond. Try again later. |

## Gets channel events

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

> - The RTM backend stores a maximum of 2,000 events. 
> - The backend returns a maximum of 1,000 events each time. 
> - We do not guarantee the time sequence of events across geographies, because we store events by regions. 
> - If you have pulled events from one region, you may get the same events when you pull from a different region. This is because we only synchronize events within a region and do not synchronize events across regions. 


### Error codes

| Error codes | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| 408         | The server request times out or the server fails to respond. Try again later. |
