
---
title: 应用企业防火墙限制
description: 
platform: All Platforms
updatedAt: Wed Sep 11 2019 02:06:35 GMT+0800 (CST)
---
# 应用企业防火墙限制
对于有外网访问限制的公司，在使用 Agora 相关服务之前，需要添加防火墙白名单。

如果你的防火墙不允许打开本文端口和白名单，可以使用 Agora 云代理功能连接到 Agora 服务。各产品云代理文档如下：

- [RTC Native SDK 使用云代理](../../cn/Agora%20Platform/cloudproxy_native.md)
- [RTC Web SDK 使用云代理](../../cn/Agora%20Platform/cloud_proxy_web.md)
- [录制 SDK 使用云代理](../../cn/Agora%20Platform/cloudproxy_recording.md)

## Agora RTC SDK

### Native SDK

防火墙端口：

| 端口 | 白名单项目                                       |
| ---------- | ------------------------------------------------ |
| TCP 端口   | 1080；8000；9700；25000；30000                   |
| UDP 端口   | 1080；4000 - 4030；7000；8000；8913；9700；25000 |

域名白名单：

```
qoslbs.agoralab.co
qos.agoralab.co
ap1.agora.io
ap2.agora.io
ap3.agora.io
ap4.agora.io
ap5.agora.io
```

### Web SDK

防火墙端口：

| 端口  | 白名单项目                                                   |
| -------- | ------------------------------------------------------------ |
| TCP 端口 | 80；443；3433；5668；5669；5866 - 5890；6080；6443；8667；9667 |
| UDP 端口 | 3478                                          |

> 如果你使用了代理服务器，则需打开端口 3433。反之，则不需要。

域名白名单：

```
 .agora.io
 .agoraio.cn
```

### 微信小程序 SDK

防火墙端口：

| 端口  | 白名单项目                                                   |
| -------- | ------------------------------------------------------------ |
| TCP 端口 | 80；443；3433；5668；5669；5866 - 5890；6080；6443；8667；9667 |
| UDP 端口 | 3478；10000 - 65535                                          |

> 如果你使用了代理服务器，则需打开端口 3433。反之，则不需要。

域名白名单：

```
miniapp.agoraio.cn
miniapp-1.agoraio.cn
miniapp-2.agoraio.cn
miniapp-3.agoraio.cn
miniapp-4.agoraio.cn
uni-webcollector.agora.io
miniapp.agoraio.cn
```

> 在微信公众平台账号的开发设置中，给予上述域名请求权限。

## Agora RTM SDK

### Native SDK

防火墙端口：

|端口 | 白名单项目        |
| -------------- | ----------------- |
| TCP 端口       | 9130；9131        |
| UDP 端口       | 8000；1080；25000 |

域名白名单：

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
```

### Web SDK

防火墙端口：

| 端口     | 白名单项目 |
| -------- | ---------- |
| TCP 端口 | 443；6151；6153        |

域名白名单：

```
.agora.io
```

## Agora Signaling SDK

防火墙端口：

| 端口     | 白名单项目        |
| -------- | ----------------- |
| TCP 端口 | 1080；8001 - 8199；10000 - 10010；10100 - 10110 |
| UDP 端口 | 8180 - 8199       |

域名白名单：

```
 .agora.io
 qoslbs.agoralab.co
 qos.agoralab.co
```

## Agora Recording SDK

防火墙端口：

| 端口     | 白名单项目                                                   |
| -------- | ------------------------------------------------------------ |
| TCP 端口 | 1080；8000                                                   |
| UDP 端口 | 双向 1080；4000 - 4030；8000；9700；25000 和所有的录制进程所使用的单向下行端口 |

域名白名单：

```
qoslbs.agoralab.co
qos.agoralab.co
ap1.agora.io
ap2.agora.io
ap3.agora.io
ap4.agora.io
ap5.agora.io
```

**Note:**

录制一个频道的内容需要开启一个对应的录制进程；单个录制进程需要使用 4 个单向下行端口。进程（包括各个录制进程和系统进程）之间不得有端口冲突。
- Agora 建议你指定录制进程使用端口的范围。你可以为多个录制进程统一配置较大的端口范围（Agora 建议 40000 ~ 41000 或更大）。此时，录制 SDK 会在指定范围内为每个录制进程分配端口，并避免端口的冲突。要设置端口范围，您需要配置参数 `lowUdpPort` 和 `highUdpPort`；
- 如果不指定参数`lowUdpPort` 和 `highUdpPort` ，录制进程所使用的端口为随机端口，会有端口冲突的风险。

## Agora Gaming SDK

防火墙端口：

| 端口     | 白名单项目                           |
| -------- | ------------------------------------ |
| TCP 端口 | 1080；8000                           |
| UDP 端口 | 1080；4000 - 4030；8000；9700；25000 |

域名白名单：

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
```

为获得优质的音视频通话体验，Agora 建议你使用 UDP 端口。与 TCP 相比，UDP 更注重通话的时效性，因此能在填补丢包的同时同时将通话延迟降到最低。
