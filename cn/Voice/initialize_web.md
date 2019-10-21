
---
title: 创建并初始化 Client 对象
description: Web SDK 初始化客户端对象
platform: Web
updatedAt: Fri Jul 19 2019 08:26:47 GMT+0800 (CST)
---
# 创建并初始化 Client 对象
## 前提条件

在创建并初始化 Client 对象前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/web_prepare.md)。

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

### 创建 Client 对象
用 `AgoraRTC.createClient` 方法创建 Client 对象，设置 `mode` 和 `codec` 。

```javascript
var client = AgoraRTC.createClient({mode: 'live', codec: "h264"});
```

### 初始化 Client 对象
创建 Client 对象后，将项目的 App ID 填入 `client.init` 方法，即可初始化 Client。

```javascript
client.init(<APPID>, function () {
  console.log("AgoraRTC client initialized");

}, function (err) {
  console.log("AgoraRTC client init failed", err);
});
```

> 请确保在调用其他 API 前先调用 `create` 和 `client.init` 方法创建并初始化 AgoraRtcEngine。

## 相关文档
初始化 Client 对象后，你可以使用 Agora SDK，依次实现如下功能进行通话/直播：
- [加入频道](../../cn/Voice/join_web_audio.md)
- [发布和订阅音频流](../../cn/Voice/publish_web_audio.md)

如果对网络或音质有特殊的需求，你还可以在加入频道前：
- [使用双声道/高音质](../../cn/Voice/audio_profile_web.md)

