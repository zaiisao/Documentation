
---
title: 控制台 RESTful API
description: 
platform: All Platforms
updatedAt: Tue Oct 29 2019 03:59:39 GMT+0800 (CST)
---
# 控制台 RESTful API
## 1. 认证

> 在使用本文 RESTful API 提供的功能前，请确认你的账号已在控制台开通指定项目的相关权限。Agora 支持自定义用户角色和相应的项目权限，详见 [控制台角色权限说明](../../cn/Agora%20Platform/manage_member.md)。

RESTful API 仅支持 HTTPS。用户必须在 Basic HTTP 请求头部填入 `Authorization` 字段进行认证。你需要在代码中传入 Customer ID 和 Customer Certificate。

登录 [Agora 控制台](http://console.agora.io)，点击右上角账户名，进入下拉菜单 RESTful API 页面，即可获取 Customer ID 和 Customer Certificate。

> Customer ID 和 Customer Certificate 仅用于访问 RESTful API。

关于如何生成 `Authorization` 字段，详见 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

## 2. 接入点

所有请求都发送给 BaseUrl：**https://api.agora.io/dev/v1**

-   请求：参数格式必须为 JSON ，内容类型: application/json
-   响应：响应内容的格式为 JSON。以下为定义的响应状态：

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Status 200</td>
<td>请求处理成功</td>
</tr>
<tr><td>Status 400</td>
<td>输入格式错误</td>
</tr>
<tr><td>Status 401</td>
<td>未经授权的（App ID/Customer Certificate匹配错误）</td>
</tr>
<tr><td>Status 404</td>
<td>API 调用错误</td>
</tr>
</tr>
<tr><td>Status 429</td>
<td>API 调用过于频繁</td>
</tr>
<tr><td>Status 500</td>
<td>服务器内部错误</td>
</tr>
</tbody>
</table>

## 3. 项目相关的 API

BaseUrl：**https://api.agora.io/dev/v1**

下图展示了项目相关 API 的使用逻辑。
![](https://web-cdn.agora.io/docs-files/1545985558459)

### 获取所有项目 \(GET\)

-   方法：GET
-   路径：BaseUrl/projects/
-   参数：None
-   响应：

    ```
    {
      "projects":[
    
                  {
    
                    "id": 'xxxx',
                    "name": 'project1',
                    "vendor_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                    "sign_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                    "recording_server": '10.2.2.8:8080',
                    "status": 1,
                    "created": 1464165672
    
                  }
    
               ]
    }
    ```

-   状态：

    -   1: 启用
    -   0: 禁用


### 获取单个项目（GET）

-   方法：GET
-   路径：BaseUrl/project/
-   参数：

    ```
    {
      "id":'xxxx',
      "name":'xxxx'
    }
    ```

-   响应：

    ```
    {
      "projects":[
    
               {
    
                    "id": 'xxxx',
                    "name": 'project1',
                    "vendor_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                    "sign_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                    "recording_server": '10.2.2.8:8080',
                    "status": 1,
                    "created": 1464165672
    
                  }
    
               ]
    }
    ```

-   状态：

    -   1: 启用
    -   0: 禁用


### 创建项目（POST）

-   方法：POST
-   路径：BaseUrl/project/
-   参数：

    ```
    {
      "name":'projectx',
      "enable_sign_key": true
    }
    ```

-   响应：

    ```
    {
      "project":
              {
    
                 "id": 'xxxx',
                 "name": 'project1',
                 "vendor_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                 "sign_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                 "status": 1,
                 "created": 1464165672
    
              }
    }
    ```


### 禁用或启用项目（POST）

-   方法：POST
-   路径：BaseUrl/project\_status/
-   参数：

    ```
    {
      "id":'xxx',
      "status": 0
    }
    ```

-   响应：

    -   成功:

        ```
        {
          "project":
                  {
        
                   "id": 'xxxx',
                   "name": 'project1',
                   "vendor_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                   "sign_key": '4855xxxxxxxxxxxxxxxxxxxxxxxxeae2',
                   "status": 0,
                   "created": 1464165672
        
                   }
        
         }
        ```

    -   如果指定的项目不存在 \(被删除或不存在\):

        ```
        status 404
        content:
        {
        
          "error_msg": "project not exist"
        
        }
        ```


### 删除项目（DELETE）

-   方法：DELETE
-   路径：BaseUrl/project/
-   参数：

    ```
    {
      "id":'xxxx'
    }
    ```

-   响应：

    -   项目已删除：

        ```
        {
          "success": true
        }
        ```

    -   未找到项目：

        ```
        status 404
        
         {
            "error_msg": "project not exist"
         }
        ```


### 设置项目的录制项目服务器 IP（POST）

-   方法：POST
-   路径：BaseUrl/recording\_config/
-   参数：

    ```
    {
      "id":'xxxx',
      "recording_server": '10.12.1.5:8080'
    }
    ```

>  - 如果您使用的 Recording SDK 版本 <= v1.9.0，请关注 `recording_server` 字段；
>  - 如果您使用的 Recording SDK 版本 \>= v1.11.0，请忽略 `recording_server` 字段。

-   响应：

    -   成功

        ```
        {
          "success": true
        }
        ```

    -   项目未找到或已禁用：

        ```
        status 404
        
         {
           "error_msg": "project not exist"
         }
        ```


### 启用项目 App Certificate（POST）

-   方法：POST
-   路径：BaseUrl/signkey/
-   参数：

    ```
    {
      "id": `xxx`,
      "enable": true
    }
    ```

-   响应：

    -   成功

        ```
        {
        
          "success": true
        
        }
        ```

    -   项目未找到或已禁用：

        ```
        status 404
        {
        
          "error_msg": "project not exist"
        
        }
        ```


### 重置项目的 App Certificate（POST）

-   方法：POST
-   路径：BaseUrl/reset\_signkey/
-   参数：

    ```
    { "id": “xxx”} // 项目 id
    ```

-   响应：

    -   成功

        ```
        {
          "success": true
        }
        ```

    -   项目未找到或已禁用：

        ```
        status 404
          {
            "error_msg": "project not exist"
          }
        ```

> 如果该项目的 App Certificate 尚未启用，调用该方法会启用 App Certificate 。

## 4. 用量相关的 API

BaseUrl：**https://api.agora.io/dev/v1**

下图展示了用量相关 API 的使用逻辑。
![](https://web-cdn.agora.io/docs-files/1545985575734)

### 获取用量数据（GET)

-   方法：GET
-   路径：BaseUrl/usage/
-   参数 (格式为一个日期到另一个日期的YYYY-MM-DD)：

    ```
    "from_date"=YYYY-MM-DD&to_date=YYYY-MM-DD&projects=id1,id2,id3
    "from_date"=2015-01-01&to_date=2015-03-21&projects=id1,id2
    ```

    您可以指定项目，但如果不指定，系统将查询该账户下的全部项目。

-   响应：

    -   成功：

        ```
        {
          "usages":[
        
                    { "project": 'xxx',
                                "daily": [
                                      { "date": 20150101, "audio": 20, "sd": 100, "hd": 132, "hdp": 225},
                                      { "date": 20150102, "audio": 20, "sd": 100, "hd": 132, "hdp": 225},
                                  ]
                                },
        
                                { "project": 'yyy',
                                  "daily": [....]
                                }
        
                  ]
        }
        ```

    -   报错: 如果指定的项目 \(projects\) 不存在，会直接被忽略。不会报错。

> 该响应中 *audio*、*sd*、*hd* 及 *hdp* 的单位为分钟。

## 5. 服务端踢人 API

BaseUrl: **https://api.agora.io/dev/v1**

下图展示了服务器踢人相关 API 的使用逻辑。
![](https://web-cdn.agora.io/docs-files/1545985590584)

<div class="alert note">本组 API 调用频率上限为每秒 10 次。</div>

用户被踢出频道后，会收到网络连接已被服务器禁止回调：
- Android: [`onConnectionStateChanged`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a31b2974a574ec45e62bb768e17d1f49e)(CONNECTION_CHANGED_BANNED_BY_SERVER)
- iOS/macOS: [`connectionChangedToState`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)(AgoraConnectionChangedBannedByServer)
- Web: [`onclient-banned`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#on)
- Windows: [`onConnectionStateChanged`](https://docs.agora.io/cn/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)(CONNECTION_CHANGED_BANNED_BY_SERVER)

### 创建规则 (POST)

该方法创建服务端踢人规则。

-   方法：POST
-   路径：BaseUrl/kicking-rule/
-   参数:

    ```
    {
				"appid":"",   // 控制台中项目的 App ID，必填
				"cname":"",   // channel name 频道名称，非必填，可以不传，但不能传 cname:""
				"uid":"",     // uid，SDK 可以获取到，非必填，可以不传，但不能传 uid:0
				"ip":"",      // IP地址需要封的用户 IP，非必填，可以不传，但不能传 ip:0
				"time": 60    // 封人时间，单位是分钟，最大 1440 分钟，最小一分钟。如果大于 1440 分钟，会被处理成 1440 分钟，如果不传默认为 1 小时。非必填。比如：time:60
				"privileges":["join_channel"]  // 默认参数
     }
    ```

> - 如果 time 字段设置为 0，则表示不封禁，服务端会对频道内符合设定规则的用户进行下线一次的操作。用户可以重新登录进入频道。 
> - 踢人规则通过 cname、uid 和 ip 三个字段组合起来过滤实现，规则如下：
>   * 如果填写 ip，不填写 cname 或 uid，则该 ip 无法登录 App 中的任何频道
>   * 如果填写 cname，不填写 uid 或 ip，则任何人都无法登录 App 中该 cname 对应的频道
>   * 如果填写 cname 和 uid，不填写 ip，则该 uid 无法登录 App 中该 cname 对应的频道

-   响应：

    ```
    {
        "status": "success",  // 请求状态
        "id": 1953            // 规则id，如：更新规则是需要带上此id
    }
    ```


### 获取规则列表 (GET)

该方法获取服务端的踢人规则列表。

-   方法：GET
-   路径：BaseUrl/kicking-rule/
-   参数：

    ```
    {
       "appid":""    // 控制台中项目的 App ID，必填
     }
    ```

-   响应：

    ```
    {
        "status": "success",                                    // 请求状态
        "rules": [
            {
                "id": 1953,                                     // 规则id，如：更新规则是需要带上此id
                "appid": ""                                     // 对应控制台中项目的appID
                "uid": 1,                                       // uid，客户端中看到
                "opid": 1406,                                   // 操作id，用于核对操作记录（查问题时使用）
                "cname": "11",                                  // 频道名
                "ip": null,                                     // ip地址
                "ts": "2018-01-09T07:23:06.000Z",               // 规则生效截止时间
                "createAt": "2018-01-09T06:23:06.000Z",         // 规则创建时间
                "updateAt": "2018-01-09T14:23:06.000Z"          // 规则更新时间
            }
        ]
    }
    ```


### 更新规则时间 (PUT)

该方法更新服务端踢人的生效时间。

-   方法：PUT
-   路径：BaseUrl/kicking-rule/
-   参数：

    ```
    {
             "appid":"",   //控制台中项目的 App ID，必填
             "id":"",      // 获取规则列表的规则 ID，必填
             "time":""     // 需要更新的封人的时间，必填
    }
    ```

-   响应：

    ```
    {
        "result": {
            "id": 1953,                         // 规则 ID，
            "ts": "2018-01-09T08:45:54.545Z"    // 规则生效截止时间
        },
        "status": "success"                     // 请求状态
    }
    ```


### 删除规则 (DELETE)

该方法删除服务端踢人规则。

-   方法：DELETE
-   路径：BaseUrl/kicking-rule/
-   参数：

    ```
    {
        "appid":"",   //控制台中项目的 App ID，必填
        "id":""       // 获取规则列表的规则 ID，必填
    }
    ```

-   响应：

    ```
    {
        "status": "success",  // 请求状态
        "id": 1953            // 规则 ID，如：更新规则是需要带上此 ID
    }
    ```


## 6. 查询在线频道信息 API

BaseUrl：**https://api.agora.io/dev/v1/**

下图展示了查询频道信息相关 API 的使用逻辑。

![](https://web-cdn.agora.io/docs-files/1545985608224)

<div class="alert note">本组 API 调用频率上限为每秒 20 次。</div>


### 查询某个用户在指定频道中的状态 (GET)

该方法查询某个用户是否在指定频道中，如果是，则给出用户在该频道中的角色等状态。

-   方法：GET
-   路径：BaseUrl/channel/user/property/
-   参数: appid, uid, cname

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appid</td>
<td>必填，控制台中项目的 appID</td>
</tr>
<tr><td>uid</td>
<td>必填，用户 ID，可以通过 SDK 获取到</td>
</tr>
<tr><td>cname</td>
<td>必填，channel name，频道名称</td>
</tr>
</tbody>
</table>

如：/channel/user/property/<appid\>/<uid\>/<channelName\>

-   响应:

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
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>join</td>
<td><p>该用户加入频道的时间戳</p>
</td></tr>
<td>success</td>
<td><p>查询请求状态</p>
<ul>
<li>true：请求成功</li>
<li>false：请求不成功</li>
</ul>
</td>
</tr>
<tr><td>in_channel</td>
<td><p>查询用户是否在频道内</p>
<ul>
<li>true：用户在频道内</li>
<li>false：用户不在频道内</li>
</ul>
</td>
</tr>
<tr><td>role</td>
<td><p>查询用户在频道内的角色</p>
<ul>
<li>0：未知</li>
<li>1：用户角色为通信用户</li>
<li>2：用户角色为直播模式视频主播</li>
<li>3：用户角色为主播模式观众</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 获取频道内用户列表 (GET)

该方法获取指定频道内的用户列表。如果在通信模式下，则返回频道内的用户列表；如果在直播模式下，则分别返回主播列表和观众列表。

-   方法：GET
-   路径：BaseUrl/channel/user/
-   参数: appid, cname

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appid</td>
<td>必填，控制台中项目的 appID</td>
</tr>
<tr><td>cname</td>
<td>必填，channel name，频道名称</td>
</tr>
</tbody>
</table>

如：/channel/user/<appid\>/<channelName\>

-   响应：不同的频道模式下，该方法返回的响应内容不同。

	```json
	// 如果是通信频道
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
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>success</td>
<td><p>查询请求状态</p>
<ul>
<li>true：请求成功</li>
<li>false：请求不成功</li>
</ul>
</td>
</tr>
<tr><td>channel_exsit</td>
<td><p>查询频道是否存在：</p>
<ul>
<li>true：频道存在</li>
<li>false：频道不存在</li>
</ul>
</td>
</tr>
<tr><td>mode</td>
<td><p>查询频道模式：</p>
<ul>
<li>1：频道为通信模式</li>
<li>2：频道为直播模式</li>
</ul>
</td>
</tr>
<tr><td>total</td>
<td>频道内的用户总人数</td>
</tr>
<tr><td>users</td>
<td>频道内所有用户的 uid</td>
</tr>
</tbody>
</table>
		
	```json
	// 如果是直播频道
	{
			"success": true,
			"data": {
					"channel_exist": true,
					"mode": 2,
					"broadcaster": [<uid>],
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
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>success</td>
<td><p>查询请求状态</p>
<ul>
<li>true：请求成功</li>
<li>false：请求不成功</li>
</ul>
</td>
</tr>
<tr><td>channel_exsit</td>
<td><p>查询频道是否存在：</p>
<ul>
<li>true：频道存在</li>
<li>false：频道不存在</li>
</ul>
</td>
</tr>
<tr><td>mode</td>
<td><p>查询频道模式：</p>
<ul>
<li>1：频道为通信模式</li>
<li>2：频道为直播模式</li>
</ul>
</td>
</tr>
<tr><td>broadcaster</td>
<td>频道内所有主播的 uid 列表</td>
</tr>
<tr><td>audience</td>
<td>频道内观众的 uid 列表，最多包含前 10000 名观众</td>
</tr>
<tr><td>audience_total</td>
<td>频道内的观众总人数</td>
</tr>
</tbody>
</table>


### 分页查询厂商频道列表 (GET)

该方法查询指定厂商的频道列表。


-   方法：GET
-   路径：BaseUrl/channel/appid/
-   参数：?page\_no=0&page\_size=100

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>page_no</td>
<td>选填，起始页码，默认值为 0</td>
</tr>
<tr><td>page_size</td>
<td>选填，每页条数，默认值为 100，最大值为 500</td>
</tr>
</tbody>
</table>

如: /channel/<appid\>

带参数: /channel/<appid\>?page\_no=0&page\_size=100

-   响应:

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
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>success</td>
<td><p>查询请求状态</p>
<ul>
<li>true：请求成功</li>
<li>false：请求不成功</li>
</ul>
</td>
</tr>
<tr><td>channel_name</td>
<td>频道名</td>
</tr>
<tr><td>user_count</td>
<td>频道内用户数量</td>
</tr>
<tr><td>total_size</td>
<td>频道总数</td>
</tr>
</tbody>
</table>


## 7. 错误代码和警告代码

详见 [错误代码和警告代码](../../cn/Agora%20Platform/the_error_native.md)。


