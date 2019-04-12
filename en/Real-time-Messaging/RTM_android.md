
---
title: RTM Quickstart Guide
description: 
platform: Android
updatedAt: Fri Apr 12 2019 12:23:27 GMT+0000 (UTC)
---
# RTM Quickstart Guide
## <a name = "create"></a>Create and Initialize an Agora RtmClient Instance

Before creating an Agora `RtmClient` instance, ensure that you prepare the development environment.

### Implementation

1. Import the following packages of the Agora RTM SDK to enable real-time messaging:

 - io.agora.rtm.RtmClient
 - io.agora.rtm.RtmClientListener
 - io.agora.rtm.RtmMessage
 - io.agora.rtm.RtmChannel
 - io.agora.rtm.RtmChannelListener
 - io.agora.rtm.RtmChannelMember
 - io.agora.rtm.ErrorInfo
 - io.agora.rtm.ResultCallback
 - io.agora.rtm.RtmStatusCode

2. Call the `createInstance` method to create an Agora `RtmClient` instance. You need to: 

 - Pass the App ID issued by Agora to you. Only apps with the same App ID can join the same channel.
 - Specify an event handler. The Agora RTM SDK uses event handlers to inform the app about RTM SDK runtime events, such as connection state changes and receiving peer-to-peer messages.

```javascript
import io.agora.rtm.ErrorInfo;
import io.agora.rtm.ResultCallback;
import io.agora.rtm.RtmClient;
import io.agora.rtm.RtmClientListener;
import io.agora.rtm.RtmMessage;
import io.agora.rtm.RtmStatusCode;
...
 
private RtmClientListener mRtmClientListener = new RtmClientListener() {
    @Override
    public void onConnectionStateChanged(int state, int reason) {
        // See the constants defined in RtmStatusCode.ConnectionState and RtmStatusCode.ConnectionChangeReason.
 
    }
 
    @Override
    public void onMessageReceived(RtmClient message, String peerId) {
 
    }
};
 
private void initializeAgoraRtmClient() {
    try {
        mRtmClient = RtmClient.createInstance(getBaseContext(), getString(R.string.agora_app_id), mRtmClientListener);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));
 
        throw new RuntimeException("NEED TO check rtm sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```

### Considerations

- The Agora RTM SDK supports multiple Agora `RtmClient` instances that are independent of each other. You can use the same context to create different Agora `RtmClient` instances, but the `RtmClientListener` interface of each Agora `RtmClient` instance must be different.
- If you no longer use an Agora `RtmClient` instance, call the `release` method to release all resources used by that instance.

### Next Steps

You have created an Agora `RtmClient` instance and can start using the Agora RTM SDK:

