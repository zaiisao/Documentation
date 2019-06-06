
---
title: Release Notes
description: 
platform: Android
updatedAt: Thu Jun 06 2019 12:43:49 GMT+0800 (CST)
---
# Release Notes
## Overview

Designed as a substitute for the legacy Agora Signaling SDK, the Agora Real-time Messaging SDK provides a more streamlined implementation and more stable messaging mechanism for you to quickly implement real-time messaging scenarios.

> For more information about the SDK features and applications, see [Product Overview](../../en/Real-time-Messaging/RTM_product.md).

## v0.9.3

v0.9.3 is released on June 7th, 2019.

### New Features

#### Sends an (offline) peer-to-peer message to a specified user (receiver)

This version allows you to send a message to a specified user when he/she is offline. If you set a message as an offline message and the specified user is offline when you send it, the RTM server caches it. Please note that for now we only cache 200 offline messages for up to seven days for each receiver. When the number of the cached messages reaches this limit, the newest message overrides the oldest one.


#### User attribute-related operations

This version allows you to set or update a user's attributes. You can:

- Substitutes the local user's attributes with new ones.
- Adds or updates the local user's attribute(s).
- Deletes the local user's attributes using attribute keys.
- Clears all attributes of the local user.
- Gets all attributes of a specified user.
- Gets the attributes of a specified user using attribute keys.

> - Only after you successfully loggin in the Agora RTM system can you execute user attribute-related operations. Otherwise, the SDK triggers the `ATTRIBUTE_OPERATION_ERR_NOT_READY` error code.
> - The attributes you set will be clears when you log out of the RTM system.
> - You can only set a maximum of 16 KB attributes in a single method call. Otherwise, the SDK triggers the `ATTRIBUTE_OPERATION_ERR_SIZE_OVERFLOW` error code. 

### Improvements

- Supports creating an RTM channel before logging in the Agora RTM system. 
- Supports creating multiple RTM channels. But a user can only join a maximum of 20 RTM channels at the same time. When the number of the joined channels exceeds 20, the SDK triggers the `JOIN_CHANNEL_ERR_FAILURE` error code. 

### Issues Fixed

- Occasional system crashes.
- A user who has logged out of the Agora RTM system appears online to the other users until 30 seconds after. 

## v0.9.2 

v0.9.2 is released on May 8th, 2019.

### New Features

- Queries the Online Status of the Specified Users.
-  Renews the Token.

### Improvements

-  Supports a `userId` that starts with a space.

### Issues fixed



## v0.9.1

v0.9.1 is released on April 4th, 2019.

### New Features

This release adds the call invitation feature, allowing you to create, send, cancel, accept, and decline a call invitation in a one-to-one or one-to-many voice/video call. 


## v0.9.0

v0.9.0 is released on February 4th, 2019.

Initial version. 

Key features:

- Sends or receives peer-to-peer messages.
- Joins or leaves a channel.
- Sends or receives channel messages.
