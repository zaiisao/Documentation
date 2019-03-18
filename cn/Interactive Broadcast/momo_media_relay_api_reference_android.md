
---
title: Agora MOMO 特殊版 SDK API 参考
description: 
platform: Android
updatedAt: Mon Mar 18 2019 06:35:42 GMT+0000 (UTC)
---
# Agora MOMO 特殊版 SDK API 参考
Agora Native SDK (Special Edition for MOMO) 基于 Agora Native SDK v2.2.0 开发，新增两个方法、两个回调以及相应的状态码、事件码和错误码。

下文列出了具体的 API 变动：

## 新增方法

### 开启跨频道连麦 (StartChannelMediaRelay)

`int StartChannelMediaRelay(ChannelMediaRelayConfiguration config);`

该方法开启跨频道连麦。

该方法调用成功后，
- 如果 `onChannelMediaRelayStateChanged` 回调返回 `RELAY_STATE_RUNNING` 和 `RELAY_OK`，`onChannelMediaRelayEvent` 回调返回 `RELAY_EVENT_PACKET_SENT_TO_DEST_CHANNEL`，则表示开始向对端传输数据。
- 如果 `onChannelMediaRelayStateChanged` 返回 `RELAY_STATE_FAILURE`，则表示连麦中出现异常，需要退出当前连麦状态。

**Note:**
- 该方法必须在成功加入频道后调用。
- 成功调用该方法后，必须调用 `StopChannelMediaRelay` 方法退出当前连麦的状态，才能再次调用。

