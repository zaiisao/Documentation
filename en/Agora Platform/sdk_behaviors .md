
---
title: SDK Reconnection Mechanism
description: 
platform: All Platforms
updatedAt: Tue May 21 2019 10:14:08 GMT+0800 (CST)
---
# SDK Reconnection Mechanism
This page shows the connection state mechanism of the Agora SDK.

The Agora SDK adds the [`onConnectionStateChanged`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)/[`connectionStateChangedToState`](https://docs.agora.io/en/Agora%20Platform/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback in v2.3.2. This callback reports the current network connection state and reasons to any state change.

### Before v2.3.2

The following diagram shows the callbacks received by the apps of UID 1 and UID 2 during which UID 1 joins the channel, experiences a network exception, loses connection, and rejoins the channel.

![](https://web-cdn.agora.io/docs-files/1557482391248)

Where:

- T0 = 0 s: The SDK receives the `joinChannel`/`joinChannelByToken` request from the app of UID 1.
- T1 ≈ T0 + 200 ms: 200 ms after calling the `joinChannel`/`joinChannelByToken` method, the app of UID 1 joins the channel and receives the `onJoinChannelSuccess`/`didJoinChannel` callback.
- T2 ≈ T1 + 100 ms: Due to network latency, the app of UID 2 detects UID 1 100 ms after the latter joins the channel. UID 2 receives the `onUserJoined`/`didJoinedOfUid` callback.
- T3: The uplink network condition of UID 1 deteriorates. The SDK automatically tries rejoining the channel.
- T4 = T3 + 4 s: If the app of UID 1 fails to receive any data from the server in 4 seconds, it receives the `onConnectionInterrupted`/`rtcEngineConnectionDidInterrupted` callback; meanwhile the SDK continues to try rejoining the channel.
- T5 = T3 + 15 s: If the app of UID 1 fails to receive any data from the server in 15 seconds, it receives the `onConnectionLost`/`rtcEngineConnectionDidLost` callback; meanwhile the SDK continue to try rejoining the channel.
- T6 = T3 + 20 s: If the app of UID 2 fails to receive any data from UID 1 in 20 seconds, the SDK decides that UID 1 is offline. The app of UID 2 receives the `onUserOffline`/`didOfflineOfUid` callback.
- T7: If the app of UID 1 successfully rejoins the channel, it receives the `onRejoinChannelSuccess`/`didRejoinChannel` callback.
- T8 ≈ T7 + 100ms: 100 ms after the app of UID 1 successfully rejoins the channel, the app of UID 2 receives the `onUserJoined`/`didJoinOfUid callback`, which means that UID 1 is online again.

### v2.3.2 and Later

The following diagram shows the callbacks received by the apps of UID 1 and UID 2 during which UID 1 joins the channel, experiences a network exception, loses connection, and eventually fails to rejoin the channel.

![](https://web-cdn.agora.io/docs-files/1557482418517)

Where:

- T0 = 0 s: The SDK receives the `joinChannel`/`joinChannelByToken` request from the app of UID 1.
- T1 ≈ T0 + 200 ms: 200 ms after calling the `joinChannel`/`joinChannelByToken` method, the app of UID 1 joins the channel.  During the process, the app of UID 1 also receives the `onConnectionStateChanged(CONNECTION_STATE_CONNECTING, CONNECTION_CHANGED_CONNECTING)`/`connectionChangedToState(AgoraConnectionStateConnecting, AgoraConnectionChangedConnecting)` callback. When successfully joining the channel, app of UID 1 receives the `onConnectionStateChanged(CONNECTION_STATE_CONNECTED, CONNECTION_CHANGED_JOIN_SUCCESS)`/`connectionChangedToState(AgoraConnectionStateConnected, AgoraConnectionChangedJoinSuccess)` and `onJoinChannelSuccess`/`didJoinChannel` callbacks. 
- T2 ≈ T1 + 100 ms: Due to network latency, the app of UID 2 detects UID 1 100 ms after the latter joins the channel. UID 2 receives the `onUserJoined`/`didJoinedOfUid` callback.
- T3: The uplink network condition of UID 1 deteriorates. The SDK automatically tries rejoining the channel.
- T4 = T3 + 4 s: If the app of UID 1 fails to receive any data from the server in 4 seconds, it receives the `onConnectionStateChanged(CONNECTION_STATE_RECONNECTING, CONNECTION_CHANGED_INTERRUPTED)`/`connectionChangedToState(AgoraConnectionStateReconnecting, AgoraConnectionChangedInterrupted)` callback; meanwhile the SDK continues to try rejoining the channel.
- T5 = T3 + 15 s: If the app of UID 1 fails to receive any data from the server in 15 seconds, it receives the `onConnectionLost`/`rtcEngineConnectionDidLost` callback; meanwhile the SDK continue to try rejoining the channel.
- T6 = T3 + 20 s: If the app of UID 2 fails to receive any data from UID 1 in 20 seconds, the SDK decides that UID 1 is offline. The app of UID 2 receives the `onUserOffline`/`didOfflineOfUid` callback.
- T7: If the app of UID 1 fails to rejoin the channel in 20 minutes, the SDK stops trying. The app of UID 1 receives the `onConnectionStateChanged(CONNECTION_STATE_FAILED, CONNECTION_CHANGED_JOIN_FAILED)`/`connectionChangedToState(AgoraConnectionStateFailed, AgoraConnectionChangedJoinFailed)` callback. UID 1 needs to leave the channel and call the `joinChannel`/`joinChannelByToken` method to join the channel.

> If UID 2 is a Web client, the behaviors of the Web app are as follows:
> - When UID 1 joins and rejoins the channel, UID 2 receives the `client.on('stream-added')` callback. 
> - If UID 2 does not receive any data from UID 1 in 10 seconds, UID 2 receives the `client.on('stream-removed')` callback.
> - If the server does not receive any data from UID 1 in 30 seconds, UID 2 receives the `client.on('peer-leave')` callback.

