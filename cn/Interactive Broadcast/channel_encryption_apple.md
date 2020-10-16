
---
title: 媒体流加密
description: 
platform: iOS,macOS
updatedAt: Thu Oct 15 2020 03:27:26 GMT+0800 (CST)
---
# 媒体流加密
## 功能描述

在实时音视频互动过程中，开发者需要对媒体流加密，从而保障用户的数据安全。Agora 提供内置加密方案和自定义加密方案，区别如下：

- 内置加密：加密模式和密钥存在于 app 和 SDK 中。
- 自定义加密：加密模式和密钥仅存在于 app 中。

你可以根据需要选择合适的加密方案。

<div class="alert note"><li>通信和直播场景均支持媒体流加密功能。但是在直播场景下，Agora 不支持将加密后的媒体流推到 CDN 上。</li><li>如需使用媒体流加密功能，请确保接收端和发送端都使用相同的加密方案，否则会出现未定义行为（例如音频无声或视频黑屏）。</li></div>

下图描述了启用媒体流加密后的数据传输流程：

![](https://web-cdn.agora.io/docs-files/1596706031835)

## 实现方法

在启用媒体流加密前，请确保已在你的项目中实现基本的实时音视频功能。详见如下文档：

- iOS: [实现音视频通话](../../cn/Interactive%20Broadcast/start_call_ios.md)或[实现互动直播](../../cn/Interactive%20Broadcast/start_live_ios.md)
- macOS: [实现音视频通话](../../cn/Interactive%20Broadcast/start_call_mac.md)或[实现互动直播](../../cn/Interactive%20Broadcast/start_live_mac.md)

此外，iOS SDK 包中有一个独立的动态加密库 `AgoraRtcCryptoLoader.framework`，你需要参考以下步骤集成并导入加密库：

1. 参考如下方法集成加密库：

 <details>
	<summary><font color="#3ab7f8">使用 CocoaPods 自动集成</font></summary>
	a. 开始前确保你已安装 Cocoapods。参考 <a href="https://guides.cocoapods.org/using/getting-started.html#getting-started">Getting Started with CocoaPods</a > 安装说明。
	<p>b. 在 Terminal 里进入项目根目录，并运行 <tt>pod init</tt> 命令。项目文件夹下会生成一个 <tt>Podfile</tt> 文本文件。</p>
	<p>c. 打开 <tt>Podfile</tt> 文件，修改文件为如下内容。注意将 <tt>Your App</tt> 替换为你的 Target 名称，并将 <tt>version</tt> 替换为你需集成的 SDK 版本。</p>
	<pre>
	# platform :ios, '9.0' use_frameworks!
	target 'Your App' do
      pod 'AgoraRtcEngine_iOS_Crypto', '~> version'
	end</pre>
	<p>d. 在 Terminal 内运行 <tt>pod update</tt> 命令更新本地库版本。</p>
	<p>e. 运行 <tt>pod install</tt> 命令安装 Agora SDK。成功安装后，Terminal 中会显示 <tt>Pod installation complete!</tt>，此时项目文件夹下会生成一个 <tt>xcworkspace</tt> 文件。</p>
	<p>f. 打开新生成的 <tt>xcworkspace</tt> 文件。</p>
</details>

 <details>
	<summary><font color="#3ab7f8">手动集成</font></summary>
	a. 将 SDK 包中的 <tt>AgoraRtcCryptoLoader.framework</tt> 复制到项目文件夹下。
	<p>b. 打开 Xcode（以 Xcode 11.0 为例），进入 <b>TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content</b> 菜单，点击 <b>+</b>，再点击 <b>Add Other…</b> 添加 <tt>AgoraRtcCryptoLoader.framework</tt>。为保证动态库的签名和 app 的签名一致，你需要将动态库的 <b>Embed</b> 属性设置为 <b>Embed & Sign</b>。</p>
	 <div class="alert warning">根据 Apple 官方要求，app 的 <b>Extension</b> 不允许包含动态库。如果工程中的 <b>Extension</b> 需要集成 SDK，则集成动态库时需将文件状态改为 <b>Do Not Embed</b>。 </div>
</details>

2. 参考如下代码在项目中导入 `AgoraRtcCryptoLoader` 类：
	
 ```swift
// Swift 
#import <AgoraRtcCryptoLoader/AgoraRtcCryptoLoader.h>
```

 ```objective-c
// Objective-C
import AgoraRtcCryptoLoader
```
	
### 使用内置的加密方案

在加入频道前，调用 `enableEncryption` 方法开启内置加密，并设置加密模式和密钥。

<div class="alert note">同一频道内所有用户必须使用相同的加密模式和密钥。</div>

Agora 支持 4 种加密模式：

- `AgoraEncryptionModeAES128XTS`: 128 位 AES 加密，XTS 模式。
- `AgoraEncryptionModeAES128ECB`: 128 位 AES 加密，ECB 模式。
- `AgoraEncryptionModeAES256XTS`: 256 位 AES 加密，XTS 模式。
- `AgoraEncryptionModeSM4128ECB`: 128 位国密 SM4 加密，ECB 模式。

#### 示例代码

```swift
// Swift
// 创建一个 AgoraEncryptionConfig 实例
let config = AgoraEncryptionConfig()
// 设置加密模式为国密 SM4 加密模式
config.encryptionMode = AgoraEncryptionMode.SM4128ECB
// 设置加密密钥
config.encryptionKey = "xxxxxxxxxxxxxxxx"
// 启用内置加密
agoraKit.enableEncryption(true, config)
```
		
```objective-c
// Objective-C
// 创建一个 AgoraEncryptionConfig 实例
AgoraEncryptionConfig *config = [[AgoraEncryptionConfig alloc] init];
// 设置加密模式为国密 SM4 加密模式，并设置加密密钥
config.encryptionMode = AgoraEncryptionModeSM4128ECB;
config.encryptionKey = @"xxxxxxxxxxxxxxxx";
// 启用内置加密
[agoraKit enableEncryption: YES encryptionConfig:config];
```

#### API 参考

[`enableEncryption`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableEncryption:encryptionConfig:)

### 使用自定义的加密方案

Agora 在各平台上提供了 C++ 的 `registerPacketObserver` 方法及 `IPacketObserver` 类，帮助你实现自定义加密功能。参考步骤如下：

1. 在加入频道前，调用 `registerPacketObserver` 注册数据包观测器，从而在语音或视频数据包传输时接收事件。

   ```c++
	 virtual int registerPacketObserver(IPacketObserver* observer);
	 ```

2. 实现一个 `IPacketObserver` 类:

   ```c++
    class IPacketObserver
    {
    public:
    
    struct Packet
    {
    // 需要发送或接收的数据的缓存地址
    const unsigned char* buffer;
    // 需要发送或接收的数据的缓存大小
    unsigned int size;
    };
    
    // 已发送音频包回调
    // 在音频包被发送给远端用户前触发
    // @param packet 详见: Packet
    // @return
    // - true: 发送音频包
    // - false: 丢弃音频包
    virtual bool onSendAudioPacket(Packet& packet) = 0;
    
    // 已发送视频包回调
    // 在视频包被发送给远端用户前触发
    // @param packet 详见: Packet
    // @return
    // - true: 发送视频包
    // - false: 丢弃视频包
    virtual bool onSendVideoPacket(Packet& packet) = 0;
    
    // 收到音频包回调
    // 在收到远端用户的音频包前触发
    // @param packet 详见: Packet
    // @return
    // - true: 发送音频包
    // - false: 丢弃音频包
    virtual bool onReceiveAudioPacket(Packet& packet) = 0;
    
    // 收到视频包回调
    // 在收到远端用户的视频包前触发
    // @param packet 详见: Packet
    // @return
    // - true: 发送视频包
    // - false: 丢弃视频包
    virtual bool onReceiveVideoPacket(Packet& packet) = 0;
    };
```

3. 继承 `IPacketObserver`，并在你的 app 上使用你自定义的数据加密算法。

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
            // 加密数据包
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
   
            for (i = 0; p < pe && i < m_txAudioBuffer.size(); ++p, ++i)
            {
                m_txAudioBuffer[i] = *p ^ 0x55;
            }
            // 将加密后数据的缓存地址和缓存大小发回 SDK
            packet.buffer = &m_txAudioBuffer[0];
            packet.size = i;
            return true;
        }
   
        virtual bool onSendVideoPacket(Packet& packet)
        {
            int i;
            // 加密数据包
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
            for (i = 0; p < pe && i < m_txVideoBuffer.size(); ++p, ++i)
            {
                m_txVideoBuffer[i] = *p ^ 0x55;
            }
            // 将加密后数据的缓存地址和缓存大小发回 SDK
            packet.buffer = &m_txVideoBuffer[0];
            packet.size = i;
            return true;
        }
   
        virtual bool onReceiveAudioPacket(Packet& packet)
        {
            int i = 0;
            // 解密数据包
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
            for (i = 0; p < pe && i < m_rxAudioBuffer.size(); ++p, ++i)
            {
                m_rxAudioBuffer[i] = *p ^ 0x55;
            }
            // 将解密后数据的缓存地址和缓存大小发回 SDK
            packet.buffer = &m_rxAudioBuffer[0];
            packet.size = i;
            return true;
        }
   
        virtual bool onReceiveVideoPacket(Packet& packet)
        {
            int i = 0;
            // 解密数据包
            const unsigned char* p = packet.buffer;
            const unsigned char* pe = packet.buffer+packet.size;
   
            for (i = 0; p < pe && i < m_rxVideoBuffer.size(); ++p, ++i)
            {
                m_rxVideoBuffer[i] = *p ^ 0x55;
            }
            // 将解密后数据的缓存地址和缓存大小发回 SDK
            packet.buffer = &m_rxVideoBuffer[0];
            packet.size = i;
            return true;
        }
   
    private:
        // 发送音频数据 buffer
        std::vector<unsigned char> m_txAudioBuffer; 
        // 发送视频数据 buffer
        std::vector<unsigned char> m_txVideoBuffer; 
        // 接收音频数据 buffer
        std::vector<unsigned char> m_rxAudioBuffer; 
        // 接收视频数据 buffer
        std::vector<unsigned char> m_rxVideoBuffer; 
    };
```

4. 调用 `registerPacketObserver` 为 `IPacketObserver` 类注册一个实例。

#### API 参考

[`registerPacketObserver`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a95b53a32d598c3d98a51c24f7f9af4b4)
