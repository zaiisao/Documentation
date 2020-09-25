
---
title: Start Live Interactive Video Streaming
description: Describes how quickly implement live interactive video streaming with Agora React Native SDK.
platform: React Native
updatedAt: Fri Sep 18 2020 04:53:00 GMT+0800 (CST)
---
# Start Live Interactive Video Streaming
Learn how to quickly implement basic live interactive video streaming with the Agora React Native SDK.

## Sample project

Agora provides an open-source sample project that implements [Agora React Native Quickstart](https://github.com/AgoraIO-Community/Agora-RN-Quickstart) on GitHub. You can download it and view the source code.

## Prerequisites

- Node 10 or later 
- React Native 0.59.10 or later
- A valid [Agora account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up) and an [App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-an-app-id)

<div class="alert note">Open the specified ports in <a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">Firewall Requirements</a> if your network has a firewall.</div>

## Set up the development environment

### Create a project

Follow the steps in [Setting up the development environment](https://reactnative.dev/docs/environment-setup) to create a React Native project. Skip to <a href="#integration">Integrate the SDK</a> if a project already exists.

### Integrate the SDK<a name="integration"></a>

This section describes how to integrate the SDK on React Native 0.60.0 or later.

<div class="alert note">If you use React Native 0.59.x, see <a href="https://github.com/AgoraIO-Community/react-native-agora/blob/master/README.md#installing-react-native--059x">Installing (React Native == 0.59.x)</a > to integrate the Agora SDK.</div>

1. Install the latest version of the Agora React Native SDK:

 **Method one: Install the SDK using yarn**
```
yarn add react-native-agora
```

 **Method two: Install the SDK using npm**
 ```
 npm i --save react-native-agora
 ```

	<div class="alert note">Do not link native modules manually, as React Native v0.60.0 and later support automatically linking native modules. For details, see <a href="https://github.com/react-native-community/cli/blob/master/docs/autolinking.md">Autolinking</a >.</div>
  
2. For iOS, you also need to run the following command:
```
npx pod-install
```

 <div class="alert note"><ul><li>This step is only required for iOS. Ensure that you have installed <b>CocoaPods</b> before the step. See <a href="https://guides.cocoapods.org/using/getting-started.html#getting-started">Getting Started with CocoaPods</a >.</li><li>The SDK uses Swift in native modules and your project should support Swift. If your React Native version is earlier than 0.62.0 and your development language is not Swift, see <a href="https://github.com/AgoraIO-Community/react-native-agora/blob/master/docs/v3/installation.ios.md#step-1-migrating-to-swift">Migrating to Swift</a >.</li></ul></div>



## Implement basic live interactive video streaming

This section describes how to use the Agora React Native SDK to start live interactive video streaming. 

### 1. Import the class

Import the `RtcEngine` class and view rendering components in your project.

```
import RtcEngine, {RtcLocalView, RtcRemoteView, VideoRenderMode} from 'react-native-agora'
```

### 2. Initialize RtcEngine

Create and initialize the `RtcEngine` object before calling any other Agora APIs.

You can also listen for callback events, such as when the local user joins the channel, when the remote user joins the channel, and when the remote user leaves the channel. 

```
async function init() {
    // Pass in the App ID to initialize the RtcEngine object.
    const engine = await RtcEngine.create(appId)
    // Enable the video module.
    await engine.enableVideo()
    // Listen for the JoinChannelSuccess callback.
    // This callback occurs when the local user successfully joins the channel.
    engine.addListener('JoinChannelSuccess', (channel, uid, elapsed) => {})
    // Listen for the UserJoined callback.
    // This callback occurs when the remote user successfully joins the channel.
    engine.addListener('UserJoined', (uid, elapsed) => {})
    // Listen for the UserOffline callback.
    // This callback occurs when the remote user leaves the channel or drops offline.
    engine.addListener('UserOffline', (uid, reason) => {})
}
```

### 3. Set the channel profile

After initializing the `RtcEngine` object, call `setChannelProfile` to set the channel profile as `LiveBroadcasting`. 

```
engine.setChannelProfile(ChannelProfile.LiveBroadcasting)
```

### 4. Set the client role

An interactive streaming channel has two client roles: `Broadcaster` and `Audience`. The default role is `Audience`.

After setting the channel profile to `LiveBroadcasting`, your app may use the following steps to set the client role:

1. Allow the user to set the role as either `Broadcaster` or `Audience`.
2. Call `setClientRole` and pass in the client role set by the user.

Note that in live interactive streaming, only the host can be heard and seen. If you want to switch the client role after joining the channel, call the `setClientRole` method.

```
// Set the client role as Broadcaster.
engine.setClientRole(ClientRole.Broadcaster)
```

### 5. Set the local video view

After setting the channel profile and client role, configure the local video view. You can call `startPreview` to enable the local video preview before joining the channel.

```
// Set the rendering mode of the video view as Hidden, which uniformly scales the video until it fills the visible boundaries.
<RtcLocalView.SurfaceView
    renderMode={VideoRenderMode.Hidden} />
```

### 6. Join a channel

After setting the user role and the local video view (for the video live streaming), you can call `joinChannel` to join a channel. In this method, set the following parameters:

- `token`: Pass a token that identifies the role and privilege of the user. You can set it to one of the following values:

  - `null` or an empty string "".
  - A temporary token generated in Console. A temporary token is valid for 24 hours. For details, see [Get a Temporary Token](https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token).
  - A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](https://docs.agora.io/en/Interactive%20Broadcast/token_server).
  
 <div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>

- `channelName`: Specify the channel name that you want to join.

- `uid`: The ID of the local user that is an integer and should be unique. If you set `uid` as 0, the SDK assigns a user ID for the local user and returns it in the `JoinChannelSuccess` callback.

```
engine.joinChannel(token, channelName, null, 0)
```

### 7. Set the remote video view

In live interactive video streaming, you should be able to see other users. After joining the channel, pass in the `uid` of the remote user sending the video and set the video view of the remote user. 

```
// Set the rendering mode of the video view as Hidden, which uniformly scales the video until it fills the visible boundaries.
<RtcRemoteView.SurfaceView
    uid={uid}
    renderMode={VideoRenderMode.Hidden} />
```

### 8. Leave the channel

Call `leaveChannel` to leave the current channel according to your scenario. For example, when the live interactive streaming ends, when you need to close the app, or when your app runs in the background.

```
engine.leaveChannel()
```

## Run the project

Run the project on your Android or iOS device. When you set the role as host and successfully start live interactive streaming, you can see the video view of yourself in the app. When you set the role as audience and successfully join live interactive streaming, you can see the video view of the host in the app.
