
---
title: 集成客户端
description: 
platform: Electron
updatedAt: Mon Jul 08 2019 03:20:53 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora SDK for Electron 进行通话/直播前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

## 前提条件

请确保满足以下开发环境要求：

- Node.js 6.9.1 及以上
- Electron 1.8.3 及以上

> 使用 Windows 平台进行开发时，请运行 `npm install -D —arch = ia32 electron` 安装 32 位的 Electron。

### 创建项目并获取 App ID

参考以下步骤，在 Dashboard 创建项目并获取项目的 App ID：

1. 进入 [Agora Dashboard](https://dashboard.agora.io/)，根据屏幕提示注册账号、创建项目。
2. 点击 Dashboard 左侧的项目管理图标，查看你所创建的项目详情。
3. 在项目详情页，你可以查看你的 App ID。

### 集成 SDK

你可以通过 npm 直接安装并导入 SDK，也可以前往官网下载 SDK 后再进行导入：

**通过 npm 直接安装 SDK**

1. 在你的项目文件路径，运行如下命令行安装最新版的 Agora SDK for Electron：

	`nmp install agora-electron-sdk`

2. 然后通过如下代码将 SDK 引入至你的项目中

	`import AgoraRtcEngine from 'agora-electron-sdk'`
	
**官网下载 SDK 并引入**

1. 前往官网 [SDK Downloads](https://docs.agora.io/cn/Agora%20Platform/downloads) 页面下载 Agora SDK for Electron
2. 将下载下来的 SDK 拷贝至你的项目根目录下
3. 通过如下代码将 SDK 引入至你的项目中

	`import AgoraRtcEngine from 'agora-electron-sdk'`

> 如果你选择官网下载并引入的方式，请务必使用 Eletron 3.0.6。

### 修改 .npmrc 文件切换预编译版本

Agora 默认使用 1.8.3 版本的 Electron 进行编译。请根据你的 Electron 版本修改 .npmrc 文件，切换预编译版本：

```
// Downloads a prebuilt add-on with Electron 1.8.3
AGORA_ELECTRON_DEPENDENT = 2.0.0
// Downloads a prebuilt add-on with Electron 3.0.6
AGORA_ELECTRON_DEPENDENT = 3.0.6
// Downloads a prebuilt add-on With Electron 4.0.0
AGORA_ELECTRON_DEPENDENT = 4.0.0
```

### 安装依赖项

在你的项目文件夹下运行 `nmp install` 安装依赖项。安装会自动触发 `npm run download`，你也可以到对应路径下手动安装。
如果你想用 Xcode 或 Visual Studio 调试，可以执行 `npm run debug` 命令行生成项目文件及带符号表的 SDK 文件。

至此，你已经将 Agora SDK for Electron 集成到你的项目中了。请参考 [Agora Electron Github Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) 在你的项目中实现相关的实时音视频功能。
