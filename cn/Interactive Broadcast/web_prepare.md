
---
title: 集成客户端
description: 
platform: Web
updatedAt: Fri Sep 06 2019 06:13:11 GMT+0800 (CST)
---
# 集成客户端
本文介绍在正式使用 Agora Web SDK 进行音视频通话前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

> 你可以访问 [Agora WebRTC Example](https://webdemo.agora.io/agora-web-showcase/) 直接尝试 Agora 提供的 demo，源码可参考 [Agora-Web-Tutorial-1to1](https://github.com/AgoraIO/Basic-Video-Call/tree/master/One-to-One-Video/Agora-Web-Tutorial-1to1) GitHub 仓库。

## 前提条件

1. 查看下表安装一款 Agora Web SDK 支持的浏览器。
  <table>
  <tr>
    <th>平台</th>
    <th>Chrome 58+</th>
    <th>Firefox 56+</th>
    <th>Safari 11+</th>
    <th>Opera 45+</th>
    <th>QQ 浏览器最新版</th>
    <th>360 安全浏览器</th>
    <th>微信浏览器</th>
  </tr>
  <tr>
    <td>Android 4.1+</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11+</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

<div class="alert info">除上表浏览器外，还有以下支持：
	<li>Agora Web SDK 2.5 及以上版本支持 Windows XP 平台的 Chrome 49 版本浏览器（仅支持 VP8 编解码，不能与 Native SDK 互通）。</li>
	<li>Agora Web SDK 2.7 及以上版本支持 Windows 10 平台的 Edge 浏览器，详见 <a href="https://docs.agora.io/cn/faq/browser_support#a-nameedgeaedge">Edge 浏览器支持</a>。</li>
	<li>Agora Web SDK 理论上还支持 360 极速浏览器，但未经过验证，不保证全部功能正常工作。</li>
</div>
<div class="alert note">以下场景中请务必将 Agora Web SDK 升级至 2.6 或更高版本:
	<li>iOS 12.1.4 及以上版本使用 Safari 浏览器</li>
	<li>macOS 上使用 Safari 12.1 及以上版本</li>
	</div>

2. 请确保已打开特定端口，详见[防火墙说明](../../cn/Agora%20Platform/firewall.md) 。
3. 请确保你已知悉发版说明中列出的问题，详见[已知问题和局限](../../cn/Interactive%20Broadcast/release_web_video.md) 及[常见问题回答](https://docs.agora.io/cn/search?type=faq&platform=Web)。


## 获取 Agora Web SDK 安装包并保存到项目下

选择如下任意一种方法获取 Agora Web SDK，并保存在你所工作的项目下：

### 方法 1. 使用 npm 获取安装包

使用该方法需要先安装 npm，详见 [npm 快速入门](https://www.npmjs.com.cn/getting-started/installing-node/)。

1. 运行安装命令
  `npm install agora-rtc-sdk`

	
2. 在你的项目文件开头添加以下代码

	```javascript
	import AgoraRTC from 'agora-rtc-sdk'
	```

### 方法 2. 使用 CDN 方法获取安装包

该方法无需在官网下载安装包。在项目相应的前端页面文件中，将如下代码添加到 `</body>` 上一行：

 ```javascript
<script src="https://cdn.agora.io/sdk/release/AgoraRTCSDK-2.9.0.js"></script>
```

### 方法 3. 从官网获取安装包

1. 从 Agora 官方网站[下载](https://docs.agora.io/cn/Agora%20Platform/downloads)最新版 Agora Web SDK 软件包。
2. 将下载下来的软件包中的 `AgoraRTCSDK-2.9.0.js` 文件保存到你所操作的项目下。
3. 在项目相应的前端页面文件中，对 `AgoraRTCSDK-2.9.0.js` 进行引用。

   ![](https://web-cdn.agora.io/docs-files/1563952664617)

> 此处的截图仅供参考，安装时请使用最新版的 SDK 和链接地址。

## 准备网页服务器

1. 安装本地网页服务器，如 Apache、 Nginx 或 Node.js 等。
2. 将下载下来的 Agora Web SDK 部署到网页服务器上。
3. 在网页服务器上用浏览器打开示例程序页面或者你自己创建的页面。

## 相关文档

完成了客户端集成，你可以使用 Agora SDK，依次实现左侧《快速开始》菜单栏下的步骤，进行通话/直播。
- 初始化
- 加入频道
- 发布和订阅流
