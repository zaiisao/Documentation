
---
title: Firewall Requirements
description: 
platform: All Platforms
updatedAt: Wed Sep 11 2019 02:21:33 GMT+0800 (CST)
---
# Firewall Requirements
This page describes the firewall requirements for different Agora SDKs. Before accessing Agora’s services, ensure that you open the local firewall ports and whitelist the domains specified in this article.

If you cannot add these ports and whitelist domains on your firewall, refer to the following guides to use the cloud proxy service to access Agora's service:
- [Cloud Proxy for the RTC Native SDK](../../en/Agora%20Platform/cloudproxy_native.md)
- [Cloud Proxy for the RTC Web SDK](../../en/Agora%20Platform/cloud_proxy_web.md)
- [Cloud Proxy for the Recording SDK](../../en/Agora%20Platform/cloudproxy_recording.md)

## Agora RTC SDK

### Native SDK

Firewall ports:

| Port type | Whitelist ports                                       |
| ---------- | ------------------------------------------------ |
| TCP ports   | 1080; 8000; 9700; 25000; 30000                   |
| UDP ports   | 1080; 4000 to 4030; 7000; 8000; 8913; 9700; 25000 |

Whitelist domains:

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

Firewall ports:

| Port type | Whitelist ports                                       |
| -------- | ------------------------------------------------------------ |
| TCP ports | 80; 443; 3433; 5668; 5669; 5866 to 5890; 6080; 6443; 8667; 9667 |
| UDP ports | 3478; 5866 - 6000 (2.9.0 or later); 10000 - 65535 (before 2.9.0)            |

Whitelist domains:

```
.agora.io
.agoraio.cn
```

## Agora RTM SDK

### Native SDK

Firewall ports:

| Port type | Whitelist ports                                       |
| -------------- | ----------------- |
| TCP ports       | 9130; 9131        |
| UDP ports       | 8000; 1080; 25000 |

Whitelist domains:

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
```

### Web SDK

Firewall ports:

| Port type | Whitelist ports                                       |
| -------- | ---------- |
| TCP ports | 443; 6151; 6153        |

Whitelist domains:

```
.agora.io
```

## Agora Signaling SDK

Firewall ports:

| Port type | Whitelist ports                                       |
| -------- | ----------------- |
| TCP ports | 1080; 8001 to 8199; 10000 to 10010; 10100 to 10110 |
| UDP ports | 8180 to 8199       |

Whitelist domains:

```
 .agora.io
 qoslbs.agoralab.co
 qos.agoralab.co
```

## Agora Recording SDK

Firewall ports:

| Port type | Whitelist ports                                       |
| -------- | ------------------------------------------------------------ |
| TCP ports | 1080; 8000                                                   |
| UDP ports | Duplex ports 1080, 4000 to 4030; 8000; 9700; 25000; and simplex downstream ports used by recording processes. |

Whitelist domains:

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

To record the content in channels, you need one recording process for each of the channels. One recording thread requires four simplex downstream ports. There must be no port conflict among processes, including system processes and all recording processes.

- Agora recommends that you specify the range of ports used by the recording processes. Configure a large range for all recording processes (Agora recommends 40000 to 41000 or larger). If so, the Recording SDK assigns ports to each recording process within the specified range and avoids port conflicts automatically. To set the port range, you need to configure the lowUdpPort and highUdpPort parameters.
- If the lowUdpPort and highUdpPort parameters are not specified, the ports used by the recording processes are at random, which may cause port conflicts.

## Agora Gaming SDK

Firewall ports:

| Port type | Whitelist ports                                       |
| -------- | ------------------------------------ |
| TCP ports | 1080; 8000                           |
| UDP ports | 1080; 4000 to 4030; 8000; 9700; 25000 |

Whitelist domains:

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
```

You must use UDP ports rather than TCP ports for superior voice and video quality since UDP prioritizes timeliness over reliability.
