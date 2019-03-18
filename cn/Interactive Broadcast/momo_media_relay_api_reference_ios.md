
---
title: Agora MOMO 特殊版 SDK API 参考
description: 
platform: iOS
updatedAt: Mon Mar 18 2019 06:35:49 GMT+0000 (UTC)
---
# Agora MOMO 特殊版 SDK API 参考
Agora Native SDK (Special Edition for MOMO) 基于 Agora Native SDK v2.2.0 开发，新增两个方法、两个回调以及相应的状态码、事件码和错误码。

下文列出了具体的 API 变动：

## 新增方法

### 开启跨频道连麦 (StartChannelMediaRelay)

`- (int)startChannelMediaRelay:(AgoraChannelMediaRelayConfiguration * _Nonnull)config;`

该方法开启跨频道连麦。

该方法调用成功后，
- 如果 `channelMediaRelayStateDidChange` 回调返回 `AgoraChannelMediaRelayState` 和 `AgoraChannelMediaRelayErrorNone`，`didReceiveChannelMediaRelayEvent` 回调返回 `AgoraChannelMediaRelayEventSentToDestinationChannel`，则表示开始向对端传输数据。
- 如果 `channelMediaRelayStateDidChange` 返回 `AgoraChannelMediaRelayStateFailure`，则表示连麦中出现异常，需要退出当前连麦状态。

**Note:**

- 该方法必须在成功加入频道后调用。
- 成功调用该方法后，必须调用 `StopChannelMediaRelay` 方法退出当前连麦的状态，才能再次调用。

