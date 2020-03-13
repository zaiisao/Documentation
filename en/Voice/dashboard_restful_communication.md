
---
title: Console RESTful API
description: 
platform: All Platforms
updatedAt: Fri Mar 13 2020 06:19:02 GMT+0800 (CST)
---
# Console RESTful API
## Authentication

> Before using the RESTful APIs, ensure that your account has enabled the relevant privilege of a specified project in Agora Console. Agora supports customizing different user roles and privileges. For more information, see [Management members](../../en/Voice/manage_member.md).

The RESTful API only supports HTTPS. Before sending HTTP requests, you must pass the basic Agora HTTP authentication (the `Authorization` parameter in the HTTP request head) with `api_key:api_secret`:

- `api_key`: Customer ID
- `api_secret`: Customer Certificate

You can find your Customer ID and Customer Certificate on the [RESTful API](https://dashboard.agora.io/restful) page in Console. See [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.


## EndPoint

All requests should be sent to BaseUrl: **https://api.agora.io/dev**.

- Request: Send all parameters using the JSON format, with content type: Content-Type: application/json.
- Response: The response content is in the JSON format. The response status is defined as follows:

| Status | Description | 
| ---------------- | ---------------- | 
| 200      | The request is successful.      |
| 400      | The input is in the wrong format. |
| 401      | Unauthorized (incorrect signature). |
| 404      | Wrong API involed. |
| 429      | Too many request. |
| 500      | Internal error of the Agora RESTful API service. |

## Project API

BaseUrl: **https://api.agora.io/dev**.

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
| `enable_sign_key` | Determines whether to enable the App Certificate: <ul><li>true: Enable the App Certificate.</li><li>false: Do not enable the App Certificate.</li></ul> |

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
| `sign_key` | The App Certificate of the project. |
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
| `sign_key` | The App Certificate of the project. |
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

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |

**Request sample**

```json
{
  "id": "xxxxx",
  "name": "xxxxx"
}
```

**Response parameter**

| Parameter | Description |
| ---------------- | ---------------- |
| `id`      | The project ID.      |
| `name` | The project name. |
| `vendor_key` | The App ID of the project. |
| `sign_key` | The App Certificate of the project. |
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
| `sign_key` | The App Certificate of the project. |
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

### Enables the App Certificate (POST)

Enables the App Certificate for a specifed project.

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
| `enable` | Determines whether to enable the App Certificate of the project: <ul><li>true: Enable the App Certificate.</li><li>false: Do not enable the App Certificate.</li></ul> |

**Request sample**

```json
{
  "id": "xxxx",
  "enable": true
}
```

**Response parameter**

- If you successfully enable the App Certificate, the response is as follows:

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

### Resets the App Certificate (POST)

Resets the App Certificate of a specified project. If the App Certificate is leaked, use this method to reset it.

If the App Certificate has not been enabled, calling this method automatically enables it.

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

- If you successfully enable the App Certificate, the response is as follows:

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

   It is optional to specify projects. If it is not specified, the usage is queried on all projects of this account.

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

> The *audio*, *sd*, *hd* and *hdp* parameters in this response are in minutes.
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
// - miniapp: Mini app.
from_date=2020-01-01&to_date=2020-01-31&project_id=id1&business=default
 ```

<div class="alert note">Ensure that you fill in a valid <tt>project_id</tt>, otherwise, you cannot fetch the usage information.</div>

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
- Android: [`onConnectionStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)(CONNECTION_CHANGED_BANNED_BY_SERVER)
- iOS/macOS: [`connectionChangedToState`](https://docs.agora.io/en/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)(AgoraConnectionChangedBannedByServer)
- Web: [`onclient-banned`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.client.html#on)
- Windows: [`onConnectionStateChanged`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)(CONNECTION_CHANGED_BANNED_BY_SERVER)

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
| `time`        | The time duration (minute) to ban the user. The value range is [1, 1440], and the default value is 60. Agora automatically sets values lower than one to one, and higher than 1440 to 1440. Setting it as 0 means that the banning rule does not take effect. The server sets all users that conform to the rule offline, and users can log in again to re-join the channel.  |
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
  "cname": "",
  "uid":"",
  "ip":"",
  "time":60,
  "privilege":["join_channel"]
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

#### Body parameter

| Parameter | Description |
| ---------------- | ---------------- |
| `appid`      | The project App ID in Console.      |

**Request sample**

```json
{
  "appid": "4855xxxxxxxxxxxxxxxxxxxxxxxxeae2"
}
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

<div class="alert note">The call frequency of this group of API is no more than twenty queries per second.</div>

### Get a User Role in the Channel (GET)

This method checks if a specified user is in a specified channel, and if yes, checks the role of this user in the channel.

-  Method: GET
-  Path: BaseUrl/v1/channel/user/property/
-  Parameters: appid, uid, cname

	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameter</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td>appid</td>
	<td>Mandatory, App ID in the Console</td>
	</tr>
	<tr><td>uid</td>
	<td>Mandatory, user ID which can be obtained by using the SDK</td>
	</tr>
	<tr><td>cname</td>
	<td>Mandatory, channel name</td>
	</tr>
	</tbody>
	</table>

Example: /channel/user/property/<appid\>/<uid\>/<channelName\>

-  Response:

	```
	{
		"success": true,
		"data": {
			"join": 1549073054,
			"in_channel": true,
			"role": 2
		}
	}
	```

	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameter</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td>join</td>
	<td><p>The timestamp when the user joins the channel</p>
	</td>
	<tr><td>success</td>
	<td><p>Checks the request state</p>
	<ul>
	<li>true: Request succeeded</li>
	<li>false: Request failed</li>
	</ul>
	</td>
	</tr>
	<tr><td>in_channel</td>
	<td><p>Checks if the user is in the channel</p>
	<ul>
	<li>true: The user is in the channel</li>
	<li>false: The user is not in the channel</li>
	</ul>
	</td>
	</tr>
	<tr><td>role</td>
	<td><p>Checks the role of the user in the channel</p>
	<ul>
	<li>0: Unknown role</li>
	<li>1: Communication user</li>
	<li>2: Video live broadcaster</li>
	<li>3: Live broadcast audience</li>
	</ul>
	</td>
	</tr>
	</tbody>
	</table>



### Get the User Role List in a Channel (GET)

This method checks the user role list in a specified channel.

-  Method: GET
-  Path: BaseUrl/v1/channel/user/
-  Parameters: appid, cname

	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameter</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td>appid</td>
	<td>Mandatory, App ID in the Console</td>
	</tr>
	<tr><td>cname</td>
	<td>Mandatory, channel name</td>
	</tr>
	</tbody>
	</table>

Example: /v1/channel/user/<appid\>/<channelName\>

-  Response: the responses of difference channel profiles differ:

	```
	// If it is a communication channel:
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
	
	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameter</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td>success</td>
	<td><p>Checks the request state:</p>
	<ul>
	<li>true: Request succeeded</li>
	<li>false: Request failed</li>
	</ul>
	</td>
	</tr>
	<tr><td>channel_exist</td>
	<td><p>Checks if the channel exits:</p>
	<ul>
	<li>true: Channel exists</li>
	<li>false: Channel does not exist</li>
	</ul>
	</td>
	</tr>
	<tr><td>mode</td>
	<td><p>Checks the channel mode:</p>
	<ul>
	<li>1: The communication mode</li>
	<li>2: The live broadcast mode</li>
	</ul>
	</td>
	</tr>
	<tr><td>total</td>
	<td>Total number of the users in the channel</td>
	</tr>
	<tr><td>users</td>
	<td>UIDs of all users in the channel</td>
	</tr>
	</tbody>
	</table>
	<br>

	```json
// If it is a live broadcast channel.
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
	
	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameter</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td>success</td>
	<td><p>Checks the request state:</p>
	<ul>
	<li>true: Request succeeded</li>
	<li>false: Request failed</li>
	</ul>
	</td>
	</tr>
	<tr><td>channel_exist</td>
	<td><p>Checks if the channel exits:</p>
	<ul>
	<li>true: Channel exists</li>
	<li>false: Channel does not exist</li>
	</ul>
	</td>
	</tr>
	<tr><td>mode</td>
	<td><p>Checks the channel mode:</p>
	<ul>
	<li>1: The communication mode</li>
	<li>2: The live broadcast mode</li>
	</ul>
	</td>
	</tr>
	<tr><td>broadcasters</td>
	<td>The ID list of all the broadcasters in the channel</td>
	</tr>
	<tr><td>audience</td>
	<td>The ID list of the first 10000 auience in the channel</td>
	</tr>
	<tr><td>audience_total</td>
	<td>The total number of auience in the channel</td>
	</tr>
	</tbody>
	</table>

	

### Get the Channel List (GET)

This method gets the channel list of a specified vendor.

-  Method: GET
-  Path: BaseUrl/v1/channel/appid/
-  Parameters: ?page\_no=0&page\_size=100 \(Optional\)

	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameter</strong></td>
	<td><strong>Name</strong></td>
	</tr>
	<tr><td>page_no</td>
	<td>Optional. The starting page; the default value is 0.</td>
	</tr>
	<tr><td>page_size</td>
	<td>Optional. The number of items in a page; the default value is 100 and the greatest value is 500.</td>
	</tr>
	</tbody>
	</table>

Example: /channel/<appid\>
Example with parameters: /channel/<appid\>page\_no=0&page\_size=100

-  Response:

    ```
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

	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Parameters</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td>success</td>
	<td><p>Checks the request state:</p>
	<ul>
	<li>true: Request succeeded</li>
	<li>false: Request failed</li>
	</ul>
	</td>
	</tr>
	<tr><td>channel_name</td>
	<td>Name of the specified channel</td>
	</tr>
	<tr><td>user_count</td>
	<td>Total number of users the channel</td>
	</tr>
	<tr><td>total_size</td>
	<td>Total number of the channel</td>
	</tr>
	</tbody>
	</table>


## Error Codes

See [Error Codes and Warning Codes](../../en/Voice/the_error_native.md).


