
---
title: 加入频道
description: android平台加入通信频道
platform: Android
updatedAt: Thu Jul 18 2019 10:33:36 GMT+0800 (CST)
---
# 加入频道
在加入频道前，请确保你已完成环境准备、安装包获取等步骤，详见[客户端集成](../../cn/Voice/android_audio.md)。

## 前提条件

该步骤中，你需要获取一个 Token，用于在加入频道时校验用户权限。

在 Dashboard 注册项目后，你可以获取一个临时 Token 用于测试。参考如下步骤获取临时 Token。

<a id="appcert"></a>

### 开启 App 证书

方法一：如果创建项目时，你直接勾选了 **APP ID + APP 证书+ Token（推荐）**。Dashboard 会自动开启 **App 证书**。

![](https://web-cdn.agora.io/docs-files/1562925509805)

方法二：如果创建项目时，你没有勾选  **APP ID + APP 证书+ Token（推荐）**，则参考如下步骤开启 App 证书。

1. 在**项目管理**页，找到刚创建的项目，点击**编辑**按钮。

![](https://web-cdn.agora.io/docs-files/1562926250060)
2. 然后点击 **App 证书**后面的**启用**按钮。

![](https://web-cdn.agora.io/docs-files/1562926258836)
3. 根据屏幕提示，在注册邮箱中确认启用 App 证书。
4. 回到**项目管理**页，会看到 **App 证书**显示已启用。

![](https://web-cdn.agora.io/docs-files/1562926274649)

**Note:** 若收件箱中没有确认邮件，请至订阅邮件或垃圾邮件中查找

### 获取临时 Token

在项目详情处，点击**生成临时 Token**，输入频道名，你就会在 **Token** 页面获取一个临时 Token。

![](https://web-cdn.agora.io/docs-files/1562926292439)

![](https://web-cdn.agora.io/docs-files/1562926303571)

## 实现方法

App 在加入频道前，需要先设置频道模式，再加入频道。

### 设置频道模式为通信
创建实例后，调用 `setChannelProfile` 方法设置频道模式。SDK 会根据所设置的频道模式使用不同的优化手段。 

在该方法中，将频道模式设置为通信模式。通信模式适用于语音或视频通话场景，如一对一聊天或群聊。频道中的任何用户都可以自由发言。该模式为默认模式。

> - 该方法必须在加入频道前调用才能生效。
> - 同一频道只能设置一种频道模式。如果需要切换频道模式，请先调用 `destroy` 方法销毁后重新创建一个 Engine 实例，再调用该方法将频道设置为其他模式。

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_COMMUNICATION);
```

### 加入通信频道
调用 `joinChannel` 方法加入频道。

在该方法中：

-   传入能标识用户角色和权限的 Token。测试环境下，你可以使用获取到的临时 Token。生产环境下，我们推荐你使用在自己的服务端生成的正式 Token。
-   传入能标识频道的频道 ID。输入相同频道 ID 的用户会进入同一个频道。
-   频道内每个用户的 UID 必须是唯一的。如果将 UID 设为 0，系统将自动分配一个 UID。

> 如果已在频道中，用户必须调用 `leaveChannel` 方法退出当前频道，才能进入下一个频道。

```
 private void joinChannel() {
    mRtcEngine.joinChannel("token", "channel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

## 相关文档

成功加入频道后，你可以使用 Agora SDK，实现如下功能进行语音通话：
* [发布和订阅音频流](../../cn/Voice/publish_android_audio.md)

如果在通话过程中，对音量、音效、音调等有特殊需求，你还可以：
* [调整通话音量](../../cn/Voice/volume_android_audio.md)
* [播放音效/音乐混音](../../cn/Voice/effect_mixing_android_audio.md)
* [使用耳返](../../cn/Voice/in-ear_android_audio.md)
* [调整音调、音色](../../cn/Voice/voice_effect_android_audio.md)
