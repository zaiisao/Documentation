
---
title: Release Notes
description: 
platform: Web
updatedAt: Tue Feb 19 2019 07:17:39 GMT+0000 (UTC)
---
# Release Notes
## Overview

The Agora Signaling SDK facilitates real-time communications through functions such as call invitations, messaging, and state synchronization.

### Known Issues and Limitations

-   Each channel can hold up to 10,000 users at the same time. To reduce data volume and stress on the SDK, Agora recommends disabling notifications of a user going online or dropping offline for a large channel holding more than 1,000 users. For more information, see the `channelSetAttr` method. 
-   Channel Messages:
    -   Each message can be up to 8-k visible characters.
    -   A user can send messages at a maximum speed of 60 messages per second. In a channel, a maximum of 200 messages can be sent each second. 
    -   Offline channel messages are not supported.
    -   Supports UTF-8 character encoding only.
-   Point-to-point messages:
    -   Each message can be up to 8-k visible characters.
    -   Offline messages are not supported.
    -   Supports UTF-8 character encoding only.
-   Channel-attributes-updated callback:
    -   A maximum of 10 channel-attributes-updated callbacks can be sent each second.


## v1.4.0 BETA

The version 1.4.0 BETA was released on August 24, 2018. See below for new features.

-   Added two parameters to the login method:
    -   <code>reconnect_count</code>, the maximum times allowed to reconnect (10 by default);
    -   <code>reconnect_timeout</code>, the reconnection timeout (30 seconds by default).
-   Added support for the UMD definition.
-   Added support for WeChat Mini-Apps.
-   Added the <code>signal.setDoLog(isEnabled, Level)</code> method. Added support for the logging level. The <code>isEnabled</code> parameter is set to false by default; the <code>Level</code> parameter is set to DEBUG by default.
-   Added automatic handling of events such as:
    -   Network disconnection, or
    -   Switching networks or changing IPs.


## v1.3.3

The version 1.3.3 was released on July 11, 2018. See below for issues fixed.

Fixed a crash issue caused by the DNS.

## v1.3.0 BETA

The version 1.3.0 BETA was released on May 4, 2018. See below for new features.

-   Added the <code>getSDKVersion</code> interface.
-   Added the <code>getStatus</code> interface.
-   Added a special <code>_timeout</code> attribute to the <code>extra</code> parameter of <code>channelInviteUser2</code> to set the call timeout equal to or greater than 1 s.
-   Added support for the npm package management tool, `AgoraSigSDK.min.js`.


## v1.2.1

The version 1.2.1 was released on March 20, 2018. See below for issues fixed.

Fixed the following issues:

-   Memory leakage.
-   Triggering of some callbacks.


## v1.2.0

The version 1.2.0 was released on March 7, 2018. See below for new features.

-   Added the <code>invoke</code> call mechanism.
-   Added internal channel attributes, including` _channel_ttl`, `_member_num`, and `_auto_update_num`.
-   If you are currently logged in, any call of the <code>login</code> method will be neglected.
-   To abandon the previous login, you only need to call <code>logout</code> before calling <code>login</code> without having to wait until it is successfully executed.


## v1.1.3

The version 1.1.3 was released on August 16, 2017. See below for improvements.

Improved the quality and stability of the signaling system login.

## v1.1.1

The version 1.1.1 was released on March 10, 2017.

Internal improvements.

## v1.0 

The version 1.0 was released on September 30, 2016.

This is the first release of the Agora Signaling SDK, which includes:

-   The Agora Signaling SDK for Android
-   The Agora Signaling SDK for iOS
-   The Agora Signaling SDK for Windows
-   The Agora Signaling SDK for Mac


The Agora Signaling SDK provides the following functions:

-   Account Information System
-   Channel System
-   Call System


You can access the Agora Signaling System in the following ways:

-   SDK Access
-   Server API Access
-   SIP Gateway Access




