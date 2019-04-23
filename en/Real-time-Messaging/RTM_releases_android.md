
---
title: Agora RTM SDK Release Notes
description: 
platform: Android
updatedAt: Tue Apr 23 2019 06:42:23 GMT+0800 (CST)
---
# Agora RTM SDK Release Notes
This page describes the differences between v0.9.1 and v0.9.0 of the Agora RTM Android SDK.

v0.9.1:
1. Renamed the IResultCallback class to ResultCallback in order to conform to the Java naming convention.
2. Removed the IStateListener class for listening to message sending states, and used the ResultCallback class instead for listening to the peer or channel message sending results.
   - Success: the SDK returns the ResultCallback.onSuccess() callback. 
   - Failure: the SDK returns the ResultCallback.onFailure() callback with the corresponding ChannelMessageError or PeerMessageError enum. 

3. Renamed the destroy() method to release() for releasing all resources used by the RtmClient instance. 

4. Removed PEER_MESSAGE_RECEIVED_BY_SERVER from the PeerMessageError enums.

| v0.9                                                         | v0.9.1                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| interface PeerMessageState  <br>int PEER_MESSAGE_INIT = 0; <br>int PEER_MESSAGE_FAILURE = 1; <br>int PEER_MESSAGE_PEER_UNREACHABLE = 2; <br>int PEER_MESSAGE_RECEIVED_BY_PEER = 3; <br>int PEER_MESSAGE_SENT_TIMEOUT = 4; | interface PeerMessageError  <br>int PEER_MESSAGE_ERR_OK = 0; <br>int PEER_MESSAGE_ERR_FAILURE = 1; <br>int PEER_MESSAGE_ERR_TIMEOUT = 2; <br>int PEER_MESSAGE_ERR_PEER_UNREACHABLE = 3; |

5. Removed CHANNEL_MESSAGE_RECEIVED_BY_SERVER from the ChannelMessageError enums. 

| v0.9                                                         | v0.9.1                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| interface ChannelMessageState  <br>int CHANNEL_MESSAGE_INIT = 0; <br>int CHANNEL_MESSAGE_FAILURE = 1; <br>int CHANNEL_MESSAGE_RECEIVED_BY_SERVER = 2; <br>int CHANNEL_MESSAGE_SENT_TIMEOUT = 4; | interface ChannelMessageError <br>int CHANNEL_MESSAGE_ERR_OK = 0; <br>int CHANNEL_MESSAGE_ERR_FAILURE = 1; <br>int CHANNEL_MESSAGE_ERR_TIMEOUT = 2; |

6. You can call the object method of rtmClient to create a peer-to-peer message.

7. Added methods and enums for the call invitation function: 

7.1 LocalInvitation.java
  - public interface LocalInvitation
  - void setContent(String content)
  - String getContent();
  - void setChannelId(String channelId);
  - String getChannelId();
  - String getResponse();
  - int getState();
7.2 RemoteInvitation.java
  - public interface RemoteInvitation 
  - String getCallerId();
  - String getContent();
  - void getChannelId();
  - void setResponse(String response)
  - String getResponse();
  - int getState
7.3 RtmCallEventListener.java
  - public interface RtmCallEventListener 
  - void onLocalInvitationReceivedByPeer;
  - void onLocalInvitationAccepted;
  - void onLocalInvitationRefused;
  - void onLocalInvitationCanceled;
  - void onLocalInvitationFailure;
  - void onRemoteInvitationReceived;
  - void onRemoteInvitationAccepted;
  - void onRemoteInvitationRefused;
  - void onRemoteInvitationCanceled;
  - void onRemoteInvitationFailure;
7.4 RtmCallManager.java
  - public abstract class RtmCallManager 
  - public abstract void setEventListener;
  - public abstract LocalInvitation createLocalInvitation;
  - public abstract void sendLocalInvitation;
  - public abstract void acceptRemoteInvitation;
  - public abstract void refuseRemoteInvitation;
  - public abstract void cancelLocalInvitation;
7.5 RtmStatusCode.java
  - interface LocalInvitationState
  - interface RemoteInvitationState
  - interface LocalInvitationError
  - interface RemoteInvitationError
  - interface InvitationApiCallError

8 Added an enum value, int CONNECTION_CHANGE_REASON_REMOTE_LOGIN = 8, to the ConnectionChangeReason interface.

