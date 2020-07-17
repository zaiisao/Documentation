
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Thu Jul 16 2020 09:53:43 GMT+0800 (CST)
---
# 校验用户权限
为保证通信安全，当用户登录 RTM 系统时，Agora 需要对其鉴权。Agora 提供如下鉴权方案，你可以根据自己的实际使用场景，选择合适的鉴权方式：

| 鉴权方案                      | 场景           |
| :---------------------------- | :------------- |
| 使用 App ID 鉴权              | 低安全需求场景 |
| 使用 RTM Token 鉴权               | 高安全需求场景 |
| 同时使用 App ID 和 RTM Token 鉴权 | 安全性升级场景 |

<div class="alert warning">为提高项目的安全性，Agora 会逐步取消对 App ID 鉴权方案的支持，建议你将所有项目升级至使用 RTM Token 鉴权。</div>

##  <a name = "appid"></a>使用 App ID 鉴权

开发者在 [Agora 控制台](https://console.agora.io/)注册账号后，可以创建多个项目，每一个项目的唯一标识就是 App ID。如果有人非法窃取了你的 App ID，他就可以在自己的项目中使用你的 App ID。因此，只使用 App ID 鉴权，安全性较低，我们推荐只在测试环境，或对安全要求不高的场景里使用 App ID 鉴权。

<div class="alert warning">为提高项目的安全性，Agora 会逐步取消对 App ID 鉴权方案的支持，建议你将所有项目升级至使用 RTM Token 鉴权。为保证不影响项目的运行，你可以<a href="#appid_token">同时使用 App ID 和 RTM Token 鉴权</a >进行灰度升级。</div>

<a name = "Get-an-App-ID"></a>参考如下步骤，获取并使用 App ID：

1. 进入**控制台**，按照屏幕提示注册账号并登录控制台。详见[注册与登录](https://docs.agora.io/cn/Agora%20Platform/sign_in_and_sign_up)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面。

3. 点击**创建**按钮。

 ![](https://web-cdn.agora.io/docs-files/1574156100068)

4. 在弹出的对话框内输入项目名称，选择 **APP ID** 为鉴权机制，并点击**提交**。

5. 项目创建成功后，你会在项目列表中看到刚刚创建的项目。点击 ![](https://web-cdn.agora.io/docs-files/1592488399929) 查看并复制该项目对应的 App ID。

 ![](https://web-cdn.agora.io/docs-files/1574921811175)

6. 在初始化客户端时，Agora 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。

7. 如果你需要升级至 RTM Token 鉴权方案，点击**编辑**进入**编辑项目**页面即可启用 App 证书。

 <div class="alert note">只有在创建项目时选择 <b>APP ID</b> 为鉴权机制，你才会看到<b>无证书</b>。<b>无证书</b>表示使用 App ID 鉴权。</div>

  ![](https://web-cdn.agora.io/docs-files/1592488560281)

## <a name = "Token"></a>使用 RTM Token 鉴权

Token 是一种动态密钥，通过 App ID、App 证书、用户名和 Token 有效时间戳等参数生成，安全性较高。在正式生产环境等对安全要求较高的场景中，我们推荐使用 RTM Token 鉴权。

### 生成 RTM Token

RTM Token 需要在你的服务端生成。参考如下步骤，在 Agora 控制台获取 App ID、启用 App 证书，然后调用 API 生成 RTM Token。

**1. 获取 App ID**

参考上文步骤[获取 App ID](#Get-an-App-ID)。

**<a name = "appcertificate"></a>2. 启用 App 证书**

App 证书是 Agora 控制台为开发项目生成的字符串。根据不同的安全需求，Agora 在项目中设置了两种 App 证书，区别如下：

- 主要证书：用于生成正式 RTM Token。启用主要证书后，你不能删除主要证书。
- 次要证书：用于生成正式 RTM Token。启用次要证书后，你可以将次要证书与主要证书互换或删除次要证书。
 
<div class="alert note"><li>通常 App 证书在启用一小时后生效。<li>将你的 App 证书保存在服务器端，且对任何客户端均不可见。<li>信令 Token 调试开关暂不影响 RTM 项目，无需设置。</li></div>
 
如果你是第一次启用 App 证书，你需要先启用主要证书。

参考如下步骤启用主要证书：

- 如果你在创建项目时，选择 **APP ID + APP 证书 + Token** 为鉴权机制，Agora 会默认为你启用主要证书。主要证书会显示在**编辑项目**页面，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592488399929) 查看并复制主要证书。
  ![](https://web-cdn.agora.io/docs-files/1592488920717)
- 如果你在创建项目时，选择 **APP ID** 为鉴权机制，那么你需要手动启用主要证书。在**编辑项目**页面，找到**主要证书**，点击**启用**。成功启用后，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592488399929) 查看并复制主要证书。
  ![](https://web-cdn.agora.io/docs-files/1592488560281)

**<a name = "generatetoken"></a>3. 生成 RTM Token**

我们在 GitHub 上提供了一个开源的 [Agora Dynamic Key](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 仓库。`./<language>/src` 文件夹下包含生成各种版本的 Dynamic key 和 RTM Token 的源码。你可以使用 `RtmTokenBuilder` 生成 RTM Token。`./<language>/sample` 文件夹下包含用于演示 RTM Token 生成的示例代码。`RtmTokenBuilderSample` 可以用来演示如何生成 RTM Token。

<div class="alert note">RTM Token 采用业界标准化的 HMAC/SHA1 加密方案，在 Node.js、Java、Python、C++ 等绝大多数通用的服务端开发平台上均可获得所需加密库。具体加密方案可参考 <a href="http://en.wikipedia.org/wiki/Hash-based_message_authentication_code">Authentication code</a >。</div>

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

**4. 更换和删除 App 证书**

开启主要证书后，你还可以启用次要证书，次要证书可以用于生成正式 RTM Token。如果主要证书存在安全风险，你可以将次要证书转换为主要证书并删除原来的主要证书，从而规避风险。

参考如下步骤更换和删除 App 证书：

1. 在**编辑项目**页面，找到**次要证书**，点击**启用**。成功启用后，用户可以使用主要证书和次要证书生成的 RTM Token 登录 RTM 系统。
   ![](https://web-cdn.agora.io/docs-files/1592489766225)
2. 点击**切换为主证书**，次要证书就会和主要证书互换。
   ![](https://web-cdn.agora.io/docs-files/1592489781768)
3. 点击**删除**即可删除原主要证书。删除后，你无法恢复该证书，且使用该证书生成的 RTM Token 会失效。你需要使用新的主要证书重新生成 RTM Token 并鉴权。
4. 删除后，次要证书的状态会变成**未启用**。下次启用时会生成新的次要证书。

### 使用 RTM Token

你可以参考如下步骤使用 RTM Token：

1. 客户端需要 RTM Token 鉴权服务时，向服务端发送获取 RTM Token 的请求。
2. 服务端收到请求后 RTM Token 生成器生成一个 RTM Token，然后将生成的 RTM Token 发送给客户端。
3. RTM Token 具有有效期。在 RTM Token 失效后，你需要在客户端调用<a href="https://docs-preview.agoralab.co/cn/Real-time-Messaging/API%20Reference/RTM_cpp/classagora_1_1rtm_1_1_i_rtm_service.html#a2c33be67bfec02d69041f1e8978f4559"><code>renewToken</code></a> 方法更新 RTM Token，并使用新的 RTM Token 登录 RTM 系统。

## <a name="appid_token"></a>同时使用 App ID 和 RTM Token 鉴权

<div class="alert warning">为提高项目的安全性，Agora 会逐步取消对 App ID 鉴权方案的支持，建议你将所有项目升级至使用 RTM Token 鉴权。</div>

如果你仅使用 App ID 鉴权，你可以参考如下步骤，同时使用 App ID 和 RTM Token 鉴权，对 app 进行灰度升级：

1. [启用主要 App 证书](#appcertificate)。

2. 成功启用主要 App 证书后，你可以使用主要证书[生成 RTM Token](#generatetoken)。

3. 同时使用 App ID 和 RTM Token 鉴权。例如，当现存用户都使用 App ID 鉴权时，你可以对新用户使用 RTM Token 鉴权，保障新老用户都可以登录 RTM 系统。再逐步将老用户的鉴权方案升级为使用 RTM Token 鉴权。
 ![](https://web-cdn.agora.io/docs-files/1593499734108)

4. 在全量用户切换至 RTM Token 鉴权后，Agora 建议你删除**无证书**。

	<div class="alert warning">一旦<b>无证书</b>被删除，你无法使用 App ID 鉴权，且项目无法重新开启<b>无证书</b>。</div>
