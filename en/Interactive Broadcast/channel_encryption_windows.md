
---
title: Channel Encryption
description: 
platform: Windows
updatedAt: Thu Aug 06 2020 11:15:46 GMT+0800 (CST)
---
# Channel Encryption
This page introduces various encryption modes. Choose one that best suits your needs.

<div class="alert note"><li>Both the communication and live-streaming scenarios support encryption. If you need to push streams to the CDN in a live-streaming channel, do not use channel encryption.<br><li>Ensure that both receivers and senders use the same encryption scheme. Otherwise, you may meet undefined behaviors such as no voice and black screen.</br></div>

The following diagram describes the encrypted data transmission process:
![](https://web-cdn.agora.io/docs-files/1590556479532)

## Scenario 1: Do Not Use Encryption

The [Agora SDK for Windows](https://docs.agora.io/en/Agora%20Platform/downloads) does not include any independent library for encryption. You are all set if you do not use encryption.

## Scenario 2: Use Encryption

### Step 1: Enable encryption.

Call the `setEncryptionSecret` method to enable built-in encryption and set the encryption password.

### Step 2: Choose the encryption mode to use.

Call the `setEncryptionMode` method to set the built-in encryption mode.

We provide an open-source demo project [OpenVideoCall-Windows](https://github.com/AgoraIO/Basic-Video-Call/tree/master/Group-Video/OpenVideoCall-Windows) that implements channel encryption on GitHub. You can try the demo and refer to the code.

## Scenario 3: Use Customized Encryption

## Step 1: Register a Packet Observer

The Agora Native SDK allows your application to register a packet observer to receive events whenever a voice or video packet is transmitting.

Register a packet observer on your application by using the following method:

```
virtual int registerPacketObserver(IPacketObserver* observer);
```

The observer must be inherited from <code>agora::IPacketObserver</code> and be implemented in C++. The following is the definition of the <code>IPacketObserver</code> class:

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

## Step 2: Implement a Customized Data Encryption Algorithm

The observer must be inherited from <code>agora::IPacketObserver</code> to be implemented in the customized data encryption algorithm on your application. The following example uses XOR for data processing. For the Agora Native SDK, sending and receiving packets are handled by different threads, which is why encryption and decryption can use different buffers:

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
                     //assign the new buffer and the length back to the SDK
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
                     //assign the new buffer and the length back to the SDK
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
                     //assign the new buffer and the length back to the SDK
                     packet.buffer = &m_rxVideoBuffer[0];
                     packet.size = i;
                     return true;
                 }

             private:
                 std::vector<unsigned char> m_txAudioBuffer; //buffer for sending the voice data
                 std::vector<unsigned char> m_txVideoBuffer; //buffer for sending the video data

                 std::vector<unsigned char> m_rxAudioBuffer; //buffer for receiving the voice data
                 std::vector<unsigned char> m_rxVideoBuffer; //buffer for receiving the video data
     };
```

## Step 3: Register the Instance

Call the <code>registerPacketObserver</code> method to register the instance of the <code>agora::IPacketObserver</code> class implemented by your application.