- [Log in Agora's RTM system](#login)
- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "login"></a>Log in Agora's RTM System

Before logging in Agora's RTM system, ensure that you [create an Agora RtmClient instance](#create).

### Implementation

Call the `login` method to log in Agora's RTM system. You need to: 

- Pass a token that identifies the role and permission of the user. Set `token` as `"null"` for low-security requirements. A token is generated at the server of the app.
- Pass a user ID that identifies the user. The `userId` parameter must not include non-printable characters, and can neither exceed 64 bytes in length nor start with a space. Do not set `userId` as `"null"`.
- Pass the `IResultCallback` callback, which indicates whether the login succeeds or fails.

```javascript
private void login() {
    mRtmClient.login(null, "demoUserId", new ResultCallback<Void>() {
        @Override
        public void onSuccess(Void responseInfo) {
            // Login succeeds.
        }
 
        @Override
        public void onFailure(ErrorInfo errorInfo) {
            int errorCode = errorInfo.getErrorCode();
            // Login fails. See the error codes defined in RtmStatusCode.LoginError.
        }
    });
}
```
You can call the `logout` method to log out of Agora's RTM system and then call the `login` method again to switch to another account.

### Next Steps
After [logging in Agora's RTM system](#login), you can:

- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "sendpeer"></a>Send a Peer-to-peer Message

After [logging in Agora's RTM system](#login), you can send a peer-to-peer message.

### Implementation
#### Send a peer-to-peer message

Call the `sendMessageToPeer` method to send a peer-to-peer message to a specific user (peer).

- Pass the `uid` of the receiver.
- Pass the `RtmMessage` instance. You can call the `createMessage` method in the `RtmMessage` interface to create an `RtmMessage` instance and create the text message by calling the `setText` method in the `RtmMessage` instance.
- Pass the `message` to be sent.
- Pass `resultCallback`, which returns the state of sending the message, such as whether or not the peer-to-peer message is received by the peer.

```javascript
private void sendP2PMessage() {
    RtmMessage message = RtmMessage.createMessage();
    message.setText("demo message content");
    mRtmClient.sendMessageToPeer("demoPeerId", message, new ResultCallback<void>() {

 
        }
    });
}
```
#### Receive a peer-to-peer message
The `onMessageReceived` callback in the `RtmClientListener` interface returns the state of sending a peer-to-peer message. In this callback: 

- The message is retrieved by calling the `message.getText` method.
- The `peerId` parameter refers to the user ID of the sender.

### Consideration

You cannot reuse an Agora `RtmMessage` object that has been received by a user.

### Next Steps
You can [send a channel message.](#sendchannel)

## <a name = "sendchannel"></a>Send a Channel Message

After [logging in Agora's RTM system](#login), you can send a channel message.

### Implementation

#### Create an Agora RtmChannel instance

Call the `createChannel` method in the `RtmClient` instance to create an Agora RTM channel.  You need to: 

- Pass the channel ID. The `channelId` parameter must be in the string format less than 64 bytes and cannot be empty or set as `"null"`. 
- Specify a reference to `RtmChannelListener`, through which the Agora RTM SDK informs the app about channel events, such as receiving channel messages and a user joining or leaving a channel.

```javascript
private RtmChannel mRtmChannel;
 
 
private RtmChannelListener mRtmChannelListener = new RtmChannelListener() {
    @Override
    public void onMessageReceived(RtmMessage message, RtmChannelMember fromMember) {
 
    }
 
    @Override
    public void onMemberJoined(RtmChannelMember member) {
 
    }
 
    @Override
    public void onMemberLeft(RtmChannelMember member) {
 
    }
 
    @Override
    public void onAttributesUpdated(Map<String, String> attributes) {
 
    }
 
    @Override
    public void onAttributesDeleted(Map<String, String> attributes) {
 
    }
};
 
 
private void createChannel() {
    mRtmChannel = mRtmClient.createChannel("demoChannelId", mRtmChannelListener);
}
```

#### Join a channel

Call the `join` method in the `RtmChannel` instance to join a channel and specify a reference to the `IResultCallback` object to indicate whether this method call succeeds or fails.

```javascript
private void joinChannel() {
    mRtmChannel.join(new IResultCallback<Void>() {
        @Override
        public void onSuccess(Void responseInfo) {
	    // Join a channel succeeds.
        }
 
        @Override
        public void onFailure(ErrorInfo errorInfo) {
            int errorCode = errorInfo.getErrorCode();
            // Join a channel fails. See the error codes defined in RtmStatusCode.JoinChannelError.
        }
    });
}
```

#### Send a channel message

Call the `sendMessage` method in the `RtmChannel` instance to send a channel message. You need to: 

- Pass the `RtmMessage` instance. You can call the `createMessage` method in the `RtmMessage` interface to create an `RtmMessage` instance and create the text message by calling the `setText` method in the `RtmMessage` instance.
- Pass `resultCallback`, which returns the state of sending the message, such as whether or not the channel message is received by the server.

```javascript
private void sendChannelMessage() {
    RtmMessage message = RtmMessage.createMessage();
    message.setText("demo message content");
    mRtmChannel.sendMessage(message, new resultCallback<void>() {

        }
    });
}
```

#### Receive a channel message
The `onMessageReceived` callback in the `RtmChannelListener` interface returns the state of sending a channel message. In this callback: 

- The message is retrieved by calling the `message.getText` method.
- The user ID of the sender is retrieved by calling the `fromMember.getUserId` method.

#### Retrieves the user list of a channel
Call the `getMembers` method in the `RtmChannel` instance to retrieve the user list of a channel.

#### Leave a channel
Call the `leave` method in the `RtmChannel` instance to leave a channel. You can rejoin the channel by calling the `join` method again.

### Considerations

- To use the channel feature, each client must first call the `createChannel` method in the `RtmClient` instance to create a channel instance. 
- You can create multiple channel instances in an `RtmClient` instance and users can join multiple channels at the same time. The `channelId` parameter and `RtmChannelListener` interface of different channels must be different.
- If you pass an illegal channel ID or the channel ID is being used, the `createChannel` method returns `"null"`.
- You cannot reuse an `RtmMessage` object that has been received by a user.
- When you leave a channel and do not join it again, you can call the `release` method in the `RtmChannel` instance to release all resources occupied by the channel instance.
- Except the callback triggered by the failure of the basic parameter validity check, all other callbacks are called asynchronously, unless otherwise specified.

### Next Steps
You can [send a peer-to-peer message.](#sendpeer)


## Send and receive a call invitation. 

After logging in the Agora RTM system, you can call the call invitation services. 

## Get an RtmCallManager

```
   RtmCallManager rtmCallMgr = rtmClient.getRtmCallManager();
```

## Set the Event Listener for the call manager

```
   rtmCallMgr.setEventListener(new RtmCallEventListener() { ... });
```

## Create a call invitaion

```
   LocalInvitation rtmLocalInvitation = rtmCallMgr.createLocalInvitation(calleeId);
```

## Send a call invitation

```
   rtmCallMgr.sendLocalInvitation(rtmLocalInvitation, new MyResultCallback());
```

## Cancel a call invitation

```
   rtmCallMgr.cancelLocalInvitation(rtmLocalInvitation, new MyResultCallback());
```


## Accept a call invitation

```
   rtmCallMgr.acceptRemoteInvitation(rtmRemoteInvitation, new MyResultCallback());
```


## Reject a call invitation

```
   rtmCallMgr.refuseRemoteInvitation(rtmRemoteInvitation, new MyResultCallback());
```
