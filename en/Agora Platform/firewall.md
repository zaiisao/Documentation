
---
title: Firewall Requirements
description: 
platform: All Platforms
updatedAt: Fri Oct 30 2020 09:49:36 GMT+0800 (CST)
---
# Firewall Requirements
## Introduction

To allow you to use Agora products in environments with restricted network access, Agora provides two solutions: the firewall whitelist and the Agora cloud proxy.

The following table lists the support of Agora products for the two solutions:

|Agora Products| Firewall Whitelist|Agora Cloud Proxy|
|---------|-------------------|------------------|
|RTC SDK (Native, third-party frameworks) | <font color="red">✘ | <font color="green">✔ |
|RTC SDK (Web) | <font color="green">✔ | <font color="green">✔|
|RTM SDK (Native, Web)|<font color="green"> ✔ | <font color="red">✘ |
|On-premise Recording SDK| <font color="red">✘ | <font color="green">✔ |
|Interactive Gaming SDK | <font color="red">✘ | <font color="red">✘ |

- When using the firewall whitelist, refer to **this page** to add the domains and ports to the firewall whitelist, and do not set restrictions on IP addresses.
- When using Agora cloud proxy, refer to the following pages:
    - [Cloud Proxy for RTC SDK (Native, third-party frameworks)](../../en/Agora%20Platform/cloudproxy_native.md)
    - [Cloud Proxy for RTC SDK (Web) ](../../en/Agora%20Platform/cloud_proxy_web.md)
    - [Cloud proxy for On-premise Recording SDK](../../en/Agora%20Platform/cloudproxy_recording.md)

<div class="alert note"><li>If you want to use Agora products across a VPN, ensure that you have contacted the VPN operator to add the corresponding ports to the whitelist; otherwise, you may encounter undefined behaviors such as call failure. </li><li>Unless otherwise specified, the source address is the client that integrates the Agora SDK. </li></div>

## RTC SDK

### RTC SDK (Web)

Add the following destination domains and the corresponding ports to the firewall whitelist:

```
.agora.io
.edge.agora.io
.agoraio.cn
.edge.agoraio.cn
```

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 80; 443; 3433; 5668; 5669; 4700 - 5000; 6080; 6443; 8667; 9667; 30011 - 30013 (for RTMP converter)| TCP              |  Allow |
| 3478; 4700 - 5000 (2.9.0 or later); 10000 - 65535 (before 2.9.0)   |  UDP  | Allow |

## RTM SDK

### RTM SDK (Native)

Add the following destination domains and the corresponding ports to the firewall whitelist:

```
.agora.io
```

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 9130；9131；9140	    | TCP              |  Allow |
| 1080; 8000; 8130; 9120; 9121; 9700; 25000	   |  UDP  | Allow |

### RTM SDK (Web)

Add the following destination domains and the corresponding ports to the firewall whitelist:

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

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 443; 6443; 9591; 9593; 9601	    | TCP              |  Allow |
