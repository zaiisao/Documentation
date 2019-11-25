
---
title: Use Cloud Proxy
description: How to enable cloud proxy for recording
platform: Linux
updatedAt: Mon Nov 18 2019 03:09:38 GMT+0800 (CST)
---
# Use Cloud Proxy
## Introduction

Enterprises with high-security requirements usually have firewalls that restrict employees from visiting unauthorized sites to protect internal information.

To ensure that enterprise users can connect to Agora's services through a firewall, Agora supports setting up a cloud proxy. 

Compared with setting a single proxy server, the cloud proxy is more flexible and stable, thus widely implemented in organizations with high-security requirements, such as large-scale enterprises, hospitals, universities, and banks.

## Implementation

The [Agora On-Premise Recording SDK v2.8.0](https://download.agora.io/ardsdk/release/Agora_Recording_SDK_for_Linux_v2.8.0.150.tar.gz) or later supports the cloud proxy. Before proceeding, ensure that you prepare the development environment and integrate the SDK. 

1. Contact sales@agora.io and provide the information on the regions using the cloud proxy, the concurrent scale, and network operators.

2. Add the following test IP addresses and ports to your whitelist.

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

 <div class="alert note">These IPs are for testing only. You need to apply for exclusive IP resources for the production environment.</div>

3. Set the `enableCloudPorxy` parameter as `true` in `RecordingConfig` when the recording app joins a channel and see if the recording works.

   > If you use the command-line demo, add `--enableCloudProxy 1` to the command when starting the recording.

4. Agora will provide the IP addresses (domain name) and ports for you to use the cloud proxy in the production environment. Add the IP addresses and ports to your whitelist.