| 名称   | 描述                                           |
|--------|------------------------------------------------|
| `config` | 详见下文 [`ChannelMediaRelayConfiguration`](#ChannelMediaRelayConfiguration) 结构。 |
| 返回值    | <li>0：方法调用成功。</li><li>&lt; 0：方法调用失败。</li> |


<a name ="ChannelMediaRelayConfiguration"></a>
**`ChannelMediaRelayConfiguration` 结构如下：**

```
public class ChannelMediaRelayConfiguration {
    private ChannelMediaInfo srcInfo;
    private ChannelMediaInfo destInfo;

    public void setSrcChannelMediaInfo(final String channelName,
                                       final String token,
                                       int srcUid) {
        srcInfo = new ChannelMediaInfo(channelName,token,srcUid);
    }

    public void setDestChannelInfo(final String channelName,
                                   final String token,
                                   int destUid) {
        destInfo = new ChannelMediaInfo(channelName,token,destUid);
    }

    public ChannelMediaInfo getSrcChannelMediaInfo(){
        return srcInfo;
    }

    public ChannelMediaInfo getDestChannelMediaInfo(){
        return destInfo;
    }

    public class ChannelMediaInfo {
        public String channelName;
        public String token;
        public int uid;

        public ChannelMediaInfo(String channelName, String token, int uid) {
            this.channelName = channelName;
            this.token = token;
            this.uid = uid;
        }
    }

}
```

<table>
  <tr>
    <th>名称</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>SrcChannelMediaInfo</td>
    <td>本端频道信息，包含三个参数：
		<br><li><code>channelName</code>：当前频道的名称。可为空，SDK 默认填充当前频道的名称。
		<br><li><code>token</code>：能加入当前频道的 Token。可为空，SDK 默认填充当前的 AppID。
		<br><li><code>uid</code>：转发服务进入本端频道的代理 uid。建议为 0，SDK 随机生成一个 uid。不能和本端频道内用户的 uid 重复。</td>
  </tr>
  <tr>
    <td>DestChannelMediaInfo</td>
    <td>对端频道信息，包含三个参数：
		<br><li><code>channelName</code>：不可为空，必须填写对端频道的名称。
		<br><li><code>token</code>：能加入对端频道的 Token。可为空，SDK 默认填写本端频道的 AppID。
		<br><li><code>uid</code>：进入对端频道的 uid。建议填写本端频道当前主播的uid，相当于主播进入了对端频道。可为 0，SDK 随机生成一个 uid。不能和对端频道内用户的 uid 重复。</td>
  </tr>
</table>

### 停止跨频道连麦 (StopChannelMediaRelay)

`int StopChannelMediaRelay();`

该方法停止跨频道连麦。

该方法调用成功后，`onChannelMediaRelayStateChanged` 回调返回 `RELAY_STATE_IDLE` 和 `RELAY_OK`，则表示正常退出连麦状态。

**Note:**
- 如果调用该方法后，当前已离开连麦状态，则再次调用无效。
- 在连麦过程中，如果网络环境较差，服务器没有接收到调用 `StopChannelMediaRelay` 方法发送的停止消息，会触发 `onChannelMediaRelayStateChanged` 回调，返回 `RELAY_STATE_FAILURE` 和 `RELAY_ERROR_SERVER_CONNECTION_LOST`。SDK 会返回初始化状态，但是服务器可能接收不到。Agora 建议您调用 `leaveChannel` 方法确保离开频道。

| 名称   | 描述                                           |
|--------|------------------------------------------------|
| 返回值    | <li>0：方法调用成功。</li><li>&lt; 0：方法调用失败。</li> |

## 新增回调

### 跨频道连麦状态信息回调 (onChannelMediaRelayStateChanged)

`void onChannelMediaRelayStateChanged(int state,int code)`

该回调提示跨频道连麦状态发生改变。

<table>
  <tr>
    <th>参数</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>state</td>
    <td>状态码 CHANNEL_MEDIA_RELAY_STATE：
		<br><li>RELAY_STATE_IDLE：初始化状态。
		<br><li>RELAY_STATE_CONNECTING：正在准备服务。
		<br><li>RELAY_STATE_RUNNING：服务正常运行。
		<br><li>RELAY_STATE_FAILURE：服务出错状态。SDK 会重置自身的状态为初始化状态。</td>
  </tr>
  <tr>
    <td>code</td>
    <td>错误码 CHANNEL_MEDIA_RELAY_ERROR，提示错误类型及原因：
		<br><li>RELAY_OK：状态正常。
		<br><li>RELAY_ERROR_SERVER_ERROR_RESPONSE：服务器返回异常。
		<br><li>RELAY_ERROR_SERVER_NO_RESPONSE：无法和服务建立连接。
		<br><li>RELAY_ERROR_NO_RESOURCE_AVAILABLE：无法申请服务，可能服务器资源不够。
		<br><li>RELAY_ERROR_FAILED_JOIN_SRC：服务器加入本地频道失败。
		<br><li>RELAY_ERROR_FAILED_JOIN_DEST：服务器加入对端频道失败。
		<br><li>RELAY_ERROR_FAILED_RECEIVE_PACKET_FROM_SRC：服务器收到本地流失败。
		<br><li>RELAY_ERROR_FAILED_SEND_PACKET_TO_DEST：服务器发送留数据到远端频道失败。
		<br><li>RELAY_ERROR_SERVER_CONNECTION_LOST：与服务器连接断开，如果需要强制中断 Relay 服务，可以调用 <code>leaveChannel</code>，等待 15s 后，退出服务。</td>
  </tr>
</table>


### 跨频道连麦事件信息回调 (onChannelMediaRelayEvent)

`void onChannelMediaRelayEvent(int code)`

该回调提示跨频道连麦服务运行时出现的事件。

<table>
  <tr>
    <th>参数</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>code</td>
		<td>事件码 CHANNEL_MEDIA_RELAY_EVENT：
		<br><li>RELAY_EVENT_NETWORK_DISCONNECTED：用户与服务连接断开，SDK 会自动重连。
		<br><li>RELAY_EVENT_NETWORK_CONNECTED：用户与服务连接建立。
		<br><li>RELAY_EVENT_PAKCET_JOINED_SRC_CHANNEL：进入源频道。
		<br><li>RELAY_EVENT_PAKCET_JOINED_DEST_CHANNEL：进入目的频道。
		<br><li>RELAY_EVENT_PACKET_SENT_TO_DEST_CHANNEL：服务开始转发流。
		<br><li>RELAY_EVENT_PACKET_RECEIVED_VIDEO_FROM_SRC：服务器收到源音频流。
		<br><li>RELAY_EVENT_PACKET_RECEIVED_AUDIO_FROM_SRC：服务器收到源视频流。
		<br><li>RELAY_EVENT_SRC_TOKEN_EXPIRED：源频道 Token 过期。
		<br><li>RELAY_EVENT_DEST_TOKEN_EXPIRED：目的频道 Token 过期。</td>
  </tr>
</table>

## 完整 API

详见[完整版 Agora Native SDK v2.2.0 API 参考](https://docs.agora.io/cn/2.2/product/Interactive%20Broadcast/API%20Reference/live_video_android?platform=Android)。







