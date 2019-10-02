
---
title: Channel Encryption
description: 
platform: Web
updatedAt: Sun Sep 29 2019 08:20:44 GMT+0800 (CST)
---
# Channel Encryption
## Introduction
The Agora SDK provides methods for you to implement built-in encryption and set encryption password.

The following figure shows how Agora’s communications use built-in encryption:

<img alt="../_images/agora-encryption_en.png" src="https://web-cdn.agora.io/docs-files/en/agora-encryption_en.png" />

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Video/start_call_web.md) or [Start a Live Broadcast](../../en/Video/start_live_web.md) for details.

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
