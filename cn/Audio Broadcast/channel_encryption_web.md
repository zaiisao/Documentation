
---
title: 媒体流加密
description: 
platform: Web
updatedAt: Tue Aug 11 2020 11:13:16 GMT+0800 (CST)
---
# 媒体流加密
## 功能描述

在实时音视频互动过程中，开发者需要对媒体流加密，从而保障用户的数据安全。Agora 提供内置加密方案，你可以设置加密模式和密钥。

<div class="alert note"><li>通信和直播场景均支持媒体流加密功能。但是在直播场景下，Agora 不支持将加密后的媒体流推到 CDN 上。</li><li>如需使用媒体流加密功能，请确保接收端和发送端都使用相同的加密方案，否则会出现未定义行为（例如音频无声或视频黑屏）。</li></div>

下图描述了启用加密功能后的数据传输流程：
![](https://web-cdn.agora.io/docs-files/1590556854574)

## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。 详见[开始音视频通话](../../cn/Audio%20Broadcast/start_call_web.md)或[开始互动直播](../../cn/Audio%20Broadcast/start_live_web.md)。

在加入频道前，调用 `setEncryptionMode` 和 `setEncryptionSecret` 设置加密模式和密钥。

<div class="alert note">同一频道内所有用户必须使用相同的加密模式和密钥。</div>

Agora 支持 4 种加密模式：

- `aes-128-xts`: 128 位 AES 加密，XTS 模式。
- `aes-256-xts`: 256 位 AES 加密，XTS 模式。
- `aes-128-ecb`: 128 位 AES 加密，ECB 模式。
- `sm4-128-ecb`: 128 位国密 SM4 加密，ECB 模式。

```javascript
// 设置加密模式为 aes-128-xts
client.setEncryptionMode("aes-128-xts");
// 设置加密密钥
client.setEncryptionSecret(password);
```

### API 参考

- [`setEncryptionMode`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)
