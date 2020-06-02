
---
title: How can I implement call notification in a call application?
description: 
platform: All Platforms
updatedAt: Tue Jun 02 2020 16:09:36 GMT+0800 (CST)
---
# How can I implement call notification in a call application?
## Description

To implement call notification, you need to integrate the Agora RTC SDK, the Agora RTM SDK, and platform-specific call APIs such as ConnectionService for Android, CallKit for iOS, and CallKeep for Flutter and React Native. The RTM SDK supports call notification only when the app is running. So, you also need to integrate platform-specific APIs to ensure that users can still receive call notifications when the app is on the background or the process is closed.

## Implementation

### Step 1: Integrate the RTC SDK and the RTM SDK

Refer to the following articles to learn how to integrate the RTC SDK and the RTM SDK:

- [RTC SDK quickstart](../../en/Interactive%20Broadcast/start_live_android.md)
- [RTM SDK quickstart](../../en/Real-time-Messaging/messaging_android.md)

### Step 2: Use the RTM SDK to implement the basic functionalities of call invitation

To implement call invitation for the RTM SDK, see [Call Invitation](../../en/Real-time-Messaging/rtm_invite_android.md). 

### Step 3: Integrate platform-specific call APIs and implement call notification

- For the Android platform, see [ConnectionService official documentation](https://developer.android.com/reference/android/telecom/ConnectionService) and the [sample projects](https://github.com/AgoraIO-Usecase/Video-Calling/tree/master/OpenDuo-Android) provided by Agora.
- For the iOS platform, see [CallKit official documentation](https://developer.apple.com/documentation/callkit) and the [sample projects](https://github.com/AgoraIO-Usecase/Video-Calling/tree/master/OpenDuo-iOS) provided by Agora.
- For the Flutter platform and the React Native platform, see [CallKeep official documentation](https://github.com/react-native-webrtc/react-native-callkeep).
