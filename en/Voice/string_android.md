
---
title: Use String User Accounts
description: 
platform: Android
updatedAt: Wed Sep 18 2019 03:25:48 GMT+0800 (CST)
---
# Use String User Accounts
## Introduction
Many apps use string usernames. To reduce development costs, Agora adds support for string user accounts. Users can now directly use their string usernames as user accounts to join the Agora channel.

For other APIs, Agora uses the integer user ID for identification. Agora maintains a mapping table object that contains the string user account and integer user ID. You can get the user ID by passing in the user account, and vice versa.

To ensure smooth communication, all the users in a channel should use the same type of user account, that is, either the integer user ID, or the string user account.



## Implementation

Ensure that you understand the steps and code logic for implmenting the basic real-time communication functions. For details, see [Start a Call](../../en/Voice/start_call_android.md) or [Start a Live Broadcast](../../en/Voice/start_live_android.md).

Follow the steps to join an Agora channel with a string user account:

1. After initializing the RtcEngine instance, call the `registerLocalUserAccount` method to register a loca user account.
2. Call the `joinChannelWithUserAccount` method to join a channel with the registered user account.
3. Call the `leaveChannel` method when you want to leave the channel.

### API call sequence

The following diagram shows how to join a channel with a string user account:

![](https://web-cdn.agora.io/docs-files/1568711868522)

**Note**:

- The `userAccount` parameter in the `registerLocalUserAccount` and `joinChannelWithUserAccount` methods is mandatory. Do not set it as null.

- The `registerLocalUserAccount` method is optional. To join a channel with a user account, you can choose either of the following:

  - Call the `registerLocalUserAccount` method to create a user account, and then the `joinChannelWithUserAccount` method to join the channel.
  - Call the `joinChannelWithUserAccount` method to join the channel.

  The difference between the two is that for the former, the time elapsed between calling the `joinChannelWithUserAccount` method and joining the channel is shorter than the latter.

- For other APIs, Agora uses the integer user ID for identification. You can call the  `getUserInfoByUid` or `getUserInfoByUserAccount` method to get the corresponding user ID or user account without maintaining the map.

### Sample Code

You can also refer to the following code snippets and implement string user accounts in your peoject:

```java
private void initializeAgoraEngine() {
  try {
    String appId = getString(R.string.agora_app_id);
    mRtcEngine = RtcEngine.create(getBaseContext(), appId, mRtcEventHandler);
    // Registers the local user account after initializing the Agora engine and before joining the channel.
    mRtcEngine.registerLocalUserAccount(appId, mLocal.userAccount);
  } catch (Exception e) {
    Log.e(LOG_TAG, Log.getStackTraceString(e));
    
    throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
  }
}
  
private void joinChannel() {
  String token = getString(R.string.agora_access_token);
  if (token.isEmpty()) {
    token = null;
  }
  // Joins the channel with the registered user account.
  mRtcEngine.joinChannelWithUserAccount(token, "stringifiedChannel1", mLocal.userAccount);
}
```

We provide an open-source [String-Account](https://github.com/AgoraIO/Advanced-Video/tree/master/String-Account) demo project that implements string user accounts on Github. You can try the demo and view the source code of the `initializeAgoraEngine` and `joinChannel` methods in the [CallActivity.java](https://github.com/AgoraIO/Advanced-Video/blob/master/String-Account/Agora-String-Account-Android/app/src/main/java/io/agora/tutorials.stringified.account/CallActivity.java) file.


### API Reference

- [`registerLocalUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa37ea6307e4d1513c0031084c16c9acb)
- [`joinChannelWithUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a310dbe072dcaec3892c4817cafd0dd88)
- [`getUserInfoByUid`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9a787b8d0784e196b08f6d0ae26ea19c)
- [`getUserInfoByUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#afd4119e2d9cc360a2b99eef56f74ae22)
- [`onLocalUserRegistered`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aca1987909703d84c912e2f1e7f64fb0b)
- [`onUserInfoUpdated`](https://docs.agora.io/en/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa3e9ead25f7999272d5700c427b2cb3d)


## Considerations
- Do not mix parameter types within the same channel. If you use SDKs that do not support string usernames, only integer user IDs can be used in the channel. The following Agora SDKs support string user accounts:
  - The Native SDK: v2.8.0 and later.
  - The Web SDK: v2.5.0 and later.
- If you change your app usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts to join the channel, ensure that the token generation script on your server is updated to the latest version, and that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.
- If the Native SDK and Web SDK join the same channel, ensure that the user identification types are the same. For the Web SDK, the `uid` parameter can be set either as a number or as a string.

