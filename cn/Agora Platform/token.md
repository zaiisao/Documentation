
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Mon Aug 05 2019 02:15:16 GMT+0800 (CST)
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
| Web SDK    | v2.4.0 及以上     | v2.4.0 之前             | `AgoraRTC.VERSION` |
| Gaming SDK | v2.2.0 及以上     | v2.2.0 之前             | `getSdkVersion`    |

> - 如果你使用的 SDK 支持的动态密钥为 Channel Key，请参考 [Channel Key 密钥说明](https://docs.agora.io/cn/Agora%20Platform/channel_key?platform=All%20Platform)。
> - 如果你需要从老版本升级到支持 Token 的版本，请参考[动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。
> - 如果你使用的是 Agora Signaling SDK，请参考[信令密钥说明 ](https://docs.agora.io/cn/Agora%20Platform/key_signaling)。
> - 如果你使用的是 Agora RTM SDK，请参考 [RTM 密钥说明](https://docs.agora.io/cn/Real-time-Messaging/RTM_key?platform=All%20Platforms)。

<a name = "appid"></a>
## App ID

每个 Agora 的项目都有一个 App ID，它是项目的唯一标识。

### 获取 App ID

1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/Agora%20Platform/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


### 使用 App ID

在调用 Agora SDK 的 API 接口实现功能，如创建对象时，SDK 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。

<a name = "Token"></a>
## Token

Token 是相比 App ID 更为复杂，也更为安全的校验方式。你需要使用 App ID 和 App 证书来生成一个 Token。

<a name = "appcertificate"></a>

### 启用 App 证书


方法一：如果创建项目时，你直接勾选了 **APP ID + APP 证书+ Token（推荐）**。Dashboard 会自动开启 **App 证书**。

![](https://web-cdn.agora.io/docs-files/1562925509805)

方法二：如果创建项目时，你没有勾选  **APP ID + APP 证书+ Token（推荐）**，则参考如下步骤开启 App 证书。

1. 在**项目管理**页，找到刚创建的项目，点击**编辑**按钮。

	![](https://web-cdn.agora.io/docs-files/1562926250060)
2. 然后点击 **App 证书**后面的**启用**按钮。

	![](https://web-cdn.agora.io/docs-files/1562926258836)
3. 根据屏幕提示，在注册邮箱中确认启用 App 证书。
4. 回到**项目管理**页，会看到 **App 证书**显示已启用。

	![](https://web-cdn.agora.io/docs-files/1562926274649)

**Note:** 若收件箱中没有确认邮件，请至订阅邮件或垃圾邮件中查找

<a name = "temptoken"></a>

### 获取临时 Token

为方便体验，Agora 支持在 Dashboard 的项目详情页，生成一个试用的临时 Token，用于加入频道。

在项目详情处，点击**生成临时 Token**，输入频道名，你就会在 **Token** 页面获取一个临时 Token。

![](https://web-cdn.agora.io/docs-files/1562926292439)

![](https://web-cdn.agora.io/docs-files/1562926303571)


<div class="alert warning"><li> 点击<b>生成临时 Token</b> 前，请确保你已开启项目 App 证书。详见[启用 App 证书](#appcertificate)。</li>
<li> 临时 Token 适用于对安全要求一般的测试场景。对于正式生产环境，我们推荐使用正式 Token。</li>
<li> 临时 Token 不适用于 Agora RTM SDK。</li></div>

### 获取正式 Token

正式的 Token 需要在你的服务端部署并生成。详见[生成 Token](../../cn/Agora%20Platform/token_server.md)。

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
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c"><span>加入频道 (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80"><span>更新 Token (renewToken)</span></a></td>
</tr>
<tr><td>iOS/macOS</td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:"><span>加入频道 (joinChannelByToken)</span></a></td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:"><span>更新 Token (renewToken)</span></a></td>
</tr>
<tr><td>Windows</td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c"><span>加入频道 (joinChannel)</span></a></td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739"><span>更新 Token (renewtoken)</span></a></td>
</tr>
<tr><td>Web</td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#join"><span>加入频道 (join)</span></a></td>
<td><a href="https://docs.agora.io/cn/Agora%20Platform/API%20Reference/web/interfaces/agorartc.client.html#renewtoken"><span>更新 Token (renewToken)</span></a></td>
</tr>
</tbody>
</table>
