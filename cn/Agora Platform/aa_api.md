
---
title: 水晶球 RESTful API (Beta)
description: AA rest api reference 
platform: All Platforms
updatedAt: Thu Oct 29 2020 09:29:29 GMT+0800 (CST)
---
# 水晶球 RESTful API (Beta)
水晶球现在提供 RESTful API，可以让你直接通过网络请求获取水晶球里的数据，在自己的网页或应用中灵活使用。

在阅读本文之前，我们推荐你先在[控制台](https://console.agora.io)中试用水晶球的界面，以便对各项参数和数据指标有更直观的了解，详情可参考[水晶球](../../cn/Agora%20Platform/aa_guide.md)。

通过 RESTful API，你可以发送请求获取以下数据：

- [实时数据](#live)
	- 指定指标每分钟的统计数据
- [大频道](#big_channel)
 - 大频道中体验异常的用户数
 - 尝试加入大频道的用户数
 - 大频道中用户对通话的评分
 - 大频道的在线用户数
- [自动诊断](#auto_diagnosis)
 - 异常诊断的详情数据
- [数据洞察](#insight)
  - 指定时间段的用量指标
- [通话调查](#search)
  - 符合指定条件的通话及基本信息
  - 某个通话的详细质量指标

当通话质量出现问题时，你还可以发送请求向 Agora [报告问题](#report)。

<div class="alert note">水晶球 RESTful API 目前处于 BETA 阶段，不对外开放。如需开通服务，请联系 <a href="mailto:sales@agora.io">sales@agora.io</a >。</div>

## 认证

使用 RESTful API 前，你需要通过 [HTTP 基本认证](https://docs.agora.io/cn/faq/restful_authentication)。

## 数据格式

所有的请求都发送给域名：`api.agora.io`。

- 请求：请求直接在 URL 中使用查询字符串参数（query String）
- 响应：响应内容的格式为 JSON

## <a name="live"></a>实时数据

通过实时数据 API，你可以获取指定指标在某一时间段内每分钟的统计数据。

### API 限制

实时数据 RESTful API 有以下限制：

| 接入点                        | 请求次数                  | 数据延迟       | 响应内容                   | 可查询时间范围 |
| :---------------------------- | :------------------------ | :------------- | :------------------------- | :------------- |
| /beta/live/scale/by_time      | 每分钟 1 次，每天 1000 次 | 60 秒 ~ 120 秒 | N/A                        | 最近 1 小时    |
| /beta/live/experience/by_time | 每分钟 1 次，每天 1000 次 | 60 秒 ~ 120 秒 | N/A                        | 最近 1 小时    |
| /beta/live/net/by_time        | 每分钟 1 次，每天 1000 次 | 60 秒 ~ 120 秒 | N/A                        | 最近 1 小时    |

> 数据延迟是指从一个通话开始到能获取该通话的数据需要的时间。

### **实时规模 API**

获取规模相关指标在某一时间段内每分钟的统计数据。

- 方法：GET
- 接入点：/beta/live/scale/by_time

#### 请求参数

实时规模 API 需要在 URL 中传入以下参数：

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。                     |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |
| `metric`     | String | 待查询指标。如果要查询多个指标，需使用逗号分隔，如 `pcu,pcc`。详情参考下表。 |

#### 指标列表

| 指标含义 | 指标名称 | 指标说明                                                     |
| :------- | :------- | :----------------------------------------------------------- |
| 用户数   | `pcu`      | 当前用户数。同用户 ID不同频道，记为多人。                     |
| 频道数   | `pcc`      | 当前频道数。从有用户进入频道到所有用户退出，记为一个通话频道。 |

#### HTTP 请求示例

```http
GET /beta/live/scale/by_time?start_ts=1548665345&end_ts=1548670821&appid=axxxxxxxxxxxxxxxxxxxx&metric=pcu,pcc HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "pcc": 42,
            "pcu": 102,
            "ts": 1548665578
        },
        ......
    ]
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。
- `message`: String 类型，错误消息。
- `data`: JSONArray 类型，指定指标每分钟的统计结果。
  - `ts`: Number 类型，Unix 时间戳 （秒）。
  - `pcu`: Number 类型，用户数。
  - `pcc`: Number 类型，频道数。

### 实时体验 API

获取体验相关指标在某一时间段内每分钟的统计数据。

- 方法：GET
- 接入点：/beta/live/experience/by_time

#### 请求参数

实时体验 API 需要在 URL 中传入以下参数：

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。                     |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |
| `metric`     | String | 待查询指标。如果要查询多个指标，需使用逗号分隔。详情参考下表。 |

#### 指标列表

| 指标含义      | 指标名称             | 指标说明                                                     |
| :------------ | :------------------- | :----------------------------------------------------------- |
| 加入成功率    | `joinSuccessRate`      | 加入频道成功人数 / 尝试加入频道人数                          |
| 5秒加入成功率 | `joinSuccess5SecsRate` | 5秒内加入频道成功人数 / 尝试加入频道人数                     |
| 视频卡顿率    | `videoFreezeRate`      | 视频卡顿时长 / 总视频时长。视频卡顿达到 600 ms，即被计入卡顿时长。 |
| 音频卡顿率    | `audioFreezeRate`      | 音频卡顿时长 / 总音频时长。音频卡顿达到 200 ms，即被计入卡顿时长。 |



#### HTTP 请求示例

```http
GET /beta/live/experience/by_time?start_ts=1548665345&end_ts=1548670821&appid=axxxxxxxxxxxxxxxxxxxx&metric=joinSuccessRate HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "joinSuccessRate": 42,
            "ts": 1548666023
        },
        ......
    ]
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。
- `message`: String 类型，错误消息。
- `data`: JSONArray 类型，指定指标每分钟的统计结果。
  - `ts`: Number 类型，Unix 时间戳 （秒）。
  - `joinSuccessRate`: Number 类型，加入频道的成功率 （%）。

### 实时网络 API

获取网络相关的指标在某一时间段内每分钟的统计数据。

- 方法：GET
- 接入点：/beta/live/net/by_time

#### 请求参数

实时网络 API 需要在 URL 中传入以下参数：

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。                     |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |
| `metric`     | String | 待查询指标。如果要查询多个指标，需使用逗号分隔。详情参考下表。 |

#### 指标列表

| 指标含义             | 指标名称                        | 指标说明                                                     |
| :------------------- | :------------------------------ | :----------------------------------------------------------- |
| 上行视频优质传输率   | `videoUpstreamExcellentTransRate` | 从发送端到 Agora SD-RTN 的视频优质传输率。视频优质传输率，指视频传输过程中丢包率小于等于 5% 的传输比例。 |
| 上行音频优质传输率   | `audioUpstreamExcellentTransRate` | 从发送端到 Agora SD-RTN 的音频优质传输率。音频优质传输率，指音频传输过程中丢包率小于等于 5% 的传输比例。 |
| 端到端视频优质传输率 | `videoEnd2EndExcellentTransRate`  | 从发送端到接收端的音频优质传输率。                           |
| 端到端音频优质传输率 | `audioEnd2EndExcellentTransRate`  | 从发送端到接收端的视频优质传输率。                           |



#### HTTP 请求示例

```http
GET /beta/live/net/by_time?start_ts=1548665345&end_ts=1548670821&appid=axxxxxxxxxxxxxxxxxxxx&metric=videoUpstreamExcellentTransRate HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "videoUpstreamExcellentTransRate": 42,
            "ts": 1548667235
        },
        ......
    ]
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。
- `message`: String 类型，错误消息。
- `data`: JSONArray 类型，指定指标每分钟的统计结果。
  - `ts`: Number 类型，Unix 时间戳 （秒）。
  - `videoUpstreamExcellentTransRate`: Number 类型，上行视频优质传输率 （%）。

## <a name="big_channel"></a>大频道

通过大频道 RESTful API，你可以获取以下数据：

- 大频道中体验异常的用户数。体验异常包含视频卡顿、加入大频道慢等。
- 尝试加入大频道的用户数。
- 大频道中用户对通话的评分。
- 大频道的在线用户数。

<div class="alert note"><li>使用大频道 RESTful API 前，你需要联系 <a href="mailto:sales@agora.io">sales@agora.io</a > 开通大频道功能。</li><li>大频道是指在线人数大于 50 人的频道，你可以在<a href="../../cn/Agora%20Platform/aa_big_channel_config.md">大频道配置</a >页面配置大频道。</li></div>

### API 限制

大频道 RESTful API 有以下限制：

- 最大请求次数：每分钟 1 次，每天 1,000 次
- 数据延迟：60 秒 - 180 秒
- 可查询时间范围：最近 1 小时

<div class="alert info">数据延迟是指从一个通话开始到能获取该通话的数据需要的时间。</div>

### <a name="video_freeze_in_all_channels"></a>获取所有大频道的视频卡顿用户数

获取指定时间范围内所有大频道的视频卡顿用户数。

- 方法: GET
- 接入点: /beta/live/quality/videofreeze/user_count

<div class="alert info">如需获取指定时间范围内指定大频道的视频卡顿用户数，查看<a href="#video_freeze_in_a_specified_channel">获取指定大频道的视频卡顿用户数</a >。</div>

#### 请求参数

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点 ，Unix 时间戳 （秒）。<p>**Note**</p><p>仅支持查询近一小时的数据。</p> |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#appid)。 |

请求示例

```http
GET /beta/live/quality/videofreeze/user_count?start_ts=1577836820&end_ts=1577837820&appid=xxxxxyyyyy HTTP/1.1 
Host: api.agora.io Accept: application/json 
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应参数

| 参数      | 类型      | 描述                                                         |
| :-------- | :-------- | :----------------------------------------------------------- |
| `code`      | Number    | [状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。 |
| `message` | String    | 方法是否调用成功：<li>`success`：调用成功。</li><li>其他：调用失败，及失败原因。</li> |
| `data`    | JSONArray | 指定指标每分钟的统计结果，包含以下参数：<li>`value`：Number 类型，视频卡顿用户数。</li><li>`ts`：Number 类型，Unix 时间戳 （秒）。</li><p>**Note**</p><p>不足一分钟的数据将不会报告。</p> |

响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "value": 22,
            "ts": 1577836880
        },
        {
            "value": 18,
            "ts": 1577836940
        },
        ...
    ]
}
```

### 获取加入大频道慢的用户数

获取指定时间范围内所有大频道中加入频道慢的用户数。

- 方法: GET
- 接入点: /beta/live/quality/slowjoining/user_count

#### 请求参数

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。<p>**Note**</p><p>仅支持查询近一小时的数据。</p> |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#appid)。                                      |

请求示例

```http
GET /beta/live/quality/slowjoining/user_count?start_ts=1577836820&end_ts=1577837820&appid=xxxxxyyyyy
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应参数

| 参数      | 类型      | 描述                                                         |
| :-------- | :-------- | :----------------------------------------------------------- |
| `code`      | Number    | [状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。                                                     |
| `message` | String    | 方法是否调用成功：<li>`success`：调用成功。</li><li>其他：调用失败，及失败原因。</li> |
| `data`    | JSONArray | 指定指标每分钟的统计结果，包含以下参数：<li>`value`：Number 类型，加入大频道慢的用户数。</li><li>`ts`：Number 类型，Unix 时间戳 （秒）。</li><p>**Note**</p><p>不足一分钟的数据将不会报告。</p> |


响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "value": 22,
            "ts": 1577836880
        },
        {
            "value": 18,
            "ts": 1577836940
        },
        ...
    ]
}
```

### 获取尝试加入大频道的用户数

获取指定时间范围内尝试加入大频道的用户数。

- 方法: GET
- 接入点: /beta/live/quality/joiningtrial/user_count

#### 请求参数

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。<p>**Note**</p><p>仅支持查询近一小时的数据。</p> |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#appid)。                                      |

请求示例

```http
GET /beta/live/quality/joiningtrial/user_count?start_ts=1577836820&end_ts=1577837820&appid=xxxxxyyyyy
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应参数

| 参数      | 类型      | 描述                                                         |
| :-------- | :-------- | :----------------------------------------------------------- |
| `code`      | Number    | [状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。                                                     |
| `message` | String    | 方法是否调用成功：<li>`success`：调用成功。</li><li>其他：调用失败，及失败原因。</li> |
| `data`    | JSONArray | 指定指标每分钟的统计结果，包含以下参数：<li>`value`：Number 类型，尝试加入大频道的用户数。</li><li>`ts`：Number 类型，Unix 时间戳 （秒）。</li><p>**Note**</p><p>不足一分钟的数据将不会报告。</p> |


响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "value": 330,
            "ts": 1577836880
        },
        {
            "value": 450,
            "ts": 1577836940
        },
        ...
    ]
}
```

###  获取用户对通话的评分

获取指定时间范围内指定大频道中用户对通话的评分。

- 方法: GET
- 接入点: /beta/bigchannel/live/quality/score

<div class="alert note">使用该方法前，你需要调用 Agora RTC SDK 的 <tt>rate</tt> 方法实现评分功能。详见<a href="https://docs.agora.io/cn/Interactive%20Broadcast/rate_call_android?platform=Android">通话后评分</a >。</div>

#### 请求参数

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。<p>**Note**</p><p>仅支持查询近一小时的数据。</p> |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#appid)。                                      |
| `cname`    | String |大频道名称。                                         |

请求示例

```http
GET /beta/bigchannel/live/quality/score?start_ts=1577836820&end_ts=1577837820&appid=xxxxxyyyyy&cname=test-cname
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应参数

