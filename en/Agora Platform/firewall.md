
---
title: Firewall Requirements
description: 
platform: All Platforms
updatedAt: Tue Nov 06 2018 09:47:26 GMT+0000 (UTC)
---
# Firewall Requirements
This page describes the firewall requirements for all Agora SDKs. Before accessing Agora’s services, please make sure that you have opened ports and whitelisted domains as specified in this article.

## Agora Native SDK

-   Open TCP ports 1080, 8000, 9700, 25000, and 30000.

-   Open UDP ports 1080, 4000-4030, 7000, 8000, 8913, 9700, and 25000.

-   Whitelist domain `.agora.io`, `vocs1.agora.io`, `vocs2.agora.io`, `vocs3.agora.io`, `vocs4.agora.io`, `vocs5.agora.io`, `qoslbs.agoralab.co`, and `qos.agoralab.co` , `ap1.agora.io`, `ap2.agora.io`, `ap3.agora.io`, `ap4.agora.io`, `ap5.agora.io`.


## Agora Web SDK

-   Open TCP ports 80, 443, 3433, 5668, 5669, 5866-5890, 6080, 6443, 8667, and 9667.

-   Open UDP ports 3478 and 10000-65535.

-   Whitelist domain `.agora.io` .


## Agora Signaling SDK

-   Open TCP ports 1080 and 8001-8199.

-   Open UDP ports 8180-8199.

-   Whitelist domain `.agora.io` .


## Agora Recording SDK

-   Open TCP ports 1080 and 8000.

-   Open UDP ports:

    -   Duplex ports: 1080, 4000-4030, 8000, 9700 and 25000;

    -   Simplex downstream ports used by recording processes.

-   Set whitelist domains: `.agora.io`, `vocs.agora.io`, `qoslbs.agora.io`, and `qos.agora.io`.

> To record the content in channels, you need one recording process for each of the channels. One recording thread requires four simplex downstream ports. There must be no port conflict among processes, including system processes and all recording processes.
> 
> -   Agora recommends that you specify the range of ports used by the recording processes. Configure a large range for all recording processes \(Agora recommends 40000 ~ 41000 or larger\). If so, the Recording SDK assigns ports to each recording process within the specified range and avoids port conflicts automatically. To set the port range, you need to configure the parameters lowUdpPort and highUdpPort;
> 
> -   If the parameters lowUdpPort and highUdpPort are not specified, the ports used by the recording processes are at random, which may cause port conflicts.


## Agora Gaming SDK

-   Open TCP ports 1080 and 8000.

-   Open UDP ports 1080, 4000-4030, 8000, 9700 and 25000.

-   Whitelist domain `.agora.io`, `vocs.agora.io`, `qoslbs.agora.io`, and `qos.agora.io` .


You must use UDP ports, rather than TCP ports, for superior voice and video quality since UDP favors timeliness over reliability.


