
---
title: 使用 String 型的用户名
description: 
platform: Web
updatedAt: Wed Aug 07 2019 02:02:17 GMT+0800 (CST)
---
# 使用 String 型的用户名
## 场景描述

很多 App 使用 String 类型的用户账号。为降低开发成本，Agora 新增支持 String 型的用户名，方便用户使用 App 账号直接加入 Agora 频道。

Agora 的其他接口仍使用 UID 作为参数。Agora 维护一个 String 型 User account 和 Int 型 UID 的映射表，方便随时通过 User account 获取 UID 或者通过 UID 获取 User account。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。

## 实现方法

从 v2.5.0 起，Web SDK 支持将 join 方法中的 `uid` 设为 Number 或 String 型。

其中，String 型的用户名最大不可超过 255 字节，且需要确保其在频道内的唯一性。支持的字符集范围如下：

- 26 个小写英文字母 a-z
- 26 个大写英文字母 A-Z
- 10 个数字 0-9
- 空格
- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","

你可以对照 API 时序图，参考下面的示例代码片段，在项目中实现使用 String 型用户名：

```javascript
// Set uid as agora and join channel 1024
client.join("<token>", "1024", "agora", function(uid) {
  console.log("client" + uid + "joined channel");
  // Create a local stream
  // ...
}, function(err) {
  console.error("client join failed", err)
  // Error handling
});
```

其中：”agora“ 就是一个 string 型的 `uid`。

### API 参考

- [`Client.join`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#join)

## 开发注意事项

- 同一频道内，Int 型的 User ID 和 String 型的 User account 不可混用。如果你的频道内有不支持 String 型 User account 的 SDK，则只能使用 Int 型的 User ID。
- 如果你将用户名切换至 String 型 User account，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。如果使用 String 型 User account 加入频道，请确保使用该 User account 或其对应的 Int 型 UID 来生成 Token。你可以调用 `getUserInfoByUserAccount` 来获取 User account 所对应的 UID。
- 如果频道中 Native SDK 和 Web SDK 互通，请确保该两者使用的用户名的类型一致。其中，Web SDK 中的 `uid` 可以设为 String 型或 Number 型。