| 参数      | 类型      | 描述                                                         |
| :-------- | :-------- | :----------------------------------------------------------- |
| `code`      | Number    | [状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。                                                     |
| `message` | String    | 方法是否调用成功：<li>`success`：调用成功。</li><li>其他：调用失败，及失败原因。</li> |
| `data`    | JSONArray | 指定指标每分钟的统计结果，包含以下参数：<li>`value`：Number 类型，所有用户对通话评分的平均值。</li><li>`ts`：Number 类型，Unix 时间戳 （秒）。</li><p>**Note**</p><p>不足一分钟的数据将不会报告。</p> |


响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "value": 5.0,
            "ts": 1577836880
        },
        {
            "value": 4.11,
            "ts": 1577836940
        },
        ...
    ]
}
```

### <a name="video_freeze_in_a_specified_channel"></a>获取指定大频道的视频卡顿用户数

获取指定时间范围内指定大频道的视频卡顿用户数。

- 方法: GET
- 接入点: /beta/bigchannel/live/quality/videofreeze/user_count

<div class="alert info">如需获取指定时间范围内的视频卡顿用户数，查看<a href="#video_freeze_in_all_channels">获取指定时间的视频卡顿用户数</a >。</div>

#### 请求参数

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。<p>**Note**</p><p>仅支持查询近一小时的数据。</p> |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#appid)。                                      |
| `cname`    | String |大频道名称。                                         |

请求示例

```http
GET /beta/bigchannel/live/quality/videofreeze/user_count?start_ts=1577836820&end_ts=1577837820&appid=xxxxxyyyyy&cname=test-cname
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应参数

