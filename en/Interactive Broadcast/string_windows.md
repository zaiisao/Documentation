
---
title: Use String User Accounts
description: 
platform: Windows
updatedAt: Wed Aug 07 2019 02:09:03 GMT+0800 (CST)
---
# Use String User Accounts
## Introduction
Many apps use string usernames. To reduce development costs, Agora adds support for string user accounts. Users can now directly use their string usernames as user accounts to join the Agora channel.

For other APIs, Agora uses the integer user ID for identification. Agora maintains a mapping table object that contains the string user account and integer user ID. You can get the user ID by passing in the user account, and vice versa.

To ensure smooth communication, all the users in a channel should use the same type of user account, that is, either the integer user ID, or the string user account.


## Implementation

Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md).

Starting with v2.8.0, you can use user accounts to identify the user.
  - `registerLocalUserAccount`: Registers a user account.
  - `joinChannelWithUserAccount`: Joins the channel with the registered user account.

The maximum string length of the user account is 255 bytes. Each user account should be unique in the channel. Supported character scopes are:

- The 26 lowercase English letters: a to z.
- The 26 uppercase English letters: A to Z.
- The 10 numbers: 0 to 9.
- The space.
- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",".

The following diagram shows how to join a channel with a string user account:

![](https://web-cdn.agora.io/docs-files/1562139189320)

**Note**:

- The `userAccount` parameter in the `registerLocalUserAccount` and `joinChannelWithUserAccount` methods is mandatory. Do not set it as null.

- The `registerLocalUserAccount` method is optional. To join a channel with a user account, you can choose either of the following:

  - Call the `registerLocalUserAccount` method to create a user account, and then the `joinChannelWithUserAccount` method to join the channel.
  - Call the `joinChannelWithUserAccount` method to join the channel.

  The difference between the two is that for the former, the time elapsed between calling the `joinChannelWithUserAccount` method and joining the channel is shorter than the latter.

- For other APIs, Agora uses the integer user ID for identification. You can call the  `getUserInfoByUid` or `getUserInfoByUserAccount` method to get the corresponding user ID or user account without maintaining the map.

## API Reference

- [`registerLocalUserAccount`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [`joinChannelWithUserAccount`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [`getUserInfoByUid`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [`getUserInfoByUserAccount`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [`onLocalUserRegistered`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [`onUserInfoUpdated`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)

## Sample Code
Agora provides an [Agora String Account](https://github.com/AgoraIO/Advanced-Video/tree/master/String-Account) sample code in the Github. You can download it and refer to the code logic in the sample code.



You can also refer to the following code snippets and implement string usernames in your peoject:

```C++
// Registers the local user account before joining the channel.
lpAgoraObject->RegisterLocalUserAccount(APP_ID, m_dlgEnterChannel.GetStringUid());
// Joins the channel with the registered user account.
lpAgoraObject->JoinChannelWithUserAccount(TOKEN, strChannelName, m_dlgEnterChannel.GetStringUid());
```



## Considerations
- Do not mix parameter types within the same channel. If you use SDKs that do not support string usernames, only integer user IDs can be used in the channel.
- If you change your app usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.
- If the Native SDK and Web SDK join the same channel, ensure that the user identification types are the same. For the Web SDK, the `uid` parameter can be set either as a number or as a string.

