
---
title: 实现七人以上视频通话
description: 17-Way Live Video Broadcast for web
platform: Web
updatedAt: Fri Nov 30 2018 06:25:36 GMT+0800 (CST)
---
# 实现七人以上视频通话
在一般的网页端视频通话场景中，如果参与人数超过七人，可能会引起音画不同步、信息丢失等问题。

如果参与视频通话的各方将订阅流设置为 **1-N** 模式，即 1 方设为大流，其余各方均设为小流，那么最多可以有 17 人加入视频通话，或进行互动直播。

本页为你展示如何使用 Agora Web SDK 在网页端部署实现七人以上视频通话或互动直播，以及相关的注意事项。


## 1. 用户角色

设置每个客户端为主播，视频的实质是主播之间的互相连麦。因为没有观众的角色也就没有观众连麦上麦。

## 2. 订阅远端视频流前的参数设置

如果已经知道要订阅主播小流，建议在 Subscribe stream 前调用 `setRemoteVideoStreamType` 预先设置流类型。

```javascript
client.setRemoteVideoStreamType(stream, streamType)
```

## 3. 大小流设置

### 3.1 目前支持的大流视频参数

大流理论上可以设置为所有支持的视频属性（Video Profile），但建议使用不超过 640x480，15 fps 的视频属性。例如：

| **分辨率** | **帧率** | **比特率** |
| ---------- | -------- | ---------- |
| 640x480    | 15 fps   | 500 Kbps   |
| 640x360    | 15 fps   | 400 Kbps   |
| 640x360    | 30 fps   | 600 Kbps   |

### 3.2 默认的小流视频参数

如果启动了双流模式，应用程序可以选择大流视频参数，Web SDK 会根据所选择的大流参数自动适配小流参数。

```javascript
localStream.setVideoProfile('480P_1')
```

**大小流对应关系如下**

| **大流参数** | **对应小流参数** |
| ------------ | ---------------- |
| 720P_5       | 120P_1           |
| 720P_6       | 120P_1           |
| 480P         | 120P_1           |
| 480P_1       | 120P_1           |
| 480P_2       | 120P_1           |
| 480P_4       | 120P_1           |
| 480P_10      | 120P_1           |
| 360P_7       | 120P_1           |
| 360P_8       | 120P_1           |
| 240P         | 120P_1           |
| 240P_1       | 120P_1           |
| 180P_4       | 120P_1           |
| 120P_3       | 120P_1           |
| 120P         | 120P_1           |
| 120P_1       | 120P_1           |
| 480P_3       | 120P_3           |
| 480P_6       | 120P_3           |
| 360P_3       | 120P_3           |
| 360P_6       | 120P_3           |
| 240P_3       | 120P_3           |
| 180P_3       | 120P_3           |
| 480P_8       | 120P_4           |
| 480P_9       | 120P_4           |
| 240P_4       | 120P_4           |
| 720P         | 90P_1            |
| 720P_1       | 90P_1            |
| 720P_2       | 90P_1            |
| 720P_3       | 90P_1            |
| 360P         | 90P_1            |
| 360P_1       | 90P_1            |
| 360P_4       | 90P_1            |
| 360P_9       | 90P_1            |
| 360P_10      | 90P_1            |
| 360P_11      | 90P_1            |
| 180P         | 90P_1            |
| 180P_1       | 90P_1            |

**各参数对应的属性如下**

| **参数** | **对应分辨率** | **对应帧率** |
| -------- | -------------- | ------------ |
| 90P_1    | 160x90         | 15           |
| 120P     | 160x120        | 15           |
| 120P_1   | 160x120        | 15           |
| 120P_3   | 120x120        | 15           |
| 120P_4   | 212x120        | 15           |
| 180P     | 320x180        | 15           |
| 180P_1   | 320X180        | 15           |
| 180P_3   | 180x180        | 15           |
| 180P_4   | 424x240        | 15           |
| 240P     | 320x240        | 15           |
| 240P_1   | 320X240        | 15           |
| 240P_3   | 240x240        | 15           |
| 240P_4   | 424x240        | 15           |
| 360P     | 640x360        | 15           |
| 360P_1   | 640X360        | 15           |
| 360P_3   | 360x360        | 15           |
| 360P_4   | 640x360        | 30           |
| 360P_6   | 360x360        | 30           |
| 360P_7   | 480x360        | 15           |
| 360P_8   | 480x360        | 30           |
| 360P_9   | 640x360        | 15           |
| 360P_10  | 640x360        | 24           |
| 360P_11  | 640x360        | 24           |
| 480P     | 640x480        | 15           |
| 480P_1   | 640x480        | 15           |
| 480P_2   | 648x480        | 30           |
| 480P_3   | 480x480        | 15           |
| 480P_4   | 640x480        | 30           |
| 480P_6   | 480x480        | 30           |
| 480P_8   | 848x480        | 15           |
| 480P_9   | 848x480        | 30           |
| 480P_10  | 640x480        | 10           |
| 720P     | 1280x720       | 15           |
| 720P_1   | 1280x720       | 15           |
| 720P_2   | 1280x720       | 15           |
| 720P_3   | 1280x720       | 30           |
| 720P_5   | 960x720        | 15           |
| 720P_6   | 960x720        | 30           |


> 由于各浏览器对小流的适配存在差异，部分浏览器的部分分辨率大小流对应关系可能存在一些偏差。

### 3.3 定制小流视频参数

如果需要手动修改小流视频参数，你也可以通过如下接口设置：

```javascript
client.setLowStreamParameter({
  width: 120,
  height: 120,
  framerate: 15,
  bitrate: 120,
})
```

