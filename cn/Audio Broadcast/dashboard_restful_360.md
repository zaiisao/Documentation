
---
title: Dashboard RESTful API
description: 
platform: All Platforms
updatedAt: Thu Sep 13 2018 10:09:26 GMT+0000 (UTC)
---
# Dashboard RESTful API
# Dashboard RESTful API

## 认证

RESTful API 仅支持 HTTPS。用户必须通过 basic HTTP 认证:

-   用户名: Customer ID

-   密钥: Customer Certificate


与 Agora SDK 所使用的 App ID 和 App Certificate 不同，Customer ID 和 Customer Certificate 仅用于访问 Restful API。


> 你可以登录 [Dashboard](https://dashboard.agora.io)，点击右上角账户名，进入下拉菜单 RESTFUL API 页面获取 Customer ID 和 Customer Certificate。 Vendor Key 和 Sign Key 在 Dashboard 里已分别改名为 App ID 和 App Certificate，但本文代码里目前仍沿用 vendor_key 和 sign_key。

## 接入点

所有请求都发送给 BaseUrl：**https://api.agora.io/dev/v2**。

-   请求：

    -   POST 方法：参数格式必须为 JSON ，内容类型: application/json

    -   GET 方法：请求 Body 为空

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
<td>未经授权的(App ID/Customer Certificate匹配错误)</td>
</tr>
<tr><td>Status 404</td>
<td>API调用错误</td>
</tr>
<tr><td>Status 500</td>
<td>服务器内部错误</td>
</tr>
</tbody>
</table>

> 为防止大量异常请求影响其他用户的正常使用，我们对 API 的调用频率做了限制。当达到限流阈值时，会返回 HTTP 错误 429 (Too Many Requests)。我们认为设置的阈值可以满足绝大多数用户的使用场景，如果您被限制，请尝试调整调用频率。如果该限制使您的需求无法得到满足，请联系 [sales@agora.io](mailto:sales@agora.io) 。

## 申请开通旁路推流功能 (POST)

该方法申请开通项目的旁路推流功能。

-   方法：POST

-   路径：BaseUrl/apps/<appid\>/pushstream/openstate

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appid</td>
<td>必填，dashboard 中项目的 App ID</td>
</tr>
</tbody>
</table>



-   响应：

    ```
    {
      "enable": true
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
<tr><td>enable</td>
<td>是否开通成功</td>
</tr>
</tbody>
</table>




## 查询是否成功开通旁路推流功能（GET)

-   方法：GET

-   路径：BaseUrl/apps/<appid\>/pushstream/openstate

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appid</td>
<td>必填，dashboard 中项目的 App ID</td>
</tr>
</tbody>
</table>



-   响应：

    ```
    {
      "enable": false
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
<tr><td>enable</td>
<td><p>是否成功开通旁路推流功能：</p>
<div><ul>
<li>true：开通成功</li>
<li>false：未开通</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>




## 查询厂商带宽功能 (GET)

该方法用于查询单个厂商的带宽，带宽的粒度为 5 分钟。

-   方法：GET

-   路径：BaseUrl/apps/<appid\>/usages/bandwidth

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appid</td>
<td>必填，dashboard 中项目的 App ID</td>
</tr>
<tr><td>date</td>
<td>必填，想要查询带宽的日期，格式为 YYYYMMDD。如果不填，则默认为前一天</td>
</tr>
</tbody>
</table>



-   响应：

    ```
    {
        date: "2018-08-07",
        unit: "kbps",
        value: {
            "00:00": 100,
            "00:05": 200,
            ...
            "23:55": 100
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
<tr><td>date</td>
<td>所查询带宽的日期</td>
</tr>
<tr><td>unit</td>
<td>带宽单位</td>
</tr>
<tr><td>value</td>
<td>带宽数据，由时间和具体使用的带宽值组成</td>
</tr>
</tbody>
</table>


> -   该方法每次只能查询指定的 App ID 某一天的带宽数据。如要查询多个 App ID 的带宽数据，建议使用轮询的办法。
> -   该方法只能查询最近一个月的带宽数据，如果时间超出此范围，会返回 400。


## 错误代码和警告代码

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_native.md)。


