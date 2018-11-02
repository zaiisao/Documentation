
---
title: SDK Behavior when a User Drops Offline or a Process Gets Kill
description: 
platform: All Platforms
updatedAt: Fri Nov 02 2018 04:18:50 GMT+0000 (UTC)
---
# SDK Behavior when a User Drops Offline or a Process Gets Kill
### Does Agora SDK have reconnection mechanism when a user drops offline or a process gets killed?

#### User Dropping Offline

User A and User B are in the same channel and User A loses network connection:

1.  User A loses connection with the server (no data was received from the server in four seconds):
    -   Android, Windows, or Linux: User A receives the `onConnectionInterrupted` callback function.
    -   iOS or macOS: User A receives the `rtcEngineConnectionDidInterrupted` callback function. 
    -   The Web: User A does not receive any callback function. 
2.  After being disconnected, user A keeps trying to connect to the other servers until a success. 
 - If User A does not reconnect to the server successfully in 10 seconds (you can set the timeout value. For more information, contact [support@agora.io](mailto:support@agora.io)): 
        -   Android, Windows, or Linux: User A receives the `onConnectionLost` callback function. 
        -   iOS or macOS: User A receives the `rtcEngineConnectionDidLost` callback function. 
        -   The Web: User A does not receive any callback function. 
	-   User B receives a callback if no packet was received from user A within a specific time frame: 
        -   Android, Windows, or Linux: If user B did not receive any packet from user A within 20 seconds, user B receives the `onUserOffline` callback function. 
        -   iOS or macOS: If user B did not receive any packet from user A within 20 seconds, user B receives the `didOfflineOfUid` callback function. 
        -   The Web: If user B did not receive any packet from user A within 10 seconds, user B receives the `client.on('stream-removed')` callback function. 
    -   If user A successfully reconnects to the server: 
        -   Android, Windows, or Linux: User A receives the `didRejoinChannel` callback function. 
        -  iOS or macOS: User A receives the `didRejoinChannel` callback function. 
        -   The Web: User A does not receive any callback function. 

If user B does not receive any callback indicating that user A has failed to reconnect, then user B will not receive a callback even if user A has successfully reconnected to the server.

If user B previously received a callback indicating that user A has failed to reconnect to the server, then: 
-   Android, Windows, or Linux: User B receives the `onUserJoined` callback function.  
-   iOS or macOS: User B receives the `didJoinedOfUid` callback function. 
-   The Web: User B receives the `client.on('stream-added')` callback function. 

#### Process Getting Killed

This scenario involves the following situations: 

- Enabling or disabling VoIP mode.
- Process getting killed.
- Closing a web page (The Web)

User A and user B are in the same channel. 

When the process of user A is killed: 

-   iOS or macOS: User A calls the `leaveChannel` method and user B will receive a callback function: 
    -   Android, Windows, or Linux: User B receives the ` onUserOffline` callback function. 
    -   iOS or macOS: User B receives the `didOfflineOfUid` callback function. 
    -   The Web: User B receives the `client.on('peer-leave')` callback function. 

-   If user A is in Android, Windows, or Linux and user B uses the Native SDK:
    -   If user A has not restarted the app and re-joined the original channel within 20 seconds (you can set the timeout value. For more information, contact [support@agora.io](mailto:support@agora.io)), user B will receive a callback function:
        -   Android, Windows, or Linux: User B receives the `onUserOffline` callback function. 
        -   iOS or macOS: User B receives the ` didOfflineOfUid` callback function. 
    -   If user A restarts the app and rejoins the original channel within 20 seconds, user B will not receive any callback function. 
-  If user A is in Android, Windows, or Linux and user B uses the Web SDK: 
     - If user A has not restarted the app and re-joined the original channel within 10 seconds, user B will receive the `client.on('stream-removed')` callback function. 
     - If user A restarts the app and rejoins the original channel, user B will not receive any callback function. 
- For the Web SDK, killing a process is equivalent to a user dropping offline. 
-  If user A is the last user in the channel, the server will destroy the channel in 10 seconds. 

