
---
title: Use String User Accounts
description: 
platform: Windows
updatedAt: Wed Sep 18 2019 03:26:38 GMT+0800 (CST)
---
# Use String User Accounts
## Introduction
Many apps use string usernames. To reduce development costs, Agora adds support for string user accounts. Users can now directly use their string usernames as user accounts to join the Agora channel.

For other APIs, Agora uses the integer user ID for identification. Agora maintains a mapping table object that contains the string user account and integer user ID. You can get the user ID by passing in the user account, and vice versa.

To ensure smooth communication, all the users in a channel should use the same type of user account, that is, either the integer user ID, or the string user account.


## Implementation

Ensure that you understand the steps and code logic for implmenting the basic real-time communication functions. For details, see [Start a Call](../../en/Voice/start_call_windows.md) or [Start a Live Broadcast](../../en/Voice/start_live_windows.md).

Follow the steps to join an Agora channel with a string user account:

1. After initializing the IRtcEngine instance, call the `registerLocalUserAccount` method to register a loca user account.
2. Call the `joinChannelWithUserAccount` method to join a channel with the registered user account.
3. Call the `leaveChannel` method when you want to leave the channel.


### API call sequence

The following diagram shows how to join a channel with a string user account:

![](https://web-cdn.agora.io/docs-files/1568717548874)

**Note**:

- The `userAccount` parameter in the `registerLocalUserAccount` and `joinChannelWithUserAccount` methods is mandatory. Do not set it as null.

- The `registerLocalUserAccount` method is optional. To join a channel with a user account, you can choose either of the following:

  - Call the `registerLocalUserAccount` method to create a user account, and then the `joinChannelWithUserAccount` method to join the channel.
  - Call the `joinChannelWithUserAccount` method to join the channel.

  The difference between the two is that for the former, the time elapsed between calling the `joinChannelWithUserAccount` method and joining the channel is shorter than the latter.

- For other APIs, Agora uses the integer user ID for identification. You can call the  `getUserInfoByUid` or `getUserInfoByUserAccount` method to get the corresponding user ID or user account without maintaining the map.

### Sample Code

You can also refer to the following code snippets and implement string usernames in your peoject:

```C++

LRESULT COpenLiveDlg::OnJoinChannel(WPARAM wParam, LPARAM lParam)
{
	IRtcEngine		*lpRtcEngine = CAgoraObject::GetEngine();
	CAgoraObject	*lpAgoraObject = CAgoraObject::GetAgoraObject();
	
	// Registers the local user account before joining the channel.
	lpAgoraObject->RegisterLocalUserAccount(APP_ID, m_dlgEnterChannel.GetStringUid());
	// Joins the channel with the registered user account.
	lpAgoraObject->JoinChannelWithUserAccount(TOKEN, strChannelName, m_dlgEnterChannel.GetStringUid());

}
```

We provide an open-source [String-Account](https://github.com/AgoraIO/Advanced-Video/tree/master/String-Account) demo project that implements string user accounts on Github. You can try the demo and view the source code of the `OnJoinChannel` methods in the [OpenLiveDlg.cpp](https://github.com/AgoraIO/Advanced-Video/blob/master/String-Account/Agora-String-Account-Windows/OpenLive/OpenLiveDlg.cpp) file.

### API Reference

- [`registerLocalUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0d44b74ced4005ee86353c13186f870d)
- [`joinChannelWithUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a14f8c308c6c57c55653552b939a8527a)
- [`getUserInfoByUid`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#abf4572004e6ceb99ce0ff76a75c69d0b)
- [`getUserInfoByUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a4f75984d3c5de5f6e3e4d8bd81e3b409)
- [`onLocalUserRegistered`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a919404869f86412e1945c730e5219b20)
- [`onUserInfoUpdated`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad086cc4d8e5555cc75a0ab264c16d5ff)

## Considerations
- Do not mix parameter types within the same channel. If you use SDKs that do not support string usernames, only integer user IDs can be used in the channel. The following Agora SDKs support string user accounts:
  - The Native SDK: v2.8.0 and later.
  - The Web SDK: v2.5.0 and later.
- If you change your app usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts to join the channel, ensure that the token generation script on your server is updated to the latest version, and that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.
- If the Native SDK and Web SDK join the same channel, ensure that the user identification types are the same. For the Web SDK, the `uid` parameter can be set either as a number or as a string.

