
---
title: 实现音频直播
description: 
platform: Electron
updatedAt: Fri Jul 31 2020 12:02:19 GMT+0800 (CST)
---
# 实现音频直播
本文介绍如何使用 Agora Electron SDK 快速实现音频互动直播。

## 示例项目

Agora 在 GitHub 提供一个开源的 [Agora Electron Quickstart](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) 示例项目。在实现相关功能前，你可以下载并查看源代码。

<div class="alert note">为避免 crash，请通过命令行运行 Agora Electron Quickstart 示例项目。</div>

## 开发环境要求

* Node.js 6.9.1 及以上
* Electron 1.8.3 或 3.0.6 或 4.2.8 或 5.0.8 或 6.1.7 或 7.1.2
* Win32（IA32）或 Darwin 操作系统

<div class="alert note">在 Windows 平台使用 Agora Electron SDK 时，请先安装 32 位的 Electron：<code>npm install -D --arch=ia32 electron</code>。否则你会收到报错：<code>Not a valid win32 application</code>。</div>

<div class="alert note">如果你的网络环境部署了防火墙，请根据<a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">应用企业防火墙限制</a>打开相关端口。</div>

## 准备开发环境

本节介绍如何创建项目，将 Agora SDK 集成进你的项目中，并添加相应的依赖项。

### 创建项目

参考 [Writing Your First Electron App](https://electronjs.org/docs/tutorial/first-app) 创建一个 Electron 项目。若已有 Electron 项目，可以直接参考 [集成 SDK](#integrate_sdk)。

<a name="integrate_sdk"></a>
### 集成 SDK 

选择如下任意一种方式将 Agora SDK 集成到你的项目中。

**方法一：通过 npm 直接安装 SDK**

1. 在你的项目文件路径，运行如下命令行安装最新版的 Agora SDK for Electron：

	```javascript
	npm install agora-electron-sdk
	```

2. 然后通过如下代码将 SDK 引入至你的项目中

	```javascript
	import AgoraRtcEngine from 'agora-electron-sdk'
	```
	

**方法二：官网下载 SDK 并引入**

<div class="alert note">如果你选择官网下载并引入的方式，请务必使用 Electron 3.0.6。</div>

1. 前往官网 [SDK Downloads](https://docs.agora.io/cn/Agora%20Platform/downloads) 页面下载 Agora SDK for Electron
2. 将下载下来的 SDK 拷贝至你的项目根目录下
3. 通过如下代码将 SDK 引入至你的项目中

	```javascript
	import AgoraRtcEngine from './agora-electron-sdk/AgoraSdk.js'
	```

### 修改配置

根据实际情况，选择如下任意一种方式，修改平台、Electron 预编译版本和架构的配置。

<div class="alert note">Agora 默认使用 1.8.3 版本的 Electron 进行编译。如果你的 Electron 版本和预编译版本不同，你会收到报错，如 <code>is compiled from Nodejs version 54, this version requires Nodejs version 64</code>。</div>

#### 修改 package.json 文件

在项目根目录下的 `package.json` 文件中修改配置。示例如下：

```json
{
  ......
  // Windows 平台
  "agora_electron": {
    "platform": "win32", // 只支持 win32
    "electron_version": "5.0.8", // 支持 1.8.3 或 3.0.6 或 4.2.8 或 5.0.8 或 6.1.7 或 7.1.2
    "prebuilt": true,
    "arch": "ia32" // 只支持 ia32
  },
}
```

```json
{
  ......
  // macOS 平台
  "agora_electron": {
    "platform": "darwin", // 只支持 darwin
    "electron_version": "5.0.8", // 支持 1.8.3 或 3.0.6 或 4.2.8 或 5.0.8 或 6.1.7 或 7.1.2
    "prebuilt": true
  },
}
```

#### 修改 .npmrc 文件

在 `.npmrc` 文件中修改配置。示例如下：

``` javascript
// Windows 平台
agora_electron_platform = win32 // 只支持 win32
agora_electron_dependent = 5.0.8 // 支持 1.8.3 或 3.0.6 或 4.2.8 或 5.0.8 或 6.1.7 或 7.1.2
agora_electron_arch = ia32 // 只支持 ia32
```

```javascript
// macOS 平台
agora_electron_platform = darwin // 只支持 darwin
agora_electron_dependent = 5.0.8 // 支持 1.8.3 或 3.0.6 或 4.2.8 或 5.0.8 或 6.1.7 或 7.1.2
```

### 安装依赖项

在你的项目文件夹下运行 `npm install` 安装依赖项。安装会自动触发 `npm run download`，你也可以到对应路径下手动安装。

如果你想用 Xcode 或 Visual Studio 调试，可以执行 `npm run debug` 命令行生成项目文件及带符号表的 SDK 文件。

至此，你已经将 Agora SDK for Electron 集成到你的项目中了。

## 实现互动直播

请参考 [Agora Electron Quickstart](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) 示例项目在你的项目中实现相关的音频互动直播功能。

## SDK 开源

[Agora SDK for Electron](https://www.npmjs.com/package/agora-electron-sdk) 在 GitHub 上开源，你可以前往参考或查阅源代码。Agora 也欢迎开发者贡献代码，以提高 Electron SDK 的易用性。

## 相关链接

[直播场景下，如何监听远端观众用户加入/离开频道的事件？](https://docs.agora.io/cn/faq/audience_event)
