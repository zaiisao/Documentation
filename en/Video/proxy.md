
---
title: Deploy the Enterprise Proxy
description: 
platform: Android,iOS,macOS,Windows
updatedAt: Fri Nov 09 2018 18:19:46 GMT+0000 (UTC)
---
# Deploy the Enterprise Proxy
This page shows how enterprises can deploy the proxy package provided by Agora to access Agora’s services through a company firewall.

> This page does not apply to the Agora Web SDK.

## Introduction

Enterprises may restrict employees’ access to unauthorized websites through a company firewall, and can deploy the proxy package provided by Agora to enable access to the Agora Cloud for Agora’s services.

## Implementations

### Deploy the Proxy Package by Agora

Agora provides an open-source proxy package for enterprises to downland and deploy.

1. Go to [https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-rtc-sdk](https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-rtc-sdk), download, and install the `install-ubuntu.sh` script.

2. Add relevant IP addresses and ports in the enterprise firewall to allow access.

3. Call the following method in the Agora SDK, and set the IP and port parameters in the method.

   ```
   setParameters("{\"rtc.proxy_server\":[0, \"IP\", port]}");
   ```

   <table>
   <colgroup>
   <col/>
   <col/>
   </colgroup>
   <tbody>
   <tr><td><strong>Name</strong></td>
   <td><strong>Description</strong></td>
   </tr>
   <tr><td>IP</td>
   <td>IP addresses that you want to add. The IP addresses are strings. For example 127.0.0.1.</td>
   </tr>
   <tr><td>port</td>
   <td>Ports that you want to add to the Agora proxy. Enter 1080.</td>
   </tr>
   </tbody>
   </table>

### Deploy Multiple Proxies

Enterprises can deploy multiple proxies to enable uninterrupted access in case the proxy server is overloaded or breaks down. Agora has yet to conduct tests on the capacity of the proxy, but estimates are that one proxy can support around 100 users.

- Round-robin DNS:

	- Deploy a Shadowsocks cluster.
	- The proxies share a single domain name.
	- The proxies use a round-robin system to ensure load balancing.
	- Update the DNS log whenever the proxy breaks down.

- Random proxy server in the app:

	The app can randomly choose which proxy to use, and then pass it to the SDK.

## Working Principles

The following figure shows how the proxy works:

<img alt="../_images/proxy_theory.jpg" src="https://web-cdn.agora.io/docs-files/en/proxy_theory.jpg" />

- The enterprise sets up a firewall that separates the enterprise network from the public Internet to limit employees’ access to unauthorized websites.
- The enterprise deploys a proxy on the public Internet. When an enterprise device accesses the Internet, the device sends a visit request to the enterprise firewall, which then sends the request to the proxy. The proxy then sends the request to the public Internet server, like a relay.
- The public Internet server sends relevant website content back to the proxy, which then sends the content to the firewall. Finally, the firewall sends the content back to the requesting device.

The following figure shows how the enterprise app accesses the Agora Cloud through the proxy:

<img alt="../_images/proxy_agora.jpg" src="https://web-cdn.agora.io/docs-files/en/proxy_agora.jpg" />
