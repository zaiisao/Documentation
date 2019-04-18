
---
title: RTM Quickstart Guide
description: v0.9.1
platform: iOS
updatedAt: Thu Apr 18 2019 02:11:27 GMT+0800 (CST)
---
# RTM Quickstart Guide
## <a name = "create"></a>Create and Initialize an AgoraRtmKit Instance

Before creating an `AgoraRtmKit` instance, ensure that you prepare the development environment.

### Implementation

1. Import the following tbd files and frameworks:

  - libc++.tbd
  - SystemConfiguration.framework
  - CoreTelephony.framework
  - libresolv.9.tbd

2. Call the `initWithAppId` method to create an `AgoraRtmKit.framework` instance. You need to: 

   - Pass the App ID issued by Agora to you. Only apps with the same App ID can join the same channel.
   - Specify an event handler. The Agora RTM SDK process event handlers with the `AgoraRtmDelegate` instance to inform the app about RTM SDK runtime events, such as connection state changes and receiving peer-to-peer messages.

```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

@property(nonatomic, strong)AgoraRtmKit* kit;

@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // Create an AgoraRtmKit instance
    _kit = [[AgoraRtmKit alloc] initWithAppId:YOUR_APP_ID delegate:self];
	// Note: The SDK does not trigger the callback if the AgoraRtmKit reference is not held and released.
	// Token is mandatory if security options are enabled for your appId.
}
```

### Considerations

- The Agora RTM SDK supports multiple `AgoraRtmKit` instances that are independent of each other. You can use the same context to create different `AgoraRtmKit` instances, but the `AgoraRtmDelegate` interface of each `AgoraRtmKit` instance must be different.
- If you no longer use an `AgoraRtmKit` instance, call the `destroyChannelWithId` method to release all resources used by that instance.

### Next Steps

You have created an `AgoraRtmKit` instance and can start using the Agora RTM SDK:

