
---
title: Channel Encryption
description: 
platform: Web
updatedAt: Mon Jul 06 2020 07:29:14 GMT+0800 (CST)
---
# Channel Encryption
## Introduction
The Agora SDK provides methods for you to implement built-in encryption and set encryption password.

<div class="alert note"><li>Both the communication and live-streaming scenarios support encryption. If you need to push streams to the CDN in a live-streaming channel, do not use channel encryption.<br><li>Ensure that all receivers and senders use the same encryption scheme. Otherwise, you may meet undefined behaviors such as no voice and black screen.</br></div>

The following diagram describes the encrypted data transmission process:
![](https://web-cdn.agora.io/docs-files/1590556634763)

## Implementation

Before proceeding, ensure that you implement a basic call or live interactive streaming in your project. See [Start a Video Call](../../en/Video/start_call_web.md) or [Start a Live Interactive Video Streaming](../../en/Video/start_live_web.md) for details.

```
//javascript
//Sets encryption mode to "aes-128-xts","aes-256-xts" or "aes-128-ecb".
client.setEncryptionMode(encryptionMode);
//Sets AES encryption password.
client.setEncryptionSecret(password);
```

### API reference

- [`setEncryptionMode`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)


## Considerations

Call this method before you call [`join`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#join) to join a channel.