| 参数      | 类型      | 描述                                                         |
| :-------- | :-------- | :----------------------------------------------------------- |
| `code`      | Number    | [状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。                                                     |
| `message` | String    | 方法是否调用成功：<li>`success`：调用成功。</li><li>其他：调用失败，及失败原因。</li> |
| `data`    | JSONArray | 指定指标每分钟的统计结果，包含以下参数：<li>`value`：Number 类型，视频卡顿用户数。</li><li>`ts`：Number 类型，Unix 时间戳 （秒）。</li><p>**Note**</p><p>不足一分钟的数据将不会报告。</p> |


响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "value": 4,
            "ts": 1577836880
        },
        {
            "value": 5,
            "ts": 1577836940
        },
        ...
    ]
}
```

###  获取指定大频道的在线用户数

获取指定时间范围内指定大频道的在线用户数。

- 方法: GET
- 接入点: /beta/bigchannel/live/scale/user_count

#### 请求参数

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。<p>**Note**</p><p>仅支持查询近一小时的数据。</p> |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#appid)。                                      |
| `cname`    | String |大频道名称。                                         |

请求示例

```http
GET /beta/bigchannel/live/scale/user_count?start_ts=1577836820&end_ts=1577837820&appid=xxxxxyyyyy&cname=test-cname
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应参数

