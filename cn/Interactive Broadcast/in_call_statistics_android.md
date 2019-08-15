
---
title: 检测通话质量
description: 
platform: Android
updatedAt: Thu Aug 15 2019 08:13:11 GMT+0800 (CST)
---
# 检测通话质量
通话质量检测功能是在 SDK **加入频道后**通过每 2 秒触发一次的回调实现。

通话质量检测包括：

- 用户上下行网络质量打分
- 本地用户视频质量统计
  - 发送质量统计
- 远端用户音频质量统计
  - 端到端质量统计
  - 网络传输层质量统计
- 远端用户视频质量统计
  - 端到端质量统计
  - 网络传输层质量统计

## 用户上下行网络质量打分

![](https://web-cdn.agora.io/docs-files/1546917962318)

### 功能描述

加入频道后，如果想检测通话中每个用户／主播的网络上下行 last mile 质量报告，则可以通过回调 `onNetworkQuality` 得到。

`onNetworkQuality` 携带 `uid` 信息，如果频道内有多个用户或主播，SDK 会多次触发该回调。打分包括：

- `txQuality`：基于当前设备到边缘服务器的上行网络质量打分（EXCELLENT～VBAD）<sup>[1]</sup>。打分依赖项：
  - 上行视频实际发送码率与目标发送码率的比率，比率越高，通话质量越高；
  - 上行丢包率；
  - 平均往返延时（RTT）；
  - 上行网络抖动（Jitter）。

- `rxQuality`：基于边缘服务器到当前设备的下行网络质量打分（EXCELLENT～VBAD）<sup>[1]</sup>。打分依赖项：
  - 下行丢包率；
  - 平均往返延时（RTT）；
  - 下行网络抖动（Jitter）。

<a name ="table"></a>
> [1] 质量打分对照表如下：
>
> | 质量打分  | 说明                                                         |
> | -------- | :----------------------------------------------------------- |
> | 0        | UNKNOWN：网络质量未知。                                        |
> | 1        | EXCELLENT：网络质量极好。                                      |
> | 2        | GOOD：用户主观感觉和 EXCELLENT 差不多，但码率可能略低于 EXCELLENT。 |
> | 3        | POOR：用户主观感受有瑕疵但不影响沟通。                       |
> | 4        | BAD：勉强能沟通但不顺畅。                                    |
> | 5        | VBAD：网络质量非常差，基本不能沟通。                         |
> | 6        | DOWN：网络连接断开，完全无法沟通。                         |

### API 参考

[`onNetworkQuality`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a76be982389183c5fe3f6e4b03eaa3bd4)

```java
void onNetworkQuality(int uid, int txQuality, int rxQuality) {
    }
```

### 注意事项

用户上下行网络质量打分 `onNetworkQuality` 不同于通话前的网络质量检测 `onLastmileQuality`：
- `onLastmileQuality` 在用户加入频道前通过主动调用 `enableLastmileTest` 方法后才会触发；`onNetworkQuality` 在用户加入频道后自动触发。
- `onLastmileQuality` 反映的是本地用户设备到边缘服务器的网络上下行质量；`onNetworkQuality` 反映的是所有用户或主播的设备到边缘服务器的网络上下行质量（如果频道内有多个用户或主播，该回调会相应多次触发）。

## 本地用户视频质量统计

### 功能描述

本地用户的视频质量统计反映的是本地视频流的实际发送帧率和实际发送码率。涉及回调：

- `onLocalVideoStats` 2 秒自动回调：反映的是当前设备发送视频流的状态。主要返回信息：
  - `sentBitrate`：实际发送码率（Kbps）。
  - `sentFrameRate`：实际发送帧率（fps）。
  - `targetBitrate`：当前编码器的目标编码码率（Kbps），该码率为 SDK 根据当前网络状况预估的一个值。
  - `targetFrameRate`：当前编码器的目标编码帧率（fps）。
  - `qualityAdaptIndication`：和上次返回的本地视频流统计信息相比，本地视频质量（主要是码率和帧率）的自适应情况：
	- `ADAPT_NONE` = 0：未进行自适应
	- `ADAPT_UP_BANDWIDTH` = 1：由于带宽增加，视频的码率和帧率向上自适应
	- `ADAPT_DOWN_BANDWIDTH` = 2：由于带宽减少，视频的码率和帧率向下自适应

### API 参考

[`onLocalVideoStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a79fb5d32bb694d24b54a523d924dc7ef)

```java
void onLocalVideoStats(LocalVideoStats stats) {
    }
```

## 远端用户音频质量统计

![](https://web-cdn.agora.io/docs-files/1546917989079)

### 功能描述

如上图所示，本功能反映远端音频流全链路的音频质量以及传输层网络状态。涉及回调：

- `onRemoteAudioStats` 2 秒自动回调：侧重反映通话中远端音频流的全链路音频质量，更贴近用户主观感受。主要返回信息：
  - `quality`：音频接收质量打分（Excellent～VeryBad）<sup>[2]</sup>。
  - `networkTransportDelay`：网络传输层延时（毫秒），networkTransportDelay = Delay 2 + Delay 3 + Delay 4。
  - `jitterBufferDelay`：接收端网络抖动延时（毫秒），jitterBufferDelay = Delay 5。
  - `audioLossRate`：音频丢帧率。

- `onRemoteAudioTransportStats` 2 秒自动回调：侧重反映通话中远端音频流的传输层网络状态，数据更为客观。主要返回信息：
  - `delay`：网络传输层延时（毫秒），delay = Delay 2 + Delay 3 + Delay 4。
  - `lost`：传输层音频丢包率（%），lost = (packetLoss 2 + packetLoss 3 + packetLoss 4)/totalPacketsSent。
  - `rxKBitRate`：音频接收码率（kbps）。

> [2] 参见上文[质量打分对照表](#table)。

### API 参考

- [`onRemoteAudioStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9eaf8021d6f0c97d056e400b50e02d54)
```java
void onRemoteAudioStats(RemoteAudioStats stats) {
    }
```
- [`onRemoteAudioTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a826009699e73d5225d4ce9e3a29b91f4)
```java
void onRemoteAudioTransportStats(int uid, int delay, int lost, int rxKBitRate) {
    }
```

### 注意事项

`onRemoteAudioStats` 与 `onRemoteAudioTransportStats` 回调的区别在于：

- `onRemoteAudioTransportStats` 用于描述用户通话中从边缘服务器到边缘服务器的网络状态，通过音频包计算，展示当前网络状态，体现真实网络客观数据，比如丢包、网络延迟。
- `onRemoteAudioStats` 反映的是全链路的音频质量，更贴近用户的主观感受。比如，当网络发生丢包时，由于 FEC（Forward Error Correction，向前纠错码）或重传恢复的应用，最终的音频丢帧率不高，则可以认为整个质量较好。 

## 远端用户视频质量统计

![](https://web-cdn.agora.io/docs-files/1546918006108)

### 功能描述

如上图所示，远端用户的视频质量统计反映远端视频流全链路的视频质量以及传输层网络状态。涉及回调：

- `onRemoteVideoStats` 2 秒自动回调：侧重反映通话中远端视频流的全链路音频质量，更贴近用户主观感受。主要返回信息：
  - `receivedBitrate`：接收到的码率（kbps）。
  - `receivedFrameRate`：接收到的帧率（fps）。
  - `rxStreamType`：视频大小流类型。

- `onRemoteVideoTransportStats` 2 秒自动回调：侧重反映通话中远端视频流的传输层网络状态，数据更为客观。主要返回信息：
  - `delay`：网络传输层延时（毫秒），delay = Delay 2 + Delay 3 + Delay 4。
  - `lost`：传输层视频包丢包率（%），lost = (packetLoss 2 + packetLoss 3 + packetLoss 4)/totalPacketsSent。
  - `rxKBitRate`：远端视频包的实际接收码率（Kbps）。 

### API 参考
- [`onRemoteVideoStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#abb7af6e2827bbd03c6ab8338a0f616ca)
```java
void onRemoteVideoStats(RemoteVideoStats stats) {
    }
```
- [`onRemoteVideoTransportStats`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a8e8bea20663388c250b299641b25ade9)
```java
void onRemoteVideoTransportStats(int uid, int delay, int lost, int rxKBitRate) {
    }
```

### 注意事项

`onRemoteVideoTransportStats` 与 `onRemoteVideoStats` 回调的区别在于：

- `onRemoteVideoTransportStats` 用于描述用户通话中从边缘服务器到边缘服务器的网络状态，通过视频包计算，展示当前网络状态，体现真实网络客观数据，比如丢包、网络延迟。
- `onRemoteVideoStats` 反映的是全链路的视频质量，更贴近用户的主观感受。


