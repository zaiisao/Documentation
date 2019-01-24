
---
title: Implement Encryption
description: 
platform: Web
updatedAt: Fri Jan 04 2019 10:38:14 GMT+0000 (UTC)
---
# Implement Encryption
## Introduction
The Agora SDK provides methods for you to implement built-in encryption and set encryption password.

The following figure shows how Agora’s communications use built-in encryption:

<img alt="../_images/agora-encryption_en.png" src="https://web-cdn.agora.io/docs-files/en/agora-encryption_en.png" />

## Implementation
Before proceeding, ensure that you prepare the development environment. See [Integrate the SDK](../../en/Video/web_prepare.md).

```
//javascript
//Sets encryption mode to "aes-128-xts","aes-256-xts" or "aes-128-ecb".
client.setEncryptionMode(encryptionMode);
//Sets AES encryption password.
client.setEncryptionSecret(password);
```

### API Reference

- [`setEncryptionMode`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)


## Considerations

Call this method before you call [`join`](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#join) to join an RTC channel.
