
---
title: 云端录制 101 错误
description: 
platform: All Platforms
updatedAt: Tue Nov 19 2019 10:42:18 GMT+0800 (CST)
---
# 云端录制 101 错误
日志文件中报错 `"ErrorUint32":101`，一般为 Token 错误引起，可能有以下几种原因：

- Token 填写错误
- Token 过期
- Native/Web SDK 使用了 Token，云端录制未使用
- 云端录制使用了 Token，Native/Web SDK 未使用
