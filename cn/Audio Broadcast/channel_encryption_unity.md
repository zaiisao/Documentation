
---
title: 媒体流加密
description: 
platform: Unity
updatedAt: Tue May 26 2020 02:50:01 GMT+0800 (CST)
---
# 媒体流加密
本文介绍媒体流加密方案。

<div class="alert note"><li>通信和直播场景均支持媒体流加密功能。但是在直播场景下，如果你需要推流到 CDN，请勿使用媒体流加密功能。<br><li>若需使用媒体流加密功能，需确保接收端和发送端都使用相同的加密方案，否则会出现未定义行为（例如音频无声或视频黑屏）。</br></div>

下图描述了启用加密功能后的数据传输流程：

![](https://web-cdn.agora.io/docs-files/1589769277502)

你下载的 [SDK 软件包](https://docs.agora.io/cn/Agora%20Platform/downloads) 里，包含以下加密库:

| 操作系统 | 加密库 |
| ---------------- | ---------------- |
| Android      | `libagora-crypto.so`: 动态加密库     |
| iOS      | <li>`AgoraRtcCryptoLoader.framework`: 静态加密库<li>`libcrypto.a`: 标准的 OpenSSL 加密库，与 OpenSSL 1.0.2 \(或更高版本\)兼容</li>      |

<div class="alert note">仅 Android 和 iOS 平台有独立加密库。</div>

## 场景 1: 不需要加密

该平台 SDK 默认不加密。在 Android 或 iOS 平台上，你可以删除相应的加密库，减少包体积。

## 场景 2: 使用内置的加密方案

### 步骤 1: 调整路径 (仅适用于 Android 和 iOS 平台）

- Android: 将 `libagora-crypto.so` 文件放在与你项目内 `libagora-rtc-sdk-jni.so` 文件所在的相同路径下。
- iOS: 打开 Xcode，进入 **TARGETS > Project Name > Build Settings > Search Paths** 菜单，在 **Framework Search Paths** 中设置你的项目文件夹地址。进入 **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content** 菜单，点击 **+** 添加 `AgoraRtcCryptoLoader.framework` 和 `libcrypto.a` 文件。
 <div class="alert note">如果项目为 Swift 项目，可参考 <a href="../../cn/Audio%20Broadcast/channel_encryption_apple.md">Swift 加密步骤</a >添加桥接文件。</div>

### 步骤 2: 启用加密功能

调用 `SetEncryptionSecret`，启用内置加密，并设置数据加密密码。

### 步骤 3: 设置加密模式

调用 `SetEncryptionMode`，设置内置的加密模式。

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
