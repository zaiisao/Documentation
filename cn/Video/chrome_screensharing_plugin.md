
---
title: Chrome 屏幕共享插件
description: 
platform: Web
updatedAt: Mon Oct 12 2020 07:34:44 GMT+0800 (CST)
---
# Chrome 屏幕共享插件
在 Chrome 上使用屏幕共享功能需要安装 Agora 提供的屏幕共享插件。

> 自 2.6.0 版本起，Agora Web SDK 支持在 Chrome 72 及以后版本实现[无插件屏幕共享](../../cn/Quickstart%20Guide/screensharing_web.md)。

## 获取 Chrome 屏幕共享插件

点击[下载](http://download.agora.io/sdk/release/chrome-extension.zip) Chrome 屏幕共享插件，并将获取到的插件包解压。完整的插件文件夹结构如下图所示：

<img alt="../_images/chrome_extension_screenshare.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_screenshare.png" style="width:500px"/>

## 添加域名

点击打开插件文件夹中的 `manifest.json` 文件，然后将你的网页域名添加到 `matches` 行。

例如，假设你使用 localhost 运行你的网页，将 `"*://localhost/*"` 添加到 `matches` 的值中：

```json
"matches": ["*://localhost/*","*://*.agora.io/*","*://webdemo.agora.io/*","*://webdemo.agorabeckon.com/*","*://videocall.agora.io/*"]
```

## 安装插件并获取插件 ID

首先打开你的 Chrome 浏览器，点击屏幕右上方的扩展按钮，选择 **更多工具** \> **扩展程序** 进入 Chrome 扩展程序页面。

<img alt="../_images/chrome_extension_install_1.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_install_1.png" />

在扩展程序页面右上角打开**开发者模式**开关。
![](https://web-cdn.agora.io/docs-files/1566269435008)

启用开发者模式后扩展程序菜单栏下方会出现三个按钮。

然后点击**加载已解压的扩展程序** ，点击你刚刚获取并解压的 **chrome-extension** 文件夹，点击**选择** 。

<img alt="../_images/chrome_extension_install_2.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_install_2.png" />

完成插件安装后，你可以直接在 Chrome 扩展程序页面查看你的插件 ID。在[使用 Chrome 插件屏幕共享](../../cn/Video/screensharing_web.md)时，你需要在 `extensionId` 中填入此插件 ID。

<img alt="../_images/chrome_extension_id.png" src="https://web-cdn.agora.io/docs-files/cn/chrome_extension_id.png" style="width: 500px;"/>
