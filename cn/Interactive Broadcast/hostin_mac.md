
---
title: 实现客户端连麦
description: 
platform: macOS
updatedAt: Fri Sep 28 2018 19:57:32 GMT+0800 (CST)
---
# 实现客户端连麦
# 实现客户端连麦

本文描述如何在客户端实现连麦功能。 目前市面上大多数直播技术方案为: 将主播端音视频数据通过 RTMP 协议推流到 CDN 云端，但存在观众观看延时大、主播无法和观众互动等缺点。 使用 Agora SDK 的连麦功能，配合直播厂商或 Agora 服务器的推流服务，可以实现:

- 观众可以随时申请成为嘉宾与主播连麦
- 多个嘉宾同时和主播连麦、进行实时互动

连麦都是在客户端实现的，如有推流需求，你可以结合 [进阶：推流到 CDN](../../cn/Quickstart%20Guide/push_stream_ios2.0.md) 选择在服务器端推流还是客户端推流。本文以两人连麦场景为例。

### 准备工作

- 如果你想实现语音连麦，确保你已经根据 [入门: 实现语音直播](../../cn/Quickstart%20Guide/broadcast_audio_mac.md) 实现语音直播功能。
- 如果你想实现音视频连麦，确保你已经根据 [入门: 实现视频直播](../../cn/Quickstart%20Guide/broadcast_video_mac.md) 实现视频直播功能。

## 示例 1

1. 用户 A 调用 `setClientRole`，将角色设置为主播, 然后调用 `joinChannelByToken` 加入频道。

   ```objective-c
   //Objective-C
   
   //设置用户角色为主播
   [engine setClientRole:AgoraClientRoleBroadcaster];
   
   //创建并加入频道
   [engine joinChannelByToken:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
   ```

   ```swift
   //Swift
   
   //设置用户角色为主播
   engine.setClientRole(.clientRole_Broadcaster)
   
   //创建并加入频道
   engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
   ```

2. 用户 B 调用 `setClientRole`，将角色设置为主播, 然后调用 `joinChannelByToken` 加入频道， 即可连麦。

   ```objective-c
   //Objective-C
   
   //设置用户角色为主播
   [engine setClientRole:AgoraClientRoleBroadcaster];
   
   //创建并加入频道
   [engine joinChannelByToken:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
   ```

   ```swift
   //Swift
   
   //设置用户角色为主播
   engine.setClientRole(.clientRole_Broadcaster)
   
   //创建并加入频道
   engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
   ```

## 示例 2

1. 用户 A 调用 `setClientRole`，将角色设置为主播, 然后调用 `joinChannelByToken` 加入频道。

   ```objective-c
   //Objective-C
   
   //设置用户角色为主播
   [engine setClientRole:AgoraClientRoleBroadcaster];
   
   //创建并加入频道
   [engine joinChannelByToken:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
   ```

   ```swift
   //Swift
   
   //设置用户角色为主播
   engine.setClientRole(.clientRole_Broadcaster)
   
   //创建并加入频道
   engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
   ```

2. 用户 B 调用  `joinChannelByToken`  以默认的观众身份加入频道，然后调用 `setClientRole` 将用户角色切换为主播后进行连麦。

   ```objective-c
   //Objective-C
   
   //创建并加入频道
   [engine joinChannelByToken:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
   
   //设置用户角色为主播
   [engine setClientRole:AgoraClientRoleBroadcaster];
   ```

   ```swift
   //Swift
   
   //创建并加入频道
   engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
   
   //设置用户角色为主播
   engine.setClientRole(.clientRole_Broadcaster)
   ```


