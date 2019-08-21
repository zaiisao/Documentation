
---
title: 云端录制 C++ API
description: 
platform: CPP
updatedAt: Wed Aug 21 2019 06:47:29 GMT+0800 (CST)
---
# 云端录制 C++ API
| **接口类**                                                   | **描述**                   |
| ------------------------------------------------------------ | -------------------------- |
| <a href="#ICloudRecorder">ICloudRecorder</a> 类              | 应用程序调用的主要方法。   |
| <a href="#ICloudRecorderObserver">ICloudRecorderObserver</a> 类 | 向应用程序发送的回调通知。 |

<a name = "ICloudRecorder"></a>

## ICloudRecorder 接口类

该接口类包含应用程序调用的主要方法：

- [创建云端录制实例 (CreateCloudRecorderInstance)](#CreateCloudRecorderInstance)
- [获取云端录制 ID (GetRecordingId)](#GetRecordingId)
- [开始云端录制 (StartCloudRecording)](#StartCloudRecording)
- [结束云端录制 (StopCloudRecording)](#StopCloudRecording)
- [释放资源 (Release)](#Release)

<a name = "CreateCloudRecorderInstance"></a>

### 创建云端录制实例 (CreateCloudRecorderInstance)

```c++
ICloudRecorder* CreateCloudRecorderInstance(ICloudRecorderObserver *observer);
```

该方法创建云端录制实例。

| 参数       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| `observer` | 云端录制 SDK 所触发的事件通过 <a href="#ICloudRecorderObserver">ICloudRecorderObserver</a> 通知应用程序。 |

#### 返回值

- `ICloudRecorder`：云端录制实例。

<a name = "GetRecordingId"></a>

### 获取云端录制 ID (GetRecordingId)

```c++
virtual RecordingId GetRecordingId() = 0;
```

该方法获取云端录制 ID。一个录制实例对应一个唯一的录制 ID。

#### 返回值

- `RecordingId`：录制 ID，是当前录制的唯一标识。

<a name = "StartCloudRecording"></a>

### 开始云端录制 (StartCloudRecording)

```c++
virtual int StartCloudRecording(
  const char* appId,
  const char* channel_name,
  const char* token,
  const uid_t uid,
  const RecordingConfig& recording_config,
  const CloudStorageConfig &storage_config) = 0;
```

该方法创建录制资源并让 Cloud Recording SDK 加入频道，随后开始录制。

| 参数               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `appId`            | 待录制频道的 App ID，详见<a href="../../cn/cloud-recording/token.md">获取 App ID</a>。 |
| `channel_name`     | 待录制频道的频道名。                                         |
| `token`            | 待录制的频道中使用的 token，详见<a href="../../cn/cloud-recording/token.md">校验用户权限</a>。 |
| `uid`              | 云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。 |
| `recording_config` | 录制的详细设置，详见 <a href="#RecordingConfig">RecordingConfig</a>。 |
| `storage_config`   | 第三方云存储的详细设置，详见下表 <a href="#CloudStorageConfig">CloudStorageConfig</a>。 |

#### 返回值

- 0：方法调用成功。
- ≠ 0：方法调用失败。

<a name = "RecordingConfig"></a>

#### RecordingConfig

```c++
RecordingConfig() :
  recording_stream_types(kStreamTypeAudioVideo),
  decryption_mode(kModeNone),
  secret(0),
  channel_type(kChannelTypeCommunication),
  audio_profile(kAudioProfileMusicStandard),
  video_stream_type(kVideoStreamTypeHigh),
  max_idle_time(30) {
}
```

| 参数                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| `recording_stream_types` | 录制的媒体流类型：<li>`kStreamTypeAudioVideo`：录制音频和视频（默认）。<li>`kStreamTypeAudioOnly`：仅录制音频。<li>`kStreamTypeVideoOnly`：仅录制视频。 |
| `decryption_mode`        | 解密方案。云端录制 SDK 可以启用内置的解密功能。解密方式必须与频道设置的加密方式一致。<li>`kModeNone`：无（默认）。<li>`kModeAes128Xts`：设置 AES128XTS 解密方案。<li>`kModeAes128Ecb`：设置 AES128ECB 解密方案。<li>`kModeAes256Xts`：设置 AES256XTS 解密方案。 |
| `secret`                 | 启用解密模式后，设置的解密密码。                             |
| `channel_type`           | 频道模式。<li>`kChannelTypeCommunication`：通信模式（默认），即常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话。 <li>`kChannelTypeLive`：直播模式，有两种用户角色，主播和观众，只有主播可以自由发言。 <p>**频道模式必须与 Agora Native/Web SDK 一致，否则可能导致问题。** |
| `audio_profile`          | 设置音频采样率，码率，编码模式和声道数。<li>`kAudioProfileMusicStandard`：指定 48 KHz采样率，音乐编码, 单声道，编码码率约 48 Kbps（默认）。 <li>`kAudioProfileMusicHighQuality`：指定 48 KHz 采样率，音乐编码, 单声道，编码码率约 128 Kbps。<li>`kAudioProfileMusicHighQualityStereo`：指定 48 KHz 采样率，音乐编码, 双声道，编码码率约 192 Kbps。 |
| `video_stream_type`      | 视频流类型。<li>`kVideoStreamTypeHigh`：视频大流（默认），即高分辨率高码率的视频流。<li>`kVideoStreamTypeLow`：视频小流，即低分辨率低码率的视频流。 |
| `max_idle_time`          | 最长空闲频道时间。默认值为 30 秒，该值需大于等于 5。如果频道内无用户的状态持续超过该时间，录制程序会自动退出。 |
| `transcoding_config`     | 视频转码的详细设置，详见 <a href="#TranscodingConfig">TranscodingConfig</a>。 |

<a name = "TranscodingConfig"></a>

#### TranscodingConfig

```c++
TranscodingConfig() :
  width(360),
  height(640),
  fps(15),
  bitrate(500),
  max_resolution_uid(0),
  layout(kMixedVideoLayoutTypeFloat) {
}
```

| 参数                 | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| `width`              | 合流宽度，单位 pixels，默认值为 360。最大分辨率为 1080p，超过最大分辨率会报错。 |
| `height`             | 合流高度，单位 pixels，默认值为 640。最大分辨率为 1080p，超过最大分辨率会报错。 |
| `fps`                | 视频帧率，单位 fps，默认值为 15。                            |
| `bitrate`            | 视频码率，单位 Kbps，默认值为 500。                          |
| `max_resolution_uid` | 如果 `layout` 设为垂直布局，用该参数设置显示大流画面的用户 ID。 |
| `layout`             | 视频合流布局，详见[设置合流布局](../../cn/cloud-recording/cloud_layout_guide.md)。<li>`kMixedVideoLayoutTypeFloat`：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。</li><li>`kMixedVideoLayoutTypeBestFit`：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。</li><li>`kMixedVideolayoutTypeVertical`：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。 |

<a name = "resolution_table"></a>

关于设置合流的推荐码率，详见**视频分辨率、帧率、码率对照表**。

| 分辨率     | 帧率（fps） | 基准码率（通信场景）（Kbps） | 直播码率（直播场景）（Kbps） |
| ---------- | ----------- | ---------------------------- | ---------------------------- |
| 160 × 120  | 15          | 65                           | 130                          |
| 120 × 120  | 15          | 50                           | 100                          |
| 320 × 180  | 15          | 140                          | 280                          |
| 180 × 180  | 15          | 100                          | 200                          |
| 240 × 180  | 15          | 120                          | 240                          |
| 320 × 240  | 15          | 200                          | 400                          |
| 240 × 240  | 15          | 140                          | 280                          |
| 424 × 240  | 15          | 220                          | 440                          |
| 640 × 360  | 15          | 400                          | 800                          |
| 360 × 360  | 15          | 260                          | 520                          |
| 640 × 360  | 30          | 600                          | 1200                         |
| 360 × 360  | 30          | 400                          | 800                          |
| 480 × 360  | 15          | 320                          | 640                          |
| 480 × 360  | 30          | 490                          | 980                          |
| 640 × 480  | 15          | 500                          | 1000                         |
| 480 × 480  | 15          | 400                          | 800                          |
| 640 × 480  | 30          | 750                          | 1500                         |
| 480 × 480  | 30          | 600                          | 1200                         |
| 848 × 480  | 15          | 610                          | 1220                         |
| 848 × 480  | 30          | 930                          | 1860                         |
| 640 × 480  | 10          | 400                          | 800                          |
| 1280 × 720 | 15          | 1130                         | 2260                         |
| 1280 × 720 | 30          | 1710                         | 3420                         |
| 960 × 720  | 15          | 910                          | 1820                         |
| 960 × 720  | 30          | 1380                         | 2760                         |



<a name = "CloudStorageConfig"></a>

#### CloudStorageConfig

```c++
struct CloudStorageConfig {
  CloudStorageVendor vendor;
  unsigned int region;
  char* bucket;
  char* access_key;
  char* secret_key;
};
```

| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| `vendor`     | 第三方云存储：<li>0：[七牛云](https://www.qiniu.com/products/kodo)。<li>1：[Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1)。<li>2：[阿里云](https://www.aliyun.com/product/oss)。 |
| `region`     | 第三方云存储指定的地区信息。<br>当 `vendor` = 0，即第三方云存储为[七牛云](https://www.qiniu.com/products/kodo)时：<li>0：Huadong <li>1：Huabei <li>2：Huanan <li>3：Beimei  <br>当 `vendor` = 1，即第三方云存储为 [Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1) 时：<li>0：US_EAST_1 <li>1：US_EAST_2 <li>2：US_WEST_1 <li>3：US_WEST_2  <li>4：EU_WEST_1 <li> 5：EU_WEST_2  <li>6：EU_WEST_3 <li>7：EU_CENTRAL_1 <li>8：AP_SOUTHEAST_1 <li>9：AP_SOUTHEAST_2 <li>10：AP_NORTHEAST_1 <li>11：AP_NORTHEAST_2 <li>12：SA_EAST_1 <li>13：CA_CENTRAL_1 <li>14：AP_SOUTH_1 <li>15：CN_NORTH_1 <li>16：CN_NORTHWEST_1 <li>17：US_GOV_WEST_1 <br>当 `vendor` = 2，即第三方云存储为[阿里云](https://www.aliyun.com/product/oss)时：<li>0：CN_Hangzhou <li>1：CN_Shanghai <li>2：CN_Qingdao <li>3：CN_Beijin  <li>4：CN_Zhangjiakou <li> 5：CN_Huhehaote  <li>6：CN_Shenzhen <li>7：CN_Hongkong <li>8：US_West_1 <li>9：US_East_1 <li>10：AP_Southeast_1 <li>11：AP_Southeast_2 <li>12：AP_Southeast_3 <li>13：AP_Southeast_5 <li>14：AP_Northeast_1 <li>15：AP_South_1 <li>16：EU_Central_1 <li>17：EU_West_1 <li>18：EU_East_1 |
| `bucket`     | 第三方云存储 bucket。                                        |
| `access_key` | 第三方云存储 access key。                                    |
| `secret_key` | 第三方云存储 secret key。                                    |

<a name = "StopCloudRecording"></a>

### 结束云端录制 (StopCloudRecording)

```c++
virtual int StopCloudRecording() = 0;
```

该方法手动结束云端录制。若未调用该方法，当频道空闲（无用户）超过预设时间（默认为30 秒）后，Cloud Recording SDK 会自动退出频道停止录制。

调用该方法结束录制后必须调用 `Release` 方法销毁实例，无法再调用其他方法。

#### 返回值：  

- 0：方法调用成功。

- ≠ 0：方法调用失败。


<a name = "Release"></a>

### 释放资源(Release)

```c++
virtual void Release(bool cancelCloudRecording = false) = 0;
```

该方法销毁 `ICloudRecorder` 实例，释放 Agora Cloud Recording SDK 使用的资源，将无法再次使用和回调该 SDK 内的其它方法。如需再次使用云端录制，必须重新创建 `ICloudRecorder` 实例。

| 参数                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `cancelCloudRecording` | 是否在服务器后台进行录制：<li>`true`：停止录制与上传 。<li>`false`：（默认）在服务器后台继续进行录制并上传。 |

<a name = "ICloudRecorderObserver"></a>

## ICloudRecorderObserver 回调接口类

该接口类用于向应用程序发送回调通知。该类包含如下回调：

- [正在连接云端录制服务器回调 (OnRecordingConnecting)](#OnRecordingConnecting)
- [开始云端录制回调 (OnRecordingStarted)](#OnRecordingStarted)
- [云端录制已停止回调 (OnRecordingStopped)](#OnRecordingStopped)
- [成功上传至第三方云存储回调 (OnRecordingUploaded)](#OnRecordingUploaded)
- [成功上传至云备份回调 (OnRecordingBackedUp)](#OnRecordingBackedUp)
- [上传进度回调 (OnRecordingUploadingProgress)](#OnRecordingUploadingProgress)
- [录制组件异常回调 (OnRecorderFailure)](#OnRecorderFailure)
- [上传组件异常回调 (OnUploaderFailure)](#OnUploaderFailure)
- [录制失败回调 (OnRecordingFatalError)](#OnRecordingFatalError)



### <a name = "OnRecordingConnecting"></a>正在连接云端录制服务器回调 (OnRecordingConnecting)

```c++
virtual void OnRecordingConnecting(RecordingId recording_id) = 0;
```

该回调方法表示应用程序正在连接云端录制服务器。

| 参数           | 描述                            |
| -------------- | ------------------------------- |
| `recording_id` | 录制 ID，是当前录制的唯一标识。 |


### <a name = "OnRecordingStarted"></a>开始云端录制回调 (OnRecordingStarted)

```c++
virtual void OnRecordingStarted(RecordingId recording_id) = 0;
```

该回调方法表示应用程序已成功开始云端录制。

| 参数           | 描述                            |
| -------------- | ------------------------------- |
| `recording_id` | 录制 ID，是当前录制的唯一标识。 |


### <a name = "OnRecordingStopped"></a>结束云端录制回调 (OnRecordingStopped)

```c++
virtual void OnRecordingStopped(RecordingId recording_id) = 0;
```

该回调方法表示应用程序已成功结束云端录制。

| 参数           | 描述                            |
| -------------- | ------------------------------- |
| `recording_id` | 录制 ID，是当前录制的唯一标识。 |


### <a name = "OnRecordingUploaded"></a>成功上传至第三方云存储回调 (OnRecordingUploaded)

```c++
virtual void OnRecordingUploaded(RecordingId recording_id,
	const char* file_name) = 0;
```

该回调方法表示录制文件已成功上传到预设的第三方云存储，可根据需求下载使用。

| 参数           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | 录制 ID，是当前录制的唯一标识。                              |
| `file_name`    | 录制产生的 M3U8 文件的文件名。每个录制实例均会生成一个 M3U8 文件，用于索引所有的录制切片 TS 文件。 |

### <a name = "OnRecordingBackedUp"></a>成功上传至云备份回调 (OnRecordingBackedUp)

```c++
virtual void OnRecordingBackedUp(RecordingId recording_id, 
	const char* file_name) = 0;
```

该回调方法表示录制文件成功上传到 Agora 云备份。

如果在录制过程中有录制文件未能成功上传至第三方云存储，Agora 服务器会自动将这部分录制文件上传至 Agora 云备份，在录制结束后触发该回调。Agora 云备份会继续尝试将这部分文件上传至设定的第三方云存储。如果等待五分钟后仍然不能正常[播放录制文件](../../cn/cloud-recording/cloud_recording_onlineplay.md)，请联系 Agora 技术支持。

| 参数           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | 录制 ID，是当前录制的唯一标识。                              |
| `file_name`    | 录制产生的 M3U8 文件的文件名。每个录制实例均会生成一个 M3U8 文件，用于索引所有的录制切片 TS 文件。 |


### <a name = "OnRecordingUploadingProgress"></a>上传进度回调 (OnRecordingUploadingProgress) 

```c++
virtual void OnRecordingUploadingProgress(RecordingId recording_id,
      unsigned int progress, const char* recording_playlist_filename) = 0;
```

该回调方法表示录制文件上传进度。录制开始后，Agora 服务器会不断向第三方云存储上传录制文件，SDK 会每分钟触发一次该回调通知应用程序上传进度。

| 参数                          | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| `recording_id`                | 录制 ID，是当前录制的唯一标识。                              |
| `progress`                    | 上传进度。显示当前上传占当前录制的百分比。                   |
| `recording_playlist_filename` | 录制产生的 M3U8 文件的文件名。每个录制实例均会生成一个 M3U8 文件，用于索引所有的录制切片 TS 文件。 |

### <a name = "OnRecorderFailure"></a>录制组件异常回调 (OnRecorderFailure)

```c++
virtual void OnRecorderFailure(RecordingId recording_id,
	RecordingErrorCode code, const char* msg) = 0;
```

该回调方法仅用来通知录制组件异常，无需对此进行任何操作。

| 参数           | 描述                            |
| -------------- | ------------------------------- |
| `recording_id` | 录制 ID，是当前录制的唯一标识。 |
| `code`         | [错误代码](#ErrorCode)。        |
| `msg`          | 错误消息。                      |


### <a name = "OnUploaderFailure"></a>上传组件异常回调 (OnUploaderFailure)

```c++
virtual void OnUploaderFailure(RecordingId recording_id,
	RecordingErrorCode code, const char* msg) = 0;
```

该回调方法仅用来通知上传组件异常，无需对此进行任何操作。

| 参数           | 描述                            |
| -------------- | ------------------------------- |
| `recording_id` | 录制 ID，是当前录制的唯一标识。 |
| `code`         | [错误代码](#ErrorCode)。        |
| `msg`          | 错误消息。                      |

### <a name = "OnRecordingFatalError"></a>录制 SDK 异常回调 (OnRecordingFatalError)

```c++
virtual void OnRecordingFatalError(RecordingId recording_id,
	RecordingErrorCode code) = 0;
```

该回调方法表示云端录制 SDK 发生了不可恢复的错误，可根据具体的错误代码判断录制后台的状态。

| 参数           | 描述                            |
| -------------- | ------------------------------- |
| `recording_id` | 录制 ID，是当前录制的唯一标识。 |
| `code`         | [错误代码](#ErrorCode)。        |


## <a name = "ErrorCode"></a>错误代码

Agora Cloud Recording SDK 在调用 API 或运行时，可能会返回如下错误代码:

| 错误代码                         | 描述                         | 解决方法                              |
| -------------------------------- | ---------------------------- | ------------------------------------- |
| `RecordingErrorOk`               | 没有错误。                   | 无。                                  |
| `RecordingErrorConenctError`     | 连接至云端录制服务器失败。   | 检查网络和 appid 的权限。             |
| `RecordingErrorDisconnected`     | 与云端录制服务器的连接中断。 | 检查网络。                            |
| `RecordingErrorInvalidParameter` | 参数不合法。                 | 检查参数。                            |
| `RecordingErrorInvalidOperation` | 不支持当前操作。             | 检查调用顺序。                        |
| `RecordingErrorNoUsers`          | 频道内无用户。               | 调用 Release (false) 来释放本地资源。 |
| `RecordingErrorNoRecordedData`   | 未生成录制数据。             | 调用 Release (false) 来释放本地资源。 |
| `RecordingErrorRecorderInit`     | 初始化异常。                 | 调用 Release (true) 并重新开始录制。  |
| `RecordingErrorRecorderFailed`   | 录制异常。                   | 调用 Release (true) 并重新开始录制。  |
| `RecordingErrorUploaderInit`     | 初始化上传组件异常。         | 调用 Release (true) 并重新开始录制。  |
| `RecordingErrorUploaderFailed`   | 上传组件异常。               | 调用 Release (true) 并重新开始录制。  |
| `RecordingErrorBackupFailed`     | 云备份异常。                 | 调用 Release (false) 来释放本地资源。 |
| `RecordingErrorRecorderExit`     | 退出异常。                   | 调用 Release (true) 并重新开始录制。  |
| `RecordingErrorGeneralError`     | 其他错误。                   | 调用 Release (false) 来释放本地资源。 |
