
---
title: Host In
description: 
platform: iOS
updatedAt: Fri Nov 02 2018 04:19:01 GMT+0000 (UTC)
---
# Host In
This page describes how to implement the host-in function at the client side.

In the current market, a live broadcast pushes the host’s voice and video data to the CDN cloud through the RTMP protocol. This results in high latency with the host and audience unable to interact with each other. With Agora’s host-in function and the push-stream function provided by live broadcast vendors or Agora servers, you can implement the following functions:

-   The audience can interact with the hosts

-   Multiple users can interact with each other

-   Users who have customization requirements can capture/modify the raw video data


The host-in function is implemented at the client. If you need the push-stream function, see [Pushing Streams to the CDN](../../en/Quickstart%20Guide/push_stream_ios2.0.md) to decide on whether to push streams at the client or server.

This page describes two host-in scenarios with two users.

## Scenario

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Scenario</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>Scenario 1</td>
<td>The Agora SDK captures both voice and video data</td>
</tr>
<tr><td>Scenario 2</td>
<td>The Agora SDK captures the voice data, while the users capture the video data</td>
</tr>
</tbody>
</table>



## Preparation

-   For the voice host-in function, implement voice live broadcast according to [Starting a Live Voice Broadcast](../../en/Quickstart%20Guide/broadcast_audio_ios.md).

-   For the video host-in function, implement video live broadcast according to [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_ios.md).


### Scenario 1

In this scenario, the Agora SDK captures both voice and video data.

## Example 1

Both users join the channel as hosts:

1.  User A calls Set the User Role \(`setClientRole`\) to set the user role as host, and then calls Join a Channel \(`joinChannelByToken`\) to join a channel.

	```objective-c
	//Objective-C

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
	```

	```swift
	//Swift

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
	```

1.  User B calls Set the User Role \(`setClientRole`\) to set the user role as host, and then calls Join a Channel \(`joinChannelByToken`\) to the join a channel.

	```objective-c
	//Objective-C

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
	```

	```swift
	//Swift

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
	```

Users A and B can start to host in.

## Example 2

One user joins the channel as a host, and the other as an audience before switching to a host:

1.  User A calls Set the User Role \(`setClientRole`\) to set the user role as host, and then calls Join a Channel \(`joinChannelByToken`\) to join a channel.

	```objective-c
	//Objective-C

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
	```

	```swift
	//Swift

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
	```

1.  User B calls Join a Channel \(`joinChannelByToken`\) to join the channel as an audience, and then calls Set the User Role \(`setClientRole`\) to switch the user role to host.

	```objective-c
	//Objective-C

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];
	```

	```swift
	//Swift

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)
	```

Users A and B can start to host in.

### Scenario 2

In this scenario, users capture the video data.

## Example 1

1.  Capture the video data \(voice data is captured by the Agora SDK\) and render the local video, see [Configure the External Video Data](../../en/Interactive%20Broadcast/custom_video_ios.md).

2.  User A calls Set the User Role \(`setClientRole`\) to set the user role as host, and then calls Join a Channel \(`joinChannelByToken`\) to join a channel.

	```objective-c
	//Objective-C

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
	```

	```swift
	//Swift

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
	```

1.  User B calls Set the User Role \(`setClientRole`\) to set the user role as host, and then calls Join a Channel \(`joinChannelByToken`\) to join a channel.

	```objective-c
	//Objective-C

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
	```

	```swift
	//Swift

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
	```

Users A and B can start to host in.

## Example 2

1.  Capture the video data \(voice data is captured by the Agora SDK\) and render the local video, see [Configure the External Video Data](../../en/Interactive%20Broadcast/custom_video_ios.md).

2.  User A calls Set the User Role \(`setClientRole`\) to set the user role as host, and then calls Join a Channel \(`joinChannelByToken`\) to join a channel.

	```objective-c
	//Objective-C

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];
	```

	```swift
	//Swift

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)

	//create and join channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)
	```

1.  User B calls Join a Channel \(`joinChannelByToken`\) to join the channel as an audience, and then calls Set the User Role \(`setClientRole`\) to switch the user role to host.

	```objective-c
	//Objective-C

	//create and join channel
	[engine joinChannelByKey:nil channelName:@"channelName" info:nil uid:0 joinSuccess:nil];

	//set the user role as host
	[engine setClientRole:AgoraRtc_ClientRole_Broadcaster];
	```

	```swift
	//Swift

	//create and join the channel
	engine.joinChannel(byKey: nil, channelName: "channelName", info: nil, uid: 0, joinSuccess: nil)

	//set the user role as host
	engine.setClientRole(.clientRole_Broadcaster)
	```

Users A and B can start to host in.


