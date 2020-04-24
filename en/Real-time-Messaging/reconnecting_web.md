
---
title: Manage Connection States
description: 
platform: Web
updatedAt: Wed Apr 22 2020 12:25:24 GMT+0800 (CST)
---
# Manage Connection States
## Connection State Definitions

The connection between the Agora RTM SDK and the Agora RTM system has the following five states:

- DISCONNECTED
- CONNECTING
- CONNECTED
- RECONNECTING
- ABORTED

> Whenever the connection switches state, the `ConnectionStateChanged` callback returns the new state and the reason of state change.

### DISCONNECTED

This is the initial connection state before the App calls the `login` method. 

- Once the App calls the `login` method, the local client receives the `ConnectionStateChanged` callback: 
  - The state switches to: `CONNECTING`;
  - Reason for the connection change: `LOGIN`. 

### CONNECTING

This state indicates that the App has called the `login` method and is now logging in the Agora RTM system. 

- If the SDK logs in the Agora RTM system,
  - The corresponding Promise resolves;
  - The local client receives the `ConnectionStateChanged` callback:
    - The new state: `CONNECTED `;
    - Reason for the connection change: `LOGIN_SUCCESS`.
- If the SDK fails to log in the Agora RTM system:
  - The corresponding Promise is rejected;
  - The local client receives the `ConnectionStateChanged`:
    - The new state: `DISCONNECTED`; 
    - Possible reasons for the connection change:
      - `LOGIN_FAILURE`: The login fails for unknow reasons. 
      - `LOGIN_TIMEOUT`: A timeout occurs. The SDK fails to log in the Agora RTM system in six seconds. 

### CONNECTED

This state indicates that the SDK has logged in the Agora RTM system and that the corresponding user is 'online'. In this state, the Agora RTM system checks the online status of the SDK through heartbeat. 
- If, for network reasons, the connection between the SDK and the Agora RTM system is interupted and cannot recover in four seconds: 
  - The local client automatically starts to reconnect to the Agora RTM system and receives the `ConnectionStateChanged` callback:
    - The new state: `RECONNECTING`;
    - Reason for the connection change: `INTERRUPTED `.
-  If another instance of the same `uid` logs in the Agora RTM system, the current instance will be kicked out: 
  - The local client receives the`ConnectionStateChanged` callback:
    - The new state: `ABORTED`;
    - Reason for the connection change: `REMOTE_LOGIN`.
- If the App calls the `logout` method to log out of the Agora RTM system: 
  - The local client receives the `ConnectionStateChanged`:
    - The new state: `DISCONNECTED`;
    - Reason for the connection change: `LOGOUT`.

### RECONNECTING

When the connection between the SDK and the Agora RTM system is interrupted and cannot recover in four seconds, the SDK enters this state. So long as the App does not call the `logout` method, the SDK keeps reconnecting to the Agora RTM system until success.

> Please note that the Agora RTM system presumes that the instance is online for 30 seconds since the connection to the SDK is interrupted. The system remove the instance from the online-user list if the SDK fails to reconnect to the Agora RTM system within 30 seconds. 

- If the SDK manages to reconnect to the Agora RTM system within 30 seconds, the SDK will take the instance to the channel that it was in when the interruption occurred：
  - The local client receives `ConnectionStateChanged` callback:
    - The new state: `CONNECTED`;
    - Reason for the connection change: `LOGIN_SUCCESS `.
- If the SDK manages to reconnect to the Agora RTM system after 30 seconds, the SDK will take the instance to the channel, which the user was in when the interruption occurred, and keep the user attributes in sync with the server：
  - The local client receives `ConnectionStateChanged` callback:
    - The new state: `CONNECTED `;
    - Reason for the connection change: `LOGIN_SUCCESS `.
  - All remote memebers of the channel(s), which the user was in when the interrruption occurred: 
    - Receive the `MemberLeft` callback 30 seconds after the interruption occurs, if the SDK still fails to reconnect. 
    - Receive the `MemberJoined` callback if the SDK manages to reconnect after 30 seconds. 
- The SDK keeps trying to reconnect to the Agora RTM system in the `RECONNECTING` state. If the token expires during this period, the SDK returns the `TokenExpired` callback. The return of this callback does not change the connection state. 
- If the SDK still cannot reconnect to the Agora RTM system, it stays in this connection state. You can call the `logout` method to log out. Then:
  - The local client receives `ConnectionStateChanged` callback:
    - The new state: `DISCONNECTED`
    - Reason for the connection change:  ` LOGOUT `.
  - The corresponding Promise resolves.

### ABORTED 

When another instance of the same `uid` logs in the Agora RTM system,  the current instance will be kicked out and switch to this state. At this point, we recommend that you call the `logout` method to log out of the system and call the `login` method at an appropriate time. 


## Considerations

- When the connection is interrupted, the SDK starts to reconnect to the Agora RTM system. This is an automatic reaction and does not need human intervention. 
- You will not receive any callback when the SDK manages to reconnect to the Agora RTM system. The callback only returns when the `login` method call succeeds. 
- If the SDK does not manage to reconnect to the Agora RTM system 30 seconds after the connection is interrupted, the Agora RTM system removes the corresponding user from the online-user list it maintains. 
