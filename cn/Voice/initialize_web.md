
---
title: 创建并初始化 Client 对象
description: Web SDK 初始化客户端对象
platform: Web
updatedAt: Thu Nov 01 2018 08:04:40 GMT+0000 (UTC)
---
# 创建并初始化 Client 对象
# 创建并初始化 Client 对象

## 创建 Client 对象
用 `AgoraRTC.createClient` 方法创建 Client 对象，设置 `mode` 和 `codec` 。

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

## 初始化 Client 对象
创建 Client 对象后，将项目的 App ID 填入 `client.init` 方法，即可初始化 Client。

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```


