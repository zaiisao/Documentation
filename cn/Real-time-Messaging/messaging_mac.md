
---
title: 发送和接收点对点消息及频道消息
description: 
platform: macOS
updatedAt: Thu Aug 20 2020 08:50:21 GMT+0800 (CST)
---
# 发送和接收点对点消息及频道消息

本页介绍在正式使用 [Agora RTM macOS SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) 进行实时消息通讯前，需要准备的开发环境要求及 SDK 集成方法等内容。

## 示例项目体验

你可以在 GitHub 下载最新版的示例项目查看相关功能的具体实现。

- [Agora-RTM-Tutorial-macOS-Objective-C](https://github.com/AgoraIO/RTM/tree/master/Agora-RTM-Tutorial-macOS-Objective-C) 示例项目的主要代码逻辑可以在以下文件查看：
  - [MainViewController.m](https://github.com/AgoraIO/RTM/blob/master/Agora-RTM-Tutorial-macOS-Objective-C/Agora-RTM-Tutorial/MainViewController.m)
  - [ChatViewController.m](https://github.com/AgoraIO/RTM/blob/master/Agora-RTM-Tutorial-macOS-Objective-C/Agora-RTM-Tutorial/ChatViewController.m)
- [Agora-RTM-Tutorial-macOS-Swift](https://github.com/AgoraIO/RTM/tree/master/Agora-RTM-Tutorial-macOS) 示例项目的主要代码逻辑可以在以下文件查看：
  - [MainViewController.swift](https://github.com/AgoraIO/RTM/blob/master/Agora-RTM-Tutorial-macOS/Agora-RTM-Tutorial/MainViewController.swift)
  - [ChatViewController.swift](https://github.com/AgoraIO/RTM/blob/master/Agora-RTM-Tutorial-macOS/Agora-RTM-Tutorial/ChatViewController.swift)


## 快速跑通示例项目

如果你是第一次使用声网的服务，我们推荐观看下面的视频，了解关于声网服务的基本信息以及如何快速跑通示例项目。

<div class="alert info">点击参与<a href="https://www.wenjuan.com/s/7FbeEz6/" target="_blank">视频教程问卷调查</a>，帮助我们改进体验。</div>

<video src="https://web-cdn.agora.io/docs-files/1583387623387" poster="https://web-cdn.agora.io/docs-files/1597912008540"   controls width = 100% height = auto>你的浏览器不支持 <code>video</code> 标签。</video>

<div class="alert note">视频中展示的 UI 可能有部分调整更新，请以当前最新版为准。</div>
## 开发环境要求



- macOS 10.0+ 真机（MacBook）。
- Xcode 9.0+ 或以上版本。

- 一个有效的 [Agora 开发者账号](https://sso.agora.io/en/signup)。

<div class="alert note">如果你的网络环境部署了防火墙，请根据<a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">应用企业防火墙限制</a >打开相关端口并设置域名白名单。</div> 


## 准备开发环境 

本节介绍如何获取 App ID、创建项目，并将 Agora RTM macOS SDK 集成至你的项目中。

### <a name="appid"></a> 获取 App ID


参考以下步骤获取一个 App ID。若已有App ID，可以直接查看[创建项目](#create)。
<details>
	<summary><font color="#3ab7f8">获取 App ID</font></summary>

1. 进入[控制台](https://console.agora.io/)，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Real-time-Messaging/sign_in_and_sign_up.md)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面，点击**创建**按钮。

![](https://web-cdn.agora.io/docs-files/1574156100068)

3. 在弹出的对话框内输入**项目名称**，选择 App ID 作为**鉴权机制**，再点击“提交”。

![](https://web-cdn.agora.io/docs-files/1574921599254)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目，并找到对应的 App ID。

![](https://web-cdn.agora.io/docs-files/1574921811175)



</details>

### <a name="create"></a>创建项目

参考以下步骤创建一个 macOS 项目。若已有 macOS 项目，可以直接查看[集成 SDK](#IntegrateSDK)。


<details>
	<summary><font color="#3ab7f8">创建 macOS 项目</font></summary>
<ol>	
<li> 打开 <b>Xcode</b> 并点击 <b>Create a new Xcode project</b>。</li>
<li> 选择项目类型为 <b>Cocoa App</b>，并点击<b>Next</b>。</li>
<li> 输入项目信息，如项目名称、开发团队信息、组织名称和语言，并点击 <b>Next</b>。</li>
 <div class="alert note">如果你没有添加过开发团队信息，会看到 <b>Add account…</b> 按钮。点击该按钮并按照屏幕提示登入 Apple ID，完成后即可选择你的账户作为开发团队。</div>
<li> 选择项目存储路径，并点击 <b>Create</b>。</li>

<li> 进入 <b>TARGETS > Project Name > General > Signing</b> 菜单，选择 <b>Automatically manage signing</b>，并在弹出菜单中点击 <b>Enable Automatic</b>。</li>
</ol>	
<img src="https://web-cdn.agora.io/docs-files/1568803584472"/>
		
</details>

### <a name="IntegrateSDK"></a>集成 SDK


选择如下任意一种方式将 Agora RTM macOS SDK 集成到你的项目中。

**方法 1：通过 Cocoapods 导入 SDK**

1. 开始前确保你已安装 **Cocoapods**。参考 [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) 安装说明。
2. 在 **Terminal** 里进入项目根目录，并运行 `pod init` 命令。项目文件夹下会生成一个 **Podfile** 文本文件。
3. 打开 **Podfile** 文件，修改文件为如下内容。注意将 `Your App` 替换为你的 Target 名称。
```
platform :macOS, '10.10' use_frameworks!
target 'Your App' do
    pod 'AgoraRtm_macOS'
end
```
4. 在 **Terminal** 内运行 `pod update` 命令更新本地库版本。
5. 运行 `pod install` 命令安装 Agora SDK。成功安装后，**Terminal** 中会显示 `Pod installation complete!`，此时项目文件夹下会生成一个 **xcworkspace** 文件。
6. 打开新生成的 **xcworkspace** 文件。

**方法 2：手动添加 SDK 到项目中**

1. 下载最新版的 [Agora RTM macOS SDK](https://docs.agora.io/cn/Agora%20Platform/downloads) 并解压。
2. 将 **libs** 文件夹内的 **AgoraRtmKit.framework** 文件复制到项目文件夹下。
3. 打开 **Xcode**，进入 **TARGETS > Project Name > Build Phases > Link Binary with Libraries** 菜单，点击 **+** 添加如下库。在添加 **AgoraRtmKit.framework** 文件时，还需在点击 **+** 后点击 **Add Other…**，找到本地文件并打开。

 - AgoraRtmKit.framework 
 - libc++.tbd
 - libresolv.tbd
 - SystemConfiguration.framework
 - CoreTelephony.framework


## 实时消息和基本频道操作

本节主要提供实现实时消息和基本频道操作的 API 调用时序图以及相关示例代码。

### API 调用时序图

#### 登录登出 Agora RTM 系统


![](https://web-cdn.agora.io/docs-files/1583998324120)

#### 收发点对点消息


![](https://web-cdn.agora.io/docs-files/1583942637899)

#### 加入离开频道


![](https://web-cdn.agora.io/docs-files/1583942656098)

#### 收发频道消息


![](https://web-cdn.agora.io/docs-files/1583942679434)

### <a name = "create"></a>初始化

调用 `initWithAppId` 方法创建一个实例。在该方法中:

- 填入获取到的 App ID。只有 App ID 相同的应用程序才能互通。
- 指定一个事件回调。SDK 通过回调通知应用程序 SDK 的状态变化和运行事件等，如: 连接状态发生变化，接收到点对点消息等。

```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

@property(nonatomic, strong)AgoraRtmKit* kit;

@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 创建一个 AgoraRtmKit 实例
    _kit = [[AgoraRtmKit alloc] initWithAppId:YOUR_APP_ID delegate:self];

}
```


> `AgoraRtmKit` 支持多实例，每个实例独立工作互不干扰，多个实例创建时可以用相同的 `context`，但是事件回调 `AgoraRtmDelegate` 必须是不同的实例。

### <a name = "login"></a>登录

App 必须在登录 RTM 服务器之后，才可以使用 RTM 的点对点消息和群聊功能，在此之前，请确保你已完成初始化。

调用 `loginByToken` 方法[登录RTM服务器](#login)。在该方法中:

- 传入能标识用户角色和权限的 `token`。如果安全要求不高，也可以将值设为 `"nil"`。`token` 需要在应用程序的服务器端生成。详见：[校验用户权限](../../cn/Real-time-Messaging/rtm_token.md)。
- 传入能标识每个用户 ID。`usedId` 为字符串，必须是可见字符（可以带空格），不能为空或者多于 64 个字符，也不能是字符串 `nil`。
- 传入结果回调，用于接收登录 RTM 服务器成功或者失败的结果回调。

```objective-c
[_kit loginByToken:nil user:@"testuser" completion:^(AgoraRtmLoginErrorCode errorCode) {
    if (errorCode != AgoraRtmLoginErrorOk) {
        NSLog(@"login failed %@", @(errorCode));
    } else {
        NSLog(@"login success");
    }
}];

#pragma AgoraRtmDelegate
...
- (void)rtmKit:(AgoraRtmKit *)kit connectionStateChanged:(AgoraRtmConnectionState)state reason:(AgoraRtmConnectionChangeReason)reason
{
    NSLog(@"Connection state changed to %@", @(reason));
}
```

如果需要退出登录，可以调用 `logoutWithCompletion` 方法。

```objective-c
[_kit logoutWithCompletion:^(AgoraRtmLogoutErrorCode errorCode) {
    if (errorCode != AgoraRtmLogoutErrorOk) {
        NSLog(@"logout failed %@", @(errorCode));
    } else {
        NSLog(@"logout success");
    }
}];
```

> 调用 `logoutWithCompletion` 方法之后可以调用 `loginByToken` 重新登录或者切换账号。

### <a name = "sendpeer"></a>点对点消息

App 在成功[登录 RTM 服务器](#login)之后，可以开始使用 RTM 的点对点消息功能。

#### 发送点对点消息

调用 `sendMessage` 方法发送点对点消息。在该方法中：

- 传入目标消息接收方的用户账号 ID。
- 传入 `AgoraRtmMessage` 对象实例。该消息对象由 `AgoraRtmMessage` 类的 `initWithText` 初始化方法创建和设置消息内容。
- 传入消息发送状态监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时，对方不可达等。

```objective-c
- (void)... {
    [_kit sendMessage:[[AgoraRtmMessage alloc] initWithText:@"testmsg"] toPeer:@"peer" completion:^(AgoraRtmSendPeerMessageState state) {
        if (state == AgoraRtmSendPeerMessageStateReceivedByPeer) {
            NSLog(@"Message successfully sent.");
        }
    }];
}

#pragma AgoraRtmDelegate

- (void)rtmKit:(AgoraRtmKit *)kit messageReceived:(AgoraRtmMessage *)message fromPeer:(NSString *)peerId
{
    NSLog(@"Message received from %@: %@", message.text, peerId);
}
```

#### 接收点对点消息

点对点消息的接收通过创建 `AgoraRtmMessage` 实例的时候传入的 `AgoraRtmDelegate` 回调接口进行监听。在该回调接口的 `MessageReceived` 回调方法中可以获取到消息文本内容和是消息发送方的用户 ID (`peerId`)。

```objective-c
- (void)rtmKit:(AgoraRtmKit *)kit messageReceived:(AgoraRtmMessage *)message fromPeer:(NSString *)peerId
{
    NSLog(@"Message received from %@: %@", message.text, peerId);
}
```


> 接收到的 `AgoraRtmMessage` 消息对象不能重复利用再用于发送。

### <a name = "sendchannel"></a>频道消息

App 在成功[登录 RTM 服务器](#login)之后，可以开始使用 RTM 的频道消息功能。

#### 创建并加入频道

1. 调用 `AgoraRtmChannel` 实例的 `createChannelWithId` 方法创建 `AgoraRtmChannel` 实例。在该方法中：
   - 传入能标识每个频道的 ID。`channelId` 为字符串，必须是可见字符（可以带空格），不能为空或者多于 64 个字符，也不能是字符串 `"nil"`。
   - 指定一个事件回调。SDK 通过回调通知应用程序频道的状态变化和运行事件等，如: 接收到频道消息、用户加入和退出频道等。
2. 用户只有在加入频道之后，才能收发频道消息和获取频道成员列表等。调用 `AgoraRtmChannel` 实例的 `joinWithCompletion` 方法加入频道。在该方法中，指定回调用于判断是否成功加入该频道。

```objective-c
@interface ViewController ()<AgoraRtmChannelDelegate, AgoraRtmDelegate>

...
@property(nonatomic, strong)AgoraRtmChannel* channel;

@implementation
- (void)... {
    _channel = [_kit createChannelWithId:@"testchannel" delegate:self];
    [_channel joinWithCompletion:^(AgoraRtmJoinChannelErrorCode state) {
        if(state == AgoraRtmJoinChannelErrorOk) {
            NSLog(@"join success");
        } else {
            NSLog(@"join failed: %@", @(state));
        }
    }];
}
#pragma AgoraRtmChannelDelegate
- (void)channel:(AgoraRtmChannel *)channel memberLeft:(AgoraRtmMember *)member
{
    NSLog(@"%@ left channel %@", member.userId, member.channelId);
}

- (void)channel:(AgoraRtmChannel *)channel memberJoined:(AgoraRtmMember *)member
{
    NSLog(@"%@ joined channel %@", member.userId, member.channelId);
}
```

#### 发送频道消息

在成功加入频道之后，用户可以开始向该频道发送消息。

调用 `AgoraRtmChannel` 实例的 `sendMessage` 方法向该频道发送消息。在该方法中：

- 传入 `AgoraRtmMessage` 对象实例。该消息对象由 `AgoraRtmMessage` 类的 `initWithText` 初始化方法创建和设置消息内容。
- 传入消息发送状态监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时等。

```objective-c
- (void)... {
    [_channel sendMessage:[[AgoraRtmMessage alloc] initWithText:@"channelmsg"] completion:^(AgoraRtmSendChannelMessageState state) {
        if(state == AgoraRtmSendChannelMessageStateReceivedByServer) {
            NSLog(@"sent success");
        }
    }];
}

#pragma AgoraRtmDelegate

- (void)channel:(AgoraRtmChannel *)channel messageReceived:(AgoraRtmMessage *)message fromMember:(AgoraRtmMember *)member
{
    NSLog(@"message received from %@ in channel %@: %@", message.text, member.channelId, member.userId);
}
```

#### 接收频道消息

频道消息的接收通过创建频道消息的时候传入的 `AgoraRtmChannelDelegate` 回调接口进行监听。在该回调接口的 `MessageReceived` 回调方法中可以获取到频道消息文本内容和频道消息的发送者的用户 ID。

```objective-c
- (void)channel:(AgoraRtmChannel *)channel messageReceived:(AgoraRtmMessage *)message fromMember:(AgoraRtmMember *)member
{
    NSLog(@"message received from %@ in channel %@: %@", message.text, member.channelId, member.userId);
}
```



#### 退出频道

调用 `AgoraRtmChannel` 实例的 `leaveWithCompletion` 方法可以退出该频道。

```objective-c
[_channel leaveWithCompletion:^(AgoraRtmLeaveChannelErrorCode state) {
    if(state == AgoraRtmLeaveChannelErrorOk) {
        NSLog(@"leave success");
    } else {
        NSLog(@"leave failed: %@", @(state));
    }
}];
```


## 开发注意事项


- RTM 支持多个相互独立的 [AgoraRtmKit](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) 实例。

- 在收发点对点消息或进行其他频道操作前，请确保你已成功登录 Agora RTM 系统（即确保已经收到 `AgoraRtmLoginErrorOk`）。

- 使用频道核心功能前必须通过调用 [createChannelWithId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/createChannelWithId:delegate:) 方法创建频道实例。
- 你可以创建多个 [AgoraRtmKit](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html) 客户端实例，但是每个客户端实例最多只能同时加入 20 个频道。每个频道都应有不同的 `channelId` 参数。

- 当离开了频道且不再加入该频道时，可以调用 [destroyChannelWithId](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmKit.html#//api/name/destroyChannelWithId:) 方法及时释放频道实例所占用的资源。

- 接收到的 [AgoraRtmMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_oc/Classes/AgoraRtmMessage.html) 消息对象不能重复利用再用于发送。



