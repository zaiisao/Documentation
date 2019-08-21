
---
title: 云端录制快速开始
description: 
platform: Linux Java
updatedAt: Wed Aug 21 2019 06:56:44 GMT+0800 (CST)
---
# 云端录制快速开始
本文介绍如何集成 Agora Cloud Recording SDK 进行通话或直播录制。

> 当云端录制 SDK 加入频道时，相当于一个哑客户端加入频道，因此需要跟 Agora Native/Web SDK 加入相同的频道，并使用相同的 App ID 和频道模式。

## 前提条件

请确保满足以下要求：

- 打开 TCP 端口：4447、8913、9700。
- 打开 UDP 端口：53、4444。
- 运行环境（物理或虚拟）：
  - Ubuntu Linux 14.04+ LTS 64 位
  - CentOS 6.5+（推荐 7.0）64 位
- 安装 Java 环境，建议安装 OpenJDK 1.6 或以上版本。
- 联系 [sales@agora.io](mailto:sales@agora.io) 开通云端录制服务。
- 开通第三方云存储服务，目前支持[七牛云](https://www.qiniu.com/products/kodo)、[阿里云](https://www.aliyun.com/product/oss)（推荐）和 [Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1)。

## 集成

1. 下载[云端录制 SDK](http://download.agora.io/acrsdk/release/Agora_Cloud_Recording_JAVA_SDK_for_Linux_FULL_v1_0_0.tar.gz)。 
2. 将云端录制 SDK 包中 `lib` 文件夹下的 `agora-cloud-recording-sdk.jar` 文件和 `libagora-cloud-recording-java.so` 文件添加到你的项目里，并确保与项目有连接。
3. Import `agora-cloud-recording-sdk.jar` 文件：

  ```bash
 import io.agora.cloud_recording.<class name>
  ```

完成集成后，你可以参照下文进行云端录制。

## 实现云端录制
一个完整的云端录制过程主要包括以下步骤：

1. [创建实例](#create)

2. [开始录制](#start)（加入频道）

3. [结束录制](#stop)（离开频道）

4. [释放资源](#release)

目前云端录制仅支持自动录制模式，也就是加入频道即自动开始录制，离开频道即自动结束录制。对于一个录制实例来说，离开频道后录制就结束了，如果需要再次录制必须创建一个新的实例。

你可以对同一个频道多次执行这个录制过程，录制你需要的内容。

### <a name="create"></a>创建实例

```java
public static CloudRecorder createCloudRecorderInstance(ICloudRecordingEventHandler handler)
```

在加入频道进行录制之前，需要调用 [`createCloudRecorderInstance`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法创建一个云端录制实例，并将其与应用程序相关联。可根据需要创建多个实例同时录制。

录制完成后，如不再需要该实例，可以通过调用 [`release`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法销毁实例，释放资源。

### <a name="start"></a>开始云端录制

```java
public RecordingErrorCode startCloudRecording(
    String appId,
    String channelName,
    String token,
    int uid,
    RecordingConfig config,
    CloudStorageConfig storageConfig)
```

创建实例后，调用 [`startCloudRecording`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法开始录制，在该方法中需要填写如下参数：

- `appId`：待录制频道的 App ID，必填。
- `channelName`：待录制频道的频道名，必填。
- `token`：待录制的频道中使用的 token，选填。详见[校验用户权限](../../cn/cloud-recording/token.md)。
- `uid`：云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。
- `config`：配置录制参数，选填，如不填则使用默认配置。详见[`RecordingConfig`](../../cn/cloud-recording/cloud_recording_api_java.md)。
- `storageConfig`：配置第三方云存储，必填。详见 [`CloudStorageConfig`](../../cn/cloud-recording/cloud_recording_api_java.md)。

> - App ID 和频道名的设置必须与待录制频道的 Agora Native/Web SDK 设置完全一致。
> - 如果待录制频道使用了 Token，`token` 参数必须设置。

调用 [`startCloudRecording`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法后，监测到频道内有用户即开始录制，并触发 [`onRecordingStarted`](../../cn/cloud-recording/cloud_recording_api_java.md) 回调，通知应用程序成功开始录制。

### <a name="stop"></a> 结束云端录制

```java
public RecordingErrorCode stopCloudRecording()
```

录制完成后，调用 [`stopCloudRecording`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法离开频道，停止录制。

> 录制停止后如果需要再次开始录制，必须重新创建实例。

调用该方法后 SDK 会触发 [`onRecordingStopped`](../../cn/cloud-recording/cloud_recording_api_java.md) 回调，通知应用程序已成功停止录制。

若未调用 [`stopCloudRecording`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法，当频道空闲（无用户）超过预设时间（默认为30 秒） 后，Cloud Recording SDK 会自动退出频道停止录制，并会收到 [`onRecorderFailure`](../../cn/cloud-recording/cloud_recording_api_java.md) 回调，错误码为 `RecordingErrorNoUsers`，通知应用程序录制退出异常。

### <a name="release"></a> 释放资源

```java
public RecordingErrorCode release(boolean cancelCloudRecording)
```

待录制完成并成功上传到第三方云存储后，你需要调用 [`release`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法销毁 `CloudRecorder` 实例，释放 SDK 使用的资源，释放资源后将无法再次使用和回调 SDK 内的其它方法。如需再次使用云端录制，必须重新创建实例。

在该方法中，`cancelCloudRecording` 参数默认为 `false`，正常情况下无需修改；如果录制出现错误需要重启录制服务，将该参数设为 `true`，具体情况请参考[错误代码](../../cn/cloud-recording/cloud_recording_api_java.md)。

## 上传和管理录制文件

开始录制后，Agora 服务器会将录制内容切片为多个 TS 文件并不断上传至预先设定的第三方云存储，直至录制结束。

### 录制 ID

录制 ID 是录制的唯一标识，每个录制实例对应一个录制 ID。

在创建录制实例后，你可以通过调用 [`getRecordingId`](../../cn/cloud-recording/cloud_recording_api_java.md) 方法获取录制 ID，所有的回调中也都包含录制 ID。

### 录制文件索引

每个录制实例均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。

M3U8 文件名由录制 ID 和频道名组成，如 `recording_id_channelName.M3U8`。

### 上传录制文件

录制文件的上传由 Agora 服务器自动完成，你需要关注下面的回调。

- 录制过程中
  - [`onRecordingUploadingProgress`](../../cn/cloud-recording/cloud_recording_api_java.md)：录制开始后约每分钟触发一次，该回调中的 `progress` 参数表示已上传文件占已录制文件的百分比。
- 停止录制后，根据上传情况，SDK 会触发以下回调之一：
  - [`onRecordingUploaded`](../../cn/cloud-recording/cloud_recording_api_java.md)：如果录制文件全部成功上传至预先设定的云存储，最后一个录制切片文件上传完成时触发该回调，通知应用程序上传完成。
  - [`onRecordingBackedUp`](../../cn/cloud-recording/cloud_recording_api_java.md)：如果有录制文件未能成功上传至第三方云存储，Agora 服务器会将这部分文件自动上传至 Agora 云备份，当录制结束后触发该回调。同时 Agora 云备份会继续尝试将这部分文件上传至设定的第三方云存储。如果等待五分钟后仍然不能正常[播放录制文件](../../cn/cloud-recording/cloud_recording_onlineplay.md)，请联系 Agora 技术支持。

以上回调中均包含 M3U8 文件名信息。

## 错误代码

SDK 可能会触发以下异常或错误回调：

- `onRecorderFailure`：录制组件异常，通常无需对此进行任何操作。
- `onUploaderFailure`：上传组件异常，通常无需对此进行任何操作。
- `onRecordingFatalError`：发生了不可恢复的错误，可根据具体的错误代码判断录制后台的状态。

具体的错误描述及解决方法请参考[错误代码](../../cn/cloud-recording/cloud_recording_api_java.md)。
