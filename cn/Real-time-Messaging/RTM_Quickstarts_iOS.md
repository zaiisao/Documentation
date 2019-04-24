
---
title: RTM 快速开始
description: 
platform: iOS
updatedAt: Wed Apr 24 2019 02:17:46 GMT+0800 (CST)
---
# RTM 快速开始
## <a name = "create"></a>初始化

在创建实例前，请确保你已完成环境准备、安装包获取等步骤。

### 实现方法

1. 导入以下包:

  - libc++.tbd
  - SystemConfiguration.framework
  - CoreTelephony.framework
  - libresolv.9.tbd

2. 登入 RTM 之前，调用 `initWithAppId` 创建一个实例。在该方法中:

 - 填入获取到的 App ID。只有 App ID 相同的应用程序才能互通。
 - 指定一个事件回调。SDK 通过回调通知应用程序 SDK 的状态变化和运行事件等，如: 连接状态发生变化，接收到点对点消息等。

```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

@property(nonatomic, strong)AgoraRtmKit* kit;

@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 创建一个 AgoraRtmKit 实例
    _kit = [[AgoraRtmKit alloc] initWithAppId:YOUR_APP_ID delegate:self];
	// Note: The SDK does not trigger the callback if the AgoraRtmKit reference is not held and released.
	// 在生产环境下 RTM Token是必须的。
}
```

### 注意事项

- `AgoraRtmKit` 支持多实例，每个实例独立工作互不干扰，多个实例创建时可以用相同的 `context`，但是事件回调 `AgoraRtmDelegate` 必须是不同的实例。
- 当 `AgoraRtmKit` 实例不再使用的时候，可以调用实例的 `destroyChannelWithId` 方法进行销毁释放资源。

### 相关文档

完成创建实例后，你可以使用 Agora RTM SDK，依次实现如下功能进行实时消息收发：

