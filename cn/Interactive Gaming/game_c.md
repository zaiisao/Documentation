
---
title: Cocos Creator 快速开始
description: 
platform: Cocos Creator
updatedAt: Wed Jul 03 2019 08:16:17 GMT+0800 (CST)
---
# Cocos Creator 快速开始
## 前提条件

请确保满足以下开发环境要求：

- Cocos Creator v2.0.9 
- 注册 Cococs Creator 账号，并完成账号认证（系统认证需要 1-3 天）
- 移动端需要 Android 或 iOS 真机
- Web 端浏览器需满足[特定要求](https://docs.agora.io/cn/Audio%20Broadcast/web_prepare?platform=Web)
- 没有连接 VPN

## 创建新的项目

如果你的项目已存在，请略过本节内容。

1. 打开 Cocos Creator，选择**新建项目**。

	 ![](https://web-cdn.agora.io/docs-files/1551852700055)

2. 在**新建项目**页面，选择**空白项目**，设置项目所在路径，点击**新建项目**。

	 ![](https://web-cdn.agora.io/docs-files/1551852902872)

至此，一个新项目就创建成功了。你会看到如下界面：

![](https://web-cdn.agora.io/docs-files/1551852922649)

## 集成 Agora 服务

1. 在 Cocos Creator 中，打开你的项目。选中**面板**，在下拉菜单中选择**服务**，屏幕右边区域会出现**服务**面板。
   ![](https://web-cdn.agora.io/docs-files/1552018474387)

2. 在**服务**面板中选中你的项目名称，然后点击**关联**。该项目名称是你之前在 Cocos 系统新建的项目名称。

	 ![](https://web-cdn.agora.io/docs-files/1554980187638)
	 
	 关联成功后，你的**服务**面板中就会出现 **Agora Service**了，如下图所示：
	 
	 ![](https://web-cdn.agora.io/docs-files/1554980246044)
	 
3. 在 **Agora Voice** 面板中点击**启用**按钮，然后在屏幕弹框中点击**确定**。

	 ![](https://web-cdn.agora.io/docs-files/1554980346105)
	 
4. 根据屏幕提示**确认开通** Agora Service 服务。

   ![](https://web-cdn.agora.io/docs-files/1554980416623)
	 
5. 开通后，你会在屏幕上看到如下界面。点击**前往控制台**，进入声网 Dashboard，系统已自动为你注册了一个声网的项目，项目详情内有你的 App ID。

   ![](https://web-cdn.agora.io/docs-files/1554980505910)
	 
   至此，你就成功地在 Cocos Creator 中为你的项目开通了 **Agora Service** 的服务，并获取了该项目的声网 App ID。在编译和运行示例代码程序，或调用 API 实现功能时，都需要使用 App ID。

   服务开通后，Cocos Creator 会自动下载和配置所有声网服务依赖的资源。

6. 在你的项目中，调用 API 实现游戏通话。关键 API 的调用方法及逻辑可参考 [`HelloAgora` Demo](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-Cocos-Creator-Voice-Agora)。
![](https://web-cdn.agora.io/docs-files/1551929077432)

> Cocos Creator 提供的 [JavaScript API](../../cn/Interactive%20Gaming/game_coco.md) 为方便使用仅实现了主要功能，如果需要其他扩展功能，请参考各平台完整版 API 文档。
