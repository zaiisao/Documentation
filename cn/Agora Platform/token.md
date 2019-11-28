
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Tue Oct 29 2019 03:27:08 GMT+0800 (CST)
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
> - 如果你使用的是 Agora RTM SDK，请参考 [RTM 密钥说明](https://docs.agora.io/cn/Real-time-Messaging/rtm_token?platform=All%20Platforms)。

<a name = "appid"></a>
## App ID

每个 Agora 的项目都有一个 App ID，它是项目的唯一标识。

### 获取 App ID

1. 进入[控制台](https://console.agora.io/)，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Agora%20Platform/sign_in_and_sign_up.md)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面，点击**创建**按钮。

![](https://web-cdn.agora.io/docs-files/1574156100068)

3. 在弹出的对话框内输入**项目名称**，选择 App ID 作为**鉴权机制**，再点击“提交”。

![](https://web-cdn.agora.io/docs-files/1574921599254)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目，并找到对应的 App ID。

![](https://web-cdn.agora.io/docs-files/1574921811175)




### 使用 App ID

在调用 Agora SDK 的 API 接口实现功能，如创建对象时，SDK 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。

<a name = "Token"></a>
## Token

Token 是相比 App ID 更为复杂，也更为安全的校验方式。你需要使用 App ID 和 App 证书来生成一个 Token。

<a name = "appcertificate"></a>

### 启用 App 证书

1. 在**项目管理**页面，点击目标项目的**编辑**按钮，进入**编辑项目**页面。

![](https://web-cdn.agora.io/docs-files/1574922261870)

2. 找到 App 证书一栏，点击**启用**按钮。

![](https://web-cdn.agora.io/docs-files/1574156526581)

3. 仔细阅读关于 App 证书的提示后，点击“启用 App 证书”。

![](https://web-cdn.agora.io/docs-files/1574159500507)

4. 声网会给你发一封邮件，按照邮件中的提示进行确认，即可启用 App 证书。

5. 成功启用后， App 证书会显示在**编辑项目**页面。

> 若收件箱中没有确认邮件，请至订阅邮件或垃圾邮件中查找。

<a name = "temptoken"></a>

### 获取临时 Token

为方便体验，Agora 支持在控制台的项目详情页，生成一个试用的临时 Token，用于加入频道。

1. 在项目详情处，点击 ![](https://web-cdn.agora.io/docs-files/1574923151660) 按钮。

![](https://web-cdn.agora.io/docs-files/1574922827899)

2. 进入 **Token** 页面，输入待加入的频道名，你就得到一个临时 Token。

![](https://web-cdn.agora.io/docs-files/1574923022040)


<div class="alert warning"> 注意：<li>生成临时 Token 前，请确保你已开启项目 App 证书。详见<a href="../../cn/Agora%20Platform/appcertificate.md">启用 App 证书</a>。</li><li>临时 Token 适用于对安全要求一般的测试场景。对于正式生产环境，我们推荐使用正式 Token。</li> <li>临时 Token 不适用于 Agora RTM SDK。</li></div>

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
