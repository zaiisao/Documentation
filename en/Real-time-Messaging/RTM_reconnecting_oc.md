
---
title: Manage Connection States
description: 
platform: iOS,macOS
updatedAt: Tue Sep 24 2019 10:54:46 GMT+0800 (CST)
---
# Manage Connection States
## Connection State Definitions

The connection between the Agora RTM SDK and the Agora RTM system has the following five states:

- AgoraRtmConnectionStateDisconnected
- AgoraRtmConnectionStateConnecting
- AgoraRtmConnectionStateConnected
- AgoraRtmConnectionStateReconnecting
- AgoraRtmConnectionStateAborted

> Whenever the connection switches state, the `connectionStateChanged` callback returns the new state and the reason of state change.

### AgoraRtmConnectionStateDisconnected

This is the initial connection state before the App calls the `loginByToken` method. 

- Once the App calls the `loginByToken` method, the local client receives the `connectionStateChanged` callback: 
  - The state switches to: `AgoraRtmConnectionStateConnecting`;
  - Reason for the connection change: `AgoraRtmConnectionChangeReasonLoginSuccess`. 

### AgoraRtmConnectionStateConnecting

This state indicates that the App has called the `loginByToken` method and is now logging in the Agora RTM system. 

- If the SDK logs in the Agora RTM system,
  - The local client receives the `AgoraRtmLoginErrorOk` error code;
  - The local client receives the `connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateConnected `;
    - Reason for the connection change: `AgoraRtmConnectionChangeReasonLoginSuccess`.
- If the SDK fails to log in the Agora RTM system:
  - The local client receives the corresponding error code.
  - The local client receives the `connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateDisconnected`; 
    - Possible reasons for the connection change:
      - `AgoraRtmConnectionChangeReasonLoginFailure`: The login fails for unknow reasons. 
      - `AgoraRtmConnectionChangeReasonLoginTimeout`: A timeout occurs. The SDK fails to log in the Agora RTM system in six seconds. 

### AgoraRtmConnectionStateConnected

This state indicates that the SDK has logged in the Agora RTM system and that the corresponding user is 'online'. In this state, the Agora RTM system checks the online status of the SDK through heartbeat. 
- If, for network reasons, the connection between the SDK and the Agora RTM system is interupted and cannot recover in four seconds: 
  - The local client automatically starts to reconnect to the Agora RTM system and receives the `connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateReconnecting`;
    - Reason for the connection change: `AgoraRtmConnectionChangeReasonInterrupted`.
-  If another instance of the same `uid` logs in the Agora RTM system, the current instance will be kicked out: 
  - The local client receives the`connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateAborted`;
    - Reason for the connection change: `AgoraRtmConnectionChangeReasonRemoteLogin`.
- If the App calls the `logout` method to log out of the Agora RTM system: 
  - The local client receives the `connectionStateChanged`:
    - The new state: `AgoraRtmConnectionStateDisconnected`;
    - Reason for the connection change: `AgoraRtmConnectionChangeReasonlogout`.

### AgoraRtmConnectionStateReconnecting

When the connection between the SDK and the Agora RTM system is interrupted and cannot recover in four seconds, the SDK enters this state. So long as the App does not call the `logoutWithCompletion` method, the SDK keeps reconnecting to the Agora RTM system until success.

> Please note that the Agora RTM system presumes that the instance is online for 30 seconds since the connection to the SDK is interrupted. The system remove the instance from the online-user list if the SDK fails to reconnect to the Agora RTM system within 30 seconds. 

- If the SDK manages to reconnect to the Agora RTM system within 30 seconds, the SDK will take the instance to the channel that it was in when the interruption occurred：
  - The local client receives `connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateConnected`;
    - Reason for the connection change: `AgoraRtmConnectionChangeReasonLoginSuccess`.
- If the SDK manages to reconnect to the Agora RTM system after 30 seconds, the SDK will take the instance to the channel, which the user was in when the interruption occurred, and keep the user attributes in sync with the server：
  - The local client receives `connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateConnected `;
    - Reason for the connection change: `AgoraRtmConnectionChangeReasonLoginSuccess`.
  - All remote memebers of the channel(s), which the user was in when the interrruption occurred: 
    - Receive the `memberLeft` callback 30 seconds after the interruption occurs, if the SDK still fails to reconnect. 
    - Receive the `memberJoined` callback if the SDK manages to reconnect after 30 seconds. 
- The SDK keeps trying to reconnect to the Agora RTM system in the `AgoraRtmConnectionStateReconnecting` state. If the token expires during this period, the SDK returns the `rtmKitTokenDidExpire` callback. The return of this callback does not change the connection state. 
- If the SDK still cannot reconnect to the Agora RTM system, it stays in this connection state. You can call the `logoutWithCompletion` method to log out. Then:
  - The local client receives `connectionStateChanged` callback:
    - The new state: `AgoraRtmConnectionStateDisconnected`
    - Reason for the connection change:  `AgoraRtmConnectionChangeReasonLogout`.
  - The local client receives the`AgoraRtmLogoutErrorOk` callback. 

### AgoraRtmConnectionStateAborted 

When another instance of the same `uid` logs in the Agora RTM system,  the current instance will be kicked out and switch to this state. At this point, we recommend that you call the `logoutWithCompletion` method to log out of the system and call the `loginByToken` method at an appropriate time. 


## Considerations

- When the connection is interrupted, the SDK starts to reconnect to the Agora RTM system. This is a automatic reaction and does not need human intervention. 
- You will not receive an `AgoraRtmLoginErrorOk` error code when the SDK manages to reconnect to the Agora RTM system. The callback only returns when the `loginByToken` method call succeeds. 
- If the SDK does not manage to reconnect to the Agora RTM system 30 seconds after the connection is interrupted, the Agora RTM system removes the corresponding user from the online-user list it maintains. 
