
---
title: Channel Encryption
description: 
platform: Web
updatedAt: Tue Aug 11 2020 11:13:22 GMT+0800 (CST)
---
# Channel Encryption
## Introduction

To improve data security, developers can encrypt users' media streams during the real-time engagement. Agora supports built-in encryption, and you can set the encryption mode and encryption secret.

<div class="alert note"><li>Both the communication and live interactive streaming scenarios support encryption, but Agora does not support pushing encrypted streams to the CDN in a live-streaming channel.</li><li>Eure that both the receivers and senders use the same encryption scheme. Otherwise, undefined behaviors such as no voice and black screen may occour.</li></div>

The following diagram describes the encrypted data transmission process:
![](https://web-cdn.agora.io/docs-files/1590556634763)

## Implementation

Before proceeding, ensure that you have implemented basic real-time functions in your project. See [Start a  Call](../../en/Video/start_call_web.md) or [Start Live Interactive Streaming](../../en/Video/start_live_web.md) for details.

Before joining a channel, call `setEncryptionMode` and `setEncryptionSecret` to set the encryption mode and encryption secret.

<div class="alert note">All users in the same channel must use the same encryption mode and encryption secret.</div>

Agora supports the following encryption modes:

- `aes-128-xts`: 128-bit AES encryption, XTS mode.
- `aes-256-xts`: 256-bit AES encryption, XTS mode.
- `aes-128-ecb`: 128-bit AES encryption, ECB mode.

```
//javascript
//Sets encryption mode to aes-128-xts.
client.setEncryptionMode("aes-128-xts");
//Sets encryption secret.
client.setEncryptionSecret(password);
```

### API reference

- [`setEncryptionMode`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)
