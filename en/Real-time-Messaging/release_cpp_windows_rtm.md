
---
title: Release Notes
description: 
platform: Windows CPP
updatedAt: Wed Oct 02 2019 07:10:25 GMT+0800 (CST)
---
# Release Notes
  ## Overview

Designed as a replacement for the legacy Agora Signaling SDK, the Agora Real-time Messaging (RTM) SDK provides a more streamlined implementation and stable messaging mechanism for you to quickly implement real-time messaging under various scenarios.

> For more information about the features and applications of the Agora RTM SDK, see [Product Overview](https://docs.agora.io/en/Real-time-Messaging/product_rtm?platform=All%20Platforms).

## v1.1.0

v1.1.0 was released on September 30, 2019. It adds the following features: 


### Sends an (offline) peer-to-peer message to a specified user (receiver)

This version allows you to send a message to a specified user when he/she is offline. If you set a message as an offline message and the specified user is offline when you send it, the RTM server caches it. Please note that for now we only cache 200 offline messages for up to seven days for each receiver. When the number of the cached messages reaches this limit, the newest message overrides the oldest one.



<a name="getcount"></a>
### Gets the member count of specified channel(s)

You can now get the member count of specified channel(s) without the need to join, by calling the [getChannelMemberCount](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a41dee47c6201acb2f29371b6e30249a5) method. You can get the member counts of a maximum of 32 channels in one method call. 


### Queries the Online Status of the Specified Users

### User attribute operations

This version allows you to set or update a user's attributes. You can:

- Substitutes the local user's attributes with new ones.
- Adds or updates the local user's attribute(s).
- Deletes the local user's attributes using attribute keys.
- Clears all attributes of the local user.
- Gets all attributes of a specified user.
- Gets the attributes of a specified user using attribute keys.

> - Only after you successfully loggin in the Agora RTM system can you execute user attribute-related operations. Otherwise, the SDK triggers the `ATTRIBUTE_OPERATION_ERR_NOT_LOGGED_IN` error code.
> - The attributes you set will be clears when you log out of the RTM system.
> - You can only set a maximum of 16 KB attributes in a single method call. Otherwise, the SDK triggers the `ATTRIBUTE_OPERATION_ERR_SIZE_OVERFLOW` error code. 


### Channel attribute operations

Supports setting or getting the attribute(s) of a specified channel. You can use this feature to create group anouncement.

Each channel attribute comes as a key-value pair. See [IRtmChannelAttribute](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_channel_attribute.html) for more information. Where: 

- The key of each channel attribute must be visible characters and not exceed 8 KB.
- Each channel attribute must not exceed 8 KB in length. 
- The overall size of the attributes of a channel must not exceed 32 KB. 
- The number of attributes of a channel must not exceed 32. 

Specific features: 

- Sets the attributes of a specified channel with new ones.
- Adds or updates the attribute(s) of a specified channel.
- Deletes the attributes of a specified channel by attribute keys.
- Clears all attributes of a specified channel.
- Gets all attributes of a specified channel.
- Gets the attributes of a specified channel by attribute keys.

When updating attributes of a channel, you can use the  [enableNotificationToChannelMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/structagora_1_1rtm_1_1_channel_attribute_options.html#a9a29721df90beca76974a5e348902530) flag to decide whether or not to notify all members of the channel about this attribute change. 


### Call invitation

Adds the call invitation feature, allowing you to create, send, cancel, accept, and decline a call invitation in a one-to-one or one-to-many voice/video call. 


### Joins or leaves a channel
### Sends or receives channel messages

<a name="oncount"></a>
### Automatically returns the latest numer of members in the current channel 

If you are already in a channel, you do not have to call the `getChannelMemberCount` method to get the member count of the current channel. We also do not recommend using `onMemberJoined` and `onMemberLeft` to keep track of the member counts. As of this release, the SDK returns to the channel members [onMemberCountUpdated](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel_event_handler.html#aff85052bb2a46c3220789c1ef90aa01e) the latest channel member count when the number of channel members changes. Note that:

- When the number of channel members ≤ 512, the SDK returns this callback when the number changes and at a MAXIMUM speed of once per second.
- When the number of channel members exceeds 512, the SDK returns this callback when the number changes and at a MAXIMUM speed of once every three seconds.



> Please treat this callback and the [onGetMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel_event_handler.html#a5e3f5a5ae0b3861de2f0310841ad0b37) callback separately: 
> - The former is an automatic callback. It returns the current numer of channel members;
> - The latter is triggered by the [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel.html#a3f9c943059ac48a568c81798da38c3cb) method. It returns a member list of the current channel. If the number of channel members exceeds 512, the SDK only returns a list of 512 randomly-selected channel members. 


###  Renews the Token

### Specifies the default path to the SDK log file

Supports changing the default path to the SDK log file using the `setLogFile` method. To avoid creating an incomplete log file, we recommend calling this method once you have created and initialized an `IRtmService` instance. 



### Sets the output log level of the SDK

Supports setting the output log level of the SDK using the `setLogFilter` method.  The log level follows the sequence of OFF, CRITICAL, ERROR, WARNING, and INFO. Choose a level to see the logs preceding that level. If, for example, you set the log level to WARNING, you see the logs within levels CRITICAL, ERROR, and WARNING. See also [LOG_FILTER_TYPE](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_cpp/namespaceagora_1_1rtm.html#af515252477afb2a71feef88113dfa481). 

> You can call this method once you have created and initializd an `IRtmService` instance. You do not have to call this method after calling the `login` method. 

### Sets the log file size in KB

Supports setting the log file size using the `setLogFileSize` method. The log file has a default size of 512 KB. File size settings of less than 512 KB or greater than 10 MB will not take effect.


> You can call this method once you have created and initializd an `IRtmService` instance. You do not have to call this method after calling the `login` method. 

