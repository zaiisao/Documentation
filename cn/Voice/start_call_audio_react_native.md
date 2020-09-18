
---
title: 实现语音通话
description: 快速实现语音通话
platform: React Native
updatedAt: Fri Sep 18 2020 04:41:38 GMT+0800 (CST)
---
# 实现语音通话
本文介绍如何使用 Agora React Native SDK 快速实现语音通话。

## 示例项目

Agora 在 GitHub 提供一个开源的 [Agora React Quickstart](https://github.com/AgoraIO-Community/Agora-RN-Quickstart) 示例项目。在实现相关功能前，你可以下载并体验。

## 前提条件

- Node 10 或以上
- React Native 0.59.10 或以上
- 有效的 [Agora 账户](https://docs.agora.io/cn/Agora%20Platform/sign_in_and_sign_up) 和 [App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#%E8%8E%B7%E5%8F%96-app-id)

<div class="alert note">如果你的网络环境部署了防火墙，请根据<a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">应用企业防火墙限制</a>打开相关端口。</div>


## 准备开发环境

### 创建项目

参考[创建 React Native 项目](https://reactnative.dev/docs/environment-setup)创建一个新项目。若已有 React Native 项目，可以直接参考<a href="#integration">集成 SDK</a>。

### 集成 SDK<a name="integration"></a>

本节介绍如何在 React Native 0.60.0 或以上版本集成 Agora React Native SDK。

<div class="alert note">如果你使用的 React Native 版本为 0.59.x， 请参考<a href="https://github.com/AgoraIO-Community/react-native-agora/blob/master/README.zh.md#安装在-react-native--059x">安装在（React Native == 0.59.x）</a >。</div>

1. 下载最新版的 Agora React Native SDK：

 **方法一**：使用 yarn 下载
```
yarn add react-native-agora
```

 **方法二**：使用 npm 下载
```
npm i --save react-native-agora
```

	<div class="alert note">React Native 0.60.0 或以上版本支持自动链接原生模块，请勿手动链接。详见  <a href="https://github.com/react-native-community/cli/blob/master/docs/autolinking.md">Autolinking</a >。</div>  

2. 对于 iOS 项目，还需执行以下命令:
```
npx pod-install
```

   <div class="alert note"><ul><li>请确保你已安装 <b>Cocoapods</b>。详见<a href="https://docs.agora.io/cn/Video/start_call_ios?platform=iOS#集成-sdk">使用 CocoaPods 自动集成</a >。</li><li>Agora React Native SDK 基于 Swift 语言开发。如果你的 React Native 版本低于 0.62.0 且开发语言不是 Swift，请参考<a href="https://github.com/AgoraIO-Community/react-native-agora/blob/master/docs/v3/installation.ios.md#step-1-migrating-to-swift">迁移至 Swift </a >集成 SDK。</li></ul></div>

   

## 实现语音通话

本节介绍如何实现语音通话。

### 1. 导入类

在项目中导入 `RtcEngine` 类和视图渲染组件。

```
import RtcEngine, {RtcLocalView, RtcRemoteView, VideoRenderMode} from 'react-native-agora'
```

### 2. 初始化 RtcEngine

在调用其他 Agora API 前，需要创建并初始化 `RtcEngine` 对象。

你还可以根据场景需要，在初始化时注册想要监听的回调事件，如本地用户加入频道，远端用户加入频道及远端用户离开频道等。

```
async function init() {
    // 填入你的 App ID, 创建并初始化 RtcEngine 实例。
    const engine = await RtcEngine.create(appId)
    // 注册 JoinChannelSuccess 回调。
    // 本地用户成功加入频道时，会触发该回调。
    engine.addListener('JoinChannelSuccess', (channel, uid, elapsed) => {})
    // 注册 UserJoined 回调。
    // 远端用户成功加入频道时，会触发该回调。
    engine.addListener('UserJoined', (uid, elapsed) => {})
    // 注册 UserOffline 回调。
    // 远端用户离开频道时，会触发该回调。
    engine.addListener('UserOffline', (uid, reason) => {})
}
```

### 6. 加入频道

完成初始化后，你就可以调用 `joinChannel` 方法加入频道。你需要在该方法中传入如下参数：

- `token`：传入能标识用户角色和权限的 Token。可设为如下一个值：

  - `null` 或者空字符串。
  - 临时 Token。临时 Token 服务有效期为 24 小时。你可以在控制台里生成一个临时 Token，详见[获取临时 Token](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#get-a-temporary-token)。
  - 在你的服务器端生成的 Token。在安全要求高的场景下，我们推荐你使用此种方式生成的 Token，详见[生成 Token](https://docs.agora.io/cn/Video/token_server)。
  
	 <div class="alert note">若项目已启用 App 证书，请使用 Token。</div>

- `channelName`：传入能标识频道的频道 ID。App ID 相同、频道 ID 相同的用户会进入同一个频道。

- `uid`： 本地用户的 ID。数据类型为整型，且频道内每个用户的 uid 必须是唯一的。若将 `uid` 设为 0，则 SDK 会自动分配一个 `uid`，并在 `JoinChannelSuccess` 回调中报告。

```
engine.joinChannel(token, channelName, null, 0)
```

### 8. 离开频道

根据场景需要，如结束通话、关闭 app 或 app 切换至后台时，调用 `leaveChannel` 离开当前通话频道。

```
engine.leaveChannel()
```

## 运行项目

在 Android 或 iOS 设备中运行该项目。当成功开始语音通话时，你可以听到对方的说话声音。