| **参数**  | **描述**                                                     |
| --------- | ------------------------------------------------------------ |
| width     | （非必填）小流视频帧的宽度。`width` 和 `height` 参数互相绑定，只有两个都填才有效，否则视为缺省，SDK 会自动适配一个默认的值。 |
| height    | （非必填）小流视频帧的高度。`width` 和 `height` 参数互相绑定，只有两个都填才有效，否则视为缺省，SDK 会自动适配一个默认的值。 |
| framerate | （非必填）小流视频帧的帧率。如果不填，则视为缺省，SDK 会自动适配一个默认的值。 |
| bitrate   | （非必填）小流视频帧的码率。如果不填，则视为缺省，SDK 会自动适配一个默认的值。 |



> - 由于不同的浏览器对于视频参数设置有不同的限制，通过使用该接口设置的视频参数不一定都会生效。目前发现的未能充分适配的浏览器有 Firefox 浏览器，对其设置帧率不生效，浏览器本身会把帧率固定在 30 fps。
> - 该方法必须在 `client.join`方法之后调用才能生效。
> - 屏幕共享只支持大流模式。

### 3.4 大小流分辨率比例

小流的分辨率比例需要和设置的 Video Profile 里的分辨率比例相同，如：

- 若大流（PC 端）选择了分辨率的长宽比为 4：3 的 640x480，则小流分辨率长宽比也应为 4：3
- 若大流（PC 端）选择了分辨率的长宽比为 4：3 的 640x360，则小流分辨率长宽比也应为 16：9

### 3.5 启用双流模式

你可以根据实际通话或直播场景，决定是否启用双流。如果视频通话有 7 人以上参加，或者应用程序里有大小窗口之分，Agora 建议你通过 `enableDualStream` 启用双流。

```javascript
client.enableDualStream();
```

### 3.6 界面布局

界面布局建议采用一大多小的窗口布局：

- 大窗口申请大流
- 小窗口申请小流

## 4. 释放内存

用户离线时，浏览器会释放 H5 格式的 Video View。

## 5. 环境要求

1. 请确保你的设备及浏览器满足以下条件：

 - Windows：Windows 7, Windows 8.x, Windows 10

    - Chrome 浏览器，Chrome 58 及以上版本（仅支持 HTTPS）
    - Firefox 浏览器，Firefox 56 及以上版本（仅支持 HTTPS），目前仅支持 8 个窗口显示

 - Mac：Safari 11 及以上

    - Chrome 浏览器，Chrome 58 及以上版本（仅支持 HTTPS）
    - Firefox 浏览器，Firefox 56 及以上版本（仅支持 HTTPS）

  > 目前网页端 17 人功能只支持 PC 端，不支持移动端。

1. 请确保你的 Agora Web SDK 在 2.1 版本及以上，支持设置双流功能

## 6. 使用示例代码实现网页端 17 人视频通话

在网页端，你可以通过如下方法实现 17 人视频通话：
![](https://web-cdn.agora.io/docs-files/1543559099965)

1. 检查浏览器兼容性 (checkSystemRequirements)

	```javascript
	checkSystemRequirements()
	```

1. 创建音视频对象 (createClient)

	```javascript
	createClient()
	```

1. 初始化客户端对象 (init)

	```javascript
	client.init(appId, onSuccess, onFailure)
	```

1. 加入 AgoraRTC 频道 (join)

	```javascript
	client.join(channelKey, channel, uid, onSuccess, onFailure)
	```

1. 创建音视频流对象 (createStream)

	```javascript
	client.createStream(spec)
	```

1. 开启双流模式 (enableDualStream)

	```javascript
	client.enableDualStream(onSuccess, onFailure)
	```

	在本地 Client 及 Stream 创建完成后，调用 `client.enableDualStream` 方法开启双流模式，才能进行大小流的切换：

	```javascript
	client.init(key, function () {
		client.join(key, channel, _uid, function (uid) {
			stream = AgoraRTC.createStream(YourConfig)    stream.init(function () {
				client.enableDualStream()
			})
		})
	})
	```

1. 设置视频大小流 (setRemoteVideoStreamType)

	```javascript
	client.setRemoteVideoStreamType(stream, streamType)
	```

	我们在 Client 的 `stream-added` 事件中，每次将新加入 Stream 切换成大流，将原先的大流切成小流：

	```javascript
	function switchStream (prev, next) {
		// set prev stream to low
			 precStream && client.setRemoteVideoStreamType(prevStream, 1)
			 // set next stream to high
				 nextStream && client.setRemoteVideoStreamType(nextStream, 0)
	}

	client.on('stream-added', function (evt) {
		let nextStream = evt.stream
		switchStream(prevStream, nextStream)
		// do sth else such as enlarging stream's containder and resize
	})
	```

	你也可以根据实际需求例如通过绑定点击事件来主动切换大小流：

	```javascript
	$('#id').dblclick(function (e) {
		var stream
		// stream = getStreamById(e.target.id)
		//switch to high
		client.setRemoteVideoStreamtype(stream, 0)
	})
	```

1. 发布本地音视频流 (publish)

	```javascript
  client.publish(stream, onFailure)
	```

1. 订阅远程音视频流 (subscribe)

	```javascript
	client.subscribe(stream, onFailure)
	```

1. (可选) 关闭双流模式 (disableDualStream)

	```javascript
	client.disableDualStream(onSuccess, onFailure)
	```

1. 离开 AgoraRTC 频道 (leave)

	```javascript
	client.leave(onSuccess, onFailure)
	```

## 7. 已知限制

- Windows 设备上使用 Firefox 浏览器加入时，最多只能订阅 8 个人的流。
- Mac OS 上使用 Firefox 浏览器退出 17 人频道时浏览器会卡死。
