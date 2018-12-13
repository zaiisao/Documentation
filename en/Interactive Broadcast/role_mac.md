
---
title: Switch the Client Role
description: 
platform: macOS
updatedAt: Thu Dec 13 2018 16:23:00 GMT+0000 (UTC)
---
# Switch the Client Role
Before switching the client role, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/mac_video.md).

## Implementation

An interactive broadcast channel consists of two user roles: the host and the audience. Call the `setClientRole` method to set the user role as broadcaster or audience according to your needs. The differences of the two roles are:

- Host (Broadcaster): A host receives and publishes the voice/video streams. The host can also interact with other hosts using voice and video.
- Audience: An audience receives the voice and video streams of the host (s).

```objective-c
//Objective-C
- (void)setClientRole() {
[self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
}
```

```swift
 //Swift
 func setClientRole() {
 agoraKit.setClientRole(.clientRole_Broadcaster)
}
```

> This method can be called either before joining a live broadcast channel or during a live broadcast:
> 
>  - Before joining the channel: Set the client role as the broadcaster or audience.
>  -  During a live broadcast: Switch the user role from audience to broadcaster or vice versa.

Suppose two users join a live broadcast channel. If both users join the channel as hosts:

1. User A calls `setClientRole`, sets the user role as boradcaster, and calls `joinChannelByToken` to join the channel.

   ```objective-c
   //Objective-C
   //Set the user role as broadcaster
   - (void)setClientRole() {
     [self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
   })
   
   //Create and join a channel
   - (void)joinChannel {
     [self.agoraKit joinChannelByToken:nil channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
       // Join channel "demoChannel1"
     }];
   }
   ```

   ```swift
   //Swift
   //Set the user role as broadcaster
   func setClientRole() {
     agoraKit.setClientRole(.clientRole_Broadcaster)
   }
   
   //Create and join a channel
   func joinChannel() {
     agoraKit.joinChannel(by Token: nil, channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
         // Join channel "demoChannel1"
     }
   }
   ```
	 
2. User B calls `setClientRole`, sets the user role as broadcaster, and calls `joinChannelByToken` to join the channel.

   ```objective-c
   //Objective-C
   //Set the user role as broadcaster
   - (void)setClientRole() {
     [self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
   })
   
   //Create and join a channel
   - (void)joinChannel {
     [self.agoraKit joinChannelByToken:nil channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
       // Join channel "demoChannel1"
     }];
   }
   ```

   ```swift
   //Swift
   //Set the user role as broadcaster
   func setClientRole() {
     agoraKit.setClientRole(.clientRole_Broadcaster)
   }
   
   //Create and join a channel
   func joinChannel() {
     agoraKit.joinChannel(by Token: nil, channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
         // Join channel "demoChannel1"
     }
   }
   ```

If one user joins the channel as a host, and the other as an audience. A while later, the other user wants to swtich to a host:

1. User A calls `setClientRole`, sets the user role as host, and then calls `joinChannelByToken` to join a channel.

   ```objective-c
   //Objective-C
   //Set the user role as broadcaster
   - (void)setClientRole() {
     [self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
   })
   
   //Create and join a channel
   - (void)joinChannel {
     [self.agoraKit joinChannelByToken:nil channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
       // Join channel "demoChannel1"
     }];
   }
   ```

   ```swift
   //Swift
   //Set the user role as broadcaster
   func setClientRole() {
     agoraKit.setClientRole(.clientRole_Broadcaster)
   }
   
   //Create and join a channel
   func joinChannel() {
     agoraKit.joinChannel(by Token: nil, channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
         // Join channel "demoChannel1"
     }
   }
   ```

2. User B calls `joinChannelByToken`, joins the channel as an audience, and then calls `setClientRole` to switch the user role to host.

   ```objective-c
//Objective-C
//Create and join a channel
   - (void)joinChannel {
     [self.agoraKit joinChannelByToken:nil channelId:@"demoChannel1" info:nil uid:0 joinSuccess:^(NSString *channel, NSUInteger uid, NSInteger elapsed) {
       // Join channel "demoChannel1"
     }];
		 
   //Set the user role as broadcaster
   - (void)setClientRole() {
     [self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
   })
   ```
	 
   ```swift
 //Swift
//Create and join a channel
   func joinChannel() {
     agoraKit.joinChannel(by Token: nil, channelId: "demoChannel1", info:nil, uid:0){[weak self] (sid, uid, elapsed) -> Void in
         // Join channel "demoChannel1"
     }
   }
	 
   //Set the client role as broadcaster
   func setClientRole() {
     agoraKit.setClientRole(.clientRole_Broadcaster)
   }
   ```


## Next Steps

Once the client role is switched to the host, you can start a live broadcast with the following step:

- [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_mac_live.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_mac.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_mac.md)
- [Adjust the Pitch and Tone](../../en/Interactive%20Broadcast/voice_effect_mac.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_mac.md)