| 参数      | 类型      | 描述                                                         |
| :-------- | :-------- | :----------------------------------------------------------- |
| `code`      | Number    | [状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。                                                     |
| `message` | String    | 方法是否调用成功：<li>`success`：调用成功。</li><li>其他：调用失败，及失败原因。</li> |
| `data`    | JSONArray | 指定指标每分钟的统计结果，包含以下参数：<li>`value`：Number 类型，大频道的在线用户数。</li><li>`ts`：Number 类型，Unix 时间戳 （秒）。</li><p>**Note**</p><p>不足一分钟的数据将不会报告。</p> |


响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "value": 160,
            "ts": 1577836880
        },
        {
            "value": 175,
            "ts": 1577836940
        },
        ...
    ]
}
```

## <a name="auto_diagnosis"></a>自动诊断

自动诊断 RESTful API 可以查询通话的异常数据。

### API 限制

自动诊断 RESTful API 有以下限制：

| 接入点                       | 请求次数                | 数据延迟 | 响应内容                | 可查询时间范围 |
| ---------------------------- | ----------------------- | -------- | ----------------------- | -------------- |
| /beta/live/diagnosis          | 每分钟 1 次，每天 1000 次 | 60 秒 ~ 120 秒 | 最多返回 10 条异常诊断信息 | 最近 1 小时    |

### 获取异常诊断数据

获取指定时间段内异常诊断的内容及根因。

- 方法：GET
- 接入点：/beta/live/diagnosis

#### 请求参数

需要在 URL 中传入以下参数：

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。                     |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |

#### HTTP 请求示例

```http
GET /beta/live/diagnosis?start_ts=1548665345&end_ts=1548670821&appid=axxxxxxxxxxxxxxxxxxxx HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

