
---
title: 使用 String 型的用户名
description: 
platform: Web
updatedAt: Tue Sep 17 2019 10:09:17 GMT+0800 (CST)
---
# 使用 String 型的用户名
## 场景描述

很多 App 使用 String 类型的用户账号。为降低开发成本，Agora 新增支持 String 型的用户名，方便用户使用 App 账号直接加入 Agora 频道。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。

## 实现方法

请确保你已了解实现基本的实时音视频功能的步骤及代码逻辑。详见[开始音视频通话](../../cn/Interactive%20Broadcast/start_call_web.md)或[开始互动直播](../../cn/Interactive%20Broadcast/start_live_web.md)。

Web SDK 支持将 join 方法中的 `uid` 设为 Number 或 String 型。因此，你可以直接在调用该方法时，传入 String 型的用户名即可。

其中，String 型的用户名最大不可超过 255 字节，且需要确保其在频道内的唯一性。支持的字符集范围如下：

- 26 个小写英文字母 a-z
- 26 个大写英文字母 A-Z
- 10 个数字 0-9
- 空格
- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","

### API 调用时序

下图展示使用 User Account 加入频道的 API 调用时序图：

![](https://web-cdn.agora.io/docs-files/1568713872144)

### 示例代码

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

其中：“agora” 就是一个 string 型的 `uid`。

### API 参考

- [`Client.join`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.client.html#join)


## 开发注意事项

- 同一频道内，Int 型和 String 型的用户名不可混用。如果你的频道内有不支持 String 型 User account 的 SDK，则只能使用 Int 型的 User ID。目前支持 String 型 User account 的 SDK 如下：
  - Native SDK：v2.8.0 及之后版本
  - Web SDK：v2.5.0 及之后版本
- 如果你将用户名切换至 String 型，请确保所有终端用户同步升级。
- 如果使用 String 型的用户名加入频道，请确保你的服务端用户生成 Token 的脚本已升级至最新版本，并使用该用户名或其对应的 Int 型 UID 来生成 Token。
- 如果频道中 Native SDK 和 Web SDK 互通，请确保该两者使用的用户名的类型一致。
