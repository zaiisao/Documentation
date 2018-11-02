
---
title: Channel
description: 
platform: Channel
updatedAt: Fri Nov 02 2018 04:18:43 GMT+0000 (UTC)
---
# Channel
### Why am I automatically logged out of a device?

You will be automatically logged out of your current device if you login another device.

### In poor network conditions, does the SDK force users to leave a channel?

No, users will not automatically leave a channel unless they do so themselves, for example, the application calls leaveChannel.

### Does each channel/room need an administrator in a call?

No, an administrator can only be implemented in the signaling layer. Your signaling server sends commands and calls SDK interfaces for call management.

### Does the client need to maintain the channel?

No, a channel is created and deleted automatically. When all participants have left the channel, the channel will be deleted automatically.

### What are the App ID and Token? How can I use them?

See [Security Keys](../../en/Agora%20Platform/token.md), for details.

### If both the App ID and Token are used at the same time, which one prevails?

Token.

### How do I check who is talking in the channel?

The following callbacks for different platforms indicate who is talking and the speaker’s volume.

* For Android and Windows: onAudioVolumeIndication
* For iOS and macOS: reportAudioVolumeIndicationOfSpeakers

By default, this function is disabled. If needed, use the enableAudioVolumeIndication() method to configure it.

### When should I use the initialization-related API methods?

* Always ensure that the following method is called before joining a channel: virtual int IRtcEngine::initialize(const RtcEngineContext& context)
* If you want to enable the video function, call the following method before joining a channel: virtual int IRtcEngine::enableVideo()
* If you want to enable message transmission in the same channel, call the following method before joining a channel: virtual int IRtcEngine::enableVendorMessage()