####  响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "user": "83817682",
            "exp_id": 4,
            "cname": "C148474015",
            "cause_tags": [
                {
                    "factor_id": 18,
                    "host_user": "44361248"
                },
                {
                    "factor_id": 23,
                    "host_user": ""
                },
                {
                    "factor_id": 18,
                    "host_user": ""
                }
            ],
            "ts": 1548668575
        },
        ......
       ]
}
```
- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](https://docs-preview.agoralab.co/cn/Agora%20Platform/aa_api?platform=All%20Platforms#code)。
- `message`: String 类型，错误消息。
- `data`: JSONArray 类型，异常诊断每分钟的统计数据。
  - `ts`: Number 类型，Unix 时间戳（秒）。
  - `user`: String 类型，出现异常的用户 ID。
  - `exp_id`: Number 类型，体验 ID，代表出现何种异常。具体参考文末的[体验 ID 映射表](#exp_id)。
  - `cname`: String 类型，出现异常的频道名。
  - `cause_tags`: JSONArray 类型，根因标签。`cause_tags` 最多包含 3 个根因标签。
    - `factor_id`: Number 类型，根因 ID，异常诊断的根因。具体参考文末的[根因 ID 映射表](#factor_id)。
    - `host_user`: String 类型。如果根因来自远端用户，则 `host_user` 为远端用户的用户 ID。如果根因来自自身，则 `host_user` 为空字符串。
    
## <a name="insight"></a>数据洞察

数据洞察可以查询指定时间范围内的用量数据。

### API 限制

数据洞察 RESTful API 有以下限制：

| 接入点                      | 请求次数                | 可查询时间范围 |
| :-------------------------- | :---------------------- | :------------- |
| /beta/insight/usage/by_time | 每分钟 1 次，每天 10 次 | 最近 7 天      |

<div class="alert note">数据洞察会在每天上午 8 点（UTC）完成前一天的数据统计，因此在该时间之前可能无法查询到前一天的数据。如果你需要更实时地查看数据，可以使用<a href="#live">实时数据</a> RESTful API。</div>

### 查询频道和用户数

获取指定时间范围内每天的通话人数、人次和频道数。

- 方法：GET
- 接入点：/beta/insight/usage/by_time

#### 请求参数

该 API 需要在 URL 中传入以下参数。

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| `start_ts` | Number | 查询的时间范围起点，Unix 时间戳 （秒）。                     |
| `end_ts`   | Number | 查询的时间范围终点，Unix 时间戳 （秒）。                     |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |
| `metric`   | String | 要查询的指标：<li>userCount：通话人数，不同频道中的相同用户 ID计为多人。</li><li>sessionCount：通话人次，用户每次加入频道计为一个通话人次。</li><li>channelCount：频道数，从有用户加入频道到所有用户离开频道计为一个通话频道。</li>支持同时查询多个指标，用逗号分隔，如 `userCount,sessionCount` |

<div class="alert note">数据洞察以天为单位统计数据，请确保指定的查询时间范围至少为一天，否则返回的数据会为空。</div>

#### HTTP 请求示例

```http
GET /beta/insight/usage/by_time?start_ts=1570579200&end_ts=1570838400&appid=axxxxxxxxxxxxxxxxxxxx&metric=userCount HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应示例

