
---
title: Peer-to-peer or Channel Messaging
description: 
platform: Android
updatedAt: Fri Sep 06 2019 04:04:52 GMT+0800 (CST)
---
# Peer-to-peer or Channel Messaging
## <a name = "create"></a>Create and Initialize an Agora RtmClient Instance

Before creating an Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance, ensure that you prepare the development environment.

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

2. Call the [createInstance](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6411640143c4d0d0cd9481937b754dbf) method to create an Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance. You need to: 

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

- The Agora RTM SDK supports multiple Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instances that are independent of each other. You can use the same context to create different Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instances, but the [RtmClientListener](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html) interface of each Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance must be different.
- If you no longer use an Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance, call the [release](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a5147d00d14afeebf0926b0d2f01079f5) method to release all resources used by that instance.

### Next steps

You have created an Agora [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance and can start using the Agora RTM SDK:

- [Log in Agora's RTM system](#login)
- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "login"></a>Log in Agora's RTM System

Before logging in Agora's RTM system, ensure that you [create an Agora RtmClient instance](#create).

### Implementation

Call the [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) method to log in Agora's RTM system. You need to: 

- Pass a token that identifies the role and permission of the user. Set `token` as `"null"` for low-security requirements. A token is generated at the server of the app.
- Pass a user ID that identifies the user. The `userId` parameter must not include non-printable characters, and can neither exceed 64 bytes in length nor start with a space. Do not set `userId` as `"null"`.
- Pass the [ResultCallback](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html) callback, which indicates whether the login succeeds or fails.

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
You can call the [logout](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a6f5695854e251ddd4ba05547ab47b317) method to log out of Agora's RTM system and then call the [login](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a995bb1b1bbfc169ee4248bd37e67b24a) method again to switch to another account.

### Next steps
After [logging in Agora's RTM system](#login), you can:

- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "sendpeer"></a>Send a Peer-to-peer Message

After [logging in Agora's RTM system](#login), you can send a peer-to-peer message.

### Implementation
#### Send a peer-to-peer message

Call the [sendMessageToPeer](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a25ab5c0126e1dc51c78b2b705de68b7a) method to send a peer-to-peer message to a specific user (peer).

- Pass the `uid` of the receiver.
- Pass the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) instance. You can call the [createMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3) method in the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) interface to create an [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) instance and create the text message by calling the [setText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2) method in the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) instance.
- Pass the `message` to be sent.
- Pass [ResultCallback](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html), which returns the state of sending the message, such as whether or not the peer-to-peer message is received by the peer.

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
The [onMessageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#af760814981718fb31d88acb8251d19b6) callback in the [RtmClientListener](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html) interface returns the state of sending a peer-to-peer message. In this callback: 

- The message is retrieved by calling the [getText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a17a3465ab00b72a0581d70bf9d22035b) method.
- The `peerId` parameter refers to the user ID of the sender.

### Consideration

You cannot reuse an Agora [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) object that has been received by a user.

### Next steps
You can [send a channel message.](#sendchannel)

## <a name = "sendchannel"></a>Send a Channel Message

After [logging in Agora's RTM system](#login), you can send a channel message.

### Implementation

#### Create an Agora RtmChannel instance

Call the [createChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a) method in the [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance to create an Agora RTM channel.  You need to: 

- Pass the channel ID. The `channelId` parameter must be in the string format, less than 64 bytes, and cannot be empty or set as `"null"`. 
- Specify a reference to [RtmChannelListener](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html), through which the Agora RTM SDK informs the app about channel events, such as receiving channel messages and a user joining or leaving a channel.

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

Call the [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) method in the [RtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html) instance to join a channel and specify a reference to the [ResultCallback](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html) object to indicate whether this method call succeeds or fails.

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

Call the [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a57087adf4227a17c774ea292840148a0) method in the [RtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html) instance to send a channel message. You need to: 

- Pass the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) instance. You can call the [createMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a77dbd15cb6c9db3844fb313bd5dceac3) method in the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) interface to create an [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) instance and create the text message by calling the [setText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a114bf5f4d728e1a5e31792491bf4a1d2) method in the [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) instance.
- Pass [ResultCallback](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html), which returns the state of sending the message, such as whether or not the channel message is received by the server.

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
The [onMessageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_client_listener.html#af760814981718fb31d88acb8251d19b6) callback in the [RtmChannelListener](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html) interface returns the state of sending a channel message. In this callback: 

- The message is retrieved by calling the [getText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html#a17a3465ab00b72a0581d70bf9d22035b) method.
- The user ID of the sender is retrieved by calling the [getUserId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel_member.html#a39c0eeffeefdc5a186e8b64d2f352912) method.

#### Retrieve the member list of a channel
Call the [getMembers](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a567aca5f866cf71c3b679ae09b4bf626) method in the [RtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html) instance to retrieve the member list of a channel.

#### Leave a channel
Call the [leave](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a9e0b6aad17bfceb3c9c939351a467d14) method in the [RtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html) instance to leave a channel. You can rejoin the channel by calling the [join](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#ad7b321869aac2822b3f88f8c01ce0d40) method again.

### Considerations

- To use the channel feature, each client must first call the [createChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a) method in the [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance to create a channel instance. 
- You can create multiple channel instances in an [RtmClient](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html) instance and users can join multiple channels at the same time. The `channelId` parameter and the [RtmChannelListener](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/interfaceio_1_1agora_1_1rtm_1_1_rtm_channel_listener.html) interface of different channels must be different.
- If you pass an illegal channel ID or if the channel ID is being used, the [createChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a) method returns `"null"`.
- You cannot reuse an [RtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_message.html) object that has been received by a user.
- When you leave a channel and do not join it again, you can call the [release](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a725d008cb19f44496948ee8f1826deaf) method in the [RtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/classio_1_1agora_1_1rtm_1_1_rtm_channel.html) instance to release all resources used by the channel instance.
- Except for the callback triggered by the failure of the basic parameter validity check, all other callbacks are called asynchronously, unless otherwise specified.

### Next steps
You can [send a peer-to-peer message.](#sendpeer)



