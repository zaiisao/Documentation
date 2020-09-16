
---
title: Firewall Requirements
description: 
platform: All Platforms
updatedAt: Tue Sep 15 2020 10:32:32 GMT+0800 (CST)
---
# Firewall Requirements
This page describes the firewall requirements for different Agora SDKs. Before accessing Agora’s services, ensure that you open the local firewall ports and whitelist the domains specified in this article.

If you cannot add these ports and whitelist domains on your firewall, Agora recommends using the cloud proxy service to access Agora's service:
- [Cloud Proxy for the RTC Native SDK](../../en/Agora%20Platform/cloudproxy_native.md)
- [Cloud Proxy for the RTC Web SDK](../../en/Agora%20Platform/cloud_proxy_web.md)
- [Cloud Proxy for the On-premise Recording SDK](../../en/Agora%20Platform/cloudproxy_recording.md)

<div class="alert note"><li>If you use Agora SDKs across the VPN, ensure that you contact the VPN operator to add the corresponding domains on the whitelist, otherwise, you may encounter undefined issues such as failing to start a call. </li><li>Unless it is otherwise specified, the sources are the clients that integrate the Agora SDK.</li></div>

## Agora RTC SDK

### Native SDK

The Agora RTC Native SDK does not support accessing Agora's service by adding whitelist domians. To do so, Agora recommends using the cloud proxy service. For details, see [Cloud Proxy for the RTC Native SDK](../../en/Agora%20Platform/cloudproxy_native.md).


### Web SDK

Add the following destination domains and the corresponding ports to the firewall whitelist:

```
.agora.io
.edge.agora.io
.agoraio.cn
.edge.agoraio.cn
```

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 80; 443; 3433; 5668; 5669; 5866 - 6000; 6080; 6443; 8667; 9667; 30011 - 30013 (for RTMP converter)| TCP              |  Allow |
| 3478; 5866 - 6000 (2.9.0 or later); 10000 - 65535 (before 2.9.0)   |  UDP  | Allow |


## Agora RTM SDK

### Native SDK

Add the following destination domains and the corresponding ports to the firewall whitelist:

```
.agora.io
```

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 9130; 9131     | TCP              |  Allow |
| 8000; 1080; 25000   |  UDP  | Allow |

### Web SDK

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
| 443; 6443; 9591; 9593     | TCP              |  Allow |

## Agora On-premise Recording SDK

The Agora On-premise Recording SDK does not support accessing Agora's service by adding whitelist domians. To do so, Agora recommends using the cloud proxy service. For details, see [Cloud Proxy for the On-premise Recording SDK](../../en/Agora%20Platform/cloudproxy_recording.md).

## Agora Gaming SDK

The Agora Gaming SDK does not support accessing Agora's service by adding whitelist domians. To do so, Agora recommends using the cloud proxy service. For details, see [Cloud Proxy for the Gaming SDK](../../en/Agora%20Platform/cloudproxy_native.md).
