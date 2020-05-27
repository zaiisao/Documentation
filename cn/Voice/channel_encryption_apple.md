
---
title: 媒体流加密
description: 
platform: iOS,macOS
updatedAt: Wed May 27 2020 05:19:59 GMT+0800 (CST)
---
# 媒体流加密
本文介绍媒体流加密方案。

<div class="alert note"><li>通信和直播场景均支持媒体流加密功能。但是在直播场景下，如果你需要推流到 CDN，请勿使用媒体流加密功能。<br><li>若需使用媒体流加密功能，需确保接收端和发送端都使用相同的加密方案，否则会出现未定义行为（例如音频无声或视频黑屏）。</br></div>

下图描述了启用加密功能后的数据传输流程：

![](https://web-cdn.agora.io/docs-files/1590556755691)

你下载的 [SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads) 里，包含:

- `AgoraRtcCryptoLoader.framework`: 该独立的静态 framework 即为加密模块。
- `libcrypto.a`: 它是一个标准的 OpenSSL 加密库，与 OpenSSL 1.0.2 \(或更高版本\)兼容。

## 场景 1: 不需要加密

如果你不需要加密，可以删除下载的 SDK 软件包里的`AgoraRtcCryptoLoader.framework` 和 `libcrypto.a` 。

## 场景 2: 使用内置的加密方案

我们在 GitHub 提供实现了媒体流加密的示例项目。你可以下载体验并参考源代码。

**iOS**
- Swift 项目： [OpenVideoCall-iOS](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-iOS)
- Objective-C 项目：[OpenVideoCall-iOS-Objective-C](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-iOS-Objective-C)

**macOS**

Swift 项目：[OpenVideoCall-macOS](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-macOS)，参考 [`RoomViewController.swift`](https://github.com/AgoraIO/Basic-Video-Call/blob/master/Group-Video/OpenVideoCall-macOS/OpenVideoCall/RoomViewController.swift#L398)

> 如果你有缩小 SDK 包的考虑，且你的 App 已经有了 `libcrypto.a` ，由于 SDK 包里也包含了一个 `libcrypto.a` ，可以共用一个 `.a` 文件。 Agora 提供的库和 App 正在使用的库不同之处在于: Agora 提供的库版本指定为 1.0.2g，与 App 使用的库的版本不同。

### Objective-C

步骤如下：

1. 在 Xcode 里设置 framework 的 search path 。

2. 在 build phase 里添加 `AgoraRtcCryptoLoader.framework` 和 `libcrypto.a` 。

3. 启用加密功能。

    调用 `setEncryptionSecret`，启用内置加密，并设置数据加密密码。

4. 设置加密模式。

    调用 `setEncryptionMode`，设置内置的加密方法。

### Swift

步骤如下：

#### 步骤 1: 请在 Xcode 里设置 framework 的 search path 设为你的项目文件夹地址。比如：

<img alt="../_images/encryption_search_path.jpg" src="https://web-cdn.agora.io/docs-files/cn/encryption_search_path.jpg"/>

#### 步骤 2: 在项目文件夹下添加桥接文件 `XXX-bridge.h`，在 **Swift Compiler - General** 下将 **Objective-C Bridging Header** 设为该桥接文件。

<img alt="../_images/encryption_select_bridgefile.jpg" src="https://web-cdn.agora.io/docs-files/cn/encryption_select_bridgefile.jpg"/>

#### 步骤 3: 点击 **build phase**，将 SDK 包 **libs** 文件夹下的 `AgoraRtcCryptoLoader.framework` 和 `libcrypto.a` 文件添加到项目文件夹的 **Frameworks** 目录下。

<img alt="../_images/encryption_add_encryptionlib.jpg" src="https://web-cdn.agora.io/docs-files/cn/encryption_add_encryptionlib.jpg"/>

#### 步骤 4:

在桥接文件里添加 `#import <AgoraRtcCryptoLoader/AgoraRtcCryptoLoader.h>` 。

#### 步骤 5: 在初始化 AgoraRtcEngine 的文件中，声明一个 AgoraRtcCryptoLoader 对象，即可在编译链接时，加载加密模块。

<img alt="../_images/encryption_declare_loader.png" src="https://web-cdn.agora.io/docs-files/cn/encryption_declare_loader.png"/>

#### 步骤 6: 启用加密功能。

调用 `setEncryptionSecret`，启用内置加密，并设置数据加密密码。

#### 步骤 7: 设置加密模式。

调用 `setEncryptionMode`，设置内置的加密方法。

## 场景 3: 使用第三方提供的加密方案

### 步骤 1: 注册数据包观测器

应用程序可以注册数据包观测器 \(Packet Observer\), 用于在语音或视频数据包传输时接收事件。在你的应用程序上，调用以下 API 注册数据包观测器：

```c++
virtual int registerPacketObserver(IPacketObserver* observer);
```

观测器必须继承 `agora::IPacketObserver` 类，且用 C++ 语言实现。以下为 `IPacketObserver` 类的定义:

```c++
class IPacketObserver
{
public:

struct Packet
{
        /** Buffer address of the sent or received data.
         */
const unsigned char* buffer;
        /** Buffer size of the sent or received data.
         */
unsigned int size;
};
/** An audio packet is sent to other users.

     @param packet See Packet.
     @return
     - true: The packet is sent successfully.
     - false: The packet is discarded.
     */
virtual bool onSendAudioPacket(Packet& packet) = 0;
/** A video packet is sent to other users.

     @param packet See Packet.
     @return
     - true: The packet is sent successfully.
     - false: The packet is discarded.
     */
virtual bool onSendVideoPacket(Packet& packet) = 0;
/** An audio packet is sent by other users.

     @param packet See Packet.
     @return
     - true: The packet is received successfully.
     - false: The packet is discarded.
*/
virtual bool onReceiveAudioPacket(Packet& packet) = 0;
/** A video packet is sent by other users.

     @param packet See Packet.
     @return
     - true: The packet is received successfully.
     - false: The packet is discarded.
*/
virtual bool onReceiveVideoPacket(Packet& packet) = 0;
};
```

### 步骤 2: 使用定制的数据加密算法

继承 `agora::IPacketObserver`，在你的应用程序上使用你自定义的数据加密算法。

```c++
class AgoraPacketObserver : public agora::IPacketObserver
 {
 public:
     AgoraPacketObserver()
     {
         m_txAudioBuffer.resize(2048);
         m_rxAudioBuffer.resize(2048);
         m_txVideoBuffer.resize(2048);
         m_rxVideoBuffer.resize(2048);
     }
     virtual bool onSendAudioPacket(Packet& packet)
     {
         int i;
         //encrypt the packet
         const unsigned char* p = packet.buffer;
         const unsigned char* pe = packet.buffer+packet.size;


          for (i = 0; p < pe && i < m_txAudioBuffer.size(); ++p, ++i)
         {
             m_txAudioBuffer[i] = *p ^ 0x55;
         }
         //assign new buffer and the length back to SDK
         packet.buffer = &m_txAudioBuffer[0];
         packet.size = i;
         return true;
     }

     virtual bool onSendVideoPacket(Packet& packet)
     {
         int i;
         //encrypt the packet
         const unsigned char* p = packet.buffer;
         const unsigned char* pe = packet.buffer+packet.size;
         for (i = 0; p < pe && i < m_txVideoBuffer.size(); ++p, ++i)
         {
             m_txVideoBuffer[i] = *p ^ 0x55;
         }
         //assign new buffer and the length back to SDK
         packet.buffer = &m_txVideoBuffer[0];
         packet.size = i;
         return true;
     }

     virtual bool onReceiveAudioPacket(Packet& packet)
     {
         int i = 0;
         //decrypt the packet
         const unsigned char* p = packet.buffer;
         const unsigned char* pe = packet.buffer+packet.size;
         for (i = 0; p < pe && i < m_rxAudioBuffer.size(); ++p, ++i)
         {
             m_rxAudioBuffer[i] = *p ^ 0x55;
         }
         //assign new buffer and the length back to SDK
         packet.buffer = &m_rxAudioBuffer[0];
         packet.size = i;
         return true;
     }

     virtual bool onReceiveVideoPacket(Packet& packet)
     {
         int i = 0;
         //decrypt the packet
         const unsigned char* p = packet.buffer;
         const unsigned char* pe = packet.buffer+packet.size;


         for (i = 0; p < pe && i < m_rxVideoBuffer.size(); ++p, ++i)
         {
             m_rxVideoBuffer[i] = *p ^ 0x55;
         }
         //assign new buffer and the length back to SDK
         packet.buffer = &m_rxVideoBuffer[0];
         packet.size = i;
         return true;
     }

 private:
     std::vector<unsigned char> m_txAudioBuffer; //buffer for sending audio data
     std::vector<unsigned char> m_txVideoBuffer; //buffer for sending video data

     std::vector<unsigned char> m_rxAudioBuffer; //buffer for receiving audio data
     std::vector<unsigned char> m_rxVideoBuffer; //buffer for receiving video data
 };
```

### 步骤 3: 注册实例

调用 `registerAgoraPacketObserver` 为应用程序使用的 `agora::IPacketObserver` 类注册一个实例。
