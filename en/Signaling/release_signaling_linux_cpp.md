
---
title: Release Notes
description: 
platform: Linux
updatedAt: Tue Feb 12 2019 06:20:57 GMT+0000 (UTC)
---
# Release Notes
## Overview

The Agora Signaling SDK facilitates real-time communications through functions such as call invitation, messaging, and state synchronization.

### Known Issues and Limitations

-   Each channel can hold up to 10,000 users at the same time. To reduce data volume and stress on the SDK, Agora recommends disabling notifications of a user going online or dropping offline for a large channel holding more than 1,000 users. For more information, see the `channelSetAttr` method. 
-   Channel Messages:
    -   Each message can be up to 8-k visible characters.
    -   A user can send messages at a maximum speed of 60 messages per second. In a channel, a maximum of 200 messages can be sent each second. 
    -   Offline channel messages are not supported.
-   Point-to-point messages:
    -   Each message can be up to 8-k visible characters.
    -   Offline messages are not supported.
-   Channel-attributes-updated callback:
    -   A maximum of 10 channel-attributes-updated callbacks can be sent each second.


## v1.4.0 BETA

The version 1.4.0 BETA was released on August 24, 2018. See below for issues fixed.

Fixed a crash issue caused by the DNS.

## v1.3.3

The version 1.3.3 was released on July 11, 2018. See below for issues fixed.

Fixed a crash issue caused by the DNS.

## v1.3.0 BETA

The version 1.3.0 BETA was released on May 4, 2018. See below for new features.

Added a special <code>_timeout</code> attribute to the <code>extra</code> parameter of <code>channelInviteUser2</code> to set the call timeout equal to or greater than 1 s.


## v1.2.1

The version 1.2.1 was released on March 20, 2018. See below for issues fixed.

Fixed the following issues:

-   Memory leakage.
-   Triggering of some callbacks.


## v1.2.0

The version 1.2.0 was released on March 7, 2018. See below for new features.

Initial release.



