
---
title: Send and Receive Image or File Messages
description: 
platform: Windows CPP,Linux CPP
updatedAt: Wed Sep 30 2020 15:45:01 GMT+0800 (CST)
---
# Send and Receive Image or File Messages
## Introduction

You can use Agora RTM SDK to send and receive image or file messages. 

The RTM SDK supports sending non-empty images and files that do not exceed 30 MB in size. Each image or file you upload to the Agora server stays for seven days and has a corresponding media ID. You can download the image or file from the Agora server with the media ID. 

The RTM SDK adds the `IImageMessage` class and the `IFileMessage` class to save and transfer media ID. Because the `IImageMessage` class and the `IFileMessage` class inherit from the `IMessage` class, you can use existing peer-to-peer or channel messaging to transfer the `IImageMessage` instance and the `IFileMessage` instance. Therefore, you can send and receive image or file messages.

You can use the `IImageMessage` class to perform the following image operations:

- Set the filename and thumbnail of the uploaded image.
- Get the size of the uploaded image.
- Get the SDK-calculated width and height of images in JPEG, JPG, BMP, and PNG format.
- Set the width and height of images. The width or height you set overwrites SDK-calculated width or height.

You can use the `IFileMessage` class to perform the following file operations:

- Set the name and thumbnail of the uploaded file.
- Get the size of the uploaded file.

## Implementation

### Send and receive image messages

<div class="alert note">Before you start, ensure that you have integrated the latest SDK into your project. You must also enable peer-to-peer and channel messages.</div>

The following steps show the general process of sending and receiving an image message:

1. Upload an image to the Agora server. When the image upload succeeds, the SDK returns an `IImageMessage` instance by callback.

   Sample code for uploading an image to the Agora server:

    ```cpp
    m_rtmService->createImageMessageByUploading(filePath.c_str(), requestId);
    ```

    If you have a corresponding media ID for a image that is still on the Agora server, you can create an `IImageMessage` instance with the following code:

    ```cpp
    m_rtmService->createImageMessageByMediaId(mediaId.c_str());
    ```

2. (Optional) Set the width, height, or thumbnail of the image by the `IImageMessage` instance.

    Sample code for setting the width and height of the image:

    ```cpp
    imageMessage->setWidth(imageWidth);
    imageMessage->setWidth(imageHeight);
    ```

    Sample code for setting the thumbnail of the image:

    ```cpp
    imageMessage->setThumbnailWidth(thumbWidth);
    imageMessage->setThumbnailHeight(thumbHeight);
    ```
  
3. Send the `IImageMessage` instance via peer-to-peer or channel messaging to a user or channel. The `IImageMessage` class inherits from the `IMessage` class, so you can use peer-to-peer or channel messaging to send the `IImageMessage` instance.

   Sample code for sending a peer-to-peer image message:

   ```cpp
   m_rtmService->sendMessageToPeer(account.c_str(), imageMessage, options);
   ```

   Sample code for sending a channel image message:

   ```cpp
   m_Channel->sendMessage(imageMessage, options);
   ```

4. The user who receives the image message can receive the corresponding callback. You can get the media ID from the `IImageMessage` instance and save the image by media ID.

    Sample code for receiving a peer-to-peer image message:

    ```cpp
    // CRTMCallBack extend from IRtmServiceEventHandler
    void CRTMCallBack::onImageMessageReceivedFromPeer(const char *peerId, const IImageMessage* message)
    {
        if(message){
            //deal with message
        }
    }
    ```

   Sample code for receiving a channel image message:

    ```cpp
    // CRTMCallBack extend from IChannelEventHandler
    void CRTMCallBack::onImageMessageReceived(const char *userId, const IImageMessage* message)
    {
        if(message){
            //deal with message
        }
    }
    ```
   
   Typically, you can download an image to local storage via media ID.

   ```cpp
   m_rtmService->downloadMediaToFile(mediaId.c_str(), filePath.c_str(), requestId);
   ```

   For scenarios that require quick save operations, you can download the image to memory:

   ```cpp
   m_rtmService->downloadMediaToMemory(mediaId.c_str(), long long &requestId);
   ```

   If you need to cancel uploading or downloading an image, refer to the following example code:

   ```cpp
   m_rtmService->cancelMediaUpload(requestId);
   ```
	 
   ```cpp
   m_rtmService->cancelMediaDownload(requestId);
   ```

### Send and receive file messages

<div class="alert note">Before you start, ensure that you have integrated the latest SDK into your project. You must also enable peer-to-peer and channel messages.</div>

The following steps show the general process of sending and receiving a file message:

1. Upload a file to the Agora server. When the file is successfully uploaded, the SDK returns an `IFileMessage` instance by callback.
   
   Sample code for uploading a file to the Agora server:

    ```cpp
    m_rtmService->createFileMessageByUploading(filePath.c_str(), requestId);
    ```

    If you have a corresponding media ID for a file that is still on the Agora server, you can create an `IFileMessage` instance with the following code:

    ```cpp
    m_rtmService->createFileMessageByMediaId(mediaId.c_str());
    ```

