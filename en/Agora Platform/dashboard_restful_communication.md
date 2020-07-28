
---
title: RESTful API
description: 
platform: All Platforms
updatedAt: Thu Jul 09 2020 02:17:28 GMT+0800 (CST)
---
# RESTful API
<div class="alert note">This article has been moved to <a href="https://docs.agora.io/en/rtc/restfulapi/">Server RESTful API</a >. This article will no longer be updated.</div>

## Authentication

<div class="alert info">Before using the RESTful APIs, ensure that your account has enabled the relevant privilege of a specified project in Agora Console. Agora supports customizing different user roles and privileges. For more information, see <a href="../../en/Agora%20Platform/manage_member.md">Management members</a >.</div>

The RESTful API only supports HTTPS. Before using the Agora RESTful API, you need to pass the basic HTTP authentication. See [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.

<div class="alert note">The call frequency of RESTful APIs is no more than ten queries per second. To apply for higher call frequency, contact  <a href="mailto:support@agora.io">support@agora.io</a >.</div>

## Endpoint

All requests should be sent to BaseUrl: **https://api.agora.io/dev**.

- Request: Send all parameters using the JSON format, with content type: Content-Type: application/json.
- Response: The response content is in the JSON format. The response status is defined as follows:

| Status | Description | 
| ---------------- | ---------------- | 
| 200      | The request is successful.      |
| 400      | The input is in the wrong format. |
| 401      | Unauthorized (incorrect signature). |
| 404      | Wrong API involved. |
| 429      | Too many requests. |
| 500      | Internal error of the Agora RESTful API service. |

## Project API

BaseUrl: **https://api.agora.io/dev**.

<div class="alert note">In this section, APIs about the App Certificate only apply to the primary certificate. </div>

The following chart shows how you can use Project APIs.
![](https://web-cdn.agora.io/docs-files/1583833115362)

### Creates a project (POST)

Creates an Agora project.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | POST      |
| Request URL  | BaseUrl/v1/project/ |

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `name`      | The project name.      |
| `enable_sign_key` | Determines whether to enable the primary App Certificate: <ul><li>true: Enable the primary App Certificate.</li><li>false: Do not enable the primary App Certificate.</li></ul> |

**Request sample**

```
{
  "name": "projectx",
  "enable_sign_key": true
}
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |
| `vendor_key` | The App ID of the project. |
| `sign_key` | The primary App Certificate of the project. |
| `recording_server` | The IP of the recording server. Pay attention to this field if you use the On-premise Recording SDK earlier than v1.9.0. |
| `status` | The status of the project: <ul><li>1: The project is enabled.</li><li>0: The project is disabled.</li></ul> |
| `created` | The Unix timestamp (ms) for creating the project. |

**Response sample**

```
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

### Gets all projects (GET)

Gets the information of all Agora projects.

**Basic information**

| Basic information | Description | 
| ---------------- | ---------------- | 
| Method      | GET      |
| Request URL | BaseUrl/v1/projects/ |

**Request parameter**

None.

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |
| `vendor_key` | The App ID of the project. |
| `sign_key` | The primary App Certificate of the project. |
| `recording_server` | The IP of the recording server. Pay attention to this field if you use the On-premise Recording SDK earlier than v1.9.0. |
| `status` | The status of the project: <ul><li>1: The project is enabled.</li><li>0: The project is disabled.</li></ul> |
| `created` | The Unix timestamp (ms) for creating the project. |

**Response sample**

```
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

### Gets a specified project (GET)

Gets the information of a specified project.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | GET      |
| Request URL | BaseUrl/v1/project/|

**Request parameter**

#### Query parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |

**Request sample**

```json
https://api.agora.io/dev/v1/project?id=xxxx&name=project1
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |
| `vendor_key` | The App ID of the project. |
| `sign_key` | The primary App Certificate of the project. |
| `recording_server` | The IP of the recording server. Pay attention to this field if you use the On-premise Recording SDK eariler than v1.9.0. |
| `status` | The status of the project: <ul><li>1: The project is enabled.</li><li>0: The project is disabled.</li></ul> |
| `created` | The Unix timestamp (ms) for creating the project. |

**Response sample**

```
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

### Disables/Enables a project (POST)

Disables/Enables a specified Agora project.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | POST     |
| Request URL | BaseUrl/v1/project_status/ |

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The projec ID.      |
| `status` | Determines whether you want to enable or disable the project: <ul><li>0: Enable the project.</li><li>1: Disable the project.</li></ul> |

**Request sample**

```json
{
  "id": "xxxx"
  "status": 0
}
```

**Response parameter**

- If you successfully enable or disable the project, this response contains the information of this project.

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |
| `vendor_key` | The App ID of the project. |
| `sign_key` | The primary App Certificate of the project. |
| `recording_server` | The IP of the recording server. Pay attention to this field if you use the On-premise Recording SDK earlier than v1.9.0. |
| `status` | The status of the project: <ul><li>1: The project is enabled.</li><li>0: The project is disabled.</li></ul> |
| `created` | The Unix timestamp (ms) for creating the project. |

```
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

- If the specified project does not exist,  the status code 404 is reported.

```json
status 404
{
  "error_msg": "project not exit"
}
```

### Deletes a project (DELETE)

Deletes a specified Agora project.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | DELETE      |
| Request URL | BaseUrl/v1/project/|

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |

**Request sample**

```json
{
  "id": "xxxxx"
}
```

**Response parameter**

- If you successfully delete the project, the response is as follows:

```json
{
  "success": true
}
```

- If the specified project does not exist, the status code 404 is reported.

```json
status 404
{
  "error_msg": "project not exit"
}
```

### Sets the IP of the recording server (POST)

Sets the IP of of the recording server for a specified project.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | POST      |
| Request URL | BaseUrl/v1/recording_config/|

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `recording_config` | The IP of the recording server. Pay attention to this field if you use the On-premise Recording SDK earlier than v1.9.0. |

**Request sample**

```json
{
  "id": "xxxx",
  "recording_server": "10.12.1.5:8080"
}
```

### Response parameter

- If you successfully set the IP for the recording server, the response is as follows:

```json
{
  "success": true
}
```

- If the specified project does not exist or has been disabled, the status code 404 is reported.

```json
status 404
{
  "error_msg": "project not exit"
}
```

### Enables the primary App Certificate (POST)

Enables the primary App Certificate for a specifed project.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | POST      |
| Request URL | BaseUrl/v1/signkey/|

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `enable` | Determines whether to enable the primary App Certificate of the project: <ul><li>true: Enable the primary App Certificate.</li><li>false: Do not enable the primary App Certificate.</li></ul> |

**Request sample**

```json
{
  "id": "xxxx",
  "enable": true
}
```

**Response parameter**

- If you successfully enable the primary App Certificate, the response is as follows:

```json
{
  "success": true
}
```

- If the specified project does not exist or has been disabled, the status code 404 is reported.

```json
status 404
{
  "error_msg": "project not exit"
}
```

### Resets the primary App Certificate (POST)

Resets the primary App Certificate of a specified project. If the primary App Certificate is leaked, use this method to reset it.

If the primary App Certificate has not been enabled, calling this method automatically enables it.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | POST      |
| Request URL | BaseUrl/v1/reset_signkey/|

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |

**Request sample**

```json
{
  "id": "xxxxx"
}
```

**Response parameter**

- If you successfully enable the primary App Certificate, the response is as follows:

```json
{
  "success": true
}
```

- If the specified project does not exist or has been disabled, the status code 404 is reported.

```json
status 404
{
  "error_msg": "project not exit"
}
```

## Usage API

BaseUrl: **https://api.agora.io/dev**.

### Fetch Usages (GET)

Agora provides two kinds of APIs to fetch usages: version 1 and version 3. For fetching more usage information, Agora recommends to use the API of [version 3](#UsageV3).

#### V1

<details>
	<summary><font color="#3ab7f8">Fetch Usages V1</font></summary>

The following chart shows how you can use Usage APIs.
![](https://web-cdn.agora.io/docs-files/1583833157222)
	
-  Method: GET
-  Path: BaseUrl/v1/usage/
-  Parameter: (from date & to date pattern: YYYY-MM-DD)

	```
	"from_date"=YYYY-MM-DD&to\_date=YYYY-MM-DD&projects=id1,id2,id3
	"from_date"=2015-01-01&to_date=2015-03-21&projects=id1,id2
	```

<div class="alert note"><li>It is optional to specify projects. If it is not specified, the usage is queried on all projects of this account.<li>You can query usage data within one year. Agora recommends not including the current day in your query period, because the data is continueously changing on that day.</li></div>

-  Response:

   -  Success:

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

   -  Error:

      If some of the specified projects do not exist, they will be omitted and no error will occur.

<div class="alert note">The audio duration (audio), SD video duration (sd), HD video duration (hd) and HDP video duration (hdp) parameters in this response are in minutes.</div>
	
</details>

<a name="UsageV3"></a>
#### V3
The following chart shows how you can use Usage APIs.
![](https://web-cdn.agora.io/docs-files/1583833175432)

-  Method: GET
-  Path: BaseUrl/v3/usage/
-  Parameter:

 ```
// from_date: The date to start query, using UTC time. For example, 2020-01-01.
// to_date: The date to end query, using UTC time. For example, 2020-01-31.
// project_id: The ID of a target project.
// business: The business type. You can choose the following values:
// - default: The default business type is RTC.
// - transcodeDuration: Transcoding.
// - recording: On-premise Recording.
// - cloudRecording: Cloud Recording.
// - miniapp: Miniapp.
from_date=2020-01-01&to_date=2020-01-31&project_id=id1&business=default
 ```

<div class="alert note"><li>Ensure that you fill in a valid <tt>project_id</tt>, otherwise, you cannot fetch the usage information.<li>You can query usage data within one year. Agora recommends not including the current day in your query period, because the data is continueously changing on that day.</li></div>

-   Response:

 ```json
 {
    "meta": {
        "durationAudioAll": {
            "en": "Total Audio Duration",
            "unit": "second"
        },
        "durationVideoHd": {
            "en": "HD Video Duration（including On-premise Recording）",
            "unit": "second"
        },
        "durationVideoHdp": {
            "en": "HDP Video Duration（including On-premise Recording)",
            "unit": "second"
        }
    },
    "usages": [
        {
            // The query date, using UTC time and Unix timestamp.
            "date": "2020-03-01T00:00:00.000Z",
            "usage": {
                // Total audio duration. See details in the durationAudioAll parameter of the meta object.
                "durationAudioAll": 0,
                // HD video duration, including the duration of On-premise Recording. See details in the durationVideoHd parameter of the meta object.
                "durationVideoHd": 0,
                 // HDP video duration, including the duration of On-premise Recording. See details in the durationVideoHdp parameter of the meta object.
                "durationVideoHdp": 0
            }
        }
    ]
}
 ```

## Ban Users at the Server

BaseUrl: **https://api.agora.io/dev**.

The following chart shows how you can use related APIs.
![](https://web-cdn.agora.io/docs-files/1583833187642)

<div class="alert note">The call frequency of this group of API is no more than ten queries per second.</div>

The banned user receives the corresponding callback as follows:
- Android: [`onConnectionStateChanged`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e) callback reports `CONNECTION_CHANGED_BANNED_BY_SERVER(3)`
- iOS/macOS: [`connectionChangedToState`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback reports `AgoraConnectionChangedBannedByServer(3)`
- Web: [`onclient-banned`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#on) callback
- Windows: [`onConnectionStateChanged`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback reports `CONNECTION_CHANGED_BANNED_BY_SERVER(3)`
- Electron: [`AgoraRtcEngine.on("connectionStateChanged")`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/electron/classes/agorartcengine.html#on) callback reports `3`
- Unity: [`OnConnectionStateChangedHandler`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/unity/namespaceagora__gaming__rtc.html#adae7694cb602375ccbc14be3062a230c) callback reports `CONNECTION_CHANGED_BANNED_BY_SERVER(3)`

### Creates a rule (POST)

Creates a banning rule.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | POST      |
| Request URL   | BaseUrl/v1/kicking-rule/ |

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.      |
| `cname`    | (Optional) The channel name. Do not set it as the empty string "".  |
| `uid`           | (Optional) The user ID. You can use the SDK APIs to get it. Do not set it as 0. | 
| `ip`             | (Optional) The IP address of the user that you want to kick out of the channel. Do not set it as 0.  |
| `time`        | The time duration (minute) to ban the user. The value range is [1, 1440], and the default value is 60. Agora automatically sets values between zero and one to one, and higher than 1440 to 1440. Setting it as 0 means that the banning rule does not take effect. The server sets all users that conform to the rule offline, and users can log in again to re-join the channel.  |
| `privileges` | The default field ["join_channel"]. |

The banning rule works based on the three fields: `cname`, `uid` and `ip`. See the following examples:
-   If you set `ip`, but not `cname` or `uid`, the rule is that users with this `ip` cannot join any channel in the app. 
-   If you set `cname`, but not `uid` or `ip`, the rule is that no one can join the channel specified by the `cname` field.
-   If you set `cname` and `uid`, but not `ip`, the rule is that the user with the ID cannot join the channel specified by the `cname` field.
-   If you set `uid`, but not `cname` or `ip`, the rule is that the user with the ID cannot join any channel in the app.

**Request sample**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
  "cname": "channel1",
  "uid":"",
  "ip":"",
  "time":60,
  "privileges":["join_channel"]
}
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `status`      | The status of this request.      |
| `id`              | The rule ID. If you want to update the rule, you need the rule ID to specify the rule. | 

**Response sample**

```json
{
  "status": "success",
  "id": 1953
}
```


### Gets the rule list (GET)

Gets the list of all banning rules.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | GET      |
| Request URL   | BaseUrl/v1/kicking-rule/ |

**Request parameter**

#### Query parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.      |

**Request sample**

```json
https://api.agora.io/dev/v1/kicking-rule?appid=4855xxxxxxxxxxxxxxxxxxxxxxxxeae2
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `status`      | The status of this request.      |
| `rules`        | The list of the kicking rule, which contains the following fields: <ul><li>`id`: The rule ID.</li><li>`appid`: The project App ID in Console.</li><li>`uid`: The user ID.</li><li>`opid`: The operation ID, which can be used to track operation records when teoubleshooting.</li><li>`cname`: The channel name.</li><li>`ip`: The IP address.</li><li>`ts`: The timestamp when this rule stops taking effect.</li><li>`createAt`: The timestamp when this rule is created.</li><li>`updateAt`: The timestamp when this rule is updated.</li></ul> |

**Response sample**

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

### Updates the rule time (PUT)

Updates the time duration when the rule taks effect.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- | 
| Method      | PUT      |
| Request URL  | BaseUrl/v1/kicking-rule/ |

**Request parameter**

#### Body parameter

| Parameter | Description | 
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.      |
| `id`             | The ID of the rule that you want to update. |
| `time`         | The time duration (minute) to ban the user. The value range is [1, 1440], and the default value is 60. Agora automatically sets values lower than one to one, and higher than 1440 to 1440. Setting it as 0 means that the banning rule does not take effect. The server sets all users that conform to the rule offline, and users can log in again to re-join the channel. |

**Request sample**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
  "id": 1953,
  "time": 60,
}
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `status`     | The status of this request. |
| `result`      | The result of the rule update: <ul><li>`id`: The rule ID.</li><li>`ts`: The timestamp when the rule stops taking effect.</li></ul>      |

**Response sample**

```json
{
  "status": "success",
  "result": {
    "id": 1953,
    "ts": "2018-01-09T08:45:54.545Z"
  }
}
```

### Deletes the rule (DELETE)

Deletes the banning rule.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | DELETE      |
| Request URL |  BaseUrl/v1/kicking-rule/ |

**Request parameter**

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.     |
| `id`             | The ID of the rule that you want to delete. |

**Request sample**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2",
  "id": 1953
}
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `status`      | The status of this request.     | 
| `id`             | The ID of the rule that you want to delete. |

**Response sample**

```json
{
  "status": "success",
  "id": 1953
}
```

## Online Statistics Query API

BaseUrl：**https://api.agora.io/dev**.

The following chart shows how you can use Online Statistics Query APIs.
![](https://web-cdn.agora.io/docs-files/1583833200074)

### Gets the user role (GET)

This method checks if a specified user is in a specified channel, and if yes, the role of this user in the channel.

- Broadcaster: A broadcaster can both send and receive streams.
- Audience: An audience can only receive streams.

<div class="alert note">To get the user role of all users in a specified channel, see <a href="#userList">Gets the user list in a channel (GET)</a ></div>

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | GET      |
| Request URL  | BaseUrl/v1/channel/user/property/ |

**Request parameter**

#### URL parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.      | 
| `uid`          | The user ID. |
| `cname`   | The channel name. |

**Request sample**

```
BaseUrl/v1/channel/user/property/{appid}/{uid}/{channelName}
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `success`  | The state of this request: <ul><li>true: Success.</li><li>false: Failure.</li></ul> |
| `data`  | The state of the user in the channel, which includes the following fields: <ul><li>`join`: The timestamp when the user joins the channel.</li><li>`in_channel`: Whether the user is in the channel: <ul><li>true: The user is in the channel.</li><li>false: The user is not in the channel.</li></ul><li>`role`: The role of the user in the channel. <ul><li>0: Unknown user role.</li><li>1: The user in a Communication channel.</li><li>2: The broadcaster in a Live-broadcast channel.</li><li>3: The audience in a Live-broadcast channel.</li></ul></li></ul> |

**Response sample**

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
### Gets the user list in a channel (GET)

This method gets the user list:

- In the Communication profile, this method gets the list of all users in the channel.
- In the Live-broadcast profile, this method gets the list of all broadcasters and audience in the channel.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method      | GET      |
| Request URL | BaseUrl/v1/channel/user/ |

**Request parameter**

#### URL parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.      |
| `cname`    | The channel name. |

**Request sample**

```
BaseUrl/v1/channel/user/{appid}/{channelName}
```

**Response parameter**

The response of this method differs with the channel profile:

- In the Communicatio profile, the response is as follows:

| Parameter | Description |
| ---------------- | ---------------- |
| `sucess`      | Checks the state of this request: <ul><li>true: Success.</li><li>false: Failure.</li></ul>      |
| `data` | The response data, which includes the following fields: <ul><li>`channel_exist`: Checks whether the channel exist.<ul><li>true: The channel exist.</li><li>false: The channel does not exist.</li></ul><li>`mode`: The channel profile: <ul><li>1: The Communication profile.</li><li>2: The Live-broadcast profile.</li></ul><li>`total`: The total number of users in the channel.</li><li>`users`: The ID list of all users in the channel.</li></ul> |

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

- In the Live-broadcast profile, the response is as follows:

| Parameter | Description |
| ---------------- | ---------------- |
| `sucess`      | Checks the state of this request: <ul><li>true: Success.</li><li>false: Failure.</li></ul>      |
| `data` | The data of user information, which includes the following fields: <ul><li>`channel_exist`: Checks whether the channel exist.<ul><li>true: The channel exist.</li><li>false: The channel does not exist.</li></ul><li>`mode`: The channel profile: <ul><li>1: The Communication profile.</li><li>2: The Live-broadcast profile.</li></ul><li>`broadcasters`: The ID list of all the broadcasters in the channel.</li><li>`audience`: The ID list of the first 10000 audience in the channel.</li></ul><li>`audience_total`: The total number of all the audience in the channel.</li></ul> |

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


### Gets the channel list (GET)

This method gets the channel list of a specified vendor.

**Basic information**

| Basic information | Description |
| ---------------- | ---------------- |
| Method     | GET      |
| Request URL | BaseUrl/v1/channel/appid/ |

**Request parameter**

#### Query parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `page_no`      | The starting page of the channel list. The default value is 0.      |
| `page_size`   | The number of items in a page. The default value is 100 and the greatest value is 500. |

**Request sample**

```
// Without the query parameter.
BaseUrl/v1/channel/{appid}

// With the query parameter.
BaseUrl/v1/channel/{appid}?page_no=0&page_size=100
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `success`     | The state of this request: <ul><li>true: Success.</li><li>false: Failure</li></ul>      |
| `data` | The response data, which includes the following fields: <ul><li>`channels`: The channel list: <ul><li>`channel_name`: The channel name.</li><li>`user_count`: The number of users in the channel</li></ul><li>`total_size`: The number of channels</li></ul> |

**Response sample**

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