- [登录](#login)
- [点对点消息](#sendpeer)
- [群聊](#sendchannel)

## <a name = "login"></a>登录

App 必须在登录 RTM 服务器之后，才可以使用 RTM 的点对点消息和群聊功能，在此之前，请确保你已完成初始化。


### 实现方法

调用 `loginByToken` 方法[登录RTM服务器](#login)。在该方法中:

- 传入能标识用户角色和权限的 `token`。如果安全要求不高，也可以将值设为 `“nil“`。`token` 需要在应用程序的服务器端生成。
- 传入能标识每个用户账号的 ID。`usedId` 为字符串，必须是可见字符（可以带空格），不能为空或者多于 64 个字符，也不能是字符串 `nil`。
- 传入结果回调，用于接收登录 RTM 服务器成功或者失败的结果回调。

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
如果需要退出登录，可以调用 `logoutWithCompletion` 方法。

```objective-c
[_kit logoutWithCompletion:^(AgoraRtmLogoutErrorCode errorCode) {
    if (errorCode != AgoraRtmLogoutErrorOk) {
        NSLog(@"login failed %@", @(errorCode));
    } else {
        NSLog(@"login success");
    }
}];
```
调用 `logoutWithCompletion` 方法之后可以调用 `loginByToken` 重新登录或者切换账号。

### 相关文档
成功登录 RTM 之后，你可以使用 Agora RTM SDK，实现如下功能：

- [点对点消息](#sendpeer)
- [群聊](#sendchannel)

## <a name = "sendpeer"></a>点对点消息

App 在成功[登录RTM服务器](#login)之后，可以开始使用 RTM 的点对点消息功能。

### 实现方法

#### 发送点对点消息

调用 `sendMessage` 方法发送点对点消息。在该方法中：

- 传入目标消息接收方的用户账号 ID。
- 传入 `AgoraRtmMessage` 对象实例。该消息对象由 `AgoraRtmMessage` 类的 `initWithText` 静态方法创建和设置消息内容。
- 传入消息发送状态监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时，对方不可达等。

```objective-c
- (void)... {
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

#### 接收点对点消息

点对点消息的接收通过创建 `AgoraRtmMessage` 实例的时候传入的 `AgoraRtmSendPeerMessageBlock` 回调接口进行监听。在该回调接口的 `MessageReceived` 回调方法中可以获取到消息文本内容和是消息发送方的用户账号 ID (`peerId`)。

### 注意事项

接收到的 `AgoraRtmMessage` 消息对象不能重复利用再用于发送。

### 相关文档

除了点对点消息，你还可以使用 Agora RTM SDK，实现[群聊](#sendchannel)功能。

## <a name = "sendchannel"></a>群聊

App 在成功[登录RTM服务器](#login)之后，可以开始使用 RTM 的群聊功能。

### 实现方法

#### 创建频道实例和加入频道

1. 调用 `AgoraRtmChannel` 实例的 `createChannelWithId` 方法创建 `AgoraRtmChannel` 实例。在该方法中：

  - 传入能标识每个频道的 ID。`channelId` 为字符串，必须是可见字符（可以带空格），不能为空或者多于64个字符，也不能是字符串 `nil`。
  - 指定一个事件回调。SDK 通过回调通知应用程序频道的状态变化和运行事件等，如: 接收到频道消息、用户加入和退出频道等。

2. 用户只有在加入频道之后，才能收发频道消息和获取频道成员列表等。调用 `AgoraRtmChannel` 实例的 `joinWithCompletion` 方法加入频道。在该方法中，指定回调用于判断是否成功加入该频道。


```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

...
@property(nonatomic, strong)AgoraRtmChannel* channel;

@implementation
- (void)... {
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
- (void)rtmKit:(AgoraRtmKit *)kit channel:(AgoraRtmChannel *)channel memberLeft:(AgoraRtmMember *)member
{
    NSLog(@"%@ left channel %@", member.userId, member.channelId);
}

- (void)rtmKit:(AgoraRtmKit *)kit channel:(AgoraRtmChannel *)channel memberJoined:(AgoraRtmMember *)member
{
    NSLog(@"%@ joined channel %@", member.userId, member.channelId);
}
```

#### 发送频道消息

在成功加入频道之后，用户可以开始向该频道发送消息。

调用 `AgoraRtmChannel` 实例的 `sendMessage` 方法向该频道发送消息。在该方法中：

- 传入 `AgoraRtmMessage`` 对象实例。该消息对象由 `AgoraRtmMessage` 类的 `initWithText` 静态方法创建和设置消息内容。
- 传入消息发送状态监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时等。

```objective-c
- (void)... {
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

#### 接收频道消息

频道消息的接收通过创建频道消息的时候传入的 `AgoraRtmSendChannelMessageBlock` 回调接口进行监听。在该回调接口的 `MessageReceived` 回调方法中可以获取到频道消息文本内容和频道消息的发送者的用户账号ID。

#### 获取频道成员列表

调用 `AgoraRtmChannel` 实例的 `getMembersWithCompletion` 方法可以获取到当前在该频道内的用户列表。 

#### 退出频道
调用 `AgoraRtmChannel` 实例的 `leaveWithCompletion` 方法可以退出该频道。

```objective-c
[_channel leaveWithCompletion:^(AgoraRtmLeaveChannelErrorCode state) {
    if(state == AgoraRtmLeaveChannelErrorOk) {
        NSLog(@"leave success");
    } else {
        NSLog(@"leave failed: %@", @(state));
    }
}];
```
退出频道之后可以调用 `joinWithCompletion` 方法再重新加入频道。

### 注意事项

- 每个客户端都需要首先调用 `AgoraRtmKit` 的 `createChannelWithId` 方法创建频道实例才能使用群聊功能，该实例只是本地的一个 `AgoraRtmChannel` 类对象实例。
- RTM 支持同时创建多个不同的频道实例并加入到多个频道中，但是每个频道实例必须使用不同的频道 ID 以及不同的 `AgoraRtmChannelDelegate` 回调。
- 如果频道 ID 非法，或者具有相同 ID 的频道实例已经在本地创建，`createChannelWithId` 将返回 `"nil"`。
- 接收到的 `AgoraRtmMessage` 消息对象不能重复利用再用于发送。
- 当离开了频道且不再加入该频道时，可以调用 `AgoraRtmChannel` 实例的 `destroyChannelWithId` 方法及时释放频道实例所占用的资源。
- 所有回调如无特别说明，除了基本的参数合法性检查失败触发的回调，均为异步调用。

### 相关文档

你也可以使用 Agora RTM SDK，实现[点对点消息](#sendpeer)功能。

## 接受发送呼叫邀请

登陆了Agora RTM 系统后可以发送或接收呼叫邀请。

### 1.呼叫邀请初始化
```swift
	let callKit = agoraKit.getRtmCall()
    callKit?.callDelegate = self
```

```oc
	AgoraRtmCallKit *callKit = [agoraKit getRtmCall];
	callKit.callDelegate = self;
```

### 2.(邀请方)发送邀请

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

### 3.(邀请方)取消邀请

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


### 4.(被邀请方)接受邀请
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
### 5.(被邀请方)拒接邀请
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