2. (Optional) Set the thumbnail of the file by the `IFileMessage` instance.

    ```cpp
    imageMessage->setThumbnail(imageData, imageSize);
    ```

3. Send the `IFileMessage` instance via peer-to-peer or channel messaging to a user or channel. The `IFileMessage` class inherits from the `IMessage` class, enabling you to use peer-to-peer or channel messaging to send the `IFileMessage` instance.

   Sample code for sending a peer-to-peer file message:

    ```cpp
    m_rtmService->sendMessageToPeer(account.c_str(), fileMessage, options);
    ```

   Sample code for sending a channel file message:

    ```cpp
    m_Channel->sendMessage(fileMessage, options);
    ```

4. The user who receives the file message can receive the corresponding callback. You can get the media ID from the `IFileMessage` instance and save the file by media ID.

    Sample code for receiving a peer-to-peer file message:

    ```cpp
    // CRTMCallBack extend from IRtmServiceEventHandler
    void CRTMCallBack::onFileMessageReceivedFromPeer(const char *peerId, const IFileMessage* message)
    {
        if(message){
            //deal with message
        }
    }
    ```

   Sample code for receiving a channel file message:

    ```cpp
    // CRTMCallBack extend from IChannelEventHandler
    void CRTMCallBack::onFileMessageReceived(const char *userId, const IFileMessage* message)
    {
        if(message){
            //deal with message
        }
    }
    ```
   
   Typically, you can download a file to local storage via media ID.

   ```cpp
   m_rtmService->downloadMediaToFile(mediaId.c_str(), filePath.c_str(), requestId);
   ```

   For scenarios that require quick save operations, you can download the file to memory:

   ```cpp
   m_rtmService->downloadMediaToMemory(mediaId.c_str(), long long &requestId);
   ```

   If you need to cancel uploading or downloading a file, refer to the following example code:

   ```cpp
   m_rtmService->cancelMediaUpload(requestId);
   ```
	 
   ```cpp
   m_rtmService->cancelMediaDownload(requestId);
   ```


## Considerations

- You must log in to the Agora RTM system before calling the methods.
- Ensure that the total size of message content, filename, and thumbnail does not exceed 32 KB for each message instance.
- The `filePath` parameter for uploading and downloading methods can only accept strings with UTF-8 format. The path must be an absolute path.
- You can only cancel an ongoing upload or download task. The request ID is no longer valid when the corresponding task completes.
- To save the downloaded file or image to local memory, you must access the corresponding memory location when the `onDownloadMediaToMemoryResult` callback completes.
- For each client, do not exceed a total of nine upload tasks and download tasks.
- You can set the width and height of the uploaded image by `IImageMessage`. The settings only exist as additional attributes and does not affect the uploaded image. The uploaded image is not resized or cropped.
- The SDK automatically sets width and height for images in JPEG, JPG, BMP, and PNG. The width or height you set overwrites the SDK-calculated width or height.
- The message sender must use the `release` method to destroy the `IImageMessage` instance when the `IImageMessage` instance is no longer needed.

## API reference

- [`createFileMessageByUploading`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a99f2137ec43be135b369b7d6927b6138)
- [`createImageMessageByUploading`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a7192d93f365c28e2d0b91547716fb5a9)
- [`cancelMediaUpload`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a0090bbb72e250ffbaedc84d9041b64b1)
- [`cancelMediaDownload`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#adc34b7acad8b845fe1242efd127d82b9)
- [`createFileMessageByMediaId`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a6e4b13011388ec45e8a02377b240506f)
- [`createImageMessageByMediaId`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a97bfb847ff876ab216cf219f4b4f856d)
- [`downloadMediaToMemory`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#ade134da2be907a8078ce693849e0cc37)
- [`downloadMediaToFile`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a70584eb57e97476b1da072f737d88c95)
- [`onMediaUploadingProgress`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a56d5464c3b5e53c44039190a3ac4dfe9)
- [`onMediaDownloadingProgress`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a9c4dfbb224f69b73f64dc1bf34f28567)
- [`onMediaCancelResult`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a64cac4d387e2bf6a419bb478358570f6)
- [`onFileMediaUploadResult`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#aadaa8cd5309e4e70ab2cbdfc1ef21241)
- [`onImageMediaUploadResult`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#abaeeaeb6d69b98510d6c3b012849251e)
- [`onFileMessageReceivedFromPeer`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a4642bb3a8ddf026617fff47d1c9f3e3a)
- [`onImageMessageReceivedFromPeer`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a2682e64be745cf7af816a12f9895ce07)
- [`onFileMessageReceived`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel_event_handler.html#a416dd103c84387a5147e962398eff8d1)
- [`onImageMessageReceived`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_channel_event_handler.html#a6d710170df9b3c1f0ef092012af2e317)
- [`onMediaDownloadToMemoryResult`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#ad0de249a8f0b79973f34f295cabe4904)
- [`onMediaDownloadToFileResult`](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service_event_handler.html#a0b6edc7b944eab02d545bb2d2d1bfe2f)
