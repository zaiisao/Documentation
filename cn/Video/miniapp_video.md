
---
title: 集成客户端
description: 
platform: 微信小程序
updatedAt: Tue Oct 22 2019 04:14:43 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora Miniapp SDK for WeChat 进行通话/直播前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

## 前提条件

请确保满足以下开发环境要求：

-   请确保已安装 [微信开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/devtools.html?t=2018323)。

-   请确保你的微信小程序基础库支持 `live-pusher` 及 `live-player` 组件，且这两个组件在微信开发者工具中打开。

-   请确保在微信公众平台账号的开发设置中，给予以下域名请求权限：

	```
	https://miniapp.agoraio.cn
	https://miniapp-1.agoraio.cn
	https://miniapp-2.agoraio.cn
	https://miniapp-3.agoraio.cn
	https://miniapp-4.agoraio.cn
	https://uni-webcollector.agora.io
	wss://miniapp.agoraio.cn
	```

-   请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


> 在集成微信小程序组件之前，Agora 建议你先阅读 [微信小程序开发官方文档](https://mp.weixin.qq.com/debug/wxadoc/dev/)。

## 开通微信小程序服务

第一次使用微信小程序支持需要先开通服务，步骤如下：

1. 登录 [控制台](https://dashboard.agora.io)，点击左侧导航栏 ![img](https://web-cdn.agora.io/docs-files/1551250582235) 按钮进入**产品用量**页面。
2. 在页面左上角展开下拉列表选择需要开通小程序支持的项目，然后点击**小程序**下的**分钟数**。
 ![](https://web-cdn.agora.io/docs-files/1566460437574)
4. 点击**开启小程序支持**。
5. 按照屏幕提示选择你用来发送请求的服务器所在地，然后点击**应用**。

开通成功后你就可以在用量页面看到小程序的使用情况。

## 创建微信小程序组件

在微信小程序中实现音视频功能，需要使用微信的 [live-player 组件](#live_player) 和 [live-pusher 组件](#live_pusher)。

<a name = "live_player"></a>
### live-player 组件

该组件用于实现微信小程序的实时音视频播放功能。开发者在创建该组件后，还需要在 js 文件中调用 API 接口对应的组件来实现该功能。详见 [微信小程序 API 说明](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-live-player.html)。

声网小程序中，创建 live-player 的示例代码如下：

```
<live-player
    id="player"
    src="{{rtmp 播放地址}}"
    mode="RTC"
    bindstatechange="playerStateChange"
    object-fit="fillCrop" />
```

<a name = "live_pusher"></a>
### live-pusher 组件

该组件用于实现微信小程序的实时音视频录制功能。开发者在创建该组件后，还需要在 js 文件中调用 API 接口对应的组件来实现该功能。详见 [微信小程序 API 说明](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-live-pusher.html)。

声网小程序中，创建 `live-pusher` 的示例代码如下：

```
<live-pusher
    url="{{rtmp 推流地址}}"
    mode="RTC"
    bindstatechange="recorderStateChange" />
```

## 集成小程序 SDK

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 声网小程序 SDK 并解压。

2.  将 SDK 包中到的 mini-app-sdk-production 文件复制到你的小程序项目文件夹中。

3.  使用 `require` 将小程序 SDK 集成到项目中：


```
const AgoraMiniappSDK = require('../../lib/mini-app-sdk-production.js');
```

其中 `../../lib/mini-app-sdk-production.js` 为你的 js 文件本地路径。

## 相关文档

完成了客户端集成后，你可以使用 Agora SDK，参考左侧《快速开始》菜单栏下的步骤，并结合下图中的 API 调用顺序，进行通话/直播：

![](https://web-cdn.agora.io/docs-files/1541990512316)

如果在使用过程中遇到问题，请参考[小程序 SDK 常见问题回答](https://docs.agora.io/cn/faq/wechat) 或 [错误代码和警告代码](../../cn/API%20Reference/the_error_wechat.md) 排除解决。
