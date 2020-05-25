
---
title: Use Cloud Proxy
description: 
platform: Unity
updatedAt: Mon May 25 2020 07:42:43 GMT+0800 (CST)
---
# Use Cloud Proxy
## Introduction

Enterprises with high-security requirements usually have firewalls that restrict employees from visiting unauthorized sites to protect internal information.

To ensure that enterprise users can connect to Agora's services through a firewall, Agora supports setting up a cloud proxy. 

Compared with setting a single proxy server, the cloud proxy is more flexible and stable, thus widely implemented in organizations with high-security requirements, such as large-scale enterprises, hospitals, universities, and banks.

## Implementation

1. Download [the latest version of the Agora RTC SDK](https://docs.agora.io/en/Agora%20Platform/downloads). Agora Unity SDK v2.9.1 or later supports the cloud proxy.
2. Integrate the SDK and prepare the development environment. For details, see the *Quickstart Guide*.
3. Contact support@agora.io and provide your App ID, and the information on the regions using the cloud proxy, the concurrent scale, and network operators.
4. Add the following test IP addresses and ports to your whitelist.

	The sources are the clients that integrate the Agora RTC SDK and Agora On-Premise Recording SDK.

 | Protocol | Destination IP address  | Port                   | Port function      |
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

 <div class="alert note">These IP addresses and ports are for testing purposes only. In a production environment, apply for the dedicated IP addresses and ports.</div>

5. Enable the cloud proxy by calling the `SetParameters("{\"rtc.enable_proxy\":true}");` method. After succesfully enabled the cloud proxy, the SDK uses the configuration of  `SetParameters("{\"rtc.proxy_server\":[1, \"ap-proxy.agora.io\", 0]}");` by default, and you don't need to call `SetParameters` again.
6. If the default cloud proxy server cannot meet your requirements, call the `SetParameters("{\"rtc.proxy_server\":[proxy_type, \"ip or dns\", port]}");` method to configure the cloud proxy server. See details in the following table:
 
| `proxy_type`                                                 | `ip or dns`                                         | `port`                        |
| ------------------------------------------------------------ | --------------------------------------------------- | ----------------------------- |
| 0: The Socks5 proxy server                         | The IP address of the server                                         | The ports of the server                 |
| 1: Use cloud proxy and configure the domain name (recommended) | ap-proxy.agora.io                                   | 0                      |
| 2: Use cloud proxy and configure the IP address (Used when the domain name is restricted) | The IP address list of the server in the format of<br/> `"[\"ip1\",\"ip2\"]"` | The port of this lbs. The default value is 0 |

7. Test if the audio or video call works.
8. Agora will provide the IP addresses (domain name) and ports for you to use the cloud proxy in the production environment. Add the IP address and ports to your whitelist.
9. When you want to disable the cloud proxy, call the `SetParameters("{\"rtc.enable_proxy\":false}");` method.

### Sample code

```c#
// Enables the cloud proxy server, and uses the default configuration.
SetParameters("{\"rtc.enable_proxy\":true}");
```

```c#
// Enables the cloud proxy server.
SetParameters("{\"rtc.enable_proxy\":true}");
// Uses the Socks5 proxy server.
SetParameters("{\"rtc.proxy_server\":[0, \"127.0.0.1\", 1080]}");
```

```c#
// Enables the cloud proxy server.
SetParameters("{\"rtc.enable_proxy\":true}");
// Uses cloud proxy and configure the domain name.
SetParameters("{\"rtc.proxy_server\":[2, \"[\"192.168.0.112\",\"127.0.0.1\"]\", 0]}");
```

## Working principles

The following diagram shows the working principles of the Agora cloud proxy.

![](https://web-cdn.agora.io/docs-files/1569400862850)

1. Before connecting to Agora SD-RTN, the Agora SDK sends the request to the cloud proxy;

2. The cloud proxy sends back the proxy information;
3. The Agora SDK sends the data to the cloud proxy, and the cloud proxy forwards the data to Agora SD-RTN;
4. Agora SD-RTN sends the data to the cloud proxy, and the cloud proxy forwards the data to the Agora SDK.


## Considerations

The `SetParameters` method must be called before joining the channel or after leaving the channel.
