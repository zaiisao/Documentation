
---
title: Dashboard RESTful API
description: 
platform: All Platforms
updatedAt: Sun May 05 2019 09:54:10 GMT+0800 (CST)
---
# Dashboard RESTful API
# Dashboard RESTful API

## 认证

RESTful API 仅支持 HTTPS。用户必须通过 basic HTTP 认证:

-   用户名: Customer ID
-   密钥: Customer Certificate


与 Agora SDK 所使用的 App ID 和 App Certificate 不同，Customer ID 和 Customer Certificate 仅用于访问 Restful API。

> 你可以登录 [Dashboard](https://dashboard.agora.io)， 点击右上角账户名，进入下拉菜单 RESTFUL API 页面获取 Customer ID 和 Customer Certificate。 Vendor Key 和 Sign Key 在 Dashboard 里已分别改名为 App ID 和 App Certificate，但本文代码里目前仍沿用 vendor_key 和 sign_key。

## 接入点

所有请求都发送给 BaseUrl：**https://api.agora.io/dev/v2**。

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

## 查询指定频道内的分角色用户列表 (GET)

该方法查询某个指定频道内的分角色用户列表。

-   方法：GET

-   路径：BaseUrl/apps/<appid\>/channels/<cname\>/users

-   参数：cursor，count

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
<tr><td>cname</td>
<td>必填，指定查询分角色用户列表的频道名</td>
</tr>
<tr><td>cursor</td>
<td>选填，本次扫描的起始位置，默认值为 0，表示从频道列表的起始处开始扫描</td>
</tr>
<tr><td>count</td>
<td>选填，本次扫描的用户数，默认值为 1000，最大值为 2000</td>
</tr>
</tbody>
</table>



-   响应：

    ```
    //如果是通信频道
    data: {
      total_count: <count>
      mode: 1,
      comm_users: [<uid>],
      cursor: <cursor>
    }
    
    // 如果是直播模式频道
    data: {
        total_count: <count>,
        mode: 2,
        broadcasters: [<uid>],
        audience: [<uid>],
        cursor: <cursor>
    }
    
    //如果频道内无活跃用户
    data: {
      total_count: 0
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
<tr><td>total_count</td>
<td>总频道数</td>
</tr>
<tr><td>mode</td>
<td><p>频道模式。</p>
<ul>
<li>1：通信频道</li>
<li>2：直播频道</li>
</ul>
</td>
</tr>
<tr><td>comm_users</td>
<td>通信频道内所有用户的 UID</td>
</tr>
<tr><td>broadcasters</td>
<td>直播频道内所有主播的 UID</td>
</tr>
<tr><td>audience</td>
<td>直播频道内所有观众的 UID</td>
</tr>
<tr><td>cursor</td>
<td><p>本次扫描位置：</p>
<ul>
<li>如果 cursor &gt; 0：说明本次查询未扫描完厂商所有频道。返回的 cursor 值可作为下次查询的开始值</li>
<li>如果 cursor = 0：说明本次查询扫描完了频道内的所有用户</li>
</ul>
</td>
</tr>
</tbody>
</table>


## 分页获取特定厂商的频道列表（GET)

> 使用该方法查询后会将结果缓存 1 分钟。因此一分钟内再次查询会从缓存结果中提取，而不再更新数据。

该方法分页获取特定厂商的频道列表。返回的频道列表中包含频道名及频道内的人数信息。

-   方法：GET

-   路径：BaseUrl/apps/<appid\>/channels

-   参数：order_by，page_no，page_size

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
<tr><td>order_by</td>
<td><p>选填，获取的厂商频道列表的排列顺序，按下列定义的方式降序排列。从如下四种排序方法中选择一种：</p>
<ul>
<li>user_size：（默认）根据频道内的用户数降序排序</li>
<li>broadcaster_size：根据频道内的主播数降序排序</li>
<li>audience_size：根据频道内的观众数降序排序</li>
<li>cname：按照频道名降序排列</li>
</ul>
</td>
</tr>
<tr><td>page_no</td>
<td>选填，起始页码，从 0 开始，默认值为 0</td>
</tr>
<tr><td>page_size</td>
<td>选填，每页条数，默认值为 100，最大值为 2000</td>
</tr>
</tbody>
</table>



-   响应：

    ```
    data: {
      channels: [<Channel>],
      total_count: <channel_count>,
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
<tr><td>channels</td>
<td>频道信息，包含 cname 和 score 字段，即频道名和频道内的人数信息</td>
</tr>
<tr><td>total_count</td>
<td>总频道数</td>
</tr>
</tbody>
</table>




## 扫描厂商频道及用户列表 (GET)

该方法扫描指定的所有频道，返回频道列表，且每个频道包含其用户列表。

-   方法：GET

-   路径：BaseUrl/apps/<appid\>/channelusers

-   参数：cursor，count

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
<tr><td>cursor</td>
<td>选填，本次扫描的起始位置，默认值为 0，表示从频道列表的起始处开始扫描</td>
</tr>
<tr><td>count</td>
<td>选填，本次扫描的频道数，默认值为 100，最大值为 200</td>
</tr>
</tbody>
</table>



-   响应：

    ```
    data: {
      total_count: <total_channel_count>,
      cursor: <cursor>,
      channels: [<ChannelUsers],
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
<tr><td>total_count</td>
<td>总频道数</td>
</tr>
<tr><td>cursor</td>
<td><p>本次扫描位置：</p>
<div><ul>
<li>如果 cursor &gt; 0：说明本次查询未扫描完厂商所有频道。返回的 cursor 值可作为下次查询的开始值</li>
<li>如果 cursor = 0：说明本次查询扫描完了频道内的所有用户</li>
</ul>
</div>
</td>
</tr>
<tr><td>channels</td>
<td><p>频道内的用户信息，具体包含：</p>
<div><ul>
<li>total_count：总频道数</li>
<li>mode：频道模式，1 为通信频道，2 为直播频道</li>
<li>comm_users：通信频道内所有用户的 UID</li>
<li>broadcasters：直播频道内所有主播的 UID</li>
<li>audience：直播频道内所有观众的 UID</li>
<li>cursor：本次扫描位置</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>




## 错误代码和警告代码

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_native.md)。


