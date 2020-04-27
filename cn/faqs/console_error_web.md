
---
title: 常见的 Web 浏览器控制台报错
description: 
platform: Web
updatedAt: Mon Apr 27 2020 17:50:28 GMT+0800 (CST)
---
# 常见的 Web 浏览器控制台报错
将 Agora Web SDK 集成到你的 Web 应用后，遇到问题时可以通过浏览器控制台打印的日志进行调试。本文列出控制台日志中常见的错误和原因。

## Cannot read property "appendChild" of null

### 错误原因

播放指定的 DOM 不存在或者 ID 没有找到。

### 解决办法

确保在调用 `Stream.play` 方法时已生成相应的父容器。

## Cannot read property 'stringuid' of undefined

### 错误原因

在还未成功加入频道时调用了 `Client.publish` 方法。

### 解决办法

检查你的集成逻辑，确保在调用 `Client.publish` 时已成功加入频道。

## Failed to execute 'addStream' on 'RTCPeerConnection': parameter 1 is not of type 'MediaStream'

### 错误原因

在 `Stream.init` 尚未成功时就调用了 `Stream.publish` 。

### 解决办法

在 `Stream.init` 成功的回调中调用 `publish` 方法。

## Failed to set remote answer sdp: Called in wrong state: kStable

### 错误原因

调用 `switchDevice` 导致报错。

### 解决办法

该错误没有任何影响，忽略即可。

## Invalid elementID

### 错误原因

 `Stream.play` 中指定的 `HTMLElementID` 不合法。

### 解决办法

确保 `HTMLElementID` 字符串仅包含 ASCII 字符集中的字母和数字，或 "_"、"-"、"."，且字符串长度大于 0 小于 256 字节。

## INVALID_VENDOR_KEY

### 错误原因

可能的原因有：

- 使用的 App ID 无效。例如 App ID 对应的 Agora 项目处于禁用状态，或者新建的 Agora 项目 App ID 尚未生效。
- 使用的 token 无效。

### 解决办法

检查 `Client.init` 中 `appId` 和 `Client.join` 中 `tokenOrKey` 的设置，确保使用有效的 App ID 和 token。

## Media access MEDIA_NOT_SUPPORT: video/audio streams not supported yet /enumerateDevices() not supported

### 错误原因

可能的原因有：

- 使用的浏览器不兼容 Agora Web SDK。
- 没有使用 HTTPS 协议或者 localhost 打开页面。

### 解决办法

- 使用 Agora Web SDK [支持的浏览器](https://docs.agora.io/cn/faq/browser_support)。
- 使用 HTTPS 协议或者 localhost 打开页面。

## None Ice Candidate not allowed

### 错误原因

建立 WebRTC 连接时未找到 [ICE](https://developer.mozilla.org/zh-CN/docs/Glossary/ICE) (Interactive Connectivity Establishment) candidate。

<div class="alert info">Candidate 是指用于媒体传输的候选地址，包含 IP 地址和端口信息。</div>

### 解决办法

根据是否开启[云代理](https://docs.agora.io/cn/Interactive%20Broadcast/cloud_proxy_web?platform=Web)，建立连接时会使用不同类型的 candidate，因此解决办法也不同。

- 如果你开启了云代理，SDK 会从 TURN Server 获取 relay candidate，所以你需要检查是否已经将 Agora 指定的云代理 IP 地址和端口添加到白名单，并确保客户端可以正常连接到 TURN Server。
- 如果没有开启云代理功能，SDK 会从本地设备获取 host candidate。获取失败通常是由于安全策略设置导致的，你可以进行以下排查。
  - 检查浏览器是否安装了禁用 WebRTC 的插件。
  - 确保本地设备的防火墙设置未禁用 UDP，且已经将[指定的域名和端口](https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms#web-sdk)添加到白名单。

## UID_CONFLICT

### 错误原因

 一个频道中有多个相同的 `uid`。

### 解决办法

确保频道中每个用户的 `uid` 是唯一的。

## Uncaught DOMException: Failed to execute 'addTransceiver' on 'RTCPeerConnection': This operation is only supported in 'unified-plan'.

### 错误原因

在浏览器上开启了模拟移动设备调试。

### 解决办法

Agora Web SDK 不支持模拟移动设备调试，请勿使用模拟器进行调试。

## Uncaught TypeError: Failed to execute 'createObjectURL' on 'URL': No function was found that matched the signature provided

### 错误原因

在浏览器上开启了模拟移动设备调试。

### 解决办法

Agora Web SDK 不支持模拟移动设备调试，请勿使用模拟器进行调试。

## User is not in the session

### 错误原因

该错误表示尚未建立连接，通常是因为调用 API 时序不正确导致的。例如调用了 `Client.leave` 之后调用 `Client.publish`。

### 解决办法

检查你的 API 调用顺序。
