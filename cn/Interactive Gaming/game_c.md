
---
title: Cocos Creator 快速开始
description: 
platform: Cocos Creator
updatedAt: Tue Oct 29 2019 03:01:33 GMT+0800 (CST)
---
# Cocos Creator 快速开始
## 前提条件

请确保满足以下开发环境要求：

- Cocos Creator v2.0.9 及以上
- 注册 Cococs Creator 账号，并根据提示绑定你的手机号码
- 移动端需要 Android 或 iOS 真机
- Web 端浏览器需满足[特定要求](https://docs.agora.io/cn/Audio%20Broadcast/web_prepare?platform=Web)
- 没有连接 VPN

## 集成 Agora 服务

创建一个新的 Cocos Creator 项目，然后按照如下步骤在你项目中开启 Agora 服务。

1. 在 Cocos Creator 中，打开你的项目。选中**面板**，在下拉菜单中选择**服务**，屏幕右边区域会出现**服务**面板。

2. 在**服务**面板中选中你的项目名称，然后点击**关联**。

	 ![](https://web-cdn.agora.io/docs-files/1562140562962)
	 
	 关联成功后，你的**服务**面板中就会出现 **Agora Service**了，如下图所示：
	 
	 ![](https://web-cdn.agora.io/docs-files/1562140613043)
	 
3. 在 **Agora Voice** 面板中点击**启用**按钮，然后在屏幕弹框中点击**确定**。

	 ![](https://web-cdn.agora.io/docs-files/1562140700136)
	 
4. 根据屏幕提示**确认开通** Agora Service 服务。

   ![](https://web-cdn.agora.io/docs-files/1554980416623)
	 
5. 开通后，你会在屏幕上看到如下界面。点击**前往控制台**，进入声网控制台。

   ![](https://web-cdn.agora.io/docs-files/1554980505910)
	 
	 系统已自动为你注册了一个声网的项目。在**项目管理**页，你可以看到项目对应的声网 App ID。在初始化引擎时需要传入 App ID。
	 
	 ![](https://web-cdn.agora.io/docs-files/1562140991881)
	 
   至此，你就成功地在 Cocos Creator 中为你的项目开通了 **Agora Service** 的服务。服务开通后，Cocos Creator 会自动下载和配置所有声网服务依赖的资源。

## 调用 API 实现游戏通话

现在你可以在项目中，调用声网 API 实现游戏通话。

![](https://web-cdn.agora.io/docs-files/1562322616769)

### 关键 API 调用时序

我们建议按照如下时序调用声网 API：

1. 初始化引擎：[agora undefined agora.joinChannel("", channel, "", 0);](../../cn/Interactive%20Gaming/game_coco.md)
3. 退出频道：[agora && agora.leaveChannel();](../../cn/Interactive%20Gaming/game_coco.md)

你还可以在 agora.on 方法中注册如下事件回调：

- 成功加入频道：`agora.on('join-channel-success', this.onJoinChannelSuccess, this);`
- 成功离开频道：`agora.on('leave-channel', this.onLeaveChannel, this);`
- 成功重新加入频道：`agora.on('rejoin-channel-success', this.onRejoinChannelSuccess, this);`

更多事件回调，请参考 [agora.on](../../cn/Interactive%20Gaming/game_coco.md) API 文档。

### 示例代码

声网提供一个开源的 [Cocos Creator Voice Demo](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-CocosCreator-Voice-Agora) 示例代码，你可以前往下载和体验，也可以参考其中的代码逻辑实现相关功能。

### 完整 API 参考

Cocos Creator 提供的 [JavaScript API](../../cn/Interactive%20Gaming/game_coco.md) 为方便使用仅实现了主要功能，如果需要其他扩展功能，请参考各平台完整版 API 文档。
