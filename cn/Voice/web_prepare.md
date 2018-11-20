
---
title: 设置开发环境
description: 
platform: Web
updatedAt: Tue Sep 18 2018 00:48:01 GMT+0800 (CST)
---
# 设置开发环境
# 设置开发环境

本文介绍在正式使用 Agora Web SDK 进行音视频通话前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

本文使用声网提供的 [示例项目](https://github.com/AgoraIO/Agora-Web-Tutorial-1to1) 进行截图。

## 前提条件

1. 准备 Windows、Mac、iOS 或 Android 的机器，并确保你的机器满足以下条件：
   - Windows 机器：Windows 7+
   - Mac 机器：Safari 11+
   - iOS 机器：iOS 11.0+，仅支持 Safari 浏览器
   - Android 机器：Android 4.1+，CPU 及内存充足
2. 选择并安装如下一款浏览器：
   - Google Chrome 浏览器，Chrome 58 及以上版本（仅支持 HTTPS）
   - Firefox 浏览器，Firefox 56 及以上版本（仅支持 HTTPS）
   - Opera 浏览器，Opera 45 及以上版本（仅支持 HTTPS）
   - Safari 浏览器，Safari 11 及以上版本（仅支持 HTTPS）
   - QQ 浏览器，最新版本（仅支持 HTTPS，仅支持 PC 设备）
3. 请确保已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md) 。
4. 请确保你已知悉发版说明中列出的问题，详见 [已知问题和局限](https://docs.agora.io/cn/2.4/product/Video/Product%20Overview/release_web_video) 及 [常见问题回答](https://docs.agora.io/cn/2.4/faq/faq/web)。

## 创建 Agora 账号并获取 App ID

1. 进入 [https://dashboard.agora.io/](https://dashboard.agora.io/) ，按照页面提示创建一个开发者账号。
2. 登陆 Dashboard 页面，点击 **添加新项目**。

	<img alt="../_images/appid_1.jpg" src="https://web-cdn.agora.io/docs-files/cn/appid_1.jpg" style="width: 1133.0px; height: 372.0px;"/>

3. 填写 **项目名**，然后点击 **提交**。
4. 在你创建的项目下，查看并获取该项目对应的 **App ID**。开启视频通话时将会用到你的 **App ID**。

	<img alt="../_images/appid_2.jpg" src="https://web-cdn.agora.io/docs-files/cn/appid_2.jpg" style="width: 1141.0px; height: 334.0px;"/>

## 获取 Agora Web SDK 安装包并保存到项目下

选择如下任意一种方法获取 Agora Web SDK，并保存在你所工作的项目下：

### 方法 1. 使用 CDN 方法获取安装包

该方法无需在官网下载安装包。在项目相应的前端页面文件中，将 `<script src="http://cdn.agora.io/sdk/web/AgoraRTCSDK-2.4-latest.js"></script>` 添加到 `</body>` 上一行：

<img alt="../_images/web_sdk_cdn.png" src="https://web-cdn.agora.io/docs-files/cn/web_sdk_cdn.png" style="width: 1002.4px; height: 91.0px;"/>

### 方法 2. 从官网获取安装包

1. 从 Agora 官方网站 [下载](https://docs.agora.io/cn/2.4/download) 最新版 Agora Web SDK 软件包。

	<img alt="../_images/web_sdk_download.png" src="https://web-cdn.agora.io/docs-files/cn/web_sdk_download.png" style="width: 414.0px; height: 97.0px;"/>

2. 将下载下来的软件包中的 `AgoraRTCSDK-2.3.1.js` 文件保存到你所操作的项目下。
3. 在项目相应的前端页面文件中，对 `AgoraRTCSDK-2.3.1.js` 进行引用。

	<img alt="../_images/web_sdk_reference.jpg" src="https://web-cdn.agora.io/docs-files/cn/web_sdk_reference.jpg" style="width: 1064.0px; height: 150.0px;"/>

> 此处的截图仅供参考，安装时请使用最新版的 SDK 和链接地址。

## 准备网页服务器

1. 安装本地网页服务器，如 Apache、 Nginx 或 Node.js 等。
2. 将下载下来的 Agora Web SDK 部署到网页服务器上。
3. 在网页服务器上用浏览器打开示例程序页面或者你自己创建的页面。
