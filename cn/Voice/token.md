
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Fri Jul 12 2019 09:58:40 GMT+0800 (CST)
---
# 校验用户权限
本文介绍如何校验用户权限。

Agora SDK 提供两种校验机制：App ID 和动态密钥。在调用 `joinChannel` 加入频道时，你需要选择一种机制进行鉴权。这两种鉴权机制的关系如下：

- App ID：易于获取，适用于对安全要求不高的场景
- 动态密钥：安全性高，适用于对安全要求较高的生产环境

## 适用范围

Agora 动态密钥分为 Channel Key 和 Token 两种。本文的动态密钥指的是 Token。

不同版本的 SDK 支持的动态密钥不同。因此，在选用校验方式前，请确认当前 SDK 的版本信息。

| Agora SDK  | 支持 Token 的版本 | 支持 Channel Key 的版本 | 查看版本信息       |
| ---------- | ----------------- | ----------------------- | ------------------ |
| Native SDK | v2.1.0 及以上     | v2.1.0 之前             | `getSdkVersion`    |
| Web SDK    | v2.4.0 及以上     | v2.4.0 之前             | `AgoraRtc.VERSION` |
| Gaming SDK | v2.2.0 及以上     | v2.2.0 之前             | `getSdkVersion`    |

> - 如果你使用的 SDK 支持的动态密钥为 Channel Key，请参考 [Channel Key 密钥说明](https://docs.agora.io/cn/Agora%20Platform/channel_key?platform=All%20Platform)。
> - 如果你需要从老版本升级到支持 Token 的版本，请参考[动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。
> - 如果你使用的是 Agora Signaling SDK，请参考[信令密钥说明 ](https://docs.agora.io/cn/Agora%20Platform/key_signaling)。
> - 如果你使用的是 Agora RTM SDK，请参考 [RTM 密钥说明](https://docs.agora.io/cn/Real-time-Messaging/RTM_key?platform=All%20Platforms)。

## 前提条件

请确保你在  [Agora Dashboard](https://dashboard.agora.io/) 完成注册，并创建了项目。

<a name = "appid"></a>
## App ID

每个 Agora 的项目都有一个 App ID，它是项目的唯一标识。

### 获取 App ID

1. 点击 **Dashboard** 左侧的**项目管理**页，查看你所创建的项目详情。
2. 在项目详情页，你可以查看你的 **App ID**。

### 使用 App ID

在调用 Agora SDK 的 API 接口实现功能，如创建对象时，SDK 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。

<a name = "Token"></a>
## Token

Token 是相比 App ID 更为复杂，也更为安全的校验方式。你需要使用 App ID 和 App 证书来生成一个 Token。

<a name = "appcertificate"></a>

### 启用 App 证书

如果你是首次创建的项目，参考如下步骤启用 App 证书：

1. 在**项目管理**页，找到刚创建的项目，点击**编辑**按钮，然后点击 **App 证书**后面的**启用**按钮。
2. 根据屏幕提示，在注册邮箱中确认启用 App 证书。
3. 回到**项目管理**页，会看到 **App 证书**显示已启用。

### 获取临时 Token

为方便体验，Agora 支持在 Dashboard 的项目详情页，生成一个试用的临时 Token。使用如下一种方式获取临时 Token：

* 在项目详情处，点击**生成临时 Token**，输入任一频道名，你就会在 **Token** 页面获取一个临时 Token。
* 在创建项目时，直接勾选 **APP ID + APP 证书+ Token（推荐）**。Dashboard 会自动开启 **App 证书**。点击**生成临时 Token** 获取即可。

临时 Token 适用于对安全要求一般的测试场景。对于正式生产环境，我们推荐使用正式 Token。

### 获取正式 Token

正式的 Token 需要在你的服务端部署并生成。具体的流程如下：

**1. 部署 Token Generator**

在使用 Token 之前，你需要先在你的服务端部署一个 Token Generator 用来生成 Token。

Agora 提供支持 C++、Go、Java、Node.js、Python 和 PHP 语言生成 Token 的[示例代码](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey)供参考。

你可以直接使用相应的示例代码，也可以使用其他语言实现生成 Token 的功能。 Agora 欢迎开发者将自己实现的动态密钥生成方式在 [GitHub](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 上提交 Pull request 贡献出来。

<a name = "Generate_Token"></a>

**2. 生成正式 Token**

服务端生成正式 Token 的流程如下：

1. App 客户端向服务端发送获取 Token 的请求
2. 服务端收到请求后 Token Generator 生成一个 Token，然后将生成的 Token 发送给 App 客户端

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
<tr><td><code>appId</code></td>
<td>你的项目的 App ID</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>你的项目的 App Certificate</td>
</tr>
<tr><td><code>channelName</code></td>
<td>用户申请进入的频道名</td>
</tr>
<tr><td><code>uid</code> </td>
<td>申请加入频道的用户 ID</td>
</tr>
<tr><td><code>expireTimestamp</code>  <sup>[1]</sup></td>
<td>Token 服务有效时间，默认为 0，表示 Token 一旦生成，永不失效。有效时间内用户可以无限次加入频道，超过有效时间用户将被踢出频道。</td>
</tr>
</tbody>
</table>

> [1] `expireTimestamp` 指 1970 年 1 月 1 日开始到 Token 服务到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个服务的有效时间是独立的，可以通过 `setPrivilege` 接口进行单独设置。


具体生成方法和参数设置，请参考[在服务端生成 Token](../../cn/null/token_server.md)。

### 使用 Token

在调用 Agora SDK 的 API 接口实现功能，如加入频道时，SDK 会需要你填入 Token 和频道名。填入你所获取到的 Token，和生成 Token 时用的频道名，就可以进入频道，实现音视频通话。

> - 请确保你在加入频道时填入的频道名与用户名和用于生成 Token 时填入的频道名、用户名一致。
> - 出于安全考虑，请在 Token 生成后 24 小时内加入频道。超过 24 小时需要重新生成 Token。
> - Token 在一段时间后会失效。当客户端收到回调提醒 Token 即将失效，或当获知 Token 已失效时，需在服务端重新生成 Token，然后调用 `renewToken` 方法进行更新。
> - Token 采用业界标准化的 HMAC/SHA1 加密方案，在 Node.js, Java, Python, C++ 等绝大多数通用的服务器端开发平台上均可获得所需库。具体加密方案可参看 [Authentication code](http://en.wikipedia.org/wiki/Hash-based_message_authentication_code)。

## 参考

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
