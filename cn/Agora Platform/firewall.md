
---
title: 应用企业防火墙限制
description: 
platform: All Platforms
updatedAt: Wed Nov 04 2020 02:39:12 GMT+0800 (CST)
---
# 应用企业防火墙限制
## 概览

为允许你在有网络访问限制的环境中使用 Agora 产品，Agora 提供两种解决方案：防火墙白名单方案和 Agora 云代理方案。

下表列出 Agora 产品对两种方案的支持情况：

|Agora 产品| 防火墙白名单|Agora 云代理|
|---------|-------------------|------------------|
|RTC SDK（Native、第三方框架）| <font color="red">✘ | <font color="green">✔ |
|RTC SDK（Web） | <font color="green">✔ | <font color="green">✔|
|RTC SDK（微信小程序）|<font color="green"> ✔ | <font color="red">✘ |
|RTM SDK（Native、Web、微信小程序）|<font color="green"> ✔ | <font color="red">✘ |
|本地服务端录制 SDK| <font color="red">✘ | <font color="green">✔ |
|互动游戏 SDK | <font color="red">✘ | <font color="red">✘ |

- 使用防火墙白名单方案时，请参考**本文**添加域名白名单，开放相应端口且不对 IP 地址设限。
- 使用 Agora 云代理方案时，请参考如下文档：
    - [RTC SDK（Native、第三方框架）的云代理](../../cn/Agora%20Platform/cloudproxy_native.md)
    - [RTC SDK（Web）的云代理](../../cn/Agora%20Platform/cloud_proxy_web.md)
    - [本地服务端录制 SDK 的云代理](../../cn/Agora%20Platform/cloudproxy_recording.md)

<div class="alert note"><li>如果你想通过 VPN 使用 Agora 的服务，请确保已联系 VPN 运营商将对应端口添加到白名单中，否则可能会遇到通话失败等未定义行为。</li><li>如无特殊说明，源地址就是集成了 Agora SDK 的客户端。</li></div>

## RTC SDK

### RTC SDK（Web）

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
.edge.agora.io
.agoraio.cn
.edge.agoraio.cn
```

| 目标端口 | 协议 | 操作 |
| -------------- | ----------------- | -------------|
|  80；443；3433；4700 - 5000；5668；5669；6080；6443；8667；9667；30011 - 30013（用于推流）| TCP | 允许 
|  3478；4700 - 5000（2.9.0 及以后版本）；10000 - 65535 （2.9.0 以前版本）| UDP | 允许 |


### RTC SDK（微信小程序）

将以下目标域名及对应的端口添加到防火墙白名单：

```
miniapp.agoraio.cn
miniapp-1.agoraio.cn
miniapp-2.agoraio.cn
miniapp-3.agoraio.cn
miniapp-4.agoraio.cn
uni-webcollector.agora.io
miniapp.agoraio.cn
```

| 目标端口 | 协议 | 操作 |
| ---------------| -------- | ------------ |
| 80；443；3433；5668；5669；5866 - 5890；6080；6443；8667；9667 | TCP | 允许 |
| 3478；10000 - 65535 | UDP | 允许 |

## RTM SDK

### RTM SDK（Native）

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
```

| 目标端口 | 协议 | 操作 |
| -------------- | ---------- | -------------- |
| 9130；9131；9140 | TCP | 允许 |
| 1080; 8000; 8130;  9120; 9121; 9700; 25000 | UDP | 允许 |

### RTM SDK（Web）

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
.agoraio.cn
.edge.agora.io
.edge.agoraio.cn
ap-web-1.agora.io
ap-web-1.agoraio.cn
ap-web-2.agora.io
ap-web-2.agoraio.cn
webcollector-rtm.agora.io
logservice-rtm.agora.io
webcollector-rtm.agoraio.cn
logservice-rtm.agoraio.cn
```

| 目标端口 | 协议 | 操作 |
| --------------- | ------------ | ------------ |
| 443; 6443; 9591; 9593; 9601 | TCP | 允许 |

### RTM SDK（微信小程序）

```
miniapp.agoraio.cn
webcollector-rtm.agora.io
logservice-rtm.agora.io
ap-web-1.agoraio.cn
ap-web-2.agoraio.cn
ap-web-3.agoraio.cn
ap-web-4.agoraio.cn
```

| 目标端口 | 协议 | 操作 |
| --------------- | ------------ | ------------ |
| 443 ; 6443 | TCP | 允许 |
