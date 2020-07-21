
---
title: 发送和接收图片或文件消息
description: 
platform: iOS,macOS
updatedAt: Tue Jul 21 2020 06:49:08 GMT+0800 (CST)
---
# 发送和接收图片或文件消息
## 功能描述

你可以使用 Agora RTM SDK 发送和接收图片或文件消息。

Agora RTM SDK 支持发送大小不超过 30 MB 的任意文件格式的非空图片或文件。每份上传到 Agora 服务器的图片或文件都对应一个 media ID，在服务端保存 7 天。你可以通过 media ID 在 7 天有效期内从 Agora 服务器下载对应的图片或文件。

Agora RTM SDK 引入了 `AgoraRtmImageMessage` 接口类和 `AgoraRtmFileMessage` 接口类用于保存和传递系统生成的 media ID。`AgoraRtmImageMessage` 接口类和 `AgoraRtmFileMessage` 接口类继承自 `AgoraRtmMessage` 接口类，所以你可以通过已有的点对点消息或频道消息发送方法传递 `AgoraRtmImageMessage` 实例和 `AgoraRtmFileMessage` 实例，从而实现图片或文件消息的发送和接收。

你可以通过 `AgoraRtmImageMessage` 对象对图片进行以下操作：

- 设置相应的上传图片的显示文件名和显示缩略图。
- 获取相应的上传图片的大小。
- 获取由 SDK 计算的 JPEG、JPG、BMP，以及 PNG 这四种格式的图片的宽高数据。
- 自行设置图片的宽和高。你自行设置的宽高数据会覆盖 SDK 计算的宽高数据。

你可以通过 `AgoraRtmFileMessage` 对象对文件进行以下操作：

- 设置相应的上传文件的显示文件名和显示缩略图。
- 获取相应的上传文件的大小。

## 实现方法

### 发送和接收图片消息

<div class="alert note">开始前请确保你已集成最新版的 Agora RTM SDK 到你的项目中，而且已实现点对点消息和频道消息功能。</div>

一般情况下的发送和接收图片消息流程如下：

1. 上传图片到 Agora 服务器。图片上传成功时，SDK 会通过回调返回一个 `AgoraRtmImageMessage` 实例。

   上传图片到 Agora 服务器：

   ```objectivec
    [AgoraRtm.kit createImageMessageByUploading:imagePath withRequest:&requestId completion:^(long long requestId, AgoraRtmImageMessage *message, AgoraRtmUploadMediaErrorCode errorCode) {
        //deal with message
    }];
   ```

   如果你存有一个已上传图片对应的 media ID 且 media ID 仍然处于 7 天有效期内，可通过如下代码直接创建一个 `AgoraRtmImageMessage` 实例:

   ```objectiveC
   [AgoraRtm.kit createImageMessageByMediaId:mediaId];
   ```

2. （可选）通过获取的实例设置图片的长宽或缩略图。
   
   设置图片长宽：

   ```objectivec
   [message setWidth:imageWidth];
   [message setHeight:imageHeight];
   ```

   设置图片缩略图：

   ```objectivec
   message.thumbnailWidth = thumbnailImage.size.width;
   message.thumbnailHeight = thumbnailImage.size.height;
   ```

3. 将 `IImageMessage` 实例通过点对点消息或频道消息的方式发送给指定用户或指定频道。

   发送图片点对点消息：

   ```objectivec
   [AgoraRtm.kit sendMessage:rtmMessage toPeer:self.mode.name sendMessageOptions:option completion:^(AgoraRtmSendPeerMessageErrorCode errorCode) {}];
   ```
   
   发送图片频道消息：

   ```objectivec
   [self.rtmChannel sendMessage:rtmMessage completion:^(AgoraRtmSendChannelMessageErrorCode errorCode) {}];
   ```