```json
{
    "code": 200,
    "message": "success",
    "data": [
        {
            "userCount": 42,
            "ts": 1570752000
        },
        ......
    ]
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](#code)。
- `message`: String 类型，错误消息。
- `data`: JSONArray 类型，由 Unix 时间戳和指标数据值组成的数组。查询的数据以天为单位计算，例如查询过去 7 天的通话人数，该数组会包含 7 组数据，每组数据包含当天 UTC 时间零点的 Unix 时间戳和通话人数。
  - `ts`: Unix 时间戳 （秒）。
  - `userCount`: 请求的指标名称，通话人数。

## <a name="search"></a>通话调查

通话调查可以搜索出现质量问题的通话并且查询具体的质量指标。

### <a name="limit"></a>API 限制

通话调查 RESTful API 有以下限制：

| 接入点                       | 请求次数                | 数据延迟 | 响应内容                | 可查询时间范围 |
| ---------------------------- | ----------------------- | -------- | ----------------------- | -------------- |
| /beta/analytics/call/lists   | 每秒 3 次，每天 1000 次 | 20 秒    | 最多返回 10 个通话      | 最近 3 天      |
| /beta/analytics/call/details | 每秒 1 次，每天 1000 次 | 150 秒   | 最多返回 3 小时通话数据 | 最近 3 天      |

> 数据延迟是指从一个通话开始到能获取该通话的数据需要的时间。

### 获取通话列表

获取符合指定条件的通话及其基本信息。

- 方法：GET
- 接入点：/beta/analytics/call/lists

#### 请求参数

该 API 需要在 URL 中传入以下参数指定搜索通话的条件。

| 参数       | 类型   | 描述                                                         |
| ---------- | ------ | ------------------------------------------------------------ |
| `start_ts` | Number | 搜索的时间范围起点。Unix 时间戳 （秒）。                     |
| `end_ts`   | Number | 搜索的时间范围终点。Unix 时间戳（秒）。                      |
| `cname`    | String | （可选）频道名称。                                           |
| `appid`    | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |

#### HTTP 请求示例

```http
GET /beta/analytics/call/lists?start_ts=1550024508&end_ts=1550025508&appid=xxxxxxxxxxxxxxxxxxxx HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应示例

