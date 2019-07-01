
---
title: Channel-related FAQ
description: 
platform: All Platforms
updatedAt: Mon Jul 01 2019 15:09:19 GMT+0800 (CST)
---
# Channel-related FAQ
### In poor network conditions, does the SDK force users to leave a channel?

No, users do not automatically leave a channel unless they do so themselves. For example, the application calls the `leaveChannel` method.

### Does each channel/room need an administrator in a call?

No, an administrator is only implemented in the business management layer. Your signaling server sends commands and calls the SDK interfaces for call management.

### Does the client need to maintain the channel?

No, a channel is created and deleted automatically. When all users leave the channel, the channel is deleted automatically.

### What are the App ID and Dynamic Key? How can I use them?

See [Use Security Keys](../../en/voice/token.md) for details.

### How do I check who is speaking in the channel?

The following callbacks indicate who is speaking and the speakers' volume.

* For Android and Windows: `onAudioVolumeIndication`
* For iOS and macOS: `reportAudioVolumeIndicationOfSpeakers`

By default, these callbacks are disabled. You can use the `enableAudioVolumeIndication` method to enable them.

### What should I be aware of for initialization in Windows?

* Always ensure that the `initialize` method is called before joining a channel.
* If you want to enable the video function, call the `enableVideo` method before joining a channel.
* If you want to enable message transmission in the same channel, call the `enableVendorMessage` method before joining a channel.
