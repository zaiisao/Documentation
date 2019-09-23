
---
title: Use Cloud Proxy
description: How to enable cloud proxy on Web
platform: Web
updatedAt: Mon Sep 23 2019 02:15:17 GMT+0800 (CST)
---
# Use Cloud Proxy
## Introduction

Enterprises with high-security requirements usually have firewalls that restrict employees from visiting unauthorized sites to protect internal information.

To ensure that enterprise users can connect to Agora's services through a firewall, Agora supports setting up a cloud proxy. 

Compared with setting a single proxy server, the cloud proxy is more flexible and stable, thus widely implemented in organizations with high-security requirements, such as large-scale enterprises, hospitals, universities, and banks.

## Prerequisites

Agora Web SDK v2.5.1 or later supports the cloud proxy. Before proceeding, ensure that you prepare the development environment. See [Integrate the SDK](../../en/Voice/web_prepare.md).

1. Contact support@agora.io and provide your App ID, and the information on the regions using the cloud proxy, the concurrent scale, and network operators.

2. Add the following test IP addresses and ports to your whitelist.

  The sources are the clients that integrate the Agora Web SDK.

	| Protocol | Destination             | Port                  | Port purpose      |
	| -------- | -------------- | --------------------- | --------------------- |
	| TCP      | 23.236.115.138 | 443, 4000<br/>3433, 3444 | Message data transmission<br/>Media data exchange |
	| TCP      |148.153.66.218 | 443, 4000<br/>3433, 3444 | Message data transmission<br/>Media data exchange |
	| TCP      | 47.74.211.17   | 443                   | Edge node communication |
	| TCP      | 52.80.192.229  | 443                   | Edge node communication |
	| TCP      | 52.52.84.170   | 443                   | Edge node communication |
	| TCP      | 47.96.234.219  | 443                   | Edge node communication |
	| UDP      | 23.236.115.138 | 3478, 3488            | Media data exchange |
	| UDP      | 148.153.66.218 | 3478, 3488            | Media data exchange |

   > These IPs are for testing only. You need to apply for exclusive IP resources for production environment.

3. Enable the cloud proxy according to the instructions in the **Implementation** section and see if the audio/video call works.

4. Agora will provide the IP addresses (domain name) and ports for you to use the cloud proxy in the production environment. Add the IP address and ports to your whitelist.

## Implementation

You can set up the cloud proxy by calling the `startProxyServer` method.

### Enable the Cloud Proxy

```javascript
var client = AgoraRTC.createClient({mode: 'live',codec: 'vp8'});

// Start the cloud proxy.
client.startProxyServer();
// Join a channel.
client.init(key, function() {
    client.join(channelKey, channel, null, function(uid) {
        .......
    })
})
```

### Disable the Cloud Proxy

To turn off the cloud proxy service：

1. Leave the channel.
2. Call the `stopProxyServer` method.
3. Join the channel.

```javascript
// After enabling the cloud proxy and joining the channel.
...
// Leave the channel.
client.leave();

// Disable the cloud proxy.
client.stopProxyServer();

// Join the channel.
client.join(channelKey, channel, null, function(uid) {
...
}
```

## Considerations

- The `startProxyServer` and  `stopProxyServer` methods must be called before joining the channel or after leaving the channel.
- The Agora Web SDK also provides the `setProxyServer` and `setTurnServer` methods for you to [deploy the proxy](../../en/Voice/proxy_web.md). The `setProxyServer` and `setTurnServer` methods cannot be used with the `startProxyServer` method at the same time, else an error occurs.
- The `stopProxyServer` method disables all proxy settings, including those set by the `setProxyServer` and `setTurnServer` methods.