| 名称     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| `config` | 详见下文 [`ChannelMediaRelayConfiguration`](#ChannelMediaRelayConfiguration) 属性定义。 |
| 返回值   | <li>0：方法调用成功。</li><li>&lt; 0：方法调用失败。</li>    |

<a name ="ChannelMediaRelayConfiguration"></a>
**`ChannelMediaRelayConfiguration` 属性定义如下：**

```
__attribute__((visibility("default"))) @interface AgoraChannelMediaRelayInfo: NSObject
@property (copy, nonatomic) NSString* _Nullable channelName;
@property (copy, nonatomic) NSString* _Nullable token;
@property (assign, nonatomic) NSUInteger uid;

- (instancetype _Nonnull)initWithChannelName:(NSString *_Nonnull)channelName token:(NSString *_Nullable)token;
@end

__attribute__((visibility("default"))) @interface AgoraChannelMediaRelayConfiguration: NSObject
@property (strong, nonatomic, readonly) AgoraChannelMediaRelayInfo *sourceInfo;
@property (strong, nonatomic, readonly) AgoraChannelMediaRelayInfo *destinationInfo;

- (instancetype _Nonnull)initWithSourceInfo:(AgoraChannelMediaRelayInfo *_Nonnull)sourceInfo destinationInfo:(AgoraChannelMediaRelayInfo *_Nonnull)destinationInfo;
@end
```

<table>
  <tr>
    <th>名称</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>sourceInfo</td>
    <td>本端 AgoraChannelMediaRelayInfo，包含三个参数：
		<br><li><code>channelName</code>：当前频道的名称。可为空，SDK 默认填充当前频道的名称。
		<br><li><code>token</code>：能加入当前频道的 Token。可为空，SDK 默认填充当前的 AppID。
		<br><li><code>uid</code>：转发服务进入本端频道的代理 uid。建议为 0，SDK 随机生成一个 uid。不能和本端频道内用户的 uid 重复。</td>
  </tr>
  <tr>
    <td>destinationInfo</td>
    <td>对端 AgoraChannelMediaRelayInfo，包含三个参数：
		<br><li><code>channelName</code>：不可为空，必须填写对端频道的名称。
		<br><li><code>token</code>：能加入对端频道的 Token。可为空，SDK 默认填写本端频道的 AppID。
		<br><li><code>uid</code>：进入对端频道的 uid。建议填写本端频道当前主播的uid，相当于主播进入了对端频道。可为 0，SDK 随机生成一个 uid。不能和对端频道内用户的 uid 重复。</td>
  </tr>
</table>

### 停止跨频道连麦 (StopChannelMediaRelay)

`- (int)stopChannelMediaRelay;`

该方法停止跨频道连麦。

该方法调用成功后，`channelMediaRelayStateDidChange` 回调返回 `AgoraChannelMediaRelayStateIdle` 和 `AgoraChannelMediaRelayErrorNone`，则表示正常退出连麦状态。

**Note:**
- 如果调用该方法后，当前已离开连麦状态，则再次调用无效。
- 在连麦过程中，如果网络环境较差，服务器没有接收到调用 `StopChannelMediaRelay` 方法发送的停止消息，会触发 `channelMediaRelayStateDidChange` 回调，返回 `AgoraChannelMediaRelayStateFailure` 和 `AgoraChannelMediaRelayErrorServerConnectionLost`。SDK 会关闭自身的连麦状态，但是服务器可能收不到。Agora 建议您调用 `leaveChannel` 方法离开频道。

| 名称   | 描述                                                      |
| ------ | --------------------------------------------------------- |
| 返回值 | <li>0：方法调用成功。</li><li>&lt; 0：方法调用失败。</li> |

## 新增回调

### 跨频道连麦状态信息回调 (channelMediaRelayStateDidChange)

`- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine channelMediaRelayStateDidChange:(AgoraChannelMediaRelayState)state error:(AgoraChannelMediaRelayError)error;`

该回调提示跨频道连麦状态发生改变。

<table>
  <tr>
    <th>参数</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>state</td>
    <td>状态码 AgoraChannelMediaRelayState：
		<br><li>AgoraChannelMediaRelayStateIdle = 0：初始化状态。
		<br><li>AgoraChannelMediaRelayStateConnecting = 1：正在准备服务。
		<br><li>AgoraChannelMediaRelayStateRunning = 2：服务正常运行。
		<br><li>AgoraChannelMediaRelayStateFailure = 3：服务出错状态。SDK 会重置自身的状态为初始化状态。</td>
  </tr>
  <tr>
    <td>code</td>
    <td>错误码 AgoraChannelMediaRelayError，提示错误类型及原因：
		<br><li>AgoraChannelMediaRelayErrorNone = 0：状态正常。
		<br><li>AgoraChannelMediaRelayErrorServerErrorResponse = 1： 服务器返回异常。
		<br><li>AgoraChannelMediaRelayErrorServerNoResponse = 2：无法和服务建立连接。
		<br><li>AgoraChannelMediaRelayErrorNoResourceAvailable = 3：无法申请服务，可能服务器资源不够。
		<br><li>AgoraChannelMediaRelayErrorFailedJoinSourceChannel = 4：服务器加入本地频道失败。
		<br><li>AgoraChannelMediaRelayErrorFailedJoinDestinationChannel = 5：服务器加入对端频道失败。
		<br><li>AgoraChannelMediaRelayErrorFailedPacketReceivedFromSource = 6：服务器收到本地流失败。
		<br><li>AgoraChannelMediaRelayErrorFailedPacketSentToDestination = 7：服务器发送留数据到远端频道失败。
		<br><li>AgoraChannelMediaRelayErrorServerConnectionLost = 8：与服务器连接断开，如果需要强制中断 Relay 服务，可以调用 <code>leaveChannel</code>，等待 15s 后，退出服务。</td>
  </tr>
</table>


### 跨频道连麦事件信息回调 (didReceiveChannelMediaRelayEvent)

`- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didReceiveChannelMediaRelayEvent:(AgoraChannelMediaRelayEvent)event;`

该回调提示跨频道连麦服务运行时出现的事件。

<table>
  <tr>
    <th>参数</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>code</td>
		<td>事件码 AgoraChannelMediaRelayEvent：
		<br><li>AgoraChannelMediaRelayEventDisconnect = 0：用户与服务连接断开，SDK会自动重连。
		<br><li>AgoraChannelMediaRelayEventConnected = 1：用户与服务连接建立。
		<br><li>AgoraChannelMediaRelayEventJoinedSourceChannel = 2：进入源频道。
		<br><li>AgoraChannelMediaRelayEventJoinedDestinationChannel = 3：进入目的频道。
		<br><li>AgoraChannelMediaRelayEventSentToDestinationChannel = 4：服务开始转发流。
		<br><li>AgoraChannelMediaRelayEventReceivedVideoPacketFromSource = 5：服务器收到源音频流。
		<br><li>AgoraChannelMediaRelayEventReceivedAudioPacketFromSource = 6：服务器收到源视频流。
		<br><li>AgoraChannelMediaRelayEventSourceTokenExpire = 7：源频道 Token 过期。
		<br><li>AgoraChannelMediaRelayEventDestinationTokenExpire = 8：目的频道 Token 过期。</td>
  </tr>
</table>

## 完整 API

详见[完整版 Agora Native SDK v2.2.0 API 参考](https://docs.agora.io/cn/2.2/product/Interactive%20Broadcast/API%20Reference/live_video_ios?platform=iOS)。
