
---
title: 审核频道内的音频
description: 
platform: All Platforms
updatedAt: Thu Oct 22 2020 02:21:28 GMT+0800 (CST)
---
# 审核频道内的音频
本文介绍如何使用阿里智能语音审核 RESTful API 对频道内的音频进行实时审核。

## 前提条件

你需要一个集成了 [Agora RTC SDK](https://docs.agora.io/cn/Agora%20Platform/terms?%20#rtc-sdk) 的应用。

## 开通阿里智能语音审核服务

第一次使用阿里智能语音审核服务之前，你需要首先在 [Agora 控制台](https://console.agora.io/)购买套餐包，开通审核服务。步骤如下：

1. 登录控制台，点击左侧导航栏按钮 ![](https://web-cdn.agora.io/docs-files/1603161685526) 进入**云市场**页面。
2. 在**云市场首页**，点击**阿里智能语音审核**，进入详情页。
3. 选择套餐，时长和数量后，完成支付。

支付成功后，你就可以在**产品用量**页面看到阿里智能语音审核的使用情况。

## 实现方法

要对频道内的音频进行实时审核，你需要调用阿里智能语音审核 RESTful API，发送 HTTPS 请求。阿里智能语音审核会将审核结果以 HTTP 请求的形式发送到你指定的地址。

下图为实现音频审核需要调用的 API 时序图。

![](https://web-cdn.agora.io/docs-files/1603161712750)

### 数据格式

所有的请求都发送给域名：`api.agora.io`。仅支持 HTTPS 协议。

- 请求：请求的格式详见下面的示例
- 响应：响应内容的格式为 JSON

> 所有的请求 URL 和请求包体内容都是区分大小写的。

### 通过 Basic HTTP 认证

Agora RESTful API 要求 Basic HTTP 认证。每次发送 HTTP 请求时，都必须在请求头部填入 `Authorization`字段。关于如何生成该字段的值，请参考 [HTTP 基本认证](https://docs.agora.io/cn/faq/restful_authentication)。

### 开始审核

#### 获取 resource ID

在开始审核前，必须调用 `acquire` 方法请求一个用于审核的 resource ID。调用该方法成功后，你可以从 HTTP 响应包体中的 `resourceId` 字段得到一个 resource ID。这个 resource ID 的时效为五分钟，你需要在五分钟内用这个 resource ID 开始审核。

> 一个 resource ID 仅可用于一次审核。

#### 加入频道

获得 resource ID 后，在五分钟内调用 `start` 方法加入频道开始审核。调用该方法成功后，你可以从 HTTP 响应包体中获得一个 sid （审核 ID)。该 ID 是一次审核周期的唯一标识。
<div class="alert note">阿里智能语音审核不支持 String 用户 ID（User Account）。请确保频道内所有用户均使用整型 UID。同时，在调用 <code>start</code> 方法时，请确保 <code>uid</code> 字段引号内为整型 UID。</div>

### 查询审核状态

审核过程中，你可以多次调用 `query` 方法查询审核状态。调用该方法成功后，你可以从 HTTP 响应包体中获得审核的状态。

### 停止审核

调用 `stop` 方法离开频道，停止审核。调用该方法成功后，你可以从 HTTP 响应包体中获得审核的状态。

> 当频道空闲（无用户）超过预设时间（默认为 30 秒） 后，阿里智能语音审核会自动停止。

## 回调通知

审核结果将直接通知到你通过 `callbackAddr` 参数设置的地址。详情见[审核结果的回调](../../cn/Video/restful_api_ali_audio.md)。

## 开发注意事项

- 调用 `start` 之前必须调用 `acquire` 获取一个 resource ID。
- 一个 resource ID 只可用于一次 `start` 请求。
- 调用 `stop` 之后不能再调用 `query`。

下面列出一些常见的调用错误：

- `acquire` ➡ `start` ➡ `start`

  用同一个 resource ID 重复调用 `start`，会收到 HTTP 状态码 201，错误码 7。

- `acquire` ➡ `start` ➡ `acquire` ➡ `start`

  用相同的参数重复调用 `acquire` 和 `start`，会收到 HTTP 状态码 400，错误码 53。

- `acquire` ➡ `start` ➡ 停止审核 ➡ `query`

  停止审核后再去调用 `query`，会收到 HTTP 状态码 404，错误码 404。停止审核有以下几种情况：

  - 主动调用 `stop`。
  - 频道内没有用户超过设定的时间，自动停止审核。
  - 异步检查参数错误导致审核停止。

- `acquire` ➡ `start` ➡ `stop` & `query`

  调用 `stop` 的过程中调用 `query`，会影响 `stop` 的响应内容，响应的 HTTP 状态码为 206。

如果你在集成和使用中遇到其他问题，可以参考[常见错误](../../cn/Video/restful_api_ali_audio.md)。

## 相关文档

- [阿里语音智能审核 RESTful API](../../cn/Audio%20Broadcast/restful_api_ali_audio.md)
