
---
title: Chrome 屏幕共享插件
description: 
platform: Web
updatedAt: Fri Aug 16 2019 13:58:55 GMT+0800 (CST)
---
# Chrome 屏幕共享插件
在 Chrome 上使用屏幕共享功能需要安装 Agora 提供的屏幕共享插件。

关于如何实现屏幕共享，详见 [实现网页端屏幕共享](../../cn/Quickstart%20Guide/screensharing_web.md)。

## 安装 Chrome 屏幕共享插件

按照以下步骤安装 Chrome 屏幕共享插件：

### 获取 Chrome 屏幕共享插件

点击[下载](http://download.agora.io/sdk/release/chrome-extension.zip) Chrome 屏幕共享插件，并将获取到的插件包解压。完整的插件文件夹结构如下图所示：

<img alt="../_images/chrome_extension_screenshare.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_screenshare.png" style="width:500px"/>


### 安装插件并获取插件 ID

在 Chrome 网页端安装获取到的 Chrome 插件。 首先打开你的 Chrome 浏览器，点击屏幕右上方的扩展按钮，选择 **更多工具** \> **扩展程序**。

<img alt="../_images/chrome_extension_install_1.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_install_1.png" />


然后点击 **加载已解压的扩展程序** ，点击你刚刚获取并解压的 **chrome-extension** 文件夹，然后点击 **选择** 。

<img alt="../_images/chrome_extension_install_2.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_install_2.png" />


完成插件安装后，你可以直接在 Chrome 浏览器界面查看你的插件 ID。

<img alt="../_images/chrome_extension_id.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_id.png" style="width: 500px;"/>


### 修改域名

点击打开插件文件夹中的 `manifest.json` 文件，然后将 `match` 行的域名替换为你的网页域名。

<img alt="../_images/chrome_extension_url.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_url.png" />



