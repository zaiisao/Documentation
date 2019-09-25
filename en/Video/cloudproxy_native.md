
---
title: Use Cloud Proxy
description: 
platform: Android,iOS,macOS,Windows
updatedAt: Wed Sep 25 2019 09:14:12 GMT+0800 (CST)
---
# Use Cloud Proxy
## Introduction

Enterprises with high-security requirements usually have firewalls that restrict employees from visiting unauthorized sites to protect internal information.

To ensure that enterprise users can connect to Agora's services through a firewall, Agora supports setting up a cloud proxy. 

Compared with setting a single proxy server, the cloud proxy is more flexible and stable, thus widely implemented in organizations with high-security requirements, such as large-scale enterprises, hospitals, universities, and banks.

## Implementation

1. Download [the latest version of the Agora Native SDK](https://docs.agora.io/en/Agora%20Platform/downloads).
2. Integrate the SDK and prepare the development environment. For details, see the **Integrate the SDK** guide.
3. Contact support@agora.io and provide your App ID, and the information on the regions using the cloud proxy, the concurrent scale, and network operators.
4. Add the following test IP addresses and ports to your whitelist.

	The sources are the clients that integrate the Agora Native or Recording SDK.

| Protocol | Destination  | Port                   | Port purpose      |
| ---- | ------------- | ---------------------- | ---------------------- |
| TCP  | 120.92.118.34 | 4000                   | Message data transmission |
| TCP  | 120.92.18.162 | 4000                   | Message data transmission |
| TCP  | 47.74.211.17  | 1080, 8000, 25000, 9700 | Edge node communication |
| TCP  | 52.80.192.229 | 1080, 8000, 25000, 9700 | Edge node communication |
| TCP  | 52.52.84.170  | 1080, 8000, 25000, 9700 | Edge node communication |
| TCP  | 47.96.234.219 | 1080, 8000, 25000, 9700 | Edge node communication |
| UDP  | 120.92.118.34 | 4500 to 4650            | Media data exchange |
| UDP  | 120.92.18.162 | 4500 to 4650            | Media data exchange |
| UDP  | 47.74.211.17  | 1080, 8000, 25000, 9700 | Edge node communication |
| UDP  | 52.80.192.229 | 1080, 8000, 25000, 9700 | Edge node communication |
| UDP  | 52.52.84.170  | 1080, 8000, 25000, 9700 | Edge node communication |
| UDP  | 47.96.234.219 | 1080, 8000, 25000, 9700 | Edge node communication |
> These IPs are for testing only. You need to apply for exclusive IP resources for the production environment.

5. Enable the cloud proxy by calling the `setParameters("{\"rtc.enable_proxy\"}:true"); ` method and see if the audio/video call works.
6. Agora will provide the IP addresses (domain name) and ports for you to use the cloud proxy in the production environment. Add the IP address and ports to your whitelist.
