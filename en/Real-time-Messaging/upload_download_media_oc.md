
---
title: Send and Receive Image or File Messages
description: 
platform: iOS,macOS
updatedAt: Wed Sep 30 2020 15:44:20 GMT+0800 (CST)
---
# Send and Receive Image or File Messages
## Introduction

You can use the Agora RTM SDK to send and receive image or file messages. 

The Agora RTM SDK supports sending non-empty images and files that do not exceed 30 MB in size. Each image or file you upload to the Agora server stays for 7 days and has a corresponding media ID. You can download the image or file from the Agora server with the media ID. 

The Agora RTM SDK adds the `AgoraRtmImageMessage` interface and the `AgoraRtmFileMessage` interface to save and transfer media ID. Because the `AgoraRtmImageMessage` interface and the `AgoraRtmFileMessage` interface inherit from the `AgoraRtmMessage` interface, you can use existing peer-to-peer or channel messaging to transfer the `AgoraRtmImageMessage` instance and the `AgoraRtmFileMessage` instance. Thus, you can send and receive image or file messages.

You can use the `AgoraRtmImageMessage` interface to perform the following image operations:

- Set the filename and thumbnail of the uploaded image.
- Get the size of the uploaded image.
- Get the SDK-calculated width and height of images in JPEG, JPG, BMP, and PNG format.
- Set the width and height of images. The width or height you set overwrites SDK-calculated width or height.

You can use the `AgoraRtmFileMessage` interface to perform the following file operations:

- Set the name and thumbnail of the uploaded file.
- Get the size of the uploaded file.

## Implementation

### Send and receive image messages

<div class="alert note">Before you start, ensure that you have integrated the latest SDK into your project. You must also enable peer-to-peer and channel messages.</div>

The following steps show the general process of sending and receiving an image message:

1. Upload an image to the Agora server. When the image is successfully uploaded, the SDK returns an `AgoraRtmImageMessage` instance by callback.

   Sample code for uploading an image to the Agora server:

   ```objectivec
    [AgoraRtm.kit createImageMessageByUploading:imagePath withRequest:&requestId completion:^(long long requestId, AgoraRtmImageMessage *message, AgoraRtmUploadMediaErrorCode errorCode) {
        //deal with message
    }];
   ```

    If you have a corresponding media ID for an image that is still on the Agora server, you can create an `AgoraRtmImageMessage` instance with the following code:

   ```objectiveC
   [AgoraRtm.kit createImageMessageByMediaId:mediaId];
   ```

2. (Optional) Set the width, height, or thumbnail of the image by the `AgoraRtmImageMessage` instance.

    Sample code for setting the width and height of the image:

   ```objectivec
   [message setWidth:imageWidth];
   [message setHeight:imageHeight];
   ```

    Sample code for setting the thumbnail of the image:

   ```objectivec
   message.thumbnailWidth = thumbnailImage.size.width;
   message.thumbnailHeight = thumbnailImage.size.height;
   ```
  
3. Send the `AgoraRtmImageMessage` instance via peer-to-peer or channel messaging to a user or channel. The `AgoraRtmImageMessage` class interface inherits from the `AgoraRtmMessage` class interface, so you can use peer-to-peer or channel messaging to send the `AgoraRtmImageMessage` instance.

   Sample code for sending a peer-to-peer image message:

   ```objectivec
   [AgoraRtm.kit sendMessage:rtmMessage toPeer:self.mode.name sendMessageOptions:option completion:^(AgoraRtmSendPeerMessageErrorCode errorCode) {}];
   ```

   Sample code for sending a channel image message:

   ```objectivec
   [self.rtmChannel sendMessage:rtmMessage completion:^(AgoraRtmSendChannelMessageErrorCode errorCode) {}];
   ```

