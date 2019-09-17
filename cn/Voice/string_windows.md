
---
title: 使用 String 型的用户名
description: 
platform: Windows
updatedAt: Thu Aug 08 2019 09:34:55 GMT+0800 (CST)
---
# 使用 String 型的用户名
## 场景描述

很多 App 使用 String 类型的用户账号。为降低开发成本，Agora 新增支持 String 型的用户名，方便用户使用 App 账号直接加入 Agora 频道。

Agora 的其他接口仍使用 UID 作为参数。Agora 维护一个 String 型 User account 和 Int 型 UID 的映射表，方便随时通过 User account 获取 UID 或者通过 UID 获取 User account。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。

## 实现方法
请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/windows_video.md)。

从 v2.8.0 起，Native SDK 新增使用 User Account 来标识用户在频道中的身份
 - `registerLocalUserAccount`：注册本地用户 User Account
 - `joinChannelWithUserAccount`：使用 User Account 加入频道


其中，String 型的用户名最大不可超过 255 字节，且需要确保其在频道内的唯一性。支持的字符集范围如下：

- 26 个小写英文字母 a-z
- 26 个大写英文字母 A-Z
- 10 个数字 0-9
- 空格
- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","

使用 User Account 加入频道的 API 调用时序图如下所示：


![](https://web-cdn.agora.io/docs-files/1562145670727)


其中：

- 在 `registerLocalUserAccount` 和 `joinChannelWithUserAccount` 方法中，`userAccount` 参数均为必填，不可为 null。
- `registerLocalUserAccount` 为选调。你可以注册后再调用 `joinChannelWithUserAccount` 方法加入频道，也可以不注册直接调用 `joinChannelWithUserAccount` 加入频道。我们建议你调用。提前调用 `registerLocalUserAccount` 可以减少调用 `joinChannelWithUserAccount` 加入频道的时间。
- 对于其他接口，Agora SDK 仍沿用 Int 型的 UID 参数标识用户身份。你可以使用 `getUserInfoByUid` 或 `getUserInfoByUserAccount` 获取对应的 User Account 或 UID，无需自己维护映射表。


### API 参考

- [`registerLocalUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [`joinChannelWithUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [`getUserInfoByUid`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [`getUserInfoByUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [`onLocalUserRegistered`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [`onUserInfoUpdated`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)

## 示例代码

Agora 提供一个[使用 String 型用户名](https://github.com/AgoraIO/Advanced-Video/tree/master/String-Account)的 Github 示例代码，你可以前往下载和体验。

你也可以参考如下代码片段，在项目中实现使用 String 型的 User account 加入频道：

```C++
// 加入频道前注册用户名
lpAgoraObject->RegisterLocalUserAccount(APP_ID, m_dlgEnterChannel.GetStringUid());
// 使用注册的用户名加入频道
lpAgoraObject->JoinChannelWithUserAccount(TOKEN, strChannelName, m_dlgEnterChannel.GetStringUid());
```


## 开发注意事项

- 同一频道内，Int 型和 String 型的用户名不可混用。如果你的频道内有不支持 String 型 User account 的 SDK，则只能使用 Int 型的 User ID。目前支持 String 型 User account 的 SDK 如下：
  - Native SDK：v2.8.0 及之后版本
  - Web SDK：v2.5.0 及之后版本
- 如果你将用户名切换至 String 型，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account 加入频道，请确保你的服务端用户生成 Token 的脚本已升级至最新版本，并使用该 User account 或其对应的 Int 型 UID 来生成 Token。
- 如果频道中 Native SDK 和 Web SDK 互通，请确保该两者使用的用户名的类型一致。其中，Web SDK 中的 `uid` 可以设为 String 型或 Number 型。
