
---
title: Peer-to-peer or Channel Messaging
description: v1.0
platform: iOS
updatedAt: Wed Sep 25 2019 06:50:50 GMT+0800 (CST)
---
# Peer-to-peer or Channel Messaging
## <a name = "create"></a>Create and Initialize an AgoraRtmKit Instance

Before creating an [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance, ensure that you prepare the development environment.

### Implementation

1. Import the following tbd files and frameworks:

  - libc++.tbd
  - SystemConfiguration.framework
  - CoreTelephony.framework
  - libresolv.9.tbd

2. Call the [initWithAppId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/initWithAppId:delegate:) method to create an [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance. You need to: 

   - Pass the App ID issued by Agora to you. Only apps with the same App ID can join the same channel.
   - Specify an event handler. The Agora RTM SDK process event handlers with the [AgoraRtmDelegate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html) instance to inform the app about runtime events, such as connection state changes and receiving peer-to-peer messages.

```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

@property(nonatomic, strong)AgoraRtmKit* kit;

@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // Create an AgoraRtmKit instance.
    _kit = [[AgoraRtmKit alloc] initWithAppId:YOUR_APP_ID delegate:self];
	// Note: The SDK does not trigger the callback if the AgoraRtmKit reference is not held and released.
	// A token is mandatory if security options are enabled for your appId.
}
```

### Considerations

- The Agora RTM SDK supports multiple [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instances that are independent of each other. You can use the same context to create different [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instances, but the [AgoraRtmDelegate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html) interface of each [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance must be different.
- If you no longer use an [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance, call the [destroyChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/destroyChannelWithId:) method to release all resources used by that instance.

### Next steps

You have created an [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance and can start using the Agora RTM SDK:

- [Log in Agora's RTM system](#login)
- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "login"></a>Log in Agora's RTM System

Before logging in Agora's RTM system, ensure that you [create an AgoraRtmKit instance](#create).

### Implementation

Call the [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:) method to log in Agora's RTM system. You need to: 

- Pass a token that identifies the role and permission of the user. Set `token` as `"nil"` for low-security requirements. A token is generated at the server of the app.
- Pass a user ID that identifies the user. The `userId` parameter must not include non-printable characters, and can neither exceed 64 bytes in length nor start with a space. Do not set `userId` as `"nil"`.
- Pass the [AgoraRtmLoginBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmLoginBlock.html) and [connectionStateChanged](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:connectionStateChanged:reason:) callbacks, which indicate whether the login succeeds or fails.

```objective-c
[_kit loginByToken:nil user:@"testuser" completion:^(AgoraRtmLoginErrorCode errorCode) {
    if (errorCode != AgoraRtmLoginErrorOk) {
        NSLog(@"login failed %@", @(errorCode));
    } else {
        NSLog(@"login success");
    }
}];

#pragma AgoraRtmDelegate
...
- (void)rtmKit:(AgoraRtmKit *)kit connectionStateChanged:(AgoraRtmConnectionState)state reason:(AgoraRtmConnectionChangeReason)reason
{
    NSLog(@"connection state changed to %@", @(reason));
}
```

You can call the [logoutWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/logoutWithCompletion:) method to log out of Agora's RTM system.


```objective-c
[_kit logoutWithCompletion:^(AgoraRtmLogoutErrorCode errorCode) {
    if (errorCode != AgoraRtmLogoutErrorOk) {
        NSLog(@"login failed %@", @(errorCode));
    } else {
        NSLog(@"login success");
    }
}];
```

After calling the [logoutWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/logoutWithCompletion:) method to log out of Agora's RTM system, you can call the [loginByToken](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/loginByToken:user:completion:)  method again to switch to another account.

### Next steps
After [logging in Agora's RTM system](#login), you can:

- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "sendpeer"></a>Send a Peer-to-peer Message

After [logging in Agora's RTM system](#login), you can send a peer-to-peer message.

### Implementation
#### Send a peer-to-peer message

Call the [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/sendMessage:toPeer:completion:) method to send a peer-to-peer message to a specific user (peer).

- Pass the user ID (`peerId`) of the receiver.
- Pass the [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) instance. You can call the [initWithText](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html#//api/name/initWithText:) method in the [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) interface to create an [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) instance and create the text message.
- Pass the `message` to be sent.
- Pass the [AgoraRtmSendPeerMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendPeerMessageBlock.html) and [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:messageReceived:fromPeer:) callbacks, which return the state of sending the message, such as whether or not the peer-to-peer message is received by the peer.

```objective-c
- (void)sendYourPeerToPeerMessage {
    [_kit sendMessage:[[AgoraRtmMessage alloc] initWithText:@"testmsg"] toPeer:@"peer" completion:^(AgoraRtmSendPeerMessageState state) {
        if (state == AgoraRtmSendPeerMessageStateReceivedByPeer) {
            NSLog(@"sent success");
        }
    }];
}

#pragma AgoraRtmDelegate
...
- (void)rtmKit:(AgoraRtmKit *)kit messageReceived:(AgoraRtmMessage *)message fromPeer:(NSString *)peerId
{
    NSLog(@"message received from %@: %@", message.text, peerId);
}
```
#### Receive a peer-to-peer message

- The sender receives the [AgoraRtmSendPeerMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendPeerMessageBlock.html) callback with the state of sending the peer-to-peer message.
- The receiver receives the [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:messageReceived:fromPeer:) callback with the sent message and the user ID (`peerId`) of the sender.

### Consideration

You cannot reuse an Agora [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) object that has been received by a user.

### Next steps
You can [send a channel message.](#sendchannel)

## <a name = "sendchannel"></a>Send a Channel Message

After [logging in Agora's RTM system](#login), you can send a channel message.

### Implementation

#### Create an AgoraRtmChannel instance and join a channel

1. Call the [createChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) method in the [AgoraRtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html) instance to create an Agora RTM channel.  You need to: 

 - Pass the channel ID. The `channelId` parameter must be in the string format, less than 64 bytes, and cannot be empty or set as `"nil"`.
 - Specify a reference to [AgoraRtmChannelDelegate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html), through which the Agora RTM SDK informs the app about channel events, such as receiving channel messages and a user joining or leaving a channel.

2. Call the [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) method in the [AgoraRtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html) instance to join a channel. The [AgoraRtmJoinChannelBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmJoinChannelBlock.html) and [memberJoined](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:memberJoined:) callbacks indicate whether this method call succeeds or fails.

```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

...
@property(nonatomic, strong)AgoraRtmChannel* channel;

@implementation
- (void)createAndJoinChannel {
    _channel = [_kit createChannelWithId:@"testchannel" delegate:self];
    [_channel joinWithCompletion:^(AgoraRtmJoinChannelErrorCode state) {
        if(state == AgoraRtmJoinChannelErrorOk) {
            NSLog(@"join success");
        } else {
            NSLog(@"join failed: %@", @(state));
        }
    }];
}
#pragma AgoraRtmChannelDelegate
...
- (void)channel:(AgoraRtmChannel *)channel memberLeft:(AgoraRtmMember *)member
{
    NSLog(@"%@ left channel %@", member.userId, member.channelId);
}

- (void)channel:(AgoraRtmChannel *)channel memberJoined:(AgoraRtmMember *)member
{
    NSLog(@"%@ joined channel %@", member.userId, member.channelId);
}
```

#### Send a channel message

Call the [sendMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/sendMessage:completion:) method in the [AgoraRtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html) instance to send a channel message. You need to: 

- Pass the [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) instance. You can call the `initWithText` method in the [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) interface to create an [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) instance and create the text message.
- Pass the [AgoraRtmSendChannelMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendChannelMessageBlock.html) and [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:) callbacks, which return the state of sending the message, such as whether or not the channel message is received by the server.

```objective-c
- (void)sendYourChannelMessage {
    [_channel sendMessage:[[AgoraRtmMessage alloc] initWithText:@"channelmsg"] completion:^(AgoraRtmSendChannelMessageState state) {
        if(state == AgoraRtmSendChannelMessageStateReceivedByServer) {
            NSLog(@"sent success");
        }
    }];
}

#pragma AgoraRtmDelegate
...
- (void)channel:(AgoraRtmChannel *)channel messageReceived:(AgoraRtmMessage *)message fromMember:(AgoraRtmMember *)member
{
    NSLog(@"message received from %@ in channel %@: %@", message.text, member.channelId, member.userId);
}
```

#### Receive a channel message

- The sender receives the [AgoraRtmSendChannelMessageBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmSendChannelMessageBlock.html) callback with the state of sending the channel message.
- The receivers receive the [messageReceived](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:messageReceived:fromMember:) callback with the sent message and the user ID (`member`) of the sender.

#### Retrieve the member list of a channel
Call the [getMembersWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/getMembersWithCompletion:) method in the [AgoraRtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html) interface to retrieve the member list of a channel. The [AgoraRtmGetMembersBlock](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmGetMembersBlock.html) callback returns the member list of the channel.

#### Leave a channel
Call the [leaveWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/leaveWithCompletion:) method in the [AgoraRtmChannel](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html) instance to leave a channel. 

```objective-c
[_channel leaveWithCompletion:^(AgoraRtmLeaveChannelErrorCode state) {
    if(state == AgoraRtmLeaveChannelErrorOk) {
        NSLog(@"leave success");
    } else {
        NSLog(@"leave failed: %@", @(state));
    }
}];
```

You can rejoin the channel by calling the [joinWithCompletion](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmChannel.html#//api/name/joinWithCompletion:) method again.

### Considerations

- To use the channel feature, each client must first call the [createChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) method in the [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance to create a channel instance. 
- You can create multiple channel instances in an [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance and users can join multiple channels at the same time. The `channelId` parameter and [AgoraRtmChannelDelegate](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html) interface of different channels must be different.
- If you pass an illegal channel ID or if the channel ID is being used, the [createChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) method returns `"nil"`.
- You cannot reuse an [AgoraRtmMessage](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) object that has been received by a user.
- When you leave a channel and do not join it again, you can call the [destroyChannelWithId](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/destroyChannelWithId:) method in the [AgoraRtmKit](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) instance to release all resources used by the channel instance.
- Except for the callback triggered by the failure of the basic parameter validity check, all other callbacks are called asynchronously, unless otherwise specified.

### Next steps
You can [send a peer-to-peer message.](#sendpeer)



