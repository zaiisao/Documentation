
---
title: 应用企业防火墙限制
description: 
platform: All Platforms
updatedAt: Fri May 22 2020 04:14:37 GMT+0800 (CST)
---
# 应用企业防火墙限制
对于有外网访问限制的公司，在使用 Agora 相关服务之前，需要添加防火墙白名单。

如果你的防火墙不允许打开本文端口和白名单，我们推荐使用 Agora 云代理功能连接到 Agora 服务。各产品云代理文档如下：

- [RTC Native SDK 使用云代理](../../cn/Agora%20Platform/cloudproxy_native.md)
- [RTC Web SDK 使用云代理](../../cn/Agora%20Platform/cloud_proxy_web.md)
- [本地服务端录制 SDK 使用云代理](../../cn/Agora%20Platform/cloudproxy_recording.md)

<div class="alert note"><li>如果你想通过 VPN 使用 Agora 的服务，请确保已联系 VPN 运营商将对应端口添加到白名单中，否则可能会遇到通话失败等未定义行为。</li><li>如无特殊说明，源地址就是集成了 Agora SDK 的客户端。</li></div>

## Agora RTC SDK

### Native SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
qoslbs.agoralab.co
qos.agoralab.co
ap1.agora.io
ap2.agora.io
ap3.agora.io
ap4.agora.io
ap5.agora.io
ap.agoraio.cn
vocs1.agora.io
vocs2.agora.io
vocs3.agora.io
vocs4.agora.io
vocs5.agora.io
```

| 目标端口 | 协议 | 操作 |
| ---------- | ------------------------------------------------ | -----------------|
| 1080；8000；9700；25000；30000     | TCP              |  允许 |
| 1080；4000 - 4030；7000；8000；8913；9700；25000   |  UDP  | 允许 |


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
|  80；443；3433；5668；5669；5866 - 6000；6080；6443；8667；9667 | TCP | 允许 |
|  3478；5866 - 6000（2.9.0 及以后版本）；10000 - 65535 （2.9.0 以前版本）| UDP | 允许 |


### 微信小程序 SDK

在微信公众平台账号的开发设置中，给予下述域名请求权限：

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

<div class="alert note">为获得优质的音视频通话体验，Agora 建议你使用 UDP 端口。与 TCP 相比，UDP 更注重通话的时效性，因此能在填补丢包的同时同时将通话延迟降到最低。</div>

## Agora RTM SDK

### Native SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
```

| 目标端口 | 协议 | 操作 |
| -------------- | ---------- | -------------- |
| 9130；9131；9140 | TCP | 允许 |
| 8000；1080；25000 | UDP | 允许 |

### Web SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
```

| 目标端口 | 协议 | 操作 |
| --------------- | ------------ | ------------ |
| 443；9591；9593；9600 | TCP | 允许 |


## Agora 本地服务端录制 SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
qoslbs.agoralab.co
qos.agoralab.co
ap1.agora.io
ap2.agora.io
ap3.agora.io
ap4.agora.io
ap5.agora.io
ap.agoraio.cn
vocs1.agora.io
vocs2.agora.io
vocs3.agora.io
vocs4.agora.io
vocs5.agora.io
```

| 目标端口 | 协议 | 操作 |
| --------------- | -------------- | ----------- |
| 1080；8000；9700；25000；30000 | TCP | 允许 |
| 双向 1080；7000；8000；8913；9700；25000；本地端口 4000-4030；所有的录制进程所使用的单向下行本地端口 | UDP | 允许 |

<div class="alert note">单向下行接口：录制一个频道的内容需要开启一个对应的录制进程；单个录制进程需要使用 4 个单向下行端口。进程（包括各个录制进程和系统进程）之间不得有端口冲突。<ul><li>Agora 建议你指定录制进程使用端口的范围。你可以为多个录制进程统一配置较大的端口范围（Agora 建议 40000 ~ 41000 或更大）。此时，录制 SDK 会在指定范围内为每个录制进程分配端口，并避免端口的冲突。要设置端口范围，您需要配置参数 <code>lowUdpPort</code> 和 <code>highUdpPort</code>。</li><li>如果不指定参数 <code>lowUdpPort</code> 和 <code>highUdpPort</code>，录制进程所使用的端口为随机端口，会有端口冲突的风险。</li></ul></div>


## Agora 游戏 SDK

将以下目标域名及对应的端口添加到防火墙白名单：

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
ap.agoraio.cn
vocs1.agora.io
vocs2.agora.io
vocs3.agora.io
vocs4.agora.io
vocs5.agora.io
```

| 目标端口 | 协议 | 操作 |
| -------------- | --------- | ------------ |
| 1080；8000 | TCP | 允许 |
| 1080；4000 - 4030；8000；9700；25000 | UDP | 允许 |
