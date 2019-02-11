
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Mon Feb 11 2019 04:01:06 GMT+0000 (UTC)
---
# 校验用户权限
本文介绍 Agora SDK 最新的鉴权机制 Token，阅读前请对照下表确认你使用的产品支持 Token：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Agora SDK</th>
<th>支持 Token 的版本</th>
</tr>
</thead>
<tbody>
<tr><td>Native</td>
<td>2.1.0 及以上版本</td>
</tr>
<tr><td>Web</td>
<td>2.4.0 及以上版本</td>
</tr>
<tr><td>Gaming</td>
<td>2.2.0 及以上版本</td>
</tr>
</tbody>
</table>


调用以下方法查看你使用的 SDK 版本信息：

- Native SDK: `getSdkVersion`
- Web SDK: `AgoraRTC.VERSION`
- Gaming SDK: `getSdkVersion`


>-   如果你使用的是 Agora Signaling SDK，请参考[信令密钥说明](../../cn/Agora%20Platform/key_signaling.md)。
-   如果你使用的产品版本不支持 Token，需使用密钥功能请参考 [Channel Key 密钥说明](../../cn/null/channel_key.md)。


## 鉴权机制简介

在调用 `joinChannel` 方法时，必须传入鉴权密钥作为参数。针对用户的项目开发需求，Agora SDK 提供了两种鉴权机制：[App ID](#APPID) 和 [Token](#Token) 。下图描述这两种鉴权机制的关系以及适用场景：

<img alt="../_images/key_relation_native.jpg" src="https://web-cdn.agora.io/docs-files/cn/key_relation_native.jpg" style="width: 840px; "/>


其中：

-   App ID 易于获取，适用于对安全要求不高的场景。

-   Token 安全性高，更适用于对安全要求较高的生产环境。

-   App Certificate 仅用于生成 Token，不可单独使用。一旦启用了 App Certificate，则必须使用 Token。

<a name = "-App-ID"></a>

## App ID

在 Agora 官网注册后，你就可以创建项目，而每个项目都有一个唯一标识，叫做 App ID。App ID 可以明确你的项目及组织身份。 不同的 App ID 在 Agora 实时网络中的通话是完全隔离的；Agora 提供的频道信息、计费、管理服务也都是基于 App ID。

App ID 提供了一个简单的方式，方便开发者使用 Agora 的服务，但你需要确保 App ID 的保密性。一旦有人非法获取了你的 App ID，他就可以在 Agora 提供的 SDK 中使用你的 App ID。因此如果你需要更高级别的安全方案，或者增加用户权限设置，Agora 建议你使用 [Token](#Token) 机制。


### 获取 App ID

1. 进入 [https://dashboard.agora.io/](https://dashboard.agora.io/) ，按照屏幕提示创建一个开发者账号。
2. 登录 Dashboard 页面，点击 **添加新项目**。

	<img alt="../_images/appid_1.jpg" src="https://web-cdn.agora.io/docs-files/cn/appid_1.jpg" />

1. 填写 **项目名**，然后点击 **提交**。
2. 在你创建的项目下，查看并获取该项目对应的 **App ID**。

	<img alt="../_images/appid_2.jpg" src="https://web-cdn.agora.io/docs-files/cn/appid_2.jpg" />


### 使用 App ID

在调用 Agora SDK 的 API 接口实现功能时，如创建对象、加入频道，SDK 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。

<a name = "Token"></a>

## Token

使用 Token 的流程如下：

1.  在你的 Server 端部署一个 Token Generator

2.  App Client 端向 Server 端发送获取 Token 的请求

3.  Server 端收到请求后 Token Generator 生成一个 Token

4.  Server 端将生成的 Token 发送给 App Client 端

5.  App Client 端调用加入频道的 API 时传入 Token 参数

6.  Token 即将过期或过期时，重复以上步骤 2-4

7.  App Client 端调用`renewToken` 更新 token


### 部署 Token Generator

在使用 Token 之前，你需要先在你的 Server 端部署一个 Token Generator 用来生成 Token。

Agora 提供以下平台生成 Token 的[示例代码](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey)供你参考：

-   C++

-   Go

-   Java

-   Node.js

-   Python

-   PHP


你可以直接使用相应的示例代码，也可以使用其他语言实现生成 Token 的功能。 Agora 欢迎开发者将自己实现的动态密钥生成方式在 [GitHub](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 上提交 Pull request 贡献出来。

<a name = "Generate_Token"></a>
### 生成 Token

生成 Token 时需要向 Server 端传入以下参数：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>名称</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr><td><code>channelName</code></td>
<td>用户申请进入的频道名</td>
</tr>
<tr><td><code>uid</code></td>
<td>申请加入频道的用户 ID</td>
</tr>
<tr><td><code>expireTimestamp</code>  <sup>[1]</sup></td>
<td>Token 服务有效时间，默认为 0，表示 Token 一旦生成，永不失效。有效时间内用户可以无限次加入频道，超过有效时间用户将被踢出频道。</td>
</tr>
</tbody>
</table>



> [1] `expireTimestamp` 指 1970 年 1 月 1 日开始到 Token 服务到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个服务的有效时间是独立的，可以通过 `setPrivilege` 接口进行单独设置。

<a id = "-App-Certificate"></a>

#### 获取 App Certificate

每个 Agora 账户下可创建多个项目，且每个项目有独立的 App ID 和 App Certificate。 在上文项目列表中查看 App ID 的地方，启用该项目的 App Certificate:

首先，点击激活项目右上方的 **编辑** 按钮。

<img alt="../_images/project_edit.png" src="https://web-cdn.agora.io/docs-files/cn/project_edit.png" />


然后，点击 App Certificate 右方的 **启用** 按钮。仔细阅读 关于 App Certificate 介绍后，根据屏幕提示，确认启用App Certificate。

<img alt="../_images/enable_app_cert.png" src="https://web-cdn.agora.io/docs-files/cn/enable_app_cert.png" />


最后，点击 App Certificate 后面的“眼睛”图标，显示完整的 App Certificate。如需隐藏 App Certificate，再次点击“眼睛”图标。

<img alt="../_images/view_app_certificate.png" src="https://web-cdn.agora.io/docs-files/cn/view_app_certificate.png" />


> -   将你的 App Certificate 保存在服务器端，且对任何客户端均不可见。
>
> -   通常 App Certificate 在启用一小时后生效。
>
> -   当项目的 App Certificate 被启用后，你必须使用 Token。例如: 在启用 App Certificate 前，你可以使用 App ID 加入频道。但启用了 App Certificate 后，你就必须使用 Token 或 Channel Key 加入频道。

<a id ="roleprivilege"></a>




### 使用 Token

每一次 Client 端的用户加入频道前:

1.  Client 端向企业自己的 Server 端申请 Token。

2.  Server 端收到请求后，通过 Agora 提供的算法生成 Token，返回给 Client 端应用程序。

3.  Client 端应用程序调用加入频道的 API 时，第一个参数要求为 Token。

4.  Agora 的服务器接收到 Token 信息，验证该通话是来自于合法用户，并允许访问 Agora SD-RTN。



> -   出于安全考虑，请在 Token 生成后 24 小时内加入频道。超过 24 小时需要重新生成 Token。

> -   采用了 Token 方案后，Agora Native SDK 的用户进入频道时，Token 将替换原来的 App ID；Agora 不建议 Agora Web SDK 的用户用 Token 替换 App ID。

> -   Token 在一段时间后会失效。当 Client 端收到回调提醒 Token 即将失效，或当获知密钥已失效时，需调用 `renewToken` 方法进行更新。

> -   Token 采用业界标准化的 HMAC/SHA1 加密方案，在 Node.js, Java, Python, C++ 等绝大多数通用的服务器端开发平台上均可获得所需库。具体加密方案可参看以下网页：[http://en.wikipedia.org/wiki/Hash-based\_message\_authentication\_code](http://en.wikipedia.org/wiki/Hash-based_message_authentication_code)


## 参考

如果你需要从老版本升级到支持 Token 的版本，请参考[动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

了解在 Server 端如何实现生成密钥的功能，请查看[在服务端生成密钥](../../cn/null/token_server.md)。


下表列出与 Token 相关的 SDK API 接口：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>平台</th>
<th>加入频道</th>
<th>更新 Token</th>
</tr>
</thead>
<tbody>
<tr><td>Android</td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c"><span>加入频道 (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80"><span>更新 Token (renewToken)</span></a></td>
</tr>
<tr><td>iOS/macOS</td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:"><span>加入频道 (joinChannelByToken)</span></a></td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:"><span>更新 Token (renewToken)</span></a></td>
</tr>
<tr><td>Windows</td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c"><span>加入频道 (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739"><span>更新 Token (renewtoken)</span></a></td>
</tr>
<tr><td>Web</td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#join"><span>加入频道 (join)</span></a></td>
<td><a href="https://docs.agora.io/cn/Voice/API%20Reference/web/interfaces/agorartc.client.html#renewtoken"><span>更新 Token (renewToken)</span></a></td>
</tr>
</tbody>
</table>




