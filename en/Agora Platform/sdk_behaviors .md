
---
title: SDK Reconnection Mechanism
description: 
platform: All Platforms
updatedAt: Fri May 10 2019 10:09:48 GMT+0800 (CST)
---
# SDK Reconnection Mechanism
### Does the Agora SDK reconnect when a user drops offline or a process gets killed?

#### User Drops Offline

User A and user B are in the same channel and user A loses network connection:

1.  User A loses connection with the server (no data is received from the server within four seconds):
    -   Android, Windows, or Linux: User A receives the `onConnectionInterrupted` callback.
    -   iOS or macOS: User A receives the `rtcEngineConnectionDidInterrupted` callback. 
    -   The Web: User A does not receive any callback. 
2.  After being disconnected, user A tries to reconnect to other servers until connection. 
 - If User A does not reconnect to a server within 10 seconds: 
        -   Android, Windows, or Linux: User A receives the `onConnectionLost` callback. 
        -   iOS or macOS: User A receives the `rtcEngineConnectionDidLost` callback. 
        -   The Web: User A does not receive any callback. 
	-   User B receives a callback if no packet is received from user A within a specific time frame: 
        -   Android, Windows, or Linux: If user B does not receive any packet from user A within 20 seconds, user B receives the `onUserOffline` callback. 
        -   iOS or macOS: If user B does not receive any packet from user A within 20 seconds, user B receives the `didOfflineOfUid` callback. 
        -   The Web: If user B does not receive any packet from user A within 10 seconds, user B receives the `client.on('stream-removed')` callback. 
    -   If user A reconnects to the server: 
        -   Android, Windows, or Linux: User A receives the `didRejoinChannel` callback. 
        -  iOS or macOS: User A receives the `didRejoinChannel` callback. 
        -   The Web: User A does not receive any callback. 

If user B does not receive any callback indicating that user A fails to reconnect, then user B does not receive any callback even if user A reconnects to the server.

If user B previously receives a callback indicating that user A fails to reconnect to the server, then: 
-   Android, Windows, or Linux: User B receives the `onUserJoined` callback.  
-   iOS or macOS: User B receives the `didJoinedOfUid` callback. 
-   The Web: User B receives the `client.on('stream-added')` callback. 

#### Process Gets Killed

This scenario involves the following situations: 

- Enables or disables VoIP mode.
- The process gets killed.
- Closes a web page.

User A and user B are in the same channel. 

When the process of user A gets killed: 

-   iOS or macOS: User A calls the `leaveChannel` method and user B receives a callback: 
    -   Android, Windows, or Linux: User B receives the ` onUserOffline` callback. 
    -   iOS or macOS: User B receives the `didOfflineOfUid` callback. 
    -   The Web: User B receives the `client.on('peer-leave')` callback. 

-   If user A is in Android, Windows, or Linux and user B uses the Native SDK:
    -   If user A does not restart the application and rejoin the original channel within 20 seconds, user B receives a callback:
        -   Android, Windows, or Linux: User B receives the `onUserOffline` callback. 
        -   iOS or macOS: User B receives the ` didOfflineOfUid` callback. 
    -   If user A restarts the application and rejoins the original channel within 20 seconds, user B does not receive any callback function. 
-  If user A is in Android, Windows, or Linux and user B uses the Web SDK: 
     - If user A does not restart the application and rejoin the original channel within 10 seconds, user B receives the `client.on('stream-removed')` callback. 
     - If user A restarts the application and rejoins the original channel, user B does not receive any callback. 
- For the Web SDK, killing a process is equivalent to a user dropping offline. 
-  If user A is the last user in the channel, the server destroys the channel in 10 seconds. 

