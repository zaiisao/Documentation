
---
title: 云端录制集成常见问题
description: Cloud recording faq
platform: All Platforms,Linux,Linux C++,Linux Java
updatedAt: Thu Oct 31 2019 11:36:45 GMT+0800 (CST)
---
# 云端录制集成常见问题
## 如何获取 M3U8 文件地址

完整的 M3U8 文件地址由云存储空间外链域名和 M3U8 文件名组成，一般在你的第三方云存储里可以直接复制。

![](https://web-cdn.agora.io/docs-files/1561621201492)

你可以通过以下方式获得 M3U8 文件名信息：

- RESTful API
  - [`query`](../../cn/cloud-recording/cloud_recording_api_rest.md) 和 [`stop`](../../cn/cloud-recording/cloud_recording_api_rest.md) 的响应中 `fileList` 字段
  - 回调事件 [`cloud_recording_file_infos`](../../cn/cloud-recording/cloud_recording_callback_rest.md) 的 `fileList` 字段
- C++
  - [`OnRecordingUploadingProgress`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `recording_playlist_filename` 参数
  - [`OnRecordingUploaded`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `file_name` 参数
  - [`OnRecordingBackedUp`](../../cn/cloud-recording/cloud_recording_api.md) 中的 `file_name` 参数
- Java
  - [`onRecordingUploadingProgress`](../../cn/cloud-recording/cloud_recording_api_java.md) 中的 `recording_playlist_filename` 参数
  - [`onRecordingUploaded`](../../cn/cloud-recording/cloud_recording_api_java.md) 中的 `file_name` 参数
  - [`onRecordingBackedUp`](../../cn/cloud-recording/cloud_recording_api_java.md) 中的 `file_name` 参数

## 如何停止云端录制

你可以调用  [`stop`](../../cn/faqs/cloud_recording_api_rest.md) 方法离开频道，停止录制。

当频道空闲（无用户）超过预设的时间（默认 30 秒）后，云端录制也会自动退出频道停止录制。你可以在开始录制的时候设置 `maxIdleTime` 参数来控制超时退出的时间。

## 上传到第三方云存储失败

上传到第三方云存储失败，可能有以下几种原因：
  - 没有发流端加入频道，录制超时。
  - Token 过期或 Token 认证失败。
  - 在调用[获取云端录制资源的 API](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#a-nameacquirea%E8%8E%B7%E5%8F%96%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6%E8%B5%84%E6%BA%90%E7%9A%84-api) 时，传入的 `uid` 参数与频道内现有的用户 ID 重复。举例来说，频道内有三个用户，UID 分别为 `123`，`234`，和 `345`，如果你传入的 `uid` 为 `123`， 则会导致录制失败。
  - 在调用[开始云端录制的 API](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#a-namestarta开始云端录制的-api) 时，传入的 `transcodingConfig` 参数值不合理，导致录制失败。请参考[如何设置录制视频的分辨率](https://docs.agora.io/cn/faq/recording_video_profile)设置该参数。
  - 你的云存储配置出错。请确保你的云存储配置填写正确：
    - bucket：云存储空间名称， 由你自己在云账户下创建。
    - accessKey：在云存储个人账户下面密钥管理里。
    - secretKey ：在云存储个人账户下面密钥管理里。

## 101 错误

日志文件中报错 `"ErrorUint32":101`，一般为 Token 错误引起，可能有以下几种原因：

- Token 填写错误
- Token 过期
- Native/Web SDK 使用了 Token，云端录制未使用
- 云端录制使用了 Token，Native/Web SDK 未使用

## 录制异常退出

录制异常退出意味着集成云端录制 SDK 的 App 崩溃，但是只要通话或直播还在继续，Agora 云端录制服务仍会继续保持录制和上传。这时使用和之前录制相同的 App ID、频道名以及 UID 重新开始录制，就可以继续控制原来的录制实例。


## 为什么无法通过浏览器调用云端录制 RESTful API
要使用云端录制 RESTful API，Web API 需要发送跨域请求。根据 CORS 规范，浏览器针对跨域请求会先发送一个 OPTIONS 请求，查询服务器是否允许跨域请求，然后才有可能发起真正的 POST 请求。但是由于云端录制 RESTful API 不支持 OPTIONS 方法，所以无法支持 Web API 调用的方式。

## Java 集成报错

### Java SDK 集成中常见的错误及解决方法

#### java.land.UnsatisfiedLinkError: no agora-cloud-recording-java in java.library.path

##### **报错原因**

系统找不到库文件 `libagora-cloud-recording-java.so`。

##### **解决方法**

1. 打印  `java.library.path`，查看 Linux 环境变量 `LD_LIBRARY_PATH` 是否配置了该库文件：

   ```bash
   System.out.println(System.getProperty("java.library.path"))
   ```

2. 建议在 Linux 系统变量 `LD_LIBRARY_PATH` 中添加 `java.library.path`，可以在 **/etc/profile** 里面添加 `LD_LIBRARY_PATH` 或者把 `libagora-cloud-recording-java.so` 直接放在Linux 系统目录 **/usr/lib** 下面。

#### java.land.NoClassDefFoundError: Could not initialize class io.agora.cloud_recording.CloudRecorder

##### **报错原因**

`classpath` 里面没有加载 `agora-cloud-recording-sdk.jar`。集成 Java SDK 时，需要加载 `agora-cloud-recording-sdk.jar` 和  `libagora-cloud-recording-java.so` ，jar 文件放在 Linux 环境变量 `classpath` 下，库文件放在 Linux 环境变量 `LD_LIBRARY_PATH` 下。

##### **解决方法**

1. 打印 `classpath` ，查看是否加载了`agora-cloud-recording-sdk.jar`。

```bash
System.out.println("类所在的路径：" + System.getProperty("java.class.path"));
```

2. 如果没有，就在 `classpath` 环境变量中添加 jar 路径。
