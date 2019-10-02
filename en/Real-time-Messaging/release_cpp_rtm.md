
---
title: Release Notes
description: 
platform: Linux CPP
updatedAt: Wed Oct 02 2019 07:09:58 GMT+0800 (CST)
---
# Release Notes
  ## Overview

Designed as a replacement for the legacy Agora Signaling SDK, the Agora Real-time Messaging (RTM) SDK provides a more streamlined implementation and stable messaging mechanism for you to quickly implement real-time messaging under various scenarios.

> For more information about the features and applications of the Agora RTM SDK, see [Product Overview](https://docs.agora.io/en/Real-time-Messaging/product_rtm?platform=All%20Platforms).

## v1.0.1

v1.0.1 is released on August 1st, 2019. 

### Issues Fixed

- When the connection to the Agora RTM system is interrupted, the SDK does not return the `onConnectionStateChanged` callback. 

## v1.0.0

v1.0.0 is released on July 24th, 2019.

### New Features

### Interconnects with the legacy Agora Signaling SDK

v1.0.0 implements the `ILocalCallinvitation::setChannelId` and `ILocalCallinvitation::getChannelId` methods. 

> - To intercommunicate with the legacy Agora Signaling SDK, you MUST set the channel ID. However, even if the callee successfully accepts the call invitation, the Agora RTM SDK does not join the channel of the specified channel ID.
> - If your App does not involve the legacy Agora Signaling SDK, we recommend using the `ILocalCallInvitation::setContent` method or the `IRemoteCallInvitation::setResponse` method to set customized contents. 

#### Specifies the default path to the SDK log file

Supports changing the default path to the SDK log file using the `setLogFile` method. To avoid creating an incomplete log file, we recommend calling this method once you have created and initialized an `IRtmService` instance. 



#### Sets the output log level of the SDK

Supports setting the output log level of the SDK using the `setLogFilter` method.  The log level follows the sequence of OFF, CRITICAL, ERROR, WARNING, and INFO. Choose a level to see the logs preceding that level. If, for example, you set the log level to WARNING, you see the logs within levels CRITICAL, ERROR, and WARNING. See also [LOG_FILTER_TYPE](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/namespaceagora_1_1rtm.html#af515252477afb2a71feef88113dfa481). 

> You can call this method once you have created and initializd an `IRtmService` instance. You do not have to call this method after calling the `login` method. 

#### Sets the log file size in KB

Supports setting the log file size using the `setLogFileSize` method. The log file has a default size of 512 KB. File size settings of less than 512 KB or greater than 10 MB will not take effect.


> You can call this method once you have created and initializd an `IRtmService` instance. You do not have to call this method after calling the `login` method. 

### Improvements

Adds error codes based on the following scenarios: 

- The Agora RTM service is not initialized.
- The method call frequency exceeds the limit. 
- The user does not call the `login` method or the method call of `login` does not succeed before calling any of the RTM core APIs. 

### Issues Fixed

- One can log in the Agora RTM system with a static App ID and an RTM token, which is generated from a dynamic App ID. 


### API Changes

- [setLogFile](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a08063b2692a6091ad5e8b30146498089): Specifies the default path to the SDK log file.
- [setLogFilter](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a41da0404ac726b7326ef1ca6213b2d61): Sets the output log level of the SDK.
- [setLogFileSize](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#ae7f4dcaf1173365ae5836bb23d5188f9): Sets the log file size in KB.



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
- A user who has logged out of the Agora RTM system appears online to the other users until 30 seconds later. 

## v0.9.2 

v0.9.2 is released on May 8th, 2019.

### New Features

- Queries the Online Status of the Specified Users.
-  Renews the Token.

### Improvements

-  Supports a `userId` that starts with a space.




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
