
---
title: 初始化客户端对象
description: 微信小程序初始化客户端
platform: 微信小程序
updatedAt: Tue Oct 29 2019 03:00:13 GMT+0800 (CST)
---
# 初始化客户端对象
## 前提条件

在初始化客户端对象前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Audio%20Broadcast/miniapp_video.md)。

初始化过程中，你需要传入一个的 App ID。因此需要现在控制台注册项目并获取 App ID。

1. 进入[控制台](https://console.agora.io/)，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Audio%20Broadcast/sign_in_and_sign_up.md)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面，点击**创建**按钮。

![](https://web-cdn.agora.io/docs-files/1574156100068)

3. 在弹出的对话框内输入**项目名称**，选择 App ID 作为**鉴权机制**，再点击“提交”。

![](https://web-cdn.agora.io/docs-files/1574921599254)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目，并找到对应的 App ID。

![](https://web-cdn.agora.io/docs-files/1574921811175)




## 实现方法
将项目的 App ID 填入 初始化客户端对象 `init` 方法，即可初始化客户端对象。

```
client.init(appId, onSuccess, onFailure);
```

> - 由于小程序 SDK 支持与 Native SDK 和 Web SDK 互通，请确保各 SDK 使用相同的 App ID。
> - 如果你的 App ID 启用了 App 证书，则在 [加入频道](../../cn/Audio%20Broadcast/join_live_mini.md) 时必须使用 Channel Key 或 Token。
> - 请确保在调用其他 API 前先调用 `client.init` 方法初始化客户端对象。


## 相关文档

完成初始化客户端对象后，你可以使用 Agora Miniapp SDK for WeChat，依次实现如下功能进行通话/直播：

- [加入频道](../../cn/Audio%20Broadcast/join_mini.md)
- [发布和订阅流](../../cn/Audio%20Broadcast/publish_mini.md)

如有需要，请在加入频道前[切换用户角色](../../cn/Audio%20Broadcast/role_mini.md)，以加入直播频道。
