
---
title: Channel Encryption
description: 
platform: iOS,macOS
updatedAt: Thu Oct 15 2020 03:27:03 GMT+0800 (CST)
---
# Channel Encryption
## Introduction

To improve data security, developers can encrypt users' media streams during the real-time engagement. Agora supports built-in encryption and customized encryption, and the differences between two encryption schemes are as follows:

- Built-in encryption: The encryption mode and encryption key exist in the app and the SDK.
- Customized encryption: The encryption mode and encryption key only exist in the app.

You can choose an encryption schema according to your needs.

<div class="alert note"><li>Both the communication and live interactive streaming scenarios support encryption, but Agora does not support pushing encrypted streams to the CDN in a live-streaming channel.</li><li>Eure that both the receivers and senders use the same encryption scheme. Otherwise, undefined behaviors such as no voice and black screen may occour.</li></div>

The following diagram describes the encrypted data transmission process:

![](https://web-cdn.agora.io/docs-files/1596711714514)

## Implementation

Before enabling the encryption, ensure that you have implemented the basic real-time communication functions in your project. For details, see the following documents:

- iOS: [Start a Video Call](../../en/Video/start_call_ios.md) or [Start a Live Interactive Video Streaming](../../en/Video/start_live_ios.md)
- macOS: [Start a Video Call](../../en/Video/start_call_mac.md) or [Start a Live Interactive Video Streaming](../../en/Video/start_live_mac.md)

Additionally, the iOS SDK includes an independent encryption library `AgoraRtcCryptoLoader.framework`, and you need to integrate and import the encryption library as follows:

1. To integrate the encryption library, do the following:

 <details>
	<summary><font color="#3ab7f8">Automatically integrate the encryption library with CocoaPods</font></summary>
	a. Ensure that you have installed CocoaPods before the following steps. See the installation guide in  <a href="https://guides.cocoapods.org/using/getting-started.html#getting-started">Getting Started with CocoaPods</a >.
	<p>b. In Terminal, go to the project path and run the <tt>pod init</tt> command to create a <tt>Podfile</tt> in the project folder.</p>
	<p>c. Open the <tt>Podfile</tt>, delete all contents and input the following contents. Remember to change <tt>Your App</tt> to the target name of your project, and change <tt>version</tt> to the version of the SDK which you want to integrate.</p>
	<pre>
	# platform :ios, '9.0' use_frameworks!
	target 'Your App' do
      pod 'AgoraRtcEngine_iOS_Crypto', '~> version'
	end</pre>
	<p>d. Go back to Terminal, and run the <tt>pod update</tt> command to update the local libraries.</p>
	<p>e. Run the <tt>pod install</tt> command to install the Agora SDK. Once you successfully install the SDK, it shows <tt>Pod installation complete!</tt> in Terminal, and you can see an <tt>xcworkspace</tt> file in the project folder.</p>
	<p>f. Open the generated <tt>xcworkspace</tt> file in Xcode.</p>
</details>

 <details>
	<summary><font color="#3ab7f8">Manually integrate the encryption library</font></summary>
	a. Copy <tt>AgoraRtcCryptoLoader.framework</tt> from the SDK package to the project folder.
	<p>b. Open Xcode (take the Xcode 11.0 as an example), go to the <b>TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content</b> menu, click <b>Add Other...</b> after clicking <b>+</b> to add <tt>AgoraRtcCryptoLoader.framework</tt>. To ensure that the signature of the dynamic library is the same as the signature of the app, you need to set the <b>Embed</b> attribute of the dynamic library to <b>Embed & Sign</b>.</p>
	
 	 <div class="alert warning">According to the requirement of Apple, the <b>Extension</b> of app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the <b>Extension</b>, change the file status as <b>Do Not Embed</b>.  </div>
	
</details>

2. To import the <tt>AgoraRtcCryptoLoader</tt> library, refer to the following sample code:

 ```swift
// Swift 
#import <AgoraRtcCryptoLoader/AgoraRtcCryptoLoader.h>
```

 ```objective-c
// Objective-C
import AgoraRtcCryptoLoader
```

### Use the built-in encryption

Before joining a channel, call `enableEncryption` to enable the built-in encryption, and set the encryption mode and encryption key.

<div class="alert note">All users in the same channel must use the same encryption mode and encryption key.</div>

Agora supports the following encryption modes:

- `AgoraEncryptionModeAES128XTS`: 128-bit AES encryption, XTS mode.
- `AgoraEncryptionModeAES128ECB`: 128-bit AES encryption, ECB mode.
- `AgoraEncryptionModeAES256XTS`: 256-bit AES encryption, XTS mode.

#### Sample code

```swift
// Swift
// Creates an AgoraEncryptionConfig instance.
let config = AgoraEncryptionConfig()
// Sets the encryption mode as AgoraEncryptionModeAES128XTS.
config.encryptionMode = AgoraEncryptionMode.AES128XTS
// Sets the encryption key.
config.encryptionKey = "xxxxxxxxxxxxxxxx"
// Enables the built-in encryption.
agoraKit.enableEncryption(true, config)
```
		
```objective-c
// Objective-C
// Creates an AgoraEncryptionConfig instance.
AgoraEncryptionConfig *config = [[AgoraEncryptionConfig alloc] init];
// Sets the encryption mode as AgoraEncryptionModeAES128XTS, and sets the encryption key.
config.encryptionMode = AgoraEncryptionModeAES128XTS;
config.encryptionKey = @"xxxxxxxxxxxxxxxx";
// Enables the built-in encryption.
[agoraKit enableEncryption: YES encryptionConfig:config];
```

#### API reference

[`enableEncryption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableEncryption:encryptionConfig:)

### Use the customized encryption

To implement the customized encryption, use `IPacketObserver` class and `registerPacketObserver` in C++ as follows:

1. Before joining a channel, call `registerPacketObserver` to register the packet observer, so that you can receive events during audio or video packet transmission.

   ```c++
	 virtual int registerPacketObserver(IPacketObserver* observer);
```

2. Implement an `IPacketObserver` class.

   ```c++
   class IPacketObserver
   {
   public:
    
   struct Packet
   {
   // Buffer address of the sent or received data.
   const unsigned char* buffer;
   // Buffer size of the sent or received data.
   unsigned int size;
   };
    
   // Occurs when the local user sends an audio packet.
   // The SDK triggers this callback before the audio packet is sent to the remote user.
   // @param packet See Packet.
   // @return
   // - true: The audio packet is sent successfully.
   // - false: The audio packet is discarded.
   virtual bool onSendAudioPacket(Packet& packet) = 0;
    
   // Occurs when the local user sends a video packet.
   // The SDK triggers this callback before the video packet is sent to the remote user.
   // @param packet See Packet.
   // @return
   // - true: The video packet is sent successfully.
   // - false: The video packet is discarded.
   virtual bool onSendVideoPacket(Packet& packet) = 0;
    
   // Occurs when the local user receives an audio packet.
   // The SDK triggers this callback before the audio packet of the remote user is received.
   // @param packet See Packet.
   // @return
   // - true: The audio packet is sent successfully.
   // - false: The audio packet is discarded.
   virtual bool onReceiveAudioPacket(Packet& packet) = 0;
    
   // Occurs when the local user receives a video packet.
   // The SDK triggers this callback before the video packet of the remote user is received.
   // @param packet See Packet.
   // @return
   // - true: The video packet is sent successfully.
   // - false: The video packet is discarded.
   virtual bool onReceiveVideoPacket(Packet& packet) = 0;
   };
```

3. Inherit the `IPacketObserver` class and use your customized encryption algorithm on your app.

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
            // Encrypts the packet.
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
    
            for (i = 0; p < pe && i < m_txAudioBuffer.size(); ++p, ++i)
            {
                m_txAudioBuffer[i] = *p ^ 0x55;
            }
            // Sends the buffer and size of the encrypted data to the SDK.
            packet.buffer = &m_txAudioBuffer[0];
            packet.size = i;
            return true;
        }
    
        virtual bool onSendVideoPacket(Packet& packet)
        {
            int i;
            // Encrypts the packet.
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
            for (i = 0; p < pe && i < m_txVideoBuffer.size(); ++p, ++i)
            {
                m_txVideoBuffer[i] = *p ^ 0x55;
            }
            // Sends the buffer and size of the encrypted data to the SDK.
            packet.buffer = &m_txVideoBuffer[0];
            packet.size = i;
            return true;
        }
    
        virtual bool onReceiveAudioPacket(Packet& packet)
        {
            int i = 0;
            // Decrypts the packet.
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
            for (i = 0; p < pe && i < m_rxAudioBuffer.size(); ++p, ++i)
            {
                m_rxAudioBuffer[i] = *p ^ 0x55;
            }
            // Sends the buffer and size of the decrypted data to the SDK.
            packet.buffer = &m_rxAudioBuffer[0];
            packet.size = i;
            return true;
        }
    
        virtual bool onReceiveVideoPacket(Packet& packet)
        {
            int i = 0;
            // Decrypts the packet.
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
    
            for (i = 0; p < pe && i < m_rxVideoBuffer.size(); ++p, ++i)
            {
                m_rxVideoBuffer[i] = *p ^ 0x55;
            }
            // Sends the buffer and size of the decrypted data to the SDK.
            packet.buffer = &m_rxVideoBuffer[0];
            packet.size = i;
            return true;
        }
    
    private:
        std::vector<unsigned char> m_txAudioBuffer; // Buffer for sending the audio data
        std::vector<unsigned char> m_txVideoBuffer; // Buffer for sending the video data
    
        std::vector<unsigned char> m_rxAudioBuffer; // Buffer for receiving the audio data
        std::vector<unsigned char> m_rxVideoBuffer; // Buffer for receiving the video data
    };
```

4. Call `registerPacketObserver` to register the `IPacketObserver` instance.

#### API reference

[`registerPacketObserver`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a95b53a32d598c3d98a51c24f7f9af4b4)
