
---
title: Customize the Video Source and Renderer
description: 
platform: iOS
updatedAt: Thu Nov 15 2018 04:20:12 GMT+0000 (UTC)
---
# Customize the Video Source and Renderer
## Scenario Description

The Agora SDK provides access to the default camera configuration. Agora provides access to customize the video source for the following functions:

- To add new functions in the SDK for the camera’s video source, such as image enhancement or using the preprocessing library.
- If an app contains a video module, the video source can be customized for code reuse.
- To use non-camera video sources, such as recorded screen data.
- Flexible video capturing device resource allocation to avoid conflicts with other services.

## Integrate the Agora SDK

See [Integrate SDK](../../en/Video/ios_video.md) .

## Customize the Video Source

Step 1. Implement `AgoraVideoSourceProtocol` to create the customized video source class:

- Specify the buffer type in `bufferType`.

  ```c++
  - (AgoraVideoBufferType)bufferType;
  ```

- Save the `AgoraVideoFrameConsumer` object in the `shouldInitialize` method.

  ```c++
  - (BOOL)shouldInitialize;
  ```

- Send the video frame after the `shouldStart` method.

  ```c++
  - (void)shouldStart;
  ```

- Send the data to the media engine through `AgoraVideoFrameConsumer`.

- Stop sending the frame in the `shouldStop` method.

  ```c++
  - (void)shouldStop;
  ```

- Remove the frame in the `shouldDispose` method.

  ```c++
  - (void)shouldDispose;
  ```

Step 2. Create the customized video source object.

Step 3. Pass the external video source to the media engine by the `setVideoSource` method.

```c++
- (void)setVideoSource:(id<AgoraVideoSourceProtocol>_Nullable)videoSource;
```

Step 4. The media engine implements the methods in `AgoraVideoSourceProtocol`.

## Customize the Video Sink

Step 1. Call the `bufferType` and `pixelFormat` methods to set the buffer type and pixel format of the video frame.

Step 2. Implement the `shouldInitialize`, `shouldStart`, `shouldStop`, and `shouldDispose` methods to manage the customized video sink.

Step 3. Implement the buffer type and pixel format as specified in Step 1 of `AgoraVideoFrameConsumer`.

Step 4. Create the customized video sink object.

Step 5. Call the `setLocalVideoRenderer` and `setRemoteVideoRenderer` methods to set the local and remote renderers.

Step 6. The media engine will call functions in `AgoraVideoSinkProtocol` according to its internal state.

## Sample App

Agora currently provides a Sample App for custom video source and video sink. You can go to Github to download [Agora Custom Media Device Sample App for iOS](https://github.com/AgoraIO/Advanced-Video/tree/master/Custom-Media-Device/Agora-Custom-Media-Device-iOS) and download it.
