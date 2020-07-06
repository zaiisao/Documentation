
---
title: Use Cloud Proxy
description: How to enable cloud proxy on Web
platform: Web
updatedAt: Mon Jul 06 2020 10:35:04 GMT+0800 (CST)
---
# Use Cloud Proxy
## Introduction

Enterprises with high-security requirements usually have firewalls that restrict employees from visiting unauthorized sites to protect internal information.

To ensure that enterprise users can connect to Agora's services through a firewall, Agora supports setting up a cloud proxy. 

Compared with setting a single proxy server, the cloud proxy is more flexible and stable, thus widely implemented in organizations with high-security requirements, such as large-scale enterprises, hospitals, universities, and banks.

## Working principles

The following diagram shows the working principles of the Agora cloud proxy.

![](https://web-cdn.agora.io/docs-files/1569400862850)

1. Before connecting to Agora SD-RTN, the Agora SDK sends the request to the cloud proxy.
2. The cloud proxy sends back the proxy information.
3. The Agora SDK sends the data to the cloud proxy, and the cloud proxy forwards the data to Agora SD-RTN.
4. Agora SD-RTN sends the data to the cloud proxy, and the cloud proxy forwards the data to the Agora SDK.

## Implementation

Agora Web SDK v2.5.1 or later supports the cloud proxy. 

1. Download [the latest version of the Agora Web SDK](https://docs.agora.io/en/Agora%20Platform/downloads).
2. Prepare the development environment. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_web.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_web.md).
3. Contact support@agora.io and provide your App ID, and the information on the regions using the cloud proxy, the concurrent scale, and network operators.
4. Add the following test IP addresses and ports to your whitelist.

  The sources are the clients that integrate the Agora Web SDK.

	| Protocol | Destination             | Port                  | Purpose      |
	| -------- | -------------- | --------------------- | --------------------- |
	| TCP      | 23.236.115.138 | 443, 4000<br/>3433 - 3460 | Message data transmission<br/>Media data exchange |
	| TCP      |148.153.66.218 | 443, 4000<br/>3433 - 3460 | Message data transmission<br/>Media data exchange |
	| TCP | 164.52.87.25  | 443, 4000<br/>3433 - 3460 | Message data transmission<br/>Media data exchange  |
	| TCP      | 47.74.211.17   | 443                   | Edge node communication |
	| TCP      | 52.80.192.229  | 443                   | Edge node communication |
	| TCP      | 52.52.84.170   | 443                   | Edge node communication |
	| TCP      | 47.96.234.219  | 443                   | Edge node communication |
	| UDP      | 23.236.115.138 | 3478 - 3500            | Media data exchange |
	| UDP      | 164.52.87.25 | 3478 - 3500            | Media data exchange |
	| UDP      | 148.153.66.218 | 3478 - 3500            | Media data exchange |

 <div class="alert note">These IPs are for testing only. You need to apply for exclusive IP resources for production environment.</div>

5. Call the `startProxyServer` method to enable the cloud proxy and see if the audio/video call works.
6. Agora will provide the IP addresses (domain name) and ports for you to use the cloud proxy in the production environment. Add the IP address and ports to your whitelist.
7. If you want to disable the cloud proxy, call the  `stopProxyServer` method.

### Sample code

### Enable the cloud proxy

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

### Disable the cloud proxy

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

### API reference

- [startProxyServer](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#startproxyserver)
- [stopProxyServer](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#stopproxyserver)

## Considerations

- The `startProxyServer` and  `stopProxyServer` methods must be called before joining the channel or after leaving the channel.
- The Agora Web SDK also provides the `setProxyServer` and `setTurnServer` methods for you to deploy the proxy. The `setProxyServer` and `setTurnServer` methods cannot be used with the `startProxyServer` method at the same time, else an error occurs.
- The `stopProxyServer` method disables all proxy settings, including those set by the `setProxyServer` and `setTurnServer` methods.