```json
{
"code": 200,
"message": "",
"has_more": false,
"call_lists": 
[
  {
  "call_id": "cxxxxxxxxxxxxxxxxxxxx",
  "cname":   "cname1",
  "created_ts": 1547448383,
  "destroyed_ts": 1547448483,
  "finished": true
  }
]
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](#code)。
- `message`: String 类型，错误消息。
- `has_more`: Boolean 类型，是否还有更多通话未列出。如果为 `true`，表示还有更多符合查询条件的通话没有在 `call_lists` 中列出。如果你要查找的通话未列出，请修改查询条件重新发送请求。
- `call_lists`: JSONArray 类型,，返回的通话。默认返回最近的 10 个通话，按通话开始时间降序排列。每个通话中包含以下信息：
  - `call_id`: String 类型，通话的唯一标识。
  - `cname`: String 类型，频道名称。
  - `created_ts`: Number 类型，通话开始的时间。Unix 时间戳（秒）。
  - `destroyed_ts`: Number 类型，通话结束的时间。Unix 时间戳（秒）。
  - `finished`: Boolean 类型，通话是否已结束。

### 获取通话指标

获取某个特定通话的具体质量指标数据。

- 方法：GET
- 接入点：/beta/analytics/call/details

#### 请求参数

该 API 需要在 URL 中传入以下参数指定要查看的通话。

| 参数                  | 类型    | 描述                                                         |
| --------------------- | ------- | ------------------------------------------------------------ |
| `start_ts`            | Number  | 通话开始的时间。Unix 时间戳（秒）。                          |
| `end_ts`              | Number  | 通话结束的时间。Unix 时间戳（秒）。                          |
| `appid`               | String  | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。 |
| `call_id`             | String  | 通话的唯一标识。                                             |
| `exclude_server_user` | Boolean | （可选）是否排除 Linux 用户，默认为 `true` 。                |
| `exclude_metrics_info` | Boolean | （可选）是否排除具体指标数据，默认为 `false`，不排除具体指标数据。 如果设为 `true`，响应中将不会包含 `metrics` 结构体。  |

#### HTTP 请求示例

```http
GET /beta/analytics/call/details?start_ts=1548665345&end_ts=1548670821&appid=axxxxxxxxxxxxxxxxxxxx&call_id=cxxxxxxxxxxxxxxxxxxxx HTTP/1.1
Host: api.agora.io
Accept: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
```

#### 响应示例

```json
{
    "code": 200,
    "message": "",
    "call_id": "00057b6093a5d3e8b1fc67aa",
    "call_info": [
        {
            "sid": "EDB224CCF4FB4F99815C24302BDF3301",
            "cname": "15056678066620",
            "uid": 630985881,
            "network": "Wi-Fi",
            "platform": "Android",
            "speaker": true,
            "sdk_version": "2.2.0.07_14",
            "device_type": "Android (6.0)",
            "join_ts": 1548670251,
            "leave_ts": 1548670815,
            "finished": true
        },
		......
		],
    "metrics": [
        {
            "sid": "EDB224CCF4FB4F99815C24302BDF3301",
            "data": [
                {
                    "mid": 20003,
                    "kvs": [
                        [
                            1548670255,
                            215
                        ],
                        [
                            1548670257,
                            129
                        ],
                        [
                            1548670259,
                            121
                        ],
						......
					],
					"peer_uid": 0,
				},
				......
			]
		},
......
]
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](#code)。
- `message`: String 类型，错误消息。
- `call_id`: String 类型，通话的唯一标识。
- `call_info`: JSONArray 类型，包含通话中各个用户的信息。最多返回 10 个用户，按照用户加入频道的时间降序排列。每个用户包含以下信息 ：
  - `sid`: String 类型，用户通话的唯一标识。
  - `cname`: String 类型，频道名称。
  - `uid`: Number 类型，用户 ID。
  - `network`: String 类型，网络类型。
  - `platform`: String 类型，平台信息。
  - `speaker`: Boolean 类型，用户在通话中是否发送音视频。
  - `sdk_version`: String 类型，SDK 版本信息。
  - `device_type`: String 类型，设备信息。
  - `join_ts`: Number 类型，用户加入频道的时间。Unix 时间戳（秒）。
  - `leave_ts`: Number 类型，用户离开频道的时间。Unix 时间戳（秒）。
  - `finished`: Boolean 类型，用户是否已离开频道。
- `metrics`: JSONArray 类型，各个用户通话（`sid`）的具体指标数据。每个用户通话包含以下信息：
  - `sid`: String 类型，用户通话的唯一标识。
  - `data`: JSONArray 类型，用户通话的质量指标数据。
    - `mid`: Number 类型，指标 ID，详见 [指标 ID 表](#mid)。
    - `peer_uid`: Number 类型，对端的用户 ID，如果为 0 表示该指标为本端数据。
    - `kvs`: 时间戳及相应时间的指标数值。

## <a name="report"></a>报告问题

向 Agora 报告通话质量问题。

- 方法：POST
- 接入点：/beta/analytics/feedback/reportV2

> 该方法每秒最多可调用 10 次。

### 参数

该 API 需要在 URL 中传入 appid 参数，你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。

该 API 需要在请求包体中传入以下参数。

| 参数           | 类型   | 描述                                                         |
| :------------- | :----- | :----------------------------------------------------------- |
| `cname`        | String | 频道名称。                                                   |
| `teacher_uid`  | Number | 老师的用户 ID。                                              |
| `student_uid`   | Number | 学生的用户 ID。                                              |
| `abnormal_uid` | Number | 体验异常端（老师或学生）的用户 ID。                          |
| `ts`           | Number | 问题发生时间，UNIX 时间戳，单位为秒。                        |
| `comments`     | String | （可选）备注。可以简单描述问题。                             |
| `type`         | Number | 问题类型：<li>1: 切课</li><li>2: 求助</li><li>3: 投诉</li><li>4: 问题反馈</li> |

### HTTP 请求示例

```http
POST /beta/analytics/feedback/reportV2?appid=axxxxxxxxxxxxxxxxxxxx HTTP/1.1
Host: api.agora.io
Content-type: application/json
Authorization: Basic ZGJhZDMyNmFkxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWQzYTczNzg2ODdiMmNiYjRh
Cache-Control: no-cache

{ 
	"cname":"android_video_engine_1561704454355",
	"teacher_uid":111,
	"comments":"bad network",
	"ts":1581387571,
	"type":1,
	"student_uid":222,
	"abnormal_uid":222
}
```

### 响应示例

```json
{
"code": 200,
"message": "",
}
```

- `code`: Number 类型，响应状态码。200 表示请求成功，详见[状态码](#code)。
- `message`: String 类型，错误消息。

## 参考

### <a name="code"></a>状态码

| 状态码 | 描述                   |
| ------ | ---------------------- |
| 200    | 请求处理成功           |
| 400    | 输入格式错误           |
| 401    | 未经授权               |
| 403    | 授权信息错误，禁止访问 |
| 404    | API 调用错误           |
| 500    | 服务器内部错误         |

### <a name="mid"></a>指标 ID 表

| `mid` | 描述                                     |
| ----- | ---------------------------------------- |
| 20001 | App 的 CPU 使用率                        |
| 20002 | 系统 CPU 使用率                          |
| 20003 | 音频发送码率，单位 Kbps                  |
| 20004 | 音频接收码率，单位 Kbps                  |
| 20005 | 音频渲染卡顿时间，单位 ms                |
| 20006 | 视频发送码率，单位 Kbps                  |
| 20007 | 视频采集帧率，单位 fps                   |
| 20008 | 视频发送帧率，单位 fps                   |
| 20009 | 视频接收码率，单位 Kbps                  |
| 20010 | 视频接收帧率，单位 fps                   |
| 20011 | 视频渲染卡顿时间，单位 ms                |
| 20013 | 视频的画面质量，该数值越低表示画面越清晰 |
| 20015 | 音频上行丢包率                           |
| 20016 | 音频端到端丢包率                         |
| 20017 | 视频上行丢包率                           |
| 20018 | 视频端到端丢包率                         |
| 20019 | 视频分辨率的宽                           |
| 20020 | 视频分辨率的高                           |
| 20021 | SDK 任务调度延迟，单位 ms                           |
| 20022 | 客户端到本地路由器的往返时延，单位 ms                           |

### <a name="exp_id"></a>体验 ID 映射表

| `exp_id` | 描述     |
| :--------- | :------- |
| 4      | 视频卡顿 |
| 5      | 音频卡顿 |
| 6      | 进频道慢 |

### <a name="factor_id"></a>根因 ID 映射表

| `factor_id` | 描述                             |
| :-------- | :------------------------------- |
| 1     | 相同 UID 互踢                    |
| 2     | 管理员踢出                       |
| 3     | 连接异常                         |
| 8     | 视频下行网络延时                 |
| 9     | 音频下行网络延时                 |
| 10    | CPU 占用高                       |
| 11    | 设备繁忙                         |
| 12    | 用户停止发送本地音频流           |
| 13    | 用户停止发送本地视频流           |
| 14    | 用户关闭音频模块                 |
| 15    | 用户关闭视频模块                 |
| 16    | 远端用户停止接收本地用户的音频流 |
| 17    | 远端用户停止接受本地用户的视频流 |
| 21    | 视频上行网络丢包                 |
| 22    | 音频上行网络丢包                 |
| 23    | 视频下行网络丢包                 |
| 24    | 音频下行网络丢包                 |
| 25    | Electron 集成                    |
| 29    | Wi-Fi（包含热点）信号差          |
| 30    | 用户被踢出频道                   |
| 31    | APP ID 无效，导致加频道失败      |
| 32    | 频道名称无效，导致加频道失败     |
| 33    | Token 无效，导致加频道失败       |
| 34    | Token 过期，导致加频道失败       |
| 35    | 用户被拦截，导致加频道失败       |
| 36    | 加入频道超时                     |
| 37    | 网络中断，触发网络重连           |
| 38    | 设置代理，触发网络重连           |
| 39    | Token 更新，触发网络重连         |
| 40    | 用户 IP 变更，触发网络重连       |
| 41    | 连接网络超时，触发网络重连       |
| 42    | 视频上行网络延时                 |
| 43    | 音频上行网络延时                 |
| 44    | 视频上行网络抖动                 |
| 45    | 音频上行网络抖动                 |
| 46    | 视频下行网络抖动                 |
| 47    | 音频下行网络抖动                 |

