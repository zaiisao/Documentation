
---
title: Release Notes
description: 
platform: Java
updatedAt: Tue Feb 12 2019 06:23:53 GMT+0000 (UTC)
---
# Release Notes
## Overview

The Agora Server SDK facilitates real-time communications through functions such as call invitations, messaging, and state synchronization.

### Known Issues and Limitation

-   Each channel can hold up to 10,000 users at the same time. To reduce data volume and stress on the SDK, Agora recommends disabling notifications of a user going online or dropping offline for a large channel holding more than 1,000 users. For more information, see the `channelSetAttr` method. 
-   Channel Messages:
    -   Each message can be up to 8 k visible characters.
    -   A user can send messages at a maximum speed of 60 messages per second. In a channel a maximum of 200 messages can be sent each second.
    -   Offline channel messages are not supported for now.
-   Point-to-point messages:
    -   Each message can be up to 8 k visible characters.
    -   Offline messages are not supported for now.
-   Channel-attributes-updated callback:
    -   A maximum of 10 of this callback can be sent each second.
-   To access Agora’s signaling system through the SIP gateway, contact [support@agora.io](mailto:support@agora.io). A self-configuration solution will be available soon


## v1.4.0 BETA

The version 1.4.0 BETA was released on August 24, 2018. See below for issues fixed.

Fixed a crash issue caused by the DNS.

## v1.3.3

The version 1.3.3 was released on July 11, 2018. See below for issues fixed.

Fixed a crash issue caused by the DNS.

## v1.3.0 BETA

The version 1.3.0 BETA was released on May 4, 2018. See below for new features.

-   Added the <code>getStatus</code> interface.
-   Added the <code>channelQueryUserNum</code> interface to query the number of the users in the channel.
-   Added the <code>invoke</code> interface.
-   Added a special attribute <code>_timeout</code> to the <code>extra</code> parameter of <code>channelInviteUser2</code>. It sets the timeout for a call and should be set to equal to or greater than 1 s.


## v1.2.1

The version 1.2.1 was released on March 20, 2018. See below for issues fixed.

Fixed the following issues:

-   Memory leakage.
-   Login mechanism.
-   Triggering of some callbacks.


## v1.2.0 

The version 1.2.0 was released on March 7, 2018. See below for new features.

-   Added the <code>invoke</code> call mechanism.
-   Added internal channel attributes, including `_channel_ttl`,  `_member_num`, and `_auto_update_num`.
-   If you are currently logged in, any call of the <code>login</code> method will be neglected.
-   To abandon the previous login, you only need to call <code>logout</code> before calling <code>login</code> without having to wait until it is successfully executed.


## v1.1.4.19

The version 1.1.4.19 was released on November 4, 2017. See below for issues fixed.

Fixed IPv6 related issues.

## v1.1.3

The version 1.1.3 was released on April 30, 2017.

Internal improvements.

## v1.0

The version 1.0 was released on September 30, 2016.

This is the first release of the Agora Server SDK.