4. 收到图片消息的用户会收到相应回调，你可以通过获取 `AgoraRtmImageMessage` 实例携带的 media ID 信息并通过 media ID 将相应图片保存至本地。

   接收图片点对点消息：

   ```objectivec
   - (void)rtmKit:(AgoraRtmKit *)kit imageMessageReceived:(AgoraRtmImageMessage *)message fromPeer:(NSString *)peerId {
    //deal with message
    }
   ```

   接收图片频道消息：

   ```objectivec
   - (void)channel:(AgoraRtmChannel *)channel imageMessageReceived:(AgoraRtmImageMessage *)message fromMember:(AgoraRtmMember *)member {
    //deal with message
    }
   ```

   一般情况下，你可以通过 media ID 直接将图片下载至本地存储：

   ```objectivec
   [AgoraRtm.kit downloadMedia:message.mediaId toFile:filePath withRequest:&requestId completion:^(long long requestId, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   对于需要快速存取已下载图片的场景，你可以将图片下载至本地内存：

   ```objectivec
   [AgoraRtm.kit downloadMediaToMemory:message.mediaId withRequest:&requestId completion:^(long long requestId, NSData *data, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   如果你要取消上传或下载图片，参考以下示例代码：

   ```objectivec
   [AgoraRtm.kit cancelMediaUpload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```
	 
	 ```objectivec
   [AgoraRtm.kit cancelMediaDownload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```

### 发送和接收文件消息

<div class="alert note">开始前请确保你已集成最新版的 Agora RTM SDK 到你的项目中，而且已实现点对点消息和频道消息功能。</div>

一般情况下的发送和接收文件消息流程如下：

1. 上传文件到 Agora 服务器。文件上传成功时，SDK 会通过回调返回一个 `AgoraRtmFileMessage` 实例。

   上传文件到 Agora 服务器：

   ```objectivec
    [AgoraRtm.kit createFileMessageByUploading:filePath withRequest:&requestId completion:^(long long requestId, AgoraRtmFileMessage *message, AgoraRtmUploadMediaErrorCode errorCode) {
        //deal with message
    }];
   ```

   如果你存有一个已上传文件对应的 media ID 且 media ID 仍然处于 7 天有效期内，可通过如下代码直接创建一个 `AgoraRtmFileMessage` 实例:

   ```objectivec
   [AgoraRtm.kit createFileMessageByMediaId:mediaId];
   ```

2. （可选）通过获取的实例设置文件的缩略图。

   ```objectivec
   UIImage *thumbnailImage = [weakSelf generateThumbnail:imagePath toByte:5 * 1024];
   if(thumbnailImage != nil) {
   message.thumbnail = imageData;
   }
   ```

3. 将 `AgoraRtmFileMessage` 实例通过点对点消息或频道消息的方式发送给指定用户或指定频道。

   发送文件点对点消息：

   ```objectivec
   [AgoraRtm.kit sendMessage:rtmMessage toPeer:self.mode.name sendMessageOptions:option completion:^(AgoraRtmSendPeerMessageErrorCode errorCode) {}];
   ```

   发送文件频道消息：

   ```objectivec
   [self.rtmChannel sendMessage:rtmMessage completion:^(AgoraRtmSendChannelMessageErrorCode errorCode) {}];
   ```

4. 收到文件消息的用户会收到相应回调，你可以通过获取 `AgoraRtmFileMessage` 实例携带的 media ID 信息并通过 media ID 将相应文件保存至本地。
	 
   接收文件点对点消息：

   ```objectivec
   - (void)rtmKit:(AgoraRtmKit *)kit fileMessageReceived:(AgoraRtmFileMessage *)message fromPeer:(NSString *)peerId {
    //deal with message
    }
   ```

   接收文件频道消息：

   ```objectivec
   - (void)channel:(AgoraRtmChannel *)channel fileMessageReceived:(AgoraRtmFileMessage *)message fromMember:(AgoraRtmMember *)member {
    //deal with message
    }
   ```

   一般情况下，你可以通过 media ID 直接将文件下载至本地存储：

   ```objectivec
   [AgoraRtm.kit downloadMedia:message.mediaId toFile:filePath withRequest:&requestId completion:^(long long requestId, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   对于需要快速存取已下载文件的场景，你可以将文件下载至本地内存：

   ```objectivec
   [AgoraRtm.kit downloadMediaToMemory:message.mediaId withRequest:&requestId completion:^(long long requestId, NSData *data, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   如果你要取消上传或下载文件，参考以下示例代码：

   ```objectivec
   [AgoraRtm.kit cancelMediaUpload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```
	 
	 ```objectivec
   [AgoraRtm.kit cancelMediaDownload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```

## 注意事项

- 你必须在成功登录 Agora RTM 系统后才能调用本功能相关的方法。
- 请确保每个消息实例的消息内容、显示文件名和显示缩略图的总大小不得超过 32 KB。
- 上传下载相关所有方法涉及的 `filePath` 参数必须是 UTF-8 编码格式字符串，而且必须是绝对路径。
- 你只能取消正在进行中的上传或下载任务。上传或下载任务结束后，对应的 request ID 将不再有效。
- 如需将下载文件或图片存储至本地内存，你必须在 `AgoraRtmDownloadMediaToMemoryBlock` 回调结束后访问相应内存地址。
- 每个客户端实例支持同时进行最多 9 个上传或下载任务（上传和下载任务一并计算）。
- 你可以通过 `AgoraRtmImageMessage` 自行设置上传图片的宽和高。设置内容只作为上传图片的附加属性存在，不影响上传图片本身内容。SDK 也不会对上传图片进行裁剪或缩放。
- SDK 会自动为 JPEG、JPG、BMP、PNG 四种格式的图片计算宽高。你自行设置的宽高数据将覆盖 SDK 计算的图片宽高。

## API 参考

- [`createFileMessageByUploading:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createFileMessageByUploading:withRequest:completion:)
- [`createImageMessageByUploading:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createImageMessageByUploading:withRequest:completion:)
- [`cancelMediaUpload:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/cancelMediaUpload:completion:)
- [`cancelMediaDownload:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/cancelMediaDownload:completion:)
- [`createFileMessageByMediaId:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createFileMessageByMediaId:)
- [`createImageMessageByMediaId:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createImageMessageByMediaId:)
- [`downloadMediaToMemory:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/downloadMediaToMemory:withRequest:completion:)
- [`downloadMedia:toFile:withRequest:completion:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/downloadMedia:toFile:withRequest:completion:)
- [`rtmKit:media:uploadingProgress:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:media:uploadingProgress:)
- [`rtmKit:media:downloadingProgress:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:media:downloadingProgress:)
- [`AgoraRtmUploadFileMediaBlock`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmUploadFileMediaBlock.html)
- [`AgoraRtmUploadImageMediaBlock`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmUploadImageMediaBlock.html)
- [`AgoraRtmCancelMediaBlock`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmCancelMediaBlock.html)
- [`AgoraRtmDownloadMediaToMemoryBlock`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmDownloadMediaToMemoryBlock.html)
- [`AgoraRtmDownloadMediaToFileBlock`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmDownloadMediaToFileBlock.html)
- [`rtmKit:fileMessageReceived:fromPeer:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:fileMessageReceived:fromPeer:)
- [`rtmKit:imageMessageReceived:fromPeer:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:imageMessageReceived:fromPeer:)
- [`channel:fileMessageReceived:fromMember:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:fileMessageReceived:fromMember:)
- [`channel:imageMessageReceived:fromMember:`](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:imageMessageReceived:fromMember:)



