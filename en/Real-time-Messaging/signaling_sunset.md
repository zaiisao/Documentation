
---
title: Agora Signaling Sunset Plan
description: 
platform: All Platforms
updatedAt: Fri May 08 2020 09:52:07 GMT+0800 (CST)
---
# Agora Signaling Sunset Plan
## Agora Signaling Sunset Plan

-  As of August 1, 2019, the Agora Signaling system was sunset. No new features will be added, and only bug fixes will be made. 
-  As of August 12, 2019, all related documents were be moved [here](https://docs.agora.io/en/Signaling/product_signaling?platform=All%20Platforms).
- As of December 31, 2019, we stopped updating the legacy Agora Signaling system, but kept the system in maintenance mode. 
- On June 30, 2020, we will shut down the Signaling servers and the Signaling service will end.

## Compatibility between the Signaling SDK and the Agora RTM SDK

Agora RTM SDK v1.0 is compatible with the legacy Agora Signaling SDK except for the following features or versions.

|  Signaling Features or Versions          | Description                                                         | Our recommendation or comments      |
| ---------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------- |
| Special features                                     | <ul><li>Special channel: <code>\__agora_channel_event</code> and  <code>\__agora_user_online</code></li>  <li>Special channel attributes: <code>_userNotification</code> and <code> _auto_update_total_num</code></li> <li>The invoke feature: <code>channel_query_userlist_all</code></li> <li> SDK customization: <code>setBackground</code></li></ul> | <ul><li>Agora.io plans to support <code>_agora_channel_event</code> and <code>_agora_user_online</code>. Expect more information on this in the future.</li> <li>No plan to support the other special features.</li></ul> |
| Earlier signaling SDK versions | Earlier than v1.1    | Upgrade to the latest Signaling SDK version to enable intercommunication with the RTM SDK.  |
| The Signaling Wechat mini-app SDK |                                                                  | Will be supported soon. |
| Sends a non-string message (Signaling Web) | Puts a non-string as `msg` in the `messageInstantSend(peer, msg, cb)` or `messageChannelSend(msg, cb)` method. | Update your integration code to enable intercommunication with the RTM SDK. |
| PSTN                      |                                                                 | No further plans to support. |
	
	
> To switch from the legacy Signaling SDK to the Agora RTM SDK, see [Agora RTM SDK vs. Signaling](../../en/Real-time-Messaging/rtm_signaling_android.md). 



