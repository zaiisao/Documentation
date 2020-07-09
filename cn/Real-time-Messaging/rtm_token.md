
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Thu Jul 09 2020 10:09:58 GMT+0800 (CST)
---
# 校验用户权限
## 简介

Agora RTM SDK 提供两种鉴权机制：App ID 和 RTM Token。这两种鉴权机制的关系以及适用场景详见下图：

![](https://web-cdn.agora.io/docs-files/1555490792498)

## 使用 App ID 鉴权

每个 Agora 账号可以创建多个项目，而每个项目都有一个唯一标识，即 App ID。如果有人非法窃取了你的 App ID，他就可以在自己的项目中使用你的 App ID。因此，只使用 App ID 鉴权，安全性较低，我们推荐只在测试环境，或对安全要求不高的场景里使用 App ID 鉴权。

<a name = "Get-an-App-ID"></a>
获取 App ID 的步骤如下：

1. 进入 [Agora 注册页面](https://sso.agora.io/cn/login) ，按照屏幕提示创建一个开发者账号。
2. 登录控制台，在首页点击**创建**按钮，填写**项目名称**后创建一个新项目。
3. 项目创建完成后，控制台会自动跳转至**项目管理**页面，在这里，你可以查看该项目对应的 App ID。

在初始化客户端时，你需要填写 `appId` 参数。

## 使用 RTM Token 鉴权

登录 Agora RTM 系统时，你需要传入 RTM Token 参数。获取和使用 RTM Token 的步骤如下：

### 步骤一：获取 App ID

参考上文步骤[获取 App ID](#Get-an-App-ID)。

### 步骤二：获取 App 证书

获取 App 证书的步骤如下：

1. 登录[控制台](https://console.agora.io)。
2. 先点击左侧导航栏**概览**按钮进入**概览**页面，再点击目标项目右方的![](https://web-cdn.agora.io/docs-files/1593661501417)图标，进入**编辑项目**页面。
3. 点击 App 证书右方的**启用**按钮。仔细阅读关于 App 证书介绍后，根据屏幕提示，确认启用 App 证书。
4. 点击 App 证书右方的 ![](https://web-cdn.agora.io/docs-files/1551773294761) 图标，显示完整的 App 证书。如需隐藏 App 证书，再次点击 ![](https://web-cdn.agora.io/docs-files/1551773306258) 图标。

<div class="alert note"><li>通常 App 证书在启用一小时后生效。当项目的 App 证书被启用后，你只能使用 RTM Token 作为鉴权方式。</li><li><b>信令 Token 调试开关</b>暂不影响 RTM 项目，无需设置。</li></div>

### 步骤三：实现 RTM Token 生成器

我们在 GitHub 上提供了一个开源的 [Agora Dynamic Key](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 仓库。`./<language>/src` 文件夹下包含生成各种版本的 Dynamic key 和 Token 的源码。你可以使用 `RtmTokenBuilder` 生成 RTM Token。`./<language>/sample` 文件夹下包含用于演示 RTM Token 生成的示例代码。`RtmTokenBuilderSample` 可以用来演示如何生成 RTM Token。

<div class="alert note">RTM Token 采用业界标准化的 HMAC/SHA1 加密方案，在 Node.js、Java、Python、C++ 等绝大多数通用的服务端开发平台上均可获得所需加密库。具体加密方案可参看 <a href="http://en.wikipedia.org/wiki/Hash-based_message_authentication_code">Authentication code</a >。</div>

`RtmTokenBuilder` 参数说明（C++）

```c++
static std::string buildToken(const std::string& appId,
                                const std::string& appCertificate,
                                const std::string& userAccount,
                                RtmUserRole userRole,
                                uint32_t privilegeExpiredTs = 0);
```

| 参数               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| appId              | 你的项目 App ID。                                            |
| appCertificate     | 你的项目 App 证书。                                          |
| userAccount        | RTM 系统用户 ID。                                   |
| userRole           | 用户角色。暂时只支持一种角色，请使用默认值 `Rtm_User`。 |
| privilegeExpiredTs | 此参数暂不生效。你无需设置此参数。 |

<div class="alert note">每个 RTM Token 的有效期都是 24 小时。</div>

### 步骤四：部署 RTM Token 生成器到生产环境

部署 RTM Token 生成器到生产环境的步骤如下：
1. 在服务端实现一个 RTM Token 生成器。
2. 客户端需要 RTM Token 鉴权服务时，向服务端发送获取 RTM Token 的请求。
3. 服务端收到请求后 RTM Token 生成器生成一个 RTM Token，然后将生成的 RTM Token 发送给客户端。
4. 客户端收到新的 RTM Token 后调用<a href="https://docs-preview.agoralab.co/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2c33be67bfec02d69041f1e8978f4559"><code>renewToken</code></a> 方法更新 RTM Token。

