
---
title: Firewall Requirements
description: 
platform: All Platforms
updatedAt: Mon May 25 2020 05:42:04 GMT+0800 (CST)
---
# Firewall Requirements
This page describes the firewall requirements for different Agora SDKs. Before accessing Agora’s services, ensure that you open the local firewall ports and whitelist the domains specified in this article.

If you cannot add these ports and whitelist domains on your firewall, Agora recommends using the cloud proxy service to access Agora's service:
- [Cloud Proxy for the RTC Native SDK](../../en/Agora%20Platform/cloudproxy_native.md)
- [Cloud Proxy for the RTC Web SDK](../../en/Agora%20Platform/cloud_proxy_web.md)
- [Cloud Proxy for the On-premise Recording SDK](../../en/Agora%20Platform/cloudproxy_recording.md)

<div class="alert note"><li>If you use Agora SDKs across the VPN, ensure that you contact the VPN operator to add the corresponding domains on the whitelist, otherwise, you may encounter undefined issues such as failing to start a call. </li><li>Unless it is otherwise specified, the sources are the clients that integrate the Agora SDK.</li></div>

## Agora RTC SDK

Use UDP ports rather than TCP ports for superior voice and video quality since UDP prioritizes timeliness over reliability.

### Native SDK

Add the following destination domains and the corresponding ports to the firewall whitelist:

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

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 1080; 8000; 9700; 25000; 30000     | TCP              |  Allow |
| 1080; 4000 to 4030; 7000; 8000; 8913; 9700; 25000   |  UDP  | Allow |


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
| 80; 443; 3433; 5668; 5669; 5866 - 6000; 6080; 6443; 8667; 9667     | TCP              |  Allow |
| 3478; 5866 - 6000 (2.9.0 or later); 10000 - 65535 (before 2.9.0)   |  UDP  | Allow |


## Agora RTM SDK

### Native SDK

Add the following destination domains and the corresponding ports to the firewall whitelist:

```
.agora.io
qoslbs.agoralab.co
qos.agoralab.co
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
| 443; 9591; 9593     | TCP              |  Allow |

## Agora On-premise Recording SDK

Add the following destination domains and the corresponding ports to the firewall whitelist:

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

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 1080; 8000; 9700; 25000; 30000    | TCP              |  Allow |
| Duplex ports 1080; 7000; 8000; 8913; 9700; 25000；local ports 4000 to 4030; simplex downstream ports used by recording processes.  |  UDP  | Allow |

<div class="alert note">Simplex downstream port: To record the content in channels, you need one recording process for each of the channels. One recording thread requires four simplex downstream ports. There must be no port conflict among processes, including system processes and all recording processes.<ul><li>Agora recommends that you specify the range of ports used by the recording processes. Configure a large range for all recording processes (Agora recommends 40000 to 41000 or larger). If so, the Recording SDK assigns ports to each recording process within the specified range and avoids port conflicts automatically. To set the port range, you need to configure the lowUdpPort and highUdpPort parameters.</li><li>If the lowUdpPort and highUdpPort parameters are not specified, the ports used by the recording processes are at random, which may cause port conflicts.</li></ul></div>

## Agora Gaming SDK

Add the following destination domains and the corresponding ports to the firewall whitelist:

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

| Destination ports | Port type | Operation |
| ---------- | ------------------------------------------------ | -----------------|
| 1080; 8000     | TCP              |  Allow |
| 1080; 4000 to 4030; 8000; 9700; 25000   |  UDP  | Allow |
