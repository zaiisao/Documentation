
---
title: 创建并初始化 Client 对象
description: Web SDK 初始化客户端对象
platform: Web
updatedAt: Tue Oct 29 2019 03:00:16 GMT+0800 (CST)
---
# 创建并初始化 Client 对象
## 前提条件

在创建并初始化 Client 对象前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/web_prepare.md)。

初始化过程中，你需要传入一个的 App ID。因此需要现在控制台注册项目并获取 App ID。

1. 进入[控制台](https://console.agora.io/)，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面，点击**创建**按钮。

![](https://web-cdn.agora.io/docs-files/1574156100068)

3. 在弹出的对话框内输入**项目名称**，选择 App ID 作为**鉴权机制**，再点击“提交”。

![](https://web-cdn.agora.io/docs-files/1574921599254)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目，并找到对应的 App ID。

![](https://web-cdn.agora.io/docs-files/1574921811175)




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

