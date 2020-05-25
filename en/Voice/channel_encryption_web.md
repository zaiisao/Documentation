
---
title: Channel Encryption
description: 
platform: Web
updatedAt: Mon May 25 2020 14:18:06 GMT+0800 (CST)
---
# Channel Encryption
## Introduction
The Agora SDK provides methods for you to implement built-in encryption and set encryption password.

<div class="alert note"><li>Both Communication and Live Broadcast support channel encryption. For live broadcasts, if you need to push streams to the CDN, do not use channel encryption.<br><li>Ensure that both receivers and senders use channel encryption, otherwise, you may meet undefined behaviors such as no voice and black screen.</br></div>

The following figure shows how Agora’s communications use built-in encryption:

![](https://web-cdn.agora.io/docs-files/1587019645125)

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Voice/start_call_web.md) or [Start a Live Broadcast](../../en/Voice/start_live_web.md) for details.

```
//javascript
//Sets encryption mode to "aes-128-xts","aes-256-xts" or "aes-128-ecb".
client.setEncryptionMode(encryptionMode);
//Sets AES encryption password.
client.setEncryptionSecret(password);
```

### API reference

- [`setEncryptionMode`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)


## Considerations

Call this method before you call [`join`](https://docs.agora.io/en/Voice/API%20Reference/web/interfaces/agorartc.client.html#join) to join a channel.