- [Log in Agora's RTM system](#login)
- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "login"></a>Log in Agora's RTM System

Before logging in Agora's RTM system, ensure that you [create an AgoraRtmKit instance](#create).

### Implementation

Call the `loginByToken` method to log in Agora's RTM system. You need to: 

- Pass a token that identifies the role and permission of the user. Set `token` as `"nil"` for low-security requirements. A token is generated at the server of the app.
- Pass a user ID that identifies the user. The `userId` parameter must not include non-printable characters, and can neither exceed 64 bytes in length nor start with a space. Do not set `userId` as `"nil"`.
- Pass the `AgoraRtmLoginBlock` and `connectionStateChanged` callbacks, which indicate whether the login succeeds or fails.

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

You can call the `logoutWithCompletion` method to log out of Agora's RTM system.


```objective-c
[_kit logoutWithCompletion:^(AgoraRtmLogoutErrorCode errorCode) {
    if (errorCode != AgoraRtmLogoutErrorOk) {
        NSLog(@"login failed %@", @(errorCode));
    } else {
        NSLog(@"login success");
    }
}];
```

After calling the `logoutWithCompletion` method to log out of Agora's RTM system, you can call the `loginByToken` method again to switch to another account.

### Next Steps
After [logging in Agora's RTM system](#login), you can:

- [Send a Peer-to-peer Message](#sendpeer)
- [Send a Channel Message](#sendchannel)

## <a name = "sendpeer"></a>Send a Peer-to-peer Message

After [logging in Agora's RTM system](#login), you can send a peer-to-peer message.

### Implementation
#### Send a peer-to-peer message

Call the `sendMessage` method to send a peer-to-peer message to a specific user (peer).

- Pass the user ID (`peerId`) of the receiver.
- Pass the `AgoraRtmMessage` instance. You can call the `initWithText` method in the `AgoraRtmMessage` interface to create an `AgoraRtmMessage` instance and create the text message.
- Pass the `message` to be sent.
- Pass the `AgoraRtmSendPeerMessageBlock` and `messageReceived` callbacks, which return the state of sending the message, such as whether or not the peer-to-peer message is received by the peer.

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

- The sender receives the `AgoraRtmSendPeerMessageBlock` callback with the state of sending the peer-to-peer message.
- The receiver receives the `messageReceived` callback with the sent message and the user ID (`peerId`) of the sender.

### Consideration

You cannot reuse an Agora `AgoraRtmMessage` object that has been received by a user.

### Next Steps
You can [send a channel message.](#sendchannel)

## <a name = "sendchannel"></a>Send a Channel Message

After [logging in Agora's RTM system](#login), you can send a channel message.

### Implementation

#### Create an AgoraRtmChannel instance and join a channel

1. Call the `createChannelWithId` method in the `AgoraRtmChannel` instance to create an Agora RTM channel.  You need to: 

 - Pass the channel ID. The `channelId` parameter must be in the string format less than 64 bytes and cannot be empty or set as `"nil"`.
 - Specify a reference to `AgoraRtmChannelDelegate`, through which the Agora RTM SDK informs the app about channel events, such as receiving channel messages and a user joining or leaving a channel.

2. Call the `joinWithCompletion` method in the `AgoraRtmChannel` instance to join a channel. The `AgoraRtmJoinChannelBlock` and `memberJoined` callbacks indicate whether this method call succeeds or fails.

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
- (void)rtmKit:(AgoraRtmKit *)kit channel:(AgoraRtmChannel *)channel memberLeft:(AgoraRtmMember *)member
{
    NSLog(@"%@ left channel %@", member.userId, member.channelId);
}

- (void)rtmKit:(AgoraRtmKit *)kit channel:(AgoraRtmChannel *)channel memberJoined:(AgoraRtmMember *)member
{
    NSLog(@"%@ joined channel %@", member.userId, member.channelId);
}
```

#### Send a channel message

Call the `sendMessage` method in the `AgoraRtmChannel` instance to send a channel message. You need to: 

- Pass the `AgoraRtmMessage` instance. You can call the `initWithText` method in the `AgoraRtmMessage` interface to create an `AgoraRtmMessage` instance and create the text message.
- Pass the `AgoraRtmSendChannelMessageBlock` and `messageReceived` callbacks, which return the state of sending the message, such as whether or not the channel message is received by the server.

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
- (void)rtmKit:(AgoraRtmKit *)kit channel:(AgoraRtmChannel *)channel messageReceived:(AgoraRtmMessage *)message fromMember:(AgoraRtmMember *)member
{
    NSLog(@"message received from %@ in channel %@: %@", message.text, member.channelId, member.userId);
}
```

#### Receive a channel message

- The sender receives the `AgoraRtmSendChannelMessageBlock` callback with the state of sending the channel message.
- The receivers receive the `messageReceived` callback with the sent message and the user ID (`member`) of the sender.

#### Retrieves the user list of a channel
Call the `getMembersWithCompletion` method in the `AgoraRtmChannel` interface to retrieve the user list of a channel. The `AgoraRtmGetMembersBlock` callback in the `AgoraRtmChannelDelegate` interface returns the user list of the channel.

#### Leave a channel
Call the `leaveWithCompletion` method in the `AgoraRtmChannel` instance to leave a channel. 

```objective-c
[_channel leaveWithCompletion:^(AgoraRtmLeaveChannelErrorCode state) {
    if(state == AgoraRtmLeaveChannelErrorOk) {
        NSLog(@"leave success");
    } else {
        NSLog(@"leave failed: %@", @(state));
    }
}];
```

You can rejoin the channel by calling the `joinWithCompletion` method again.

### Considerations

- To use the channel feature, each client must first call the `createChannelWithId` method in the `AgoraRtmChannel` instance to create a channel instance. 
- You can create multiple channel instances in an `AgoraRtmChannel` instance and users can join multiple channels at the same time. The `channelId` parameter and `AgoraRtmChannelDelegate` interface of different channels must be different.
- If you pass an illegal channel ID or the channel ID is being used, the `createChannelWithId` method returns `"nil"`.
- You cannot reuse an `AgoraRtmMessage` object that has been received by a user.
- When you leave a channel and do not join it again, you can call the `destroyChannelWithId` method in the `AgoraRtmChannel` instance to release all resources occupied by the channel instance.
- Except the callback triggered by the failure of the basic parameter validity check, all other callbacks are called asynchronously, unless otherwise specified.

### Next Steps
You can [send a peer-to-peer message.](#sendpeer)

## Send and receive a call invitation

After loggin in the Agora RTM system, you can call the call invitation services. 

### 1.Initialize the call invitation
```swift
	let callKit = agoraKit.getRtmCall()
    callKit?.callDelegate = self
```

```oc
	AgoraRtmCallKit *callKit = [agoraKit getRtmCall];
	callKit.callDelegate = self;
```

### 2.Send a call invitation to the remote callee

```swift
   let invitation = AgoraRtmLocalInvitation(calleeId: "calleedId")
   invitation.content = "content"
   callKit?.send(invitation, completion: { (errorCode) in
   		print("errorCode: \(errorCode.rawValue)")
   })

```

```oc
	AgoraRtmLocalInvitation *invitation = [[AgoraRtmLocalInvitation alloc] initWithCalleeId:@"calleedId"];
	invitation.content = @"content";
	[callKit sendLocalInvitation:invitation completion:^(AgoraRtmInvitationApiCallErrorCode errorCode) {
        NSLog(@"errorCode: %d", errorCode);
    }];
```

### 3. Cancel the call invitation initiated by me
```swift
	callKit?.cancel(invitation, completion: { (errorCode) in
   		print("errorCode: \(errorCode.rawValue)")
   })
```

```oc
	[callKit cancelLocalInvitation:invitation completion:^(AgoraRtmInvitationApiCallErrorCode errorCode) {
        NSLog(@"errorCode: %d", errorCode);
    }];
```


### 4.Accept an incoming call invitation
```swift
func rtmCallKit(_ callKit: AgoraRtmCallKit, remoteInvitationReceived remoteInvitation: AgoraRtmRemoteInvitation) {
      	 callKit?.accept(remoteInvitation, completion: { (error) in
            print("errorCode: \(errorCode.rawValue)")
        })
 }
```

```oc
- (void)rtmCallKit:(AgoraRtmCallKit * _Nonnull)callKit remoteInvitationReceived:(AgoraRtmRemoteInvitation * _Nonnull)remoteInvitation {
		  [callKit acceptRemoteInvitation:remoteInvitation completion:^(AgoraRtmInvitationApiCallErrorCode errorCode) {
        	   NSLog(@"errorCode: %d", errorCode);
         }]; 
}
```
### 5. Refuse an incoming call invitation
```swift
func rtmCallKit(_ callKit: AgoraRtmCallKit, remoteInvitationReceived remoteInvitation: AgoraRtmRemoteInvitation) {
        callKit?.cancel(remoteInvitation, completion: { (error) in
            print("errorCode: \(errorCode.rawValue)")
        })
 }
```

```oc
- (void)rtmCallKit:(AgoraRtmCallKit * _Nonnull)callKit remoteInvitationReceived:(AgoraRtmRemoteInvitation * _Nonnull)remoteInvitation {
		  [callKit cancelRemoteInvitation:remoteInvitation completion:^(AgoraRtmInvitationApiCallErrorCode errorCode) {
        	   NSLog(@"errorCode: %d", errorCode);
         }]; 
}
```

