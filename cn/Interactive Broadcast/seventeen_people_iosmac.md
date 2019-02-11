
---
title: 实现七人以上视频通话
description: 
platform: iOS,macOS
updatedAt: Mon Feb 11 2019 11:58:18 GMT+0000 (UTC)
---
# 实现七人以上视频通话
## 场景描述

在七人以上的视频会议场景中，如果连麦人数过多，可能会引起音画不同步、信息丢失等问题。 如果参与连麦的各方将订阅流设置为 **1-N** 模式，即将 1 方设为大流，其余各方设为小流，那么最多可以支持 17 人互动视频通话，并支持实时转码推流。

本文展示七人以上视频通话的适用场景及相关接口，以及使用该功能的相关注意事项。

以在线直播课为例，七人以上视频通话支持实现如下功能：

- 老师和学生从 PC 端或手机端以主播身份加入课堂，并调用 `enableDualStreamMode` 开启双流模式。
- 老师在讲课过程中通过信令系统指定学生上讲台。最多支持 5 人同时上讲台。学生上讲台后，学生可以看到老师和讲台上的学生。
- 信令系统告知频道内哪个学生上了讲台。老师和学生可以调用 `setRemoteVideoStreamType` 订阅想要看到的学生的大流。PC 端最多支持 1 个大流 16 个小流；移动端最多支持 1 个大流 6 个小流。订阅后，如果本人原来是大流，会变成小流。

## 参数设置

### 1. 用户角色

七人以上视频通话场景设置每个客户端为主播，视频的实质是主播之间的互相连麦。因为没有观众的角色也就没有观众连麦上麦。

### 2. 进入频道前的参数设置

在 `joinChannel` 前需要进行如下设置:

- 开启多人视频通信模式：`setParameters("{\"che.audio.live_for_comm\":true}");`
- 开启全面丢包对抗模式：`setParameters("{\"che.video.moreFecSchemeEnable\":true}");`

### 3. 大小流设置

#### 3.1 启用双流模式

根据应用场景，可以决定是否启用双流（视频大流和小流），其中大流指高分辨率、高码率的视频流，小流指低分辨率、低码率的视频流。如果应用程序里有大小窗口之分，一般建议启用双流。你可以通过 `enableDualStreamMode` 启用双流模式。

#### 3.2 目前支持的大流视频参数

大流理论上可以设置为所有直播的视频参数 \(Video Profile\)，但建议使用不超过 640 &times; 480，30 fps 的 profile, 例如:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>分辨率</strong></td>
<td><strong>帧率</strong></td>
<td><strong>比特率</strong></td>
</tr>
<tr><td>640 &times; 480</td>
<td>15 fps</td>
<td>500 Kbps</td>
</tr>
<tr><td>640 &times; 360</td>
<td>15 fps</td>
<td>400 Kbps</td>
</tr>
<tr><td>640 &times; 360</td>
<td>24 fps</td>
<td>800 Kbps</td>
</tr>
</tbody>
</table>

#### 3.3 定制默认的小流视频参数设置

如果启用双流模式, 应用程序可以自定义默认的小流视频参数。

比如若需要设置的 Video Profile 为 320 &times; 180, 15 fps, 140 Kbps：

```
setParameters("{\"che.video.lowBitRateStreamParameter\":{\"width\":320,\"height\":180,\"frameRate\":15,\"bitRate\":140}}"
```

#### 3.4 大小流分辨率比例

小流的分辨率比例需要和设置的 Video Profile 里的分辨率比例相同，比如：

- 若大流（PC 端）选择了分辨率的长宽比为 4:3 的 640 &times; 480，则小流分辨率长宽也应为 4:3 。
- 若大流（iOS 端）选择了分辨率的长宽比为 9:16 的 360 &times; 640，则小流分辨率长宽比也应为 9:16 。

#### 3.5 界面布局

界面布局建议采用一大多小的窗口布局：

- 大窗口申请大流
- 小窗口申请小流

### 4. 内存释放

在用户离线时，应用程序可以选择彻底释放该 `uid` 占用的 `view`。建议步骤如下：

1. 收到该 uid 离线的回调时，调用 `setupRemoteVideo` 方法。
2. 把其中的参数 `remote` 的 `view` 设为空。

## 实现方法

#### *Publisher*

```swift
// enable dual stream mode, once this is on, two types of resolution
// will be push out at the same time
agoraKit.enableDualStreamMode(true)
```

```oc
// enable dual stream mode, once this is on, two types of resolution
// will be push out at the same time
[agoraKit enableDualStreamMode: YES];
```

#### *Subscriber*

```swift
// resign callback
func rtcEngine(_ engine: AgoraRtcEngineKit, didJoinedOfUid uid: UInt, elapsed: Int) { 
	// it's important to dynamically set high/low stream type
	// based on real-life situation, setting unimportant streams
	// to low stream can significantly reduce stream bandwidth
	// usage, thus greatly improving the use experiences

	if isHighStream {
		agoraKit.setRemoteVideoStream(uid, type: .high)
	} else {
		agoraKit.setRemoteVideoStream(uid, type: .low)
	}
}
```

```oc
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didJoinedOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed {
	if (isHighStream) {
		[agoraKit setRemoteVideoStream: uid type:AgoraVideoStreamTypeHigh];
	} else {
		[agoraKit setRemoteVideoStream: uid type:AgoraVideoStreamTypeLow];
	}
}
```

## API

- [enableDualStreamMode](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)
- [setRemoteVideoStream:type:](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVideoStream:type:)
- [rtcEngine:didJoinedOfUid:elapsed:](https://docs.agora.io/cn/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinedOfUid:elapsed:)

## Demo 地址

https://github.com/AgoraIO/Advanced-Video/tree/master/Large-Group-Video-Chat


