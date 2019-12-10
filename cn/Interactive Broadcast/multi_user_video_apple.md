
---
title: 七人以上视频场景
description: 
platform: iOS,macOS
updatedAt: Tue Dec 10 2019 04:21:38 GMT+0800 (CST)
---
# 七人以上视频场景
## 功能描述

在一般的视频直播场景中，如果参与的主播超过七人，可能会引起音画不同步和信息丢失的问题。如果参与连麦的各方将订阅流设置为 1-N 模式，即订阅 1 路大流和 N 路小流，那么直播频道内的主播最多可以支持 17 人。本页为你展示如何使用 Agora SDK  实现七人以上视频互动直播和相关的注意事项。


## 实现方法

在实现七人以上视频场景前，请确保已在你的项目中实现基本的实时音视频功能。详见[开始互动直播](../../cn/Interactive%20Broadcast/start_live_ios.md)。

参考如下步骤，在你的项目中实现七人以上视频场景功能：

1. 加入频道前，调用 `setParameters("{"che.audio.live_for_comm":true}")` 开启多人视频直播模式。

> 如果使用早于 v2.3.2 的 SDK，你还需要调用 `setParameters("{\"che.video.moreFecSchemeEnable\":true}”) ` 开启 ULP FEC，提高视频数据传输的可靠性。

2. 加入频道后，直播频道内的主播各方调用 `enableDualStreamMode` 方法开启[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。开启双流模式后，SDK 会根据大流参数自动设置发送的视频小流参数，详见[大小流对应规则](#rule)。

3. 频道内主播各方调用 `setRemoteVideoStream` 方法将订阅的一路视频流设置为大流，其余路视频流均设置为小流。详见[大小流参数推荐值](#reco)。

4. （可选）频道内主播自定义发送的视频小流参数。

   ```c++
   // 自定义视频小流参数设置：320 px × 180 px, 15 fps, 140 Kbps
   setParameters("{\"che.video.lowBitRateStreamParameter\":{\"width\":320,\"height\":180,\"frameRate\":15,\"bitRate\":140}}");
   ```

   <div class="alert note">视频小流的宽高比例需要和视频大流的宽高比例相同。推荐你使用分辨率不超过 320 x 180 或 180 x 320，码率不超过 140 Kbps 的小流参数。</div>

### 示例代码 

```objective-c
//Objective-C
// 开启视频双流模式。
[agoraKit enableDualStreamMode: YES];

// 将订阅的该路视频流设置为大流。
[agoraKit setRemoteVideoStream: uid type:AgoraVideoStreamTypeHigh];

// 将订阅的该路视频流设置为小流。
[agoraKit setRemoteVideoStream: uid type:AgoraVideoStreamTypeLow];
```

```swift
//Swift
// 开启视频双流模式。
agoraKit.enableDualStreamMode(true)

// 将订阅的该路视频流设置为大流。
agoraKit.setRemoteVideoStream(uid, type: .high)

// 将订阅的该路视频流设置为小流。
agoraKit.setRemoteVideoStream(uid, type: .low)
```

我们在 GitHub 提供一个开源的 [Large-Group-Video-Chat](https://github.com/AgoraIO/Advanced-Video/tree/master/Large-Group-Video-Chat) 示例项目。

### API 参考

- [`enableDualStreamMode`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)
- [`setRemoteVideoStream`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVideoStream:type:)
- [`setParameters`](https://docs.agora.io/cn/Interactive%20Broadcast/.API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setParameters:)

## 开发注意事项

- Agora 建议你在界面布局里采用一大多小的窗口布局：大窗口申请大流；小窗口申请小流。

- 若有主播离线，频道内其他主播可以在收到该主播离线回调后，调用 ` setupRemoteVideo` 方法并将该主播的 `view` 设为空，应用程序即可彻底释放该主播占用的 `view`。

<a name="reco"></a>
## 大小流参数推荐值
为避免多人视频时的高负载和不稳定性，Agora 推荐你根据下表在 [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) 设置视频大流参数。同时，表中也展示了其对应的小流参数值。

| 大流分辨率（宽 x 高） | 大流帧率（fps） | 大流码率（Kbps） | 小流分辨率（宽 x 高） | 小流码率 （Kbps） | 小流帧率 (fps） |
| :-------------------- | :-------------- | :--------------- | :-------------------- | :---------------- | :------------------------ |
| 640 x 480             | 15              | 500              | 160 x 120             | 45                | 15                        |
| 640 x 360             | 15              | 400              | 192 x 108             | 50                |            15                |
| 640 x 360             | 24              | 800              | 192 x 108             | 50                |        15                    |
| 480 x 640             | 15              | 500              | 120 x 160             | 45                |         15                   |
| 360 x 640             | 15              | 400              | 108 x 192             | 50                |          15                  |
| 360 x 640             | 24              | 800              | 108 x 192             | 50                |           15                 |


<a name="rule"></a>
## 大小流对应规则

| 大流宽高比例 | 小流分辨率（宽 x 高）                                        | 小流码率 （Kbps） | 小流帧率 (fps） |
| :----------- | :----------------------------------------------------------- | :---------------- | :------------------------ |
| 1 : 1        | 160 x 160                                                    | 68                | 15                        |
| 3 : 4        | 120 x 160                                                    | 45                |       15                    |
| 4 : 3        | 160 x 120                                                    | 45                |        15                   |
| 9 : 16       | 108 x 192                                                    | 50                |        15                   |
| 16 : 9       | 192 x 108                                                    | 50                |          15                 |
| 其他宽高比例 | 宽高中较大值：160 px<br>宽高中较小值：由宽高中较大值和宽高比例算出 | 68                |                      15     |




