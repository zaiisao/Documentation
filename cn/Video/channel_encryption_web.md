
---
title: 媒体流加密
description: 
platform: Web
updatedAt: Sun Sep 29 2019 08:20:44 GMT+0800 (CST)
---
# 媒体流加密
## 功能描述

如果你需要启用加密功能，Agora 提供加密方案与设置加密密码方法。

下图描述了启用内置加密方案的声网音视频通信方案：

<img alt="../_images/agora-encryption.png" src="https://web-cdn.agora.io/docs-files/cn/agora-encryption.png" style="width: 840px;"/>


## 实现方法

开始前请确保已在你的项目中实现基本的实时音视频功能。 详见[开始音视频通话](../../cn/Video/start_call_web.md)或[开始互动直播](../../cn/Video/start_live_web.md)。

```javascript
// 设置加密方案
// 在 "aes-128-xts"，"aes-256-xts" 和 "aes-128-ecb" 中选择一种方案
client.setEncryptionMode(encryptionMode);
// 设置 AES 加密密码
client.setEncryptionSecret(password);
```

### API 参考

- [`setEncryptionMode`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)

## 开发注意事项

请确保在调用 [`join`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#join) 加入频道之前设置加密。


