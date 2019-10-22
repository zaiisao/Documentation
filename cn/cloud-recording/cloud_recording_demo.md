
---
title: Demo 使用指南 (Deprecated)
description: 
platform: Linux CPP
updatedAt: Tue Oct 22 2019 01:22:17 GMT+0800 (CST)
---
# Demo 使用指南 (Deprecated)
<div class="alert note">云端录制 SDK 停服计划：<li>从 11月 15 日起，云端录制 SDK 进入停止维护阶段，但你仍可使用云端录制 SDK。</li><li>从 12 月 15 日起，云端录制 SDK 将停止服务。停止服务后，你将无法使用云端录制 SDK。</li></div>

本文介绍如何通过命令行运行 C++ demo 进行录制。 你也可以通过调用 API 实现录制，详见[调用 API 录制](../../cn/cloud-recording/cloud_recording_quickstart.md)。无论是使用命令行，还是调用 API，实现的都是相同的功能，你可以根据个人习惯选择其中一种方式。

## 前提条件

请确保满足以下要求：

- 打开 TCP 端口：4447、8913、9700。
- 打开 UDP 端口：53、4444。
- 运行环境（物理或虚拟）：
  - Ubuntu Linux 14.04+ LTS 64 位
  - CentOS 6.5+（推荐 7.0）64 位