4. The user who receives the image message can receive the corresponding callback. You can get the media ID from the `AgoraRtmImageMessage` instance and save the image by media ID.

    Sample code for receiving a peer-to-peer image message:

   ```objectivec
   - (void)rtmKit:(AgoraRtmKit *)kit imageMessageReceived:(AgoraRtmImageMessage *)message fromPeer:(NSString *)peerId {
    //deal with message
    }
   ```

   Sample code for receiving a channel image message:

   ```objectivec
   - (void)channel:(AgoraRtmChannel *)channel imageMessageReceived:(AgoraRtmImageMessage *)message fromMember:(AgoraRtmMember *)member {
    //deal with message
    }
   ```
   
   Typically, you can download an image to local storage via media ID.

   ```objectivec
   [AgoraRtm.kit downloadMedia:message.mediaId toFile:filePath withRequest:&requestId completion:^(long long requestId, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   For scenarios that require quick save operations, you can download the image to memory:

   ```objectivec
   [AgoraRtm.kit downloadMediaToMemory:message.mediaId withRequest:&requestId completion:^(long long requestId, NSData *data, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   If you need to cancel uploading or downloading an image, refer to the following example code:

   ```objectivec
   [AgoraRtm.kit cancelMediaUpload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```

   ```objectivec
   [AgoraRtm.kit cancelMediaDownload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```


### Send and receive file messages

<div class="alert note">Before you start, ensure that you have integrated the latest SDK into your project. You must also enable peer-to-peer and channel messages.</div>

The following steps show the general process of sending and receiving a file message:

1. Upload a file to the Agora server. When the file is successfully uploaded, the SDK returns an `AgoraRtmFileMessage` instance by callback.
   
   Sample code for uploading a file to the Agora server:

   ```objectivec
    [AgoraRtm.kit createFileMessageByUploading:filePath withRequest:&requestId completion:^(long long requestId, AgoraRtmFileMessage *message, AgoraRtmUploadMediaErrorCode errorCode) {
        //deal with message
    }];
   ```

    If you have a corresponding media ID for a file that is still on the Agora server, you can create an `AgoraRtmFileMessage` instance with the following code:

   ```objectivec
   [AgoraRtm.kit createFileMessageByMediaId:mediaId];
   ```

2. (Optional) Set the thumbnail of the file by the `AgoraRtmFileMessage` instance.

   ```objectivec
   UIImage *thumbnailImage = [weakSelf generateThumbnail:imagePath toByte:5 * 1024];
   if(thumbnailImage != nil) {
   message.thumbnail = imageData;
   }
   ```

3. Send the `AgoraRtmFileMessage` instance via peer-to-peer or channel messaging to a user or channel. The `AgoraRtmFileMessage` class interface inherits from the `AgoraRtmMessage` class interface, so you can use peer-to-peer or channel messaging to send the `AgoraRtmFileMessage` instance.

   Sample code for sending a peer-to-peer file message:

   ```objectivec
   [AgoraRtm.kit sendMessage:rtmMessage toPeer:self.mode.name sendMessageOptions:option completion:^(AgoraRtmSendPeerMessageErrorCode errorCode) {}];
   ```

   Sample code for sending a channel file message:

   ```objectivec
   [self.rtmChannel sendMessage:rtmMessage completion:^(AgoraRtmSendChannelMessageErrorCode errorCode) {}];
   ```

4. The user who receives the file message can receive the corresponding callback. You can get the media ID from the `AgoraRtmFileMessage` instance and save the file by media ID.

   Sample code for receiving a peer-to-peer file message:

   ```objectivec
   - (void)rtmKit:(AgoraRtmKit *)kit fileMessageReceived:(AgoraRtmFileMessage *)message fromPeer:(NSString *)peerId {
    //deal with message
    }
   ```

   Sample code for receiving a channel file message:

   ```objectivec
   - (void)channel:(AgoraRtmChannel *)channel fileMessageReceived:(AgoraRtmFileMessage *)message fromMember:(AgoraRtmMember *)member {
    //deal with message
    }
   ```
   
   Typically, you can download a file to local storage via media ID.

   ```objectivec
   [AgoraRtm.kit downloadMedia:message.mediaId toFile:filePath withRequest:&requestId completion:^(long long requestId, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   For scenarios that require quick save operations, you can download the file to memory:

   ```objectivec
   [AgoraRtm.kit downloadMediaToMemory:message.mediaId withRequest:&requestId completion:^(long long requestId, NSData *data, AgoraRtmDownloadMediaErrorCode errorCode) {}];
   ```

   If you need to cancel uploading or downloading a file, refer to the following example code:

   ```objectivec
   [AgoraRtm.kit cancelMediaUpload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```
	 
   ```objectivec
   [AgoraRtm.kit cancelMediaDownload:&requestId completion:^(long long requestId, AgoraRtmCancelMediaErrorCode errorCode) {}];
   ```


## Considerations

- You must log in to the Agora RTM system before calling the methods.
- Ensure that the total size of message content, filename, and thumbnail must not exceed 32 KB for each message instance.
- The `filePath` parameter for uploading and downloading methods can only accept strings with UTF-8 format. The path must be an absolute path.
- You can only cancel an ongoing upload or download task. The request ID is no longer valid when the corresponding task completes.
- To save the downloaded file or image to local memory, you must access the corresponding memory location when the `AgoraRtmDownloadMediaToMemoryBlock` callback completes.
- For each client, do not exceed a total of nine upload tasks and download tasks.
- You can set the width and height of the uploaded image by `AgoraRtmImageMessage`. The settings only exist as additional attributes and does not affect the uploaded image. The uploaded image is not resized or cropped.
- The SDK automatically sets width and height for images in JPEG, JPG, BMP, and PNG format. The width or height you set overwrites the SDK-calculated width or height.

## API reference

- [`createFileMessageByUploading:withRequest:completion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createFileMessageByUploading:withRequest:completion:)
- [`createImageMessageByUploading:withRequest:completion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createImageMessageByUploading:withRequest:completion:)
- [`cancelMediaUpload:completion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/cancelMediaUpload:completion:)
- [`cancelMediaDownload:completion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/cancelMediaDownload:completion:)
- [`createFileMessageByMediaId:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createFileMessageByMediaId:)
- [`createImageMessageByMediaId:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createImageMessageByMediaId:)
- [`downloadMediaToMemory:withRequest:completion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/downloadMediaToMemory:withRequest:completion:)
- [`downloadMedia:toFile:withRequest:completion:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/downloadMedia:toFile:withRequest:completion:)
- [`rtmKit:media:uploadingProgress:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:media:uploadingProgress:)
- [`rtmKit:media:downloadingProgress:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:media:downloadingProgress:)
- [`AgoraRtmUploadFileMediaBlock`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmUploadFileMediaBlock.html)
- [`AgoraRtmUploadImageMediaBlock`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmUploadImageMediaBlock.html)
- [`AgoraRtmCancelMediaBlock`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmCancelMediaBlock.html)
- [`AgoraRtmDownloadMediaToMemoryBlock`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmDownloadMediaToMemoryBlock.html)
- [`AgoraRtmDownloadMediaToFileBlock`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Blocks/AgoraRtmDownloadMediaToFileBlock.html)
- [`rtmKit:fileMessageReceived:fromPeer:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:fileMessageReceived:fromPeer:)
- [`rtmKit:imageMessageReceived:fromPeer:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmDelegate.html#//api/name/rtmKit:imageMessageReceived:fromPeer:)
- [`channel:fileMessageReceived:fromMember:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:fileMessageReceived:fromMember:)
- [`channel:imageMessageReceived:fromMember:`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_oc/Protocols/AgoraRtmChannelDelegate.html#//api/name/channel:imageMessageReceived:fromMember:)
