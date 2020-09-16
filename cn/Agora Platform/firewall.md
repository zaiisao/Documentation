
---
title: 应用企业防火墙限制
description: 
platform: All Platforms
updatedAt: Tue Sep 15 2020 10:23:47 GMT+0800 (CST)
---
# 应用企业防火墙限制
## 概览

对于有外网访问限制的网络环境，在使用 Agora 相关服务之前，需要添加本文中的域名白名单、不对 IP 地址设限、且开放相应端口。

如果你的防火墙不能完全满足上述操作，我们推荐使用 Agora 云代理功能连接到 Agora 服务。各产品云代理文档如下：

- [RTC Native SDK 使用云代理](../../cn/Agora%20Platform/cloudproxy_native.md)
- [RTC Web SDK 使用云代理](../../cn/Agora%20Platform/cloud_proxy_web.md)
- [本地服务端录制 SDK 使用云代理](../../cn/Agora%20Platform/cloudproxy_recording.md)

<div class="alert note"><li>如果你想通过 VPN 使用 Agora 的服务，请确保已联系 VPN 运营商将对应端口添加到白名单中，否则可能会遇到通话失败等未定义行为。</li><li>如无特殊说明，源地址就是集成了 Agora SDK 的客户端。</li></div>

## Agora RTC SDK

### Native SDK

Agora RTC Native SDK 不支持通过添加域名及端口的方式提供音视频功能。Agora 推荐你参考[使用云代理](../../cn/Agora%20Platform/cloudproxy_native.md)进行连接。

### Web SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
.edge.agora.io
.agoraio.cn
.edge.agoraio.cn
```

| 目标端口 | 协议 | 操作 |
| -------------- | ----------------- | -------------|
|  80；443；3433；5668；5669；5866 - 6000；6080；6443；8667；9667；30011 - 30013（用于推流）| TCP | 允许 |
|  3478；5866 - 6000（2.9.0 及以后版本）；10000 - 65535 （2.9.0 以前版本）| UDP | 允许 |


### 微信小程序 SDK

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

## Agora RTM SDK

### Native SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
```

| 目标端口 | 协议 | 操作 |
| -------------- | ---------- | -------------- |
| 9130；9131；9140 | TCP | 允许 |
| 1080; 8000; 8130;  9120; 9121; 9700; 25000 | UDP | 允许 |

### Web SDK

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

### 微信小程序 SDK

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

## Agora 本地服务端录制 SDK

Agora 本地服务端录制 SDK 不支持通过添加域名及端口的方式提供音视频录制功能。Agora 推荐你参考[使用云代理](../../cn/Agora%20Platform/cloudproxy_recording.md)进行连接。


## Agora 游戏 SDK

Agora 游戏 SDK 不支持通过添加域名及端口的方式提供音视频功能。Agora 推荐你参考[使用云代理](../../cn/Agora%20Platform/cloudproxy_native.md)进行连接。
