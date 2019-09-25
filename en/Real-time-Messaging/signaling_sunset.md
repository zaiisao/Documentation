
---
title: Agora Signaling Sunset Plan
description: 
platform: All Platforms
updatedAt: Wed Sep 25 2019 07:06:09 GMT+0800 (CST)
---
# Agora Signaling Sunset Plan
## Agora Signaling Sunset Plan

-  As of August 1st, 2019, the Agora Signaling system will enter the sunset phase. We will not add new features during this period, but only fix reported bugs. As of August 12, 2019, all related documents will be moved [here](https://docs.agora.io/en/Signaling/product_signaling?platform=All%20Platforms).
- As of December 31st, we will stop updating the Agora Signaling system, and only ensure that the Agora Signaling SDK can function in the interim. 
- On June 30, 2020, we will put an end to our Signaling service and shut down the Signaling servers.

## Compatibility between the Signaling SDK and the Agora RTM SDK

Agora RTM SDK v1.0 is compatible with the legacy Agora Signling SDK except for the following features or versions.  Our Dashboard will soon send you a note about this if you are using features that the Agora RTM SDK does not support. Please keep tuned. 

|  Signaling Features or Versions          | Description                                                         | Our recommendation or comments      |
| ---------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------- |
| Special features                                     | <li>Special channel: <code>\__agora_channel_event</code>、<code>\__agora_user_online</code>  <li>Special channel attributes: <code>_userNotification</code>, <code> _auto_update_total_num</code> <li>The invoke feature: <code>channel_query_userlist_all</code> <li> SDK customization: <code>setBackground</code> | <li>We plan to support <code>_agora_channel_event</code> and <code>_agora_user_online</code>, and will contact you when that happens. <li> No plan to support the other special features. |
| Earlier signaling SDK versions | Earlier than v1.1    | Upgrade to the latest Signaling SDK version and have us enable intercommunication with the RTM SDK.  |
| The Signaling Wechat mini-app SDK |                                                                  | Plan to support it soon. |
| Sends a non-string message (Signaling Web) | Puts a non-string as `msg` in the `messageInstantSend(peer, msg, cb)` or `messageChannelSend(msg, cb)` method. | Update your integration code and have us enable intercommunication with the RTM SDK. |
| PSTN                      |                                                                 | No plan to support. |
	
We will continue to drive solutions for incompatibility issues. Should you come across any compatibility issue or have special requirements, please contact our support team or send a note to sales-us@agora.io.
	
> See the [Agor RTM SDK vs. Signaling](../../en/Real-time-Messaging/RTM_vs_signaling_android.md), if you want to switch from the legacy Signaling SDK to the Agora RTM SDK. 




