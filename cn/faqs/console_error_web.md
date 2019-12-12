
---
title: 常见的控制台报错
description: 
platform: Web
updatedAt: Mon Dec 02 2019 17:11:31 GMT+0800 (CST)
---
# 常见的控制台报错
将 Agora Web SDK 集成到你的 Web 应用后，遇到问题时可以通过控制台打印的日志进行调试。本文列出控制台日志中常见的错误和原因。

## User is not in the session

### 错误原因

该错误表示尚未建立连接，通常是因为调用 API 时序不正确导致的。例如调用了 `Client.leave` 之后调用 `Client.publish`。

### 解决办法

检查你的 API 调用顺序。

## Cannot read property "appendChild" of null

### 错误原因

播放指定的 DOM 不存在或者 ID 没有找到。

### 解决办法

确保在调用 `Stream.play` 方法时已生成相应的父容器。

## Invalid elementID

### 错误原因

 `Stream.play` 中指定的 `HTMLElementID` 不合法。

### 解决办法

确保 `HTMLElementID` 字符串仅包含 ASCII 字符集中的字母和数字，或 "_"、"-"、"."，且字符串长度大于 0 小于 256 字节。

## UID_CONFLICT

### 错误原因

 一个频道中有多个相同的 `uid`。

### 解决办法

确保频道中每个用户的 `uid` 是唯一的。

## Failed to execute 'addStream' on 'RTCPeerConnection': parameter 1 is not of type 'MediaStream'

### 错误原因

在 `Stream.init` 尚未成功时就调用了 `Stream.publish` 。

### 解决办法

在 `Stream.init` 成功的回调中调用 `publish` 方法。

## Media access MEDIA_NOT_SUPPORT: video/audio streams not supported yet /enumerateDevices() not supported

### 错误原因

可能的原因有：

- 使用的浏览器不兼容 Agora Web SDK。
- 没有使用 HTTPS 协议或者 localhost 打开页面。

### 解决办法

- 使用 Agora Web SDK [支持的浏览器](https://docs.agora.io/cn/faq/browser_support)。
- 使用 HTTPS 协议或者 localhost 打开页面。

## Uncaught TypeError: Failed to execute 'createObjectURL' on 'URL': No function was found that matched the signature provided

### 错误原因

在浏览器上开启了模拟移动设备调试。

### 解决办法

Agora Web SDK 不支持模拟移动设备调试，请勿使用模拟器进行调试。

## Uncaught DOMException: Failed to execute 'addTransceiver' on 'RTCPeerConnection': This operation is only supported in 'unified-plan'.

### 错误原因

在浏览器上开启了模拟移动设备调试。

### 解决办法

Agora Web SDK 不支持模拟移动设备调试，请勿使用模拟器进行调试。

## INVALID_VENDOR_KEY

### 错误原因

可能的原因有：

- 使用的 App ID 无效。例如 App ID 对应的 Agora 项目处于禁用状态，或者新建的 Agora 项目 App ID 尚未生效。
- 使用的 token 无效。

### 解决办法

检查 `Client.init` 中 `appId` 和 `Client.join` 中 `tokenOrKey` 的设置，确保使用有效的 App ID 和 token。

## Failed to set remote answer sdp: Called in wrong state: kStable

### 错误原因

调用 `switchDevice` 导致报错。

### 解决办法

该错误没有任何影响，忽略即可。

## Cannot read property 'stringuid' of undefined

### 错误原因

在还未成功加入频道时调用了 `Client.publish` 方法。

### 解决办法

检查你的集成逻辑，确保在调用 `Client.publish` 时已成功加入频道。