- 安装编译器 g++ 4.8 或以上版本。
- 联系 [sales@agora.io](mailto:sales@agora.io) 开通云端录制服务。
- 开通第三方云存储服务，目前支持[七牛云](https://www.qiniu.com/products/kodo)，[阿里云](https://www.aliyun.com/product/oss)和 [Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1)。

## 运行 Demo

1. 下载[云端录制 SDK](http://download.agora.io/acrsdk/release/Agora_Cloud_Recording_SDK_for_Linux_v1_0_0_FULL.tar.gz) 并解压缩。
2. 打开命令行工具，进入 SDK 包中 **cloud_recording_demo.bin** 所在目录，执行 `./cloud_recording_demo.bin` 命令，即可看到录制相关的参数和选项，如下所示：

 ```bash
Usage: 
  ./cloud_recording_demo.bin --appId STRING --channelName STRING --uid UINTEGER32 --token STRING --recordingStreamType UINTEGER32 --videoStreamType UINTEGER32 --decryption_mode UINTEGER32 --secret STRING --channelType UINTEGER32 --audioProfile UINTEGER32 --mixWidth UINTEGER32 --mixHeight UINTEGER32 --fps UINTEGER32 --bitrate UINTEGER32 --maxResolutionUid UINTEGER32 --mixedVideoLayoutType UINTEGER32 --maxIdleTime UINTEGER32 --vendor UINTEGER32 --region UINTEGER32 --bucket STRING --accessKey STRING --secretKey STRING 
                             --appId     (app id)
                             --channelName     (channel name)
                             --uid     (User id)
                             --token     (channel token/Optional)
                             --recordingStreamType     (stream types (Optional: 0 for audio only,  1 for video only, 2 for audio and video, default 2))
                             --videoStreamType     (video stream type(Optional: 0 for high video, 1 for low video, default 0. Only works when recordingStreamType is not 0.))
                             --decryption_mode     (decryption mode(Optional, 0 for none, 1 for aes-128-xts,2 for aes-128-ecb, 3 for aes-256-xts, no decryption by default.))
                             --secret     (secret(Optional, emptry by default))
                             --channelType     (channel type (Optional 0 for communication, 1 for live, default 0))
                             —audioProfile     (audio profile, (Optional 0 for single channel mono, 1 for single channel music, 2 for multi channel music. default 0))
                             —mixWidth     (transcoding mixing width (Optional, defualt 640))
                             —mixHeight     (transcoding mixing height (Optional, default 360))
                             —fps     (transcoding fps(Optional, default 15))
                             —bitrate     (transcoding bitrate(Optional, 500 by default))
                             —maxResolutionUid     (transcoding max resolution uid(Optional))
                             —mixedVideoLayoutType     (mixed video layout mode(Optional, 0 for float, 1 for BestFit, 2 for vertical, 0 by defualt))
                             —maxIdleTime     (max idle time(Optional, 30 by default))
                             —vendor     (cloud storage vendor(0 for Qiniu, 1 for AWS, 2 for Aliyun))
                             —region     (cloud storage region)
                             —bucket     (cloud storage bucket)
                             —accessKey     (cloud storage access key)
                             —secretKey     (cloud storage secret key)
```

## 开始云端录制

按照你的需要在命令行中输入以下参数的设置并执行，即可开始云端录制服务。

命令行示例：
```bash
$ ./cloud_recording_demo.bin --appId <你的app ID> --channelName <频道名> --uid <录制uid> --vendor 0 --region 0 --bucket <云空间名称> --accessKey <accesskey> --secretKey <secretKey> --channelType <0 通信模式，1 直播模式>
```

> - `uid` 不可以设为 0。
> - `appId`，`uid`，`channelName` ，`vendor`， `region`，`bucket`，`accessKey` 和 `secretKey` 这几个参数必须设置，若不设置则无法录制，其他参数可根据需要自行选择是否设置，若不设置则使用默认值。
> - 如果 Agora Native/Web SDK 使用了 Token，必须设置 `token` 参数。
> - 当云端录制 SDK 加入频道时，相当于一个哑客户端加入频道，因此需要跟 Agora Native/Web SDK 加入相同的频道，并使用相同的 App ID 和频道模式。

| 参数                 | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| appId                | 待录制频道的 App ID，必须和 Agora Native/Web SDK 的 App ID 一致，详见<a href="../../cn/cloud-recording/token.md">获取 App ID</a>。 |
| channelName          | 待录制频道的频道名。                                         |
| uid                  | 云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，不可设置为 0，需保证唯一性。 |
| token                | 待录制的频道中使用的 token，详见<a href="../../cn/cloud-recording/token.md">校验用户权限</a>。 |
| recordingStreamType  | 录制的媒体流类型：<li>0：仅录制音频。<li>1：仅录制视频。<li>2：录制音频和视频（默认）。 |
| videoStreamType      | 视频流类型。<li>0：视频大流（默认），即高分辨率高码率的视频流。<li>1：视频小流，即低分辨率低码率的视频流。 |
| decryption_mode      | 解密方案。云端录制 SDK 可以启用内置的解密功能。解密方式必须与频道设置的加密方式一致。<li>0：无（默认）。<li>1：设置 AES128XTS 解密方案。<li>2：设置 AES128ECB 解密方案。<li>3：设置 AES256XTS 解密方案。 |
| secret               | 解密密码，默认为空。仅当 decryption_mode 不为 0 时有效。     |
| channelType          | 频道模式。<li>0：通信模式（默认），即常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话。 <li>1：直播模式，有两种用户角色，主播和观众，只有主播可以自由发言。 <p>**频道模式必须与 Agora Native/Web SDK 一致，否则可能导致问题。** |
| audioProfile         | 设置音频采样率，码率，编码模式和声道数。<li>0：指定 48 KHz采样率，音乐编码, 单声道，编码码率约 48 Kbps（默认）。 <li>1：指定 48 KHz 采样率，音乐编码, 单声道，编码码率约 128 Kbps。<li>2：指定 48 KHz 采样率，音乐编码, 双声道，编码码率约 192 Kbps。 |
| mixWidth             | 合流宽度，单位 pixels，默认值为 360。详细设置请参考<a href="../../cn/cloud-recording/cloud_recording_api.md">分辨率、帧率、码率对照表</a>。 |
| mixHeight            | 合流高度，单位 pixels，默认值为 640。详细设置请参考<a href="../../cn/cloud-recording/cloud_recording_api.md">分辨率、帧率、码率对照表</a>。 |
| fps                  | 视频帧率，单位 fps，默认值为 15。详细设置请参考<a href="../../cn/cloud-recording/cloud_recording_api.md">分辨率、帧率、码率对照表</a>。 |
| bitrate              | 视频码率，单位 Kbps，默认值为 500。详细设置请参考<a href="../../cn/cloud-recording/cloud_recording_api.md">分辨率、帧率、码率对照表</a>。 |
| maxResolutionUid     | 如果 `mixedVideoLayoutType` 设为垂直布局，用该参数设置显示大流画面的用户 ID。 |
| mixedVideoLayoutType | 视频合流布局，详见[设置合流布局](../../cn/cloud-recording/cloud_layout_guide.md)。<li>0：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。<li>1：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。<li>2：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。 |
| maxIdleTime          | 最长空闲频道时间。默认值为 30 秒，该值需大于等于 5。如果频道内无用户的状态持续超过该时间，录制程序会自动退出。 |
| vendor               | 第三方云存储：<li>0：[七牛云](https://www.qiniu.com/products/kodo)。<li>1：[Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1)。<li>2：[阿里云](https://www.aliyun.com/product/oss)。 |
| `region`             | 第三方云存储指定的地区信息。<br>当 `vendor` = 0，即第三方云存储为[七牛云](https://www.qiniu.com/products/kodo)时：<li>0：Huadong <li>1：Huabei <li>2：Huanan <li>3：Beimei  <br>当 `vendor` = 1，即第三方云存储为 [Amazon S3](https://aws.amazon.com/cn/s3/?nc2=h_m1) 时：<li>0：US_EAST_1 <li>1：US_EAST_2 <li>2：US_WEST_1 <li>3：US_WEST_2  <li>4：EU_WEST_1 <li> 5：EU_WEST_2  <li>6：EU_WEST_3 <li>7：EU_CENTRAL_1 <li>8：AP_SOUTHEAST_1 <li>9：AP_SOUTHEAST_2 <li>10：AP_NORTHEAST_1 <li>11：AP_NORTHEAST_2 <li>12：SA_EAST_1 <li>13：CA_CENTRAL_1 <li>14：AP_SOUTH_1 <li>15：CN_NORTH_1 <li>16：CN_NORTHWEST_1 <li>17：US_GOV_WEST_1 <br>当 `vendor` = 2，即第三方云存储为[阿里云](https://www.aliyun.com/product/oss)时：<li>0：CN_Hangzhou <li>1：CN_Shanghai <li>2：CN_Qingdao <li>3：CN_Beijin  <li>4：CN_Zhangjiakou <li> 5：CN_Huhehaote  <li>6：CN_Shenzhen <li>7：CN_Hongkong <li>8：US_West_1 <li>9：US_East_1 <li>10：AP_Southeast_1 <li>11：AP_Southeast_2 <li>12：AP_Southeast_3 <li>13：AP_Southeast_5 <li>14：AP_Northeast_1 <li>15：AP_South_1 <li>16：EU_Central_1 <li>17：EU_West_1 <li>18：EU_East_1 |
| bucket               | 第三方云存储 bucket。                                        |
| accessKey            | 第三方云存储 access key。                                    |
| secretKey            | 第三方云存储 secret key。                                    |

## 停止云端录制

当频道空闲（无用户）超过预设时间（默认为 30 秒）后，demo 会自动退出频道停止录制。

## 上传和管理录制文件

开始录制后，Agora 服务器会将录制内容切片为多个 TS 文件并不断上传至预先设定的第三方云存储，直至录制结束。

### 录制 ID

录制 ID 是录制的唯一标识，每次录制对应一个录制 ID。

录制开始后，命令行返回的所有回调中都会包含录制 ID 和录制频道名，用下划线隔开，如下图所示：
	
![](https://web-cdn.agora.io/docs-files/1556079007410)

### 录制文件索引

每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。

M3U8 文件名由录制 ID 和频道名组成，如 `recording_id_channelName.M3U8`。

### 上传录制文件

录制文件的上传由 Agora 服务器自动完成，你需要关注下面的回调。

- 录制过程中
  - `OnRecordingUploadingProgress`：录制开始后约每分钟触发一次，该回调中的 `progress` 参数表示已上传文件占已录制文件的百分比。
- 停止录制后，根据上传情况，会触发以下回调之一：
  - `OnRecordingUploaded`：如果录制文件全部成功上传至预先设定的云存储，最后一个录制切片文件上传完成时触发该回调，通知应用程序上传完成。
  - `OnRecordingBackedUp`：如果有录制文件未能成功上传至第三方云存储，Agora 服务器会将这部分文件自动上传至 Agora 云备份，当录制结束后会触发该回调。

以上回调中均包含 M3U8 文件名信息。

录制文件上传完成后，你可以在预先设定的第三方云存储空间查看，如下图所示：

![](https://web-cdn.agora.io/docs-files/1556078908213)

## 查看日志文件

运行 demo 进行云端录制后，会在 **$(pwd)/cloud_recording_log** 文件夹中生成以录制 ID 命名的 .log 日志文件，该文件可以帮助查找问题原因。

## 状态信息反馈

在运行 demo 进行云端录制的过程中，命令行会出现如下信息提示当前运行状态。

### 正常状态

| 状态信息                     | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| OnRecordingConnecting        | 应用程序正在连接云端录制服务器。                             |
| OnRecordingStarted           | 应用程序已成功开始云端录制。                                 |
| OnRecordingStopped           | 应用程序已成功结束云端录制。                                 |
| OnRecordingUploaded          | 录制文件已成功上传到预设的第三方云存储。                     |
| OnRecordingBackedup          | 录制文件成功上传到 Agora 云备份。当录制文件上传至预设的第三方云存储失败时，Agora 云端录制服务器会自动备份录制文件，避免文件丢失。 |
| OnRecordingUploadingProgress | 录制文件的上传进度，包括录制 ID，上传进度和录制产生的 M3U8 文件名等信息。 |
| Recorder exit.Wait uploading | 频道录制结束，录制数据等待上传。                             |
| Stopped                      | 本次录制会话结束。                                           |

### 异常状态


| 状态信息                                            | 描述                         |
| --------------------------------------------------- | ---------------------------- |
| Cloud recording recorder init fail.                 | 录制组件初始化失败。         |
| Cloud recording recorder failed.                    | 录制组件异常。               |
| Cloud recording uploader init fail.                 | 上传组件初始化异常。         |
| Cloud recording uploader failed.                    | 上传组件异常。               |
| Cloud recording connection lost.                    | 与云端录制服务器的连接中断。 |
| Recording parameter not right, please have a check. | 参数不合法。                 |
| Current operation is not supported.                 | 不支持当前操作。             |
| Can't connect to cloud recording.                   | 连接至云端录制服务器失败。   |
| No recorded data.                                   | 未生成录制数据。             |
| Cloud recording backup failed.                      | 云备份异常。                 |
| No users in channel.                                | 频道内无用户                 |
| Error occur.                                        | 其他错误。                   |
