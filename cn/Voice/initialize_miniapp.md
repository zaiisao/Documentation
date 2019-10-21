
---
title: 初始化客户端对象
description: 微信小程序初始化客户端
platform: 微信小程序
updatedAt: Fri Jul 19 2019 08:25:52 GMT+0800 (CST)
---
# 初始化客户端对象
## 前提条件

在初始化客户端对象前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/miniapp_video.md)。

初始化过程中，你需要传入一个的 App ID。因此需要现在 Agora Dashboard 注册项目并获取 App ID。

1. 进入 [控制台](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。
2. 点击**项目列表**处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


## 实现方法
将项目的 App ID 填入 初始化客户端对象 `init` 方法，即可初始化客户端对象。

```
client.init(appId, onSuccess, onFailure);
```

> - 由于小程序 SDK 支持与 Native SDK 和 Web SDK 互通，请确保各 SDK 使用相同的 App ID。
> - 如果你的 App ID 启用了 App Certificate，则在 [加入频道](../../cn/Voice/join_live_mini.md) 时必须使用 Channel Key 或 Token。
> - 请确保在调用其他 API 前先调用 `client.init` 方法初始化客户端对象。


## 相关文档

完成初始化客户端对象后，你可以使用 Agora Miniapp SDK for WeChat，依次实现如下功能进行通话/直播：

- [加入频道](../../cn/Voice/join_mini.md)
- [发布和订阅流](../../cn/Voice/publish_mini.md)

如有需要，请在加入频道前[切换用户角色](../../cn/Voice/role_mini.md)，以加入直播频道。
