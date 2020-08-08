
---
title: 媒体流加密
description: 
platform: Windows
updatedAt: Thu Aug 06 2020 11:11:50 GMT+0800 (CST)
---
# 媒体流加密
本文描述如何使用 Agora 内置的加密方案。

<div class="alert note"><li>通信和直播场景均支持媒体流加密功能。但是在直播场景下，如果你需要推流到 CDN，请勿使用媒体流加密功能。<br><li>若需使用媒体流加密功能，需确保接收端和发送端都使用相同的加密方案，否则会出现未定义行为（例如音频无声或视频黑屏）。</br></div>

下图描述了启用加密功能后的数据传输流程：
![](https://web-cdn.agora.io/docs-files/1590556755691)

## 场景 1: 不需要加密

该平台 SDK 默认不加密。

## 场景 2: 需要加密

### 步骤 1: 启用加密功能

调用 `setEncryptionSecret`，启用内置加密，并设置数据加密密码。

### 步骤 2: 设置加密模式

调用 `setEncryptionMode`，设置内置的加密方法。

我们在 GitHub 提供一个实现了媒体流加密的示例项目 [OpenVideoCall-Windows](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-Windows)。你可以下载体验并参考代码。

## 场景 3: 使用自定义的加密算法

### 步骤 1: 注册数据包观测器

应用程序可以注册数据包观测器 (Packet Observer), 用于在语音或视频数据包传输时接收事件。在你的应用程序上，调用以下 API 注册数据包观测器：

```
virtual int registerPacketObserver(IPacketObserver* observer);
```

观测器必须继承 <code>agora::IPacketObserver</code> 类，且用 C++ 语言实现。以下为 <code>IPacketObserver</code> 类的定义:

```
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

继承 <code>agora::IPacketObserver</code>，在你的应用程序上使用你自定义的数据加密算法。

```
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

调用 `registerPacketObserver` 为应用程序使用的 `agora::IPacketObserver` 类注册一个实例。
