
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Fri Jun 19 2020 14:28:39 GMT+0800 (CST)
---
# 校验用户权限
为保证通信安全，当用户加入 RTC 频道或服务端开始录制时，Agora 需要对其鉴权。Agora 提供如下鉴权方案，你可以根据自己的实际使用场景，选择合适的鉴权方式：

| 鉴权方案                      | 场景           |
| :---------------------------- | :------------- |
| 使用 App ID 鉴权              | 低安全需求场景 |
| 使用 Token 鉴权               | 高安全需求场景 |
| 同时使用 App ID 和 Token 鉴权 | 安全性升级场景 |

<div class="alert warning">为提高项目的安全性，Agora 会逐步取消对 App ID 鉴权方案的支持，建议你将所有项目升级至使用 Token 鉴权。</div>

<a name = "appid"></a>
##  使用 App ID 鉴权

开发者在 [Agora 控制台](https://console.agora.io/)注册账号后，可以创建多个项目，每一个项目的唯一标识就是 App ID。如果有人非法窃取了你的 App ID，他就可以在自己的项目中使用你的 App ID。因此，只使用 App ID 鉴权，安全性较低，我们推荐只在测试环境，或对安全要求不高的场景里使用 App ID 鉴权。

<div class="alert warning">为提高项目的安全性，Agora 会逐步取消对 App ID 鉴权方案的支持，建议你将所有项目升级至使用 Token 鉴权。为保证不影响项目的运行，你可以<a href="#appid_token">同时使用 App ID 和 Token 鉴权</a >进行灰度升级。</div>

<a name = "getappid"></a>
参考如下步骤，获取并使用 App ID：

1. 进入**控制台**，按照屏幕提示注册账号并登录控制台。详见[注册与登陆](https://docs.agora.io/cn/Agora%20Platform/sign_in_and_sign_up)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面。

3. 点击**创建**按钮。

 ![](https://web-cdn.agora.io/docs-files/1574156100068)

4. 在弹出的对话框内输入项目名称，选择 **APP ID** 为鉴权机制，并点击**提交**。

5. 项目创建成功后，你会在项目列表中看到刚刚创建的项目。点击 ![](https://web-cdn.agora.io/docs-files/1592488399929) 查看并复制该项目对应的 App ID。

 ![](https://web-cdn.agora.io/docs-files/1574921811175)

6. 在调用 Agora 的 API 接口实现功能，如 SDK 初始化时，Agora 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。

7. 如果你需要升级至 Token 鉴权方案，点击**编辑**进入**编辑项目**页面即可启用 App 证书。

 <div class="alert note">只有在创建项目时选择 <b>APP ID</b> 为鉴权机制，你才会看到<b>无证书</b>。<b>无证书</b>表示使用 App ID 鉴权。</div>

  ![](https://web-cdn.agora.io/docs-files/1592488560281)

<a name = "Token"></a>
## 使用 Token 鉴权

Token 是一种动态密钥，通过 App ID、App 证书、用户名、频道名和 Token 有效时间戳等参数生成，安全性较高。在正式生产环境等对安全要求较高的场景中，我们推荐使用 Token 鉴权。

### Token 适用范围

如下产品可以用 Token 进行鉴权：

| 产品                      | 支持 Token 的版本 | 查看版本信息       |
| :------------------------ | :---------------- | :----------------- |
| 语音或视频 SDK (Native)   | v2.1.0 及以上     | `getSdkVersion`    |
| 语音或视频 SDK (Web)      | v2.4.0 及以上     | `AgoraRTC.VERSION` |
| 语音或视频 SDK (Electron) | 所有版本          | `getVersion`       |
| 语音或视频 SDK (Unity)    | 所有版本          | `GetSdkVersion`    |
| 本地服务端录制 SDK        | v2.1.0 及以上     | /                  |
| 云端录制录制              | 所有版本          | /                  |
| 实时码流加速 SDK          | 所有版本          | /                  |
| 互动游戏 SDK              | v2.2.0 及以上     | `getVersion`       |

如果你使用的产品版本低于上表中的版本，你可以升级版本或[使用 Channel Key 鉴权](https://docs.agora.io/cn/Agora%20Platform/channel_key?platform=All%20Platform)。如果你从使用支持 Channel Key 的版本升级到使用 Token 的版本，请参考[动态密钥升级指南](https://docs.agora.io/cn/Agora%20Platform/token_migration)。

### 生成 Token

Token 需要在你的服务端生成。参考如下步骤，在 Agora 控制台获取 App ID、启用 App 证书，然后调用 API 生成 Token。

**1. 获取 App ID**

参考上文步骤[获取 App ID](#getappid)。

<a name = "appcertificate"></a>
**2. 启用 App 证书**

App 证书是 Agora 控制台为开发项目生成的字符串。根据不同的安全需求，Agora 在项目中设置了两种 App 证书，区别如下：

- 主要证书：用于生成临时 Token 和正式 Token。启用主要证书后，你不能直接删除主要证书。
- 次要证书：只可用于生成正式 Token。启用次要证书后，你可以将次要证书与主要证书互换或删除次要证书。

如果你是第一次启用 App 证书，你需要先启用主要证书。

参考如下步骤启用主要证书：

- 如果你在创建项目时，选择 **APP ID + APP 证书 + Token** 为鉴权机制，Agora 会默认为你启用主要证书。主要证书会显示在**编辑项目**页面，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592488399929) 查看并复制主要证书。
  ![](https://web-cdn.agora.io/docs-files/1592488920717)
- 如果你在创建项目时，选择 **APP ID** 为鉴权机制，那么你需要手动启用主要证书。在**编辑项目**页面，找到**主要证书**，点击**启用**。成功启用后，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592488399929) 查看并复制主要证书。
  ![](https://web-cdn.agora.io/docs-files/1592488560281)

**3. 生成 Token**

<a name = "get-a-temporary-token"></a>
- 为方便在测试阶段中鉴权，Agora 控制台提供临时 Token。点击**生成临时 Token** 展开生成步骤：

 <details>
	<summary><font color="#3ab7f8">生成临时 Token</font></summary>
	
	<div class="alert note"><li>生成临时 Token 前，请确保你已开启主要 App 证书。详见<a href="#appcertificate">启用主要 App 证书</a >。<li>临时 Token 适用于对安全要求一般的测试场景。对于正式生产环境，我们推荐使用正式 Token。<li>临时 Token 不适用于 Agora RTM SDK。</li></div>
	
	进入<b>项目管理</b>页面，在项目列表中，点击 ![](https://web-cdn.agora.io/docs-files/1574923151660) 按钮。 
	
	![](https://web-cdn.agora.io/docs-files/1574922827899)
	
	进入 <b>Token</b> 页面，输入待加入的频道名，并点击<b>生成临时 Token</b> 即可。 
	
	![](https://web-cdn.agora.io/docs-files/1574928082984)

</details>
  
<a name = "generatetoken"></a>
- 在正式生产环境中，Agora 建议你在服务端调用 `buildTokenWithUid` 生成正式 Token。详见[生成 Token](https://docs.agora.io/cn/Audio%20Broadcast/token_server_cpp)。

<div class="alert note"><li>Agora 支持使用 C++、Java、Python、PHP 等语言在你的服务端生成正式 Token。本节 API 注释以 C++ 示例代码为例。<li>Token 采用业界标准化的 HMAC/SHA1 加密方案，在 Node.js、Java、Python、C++ 等绝大多数通用的服务端开发平台上均可获得所需加密库。具体加密方案可参看 <a href="http://en.wikipedia.org/wiki/Hash-based_message_authentication_code">Authentication code</a >。</li></div>
   
  示例代码
 
 ```c++
	static std::string buildTokenWithUid(
        const std::string& appId,
        const std::string& appCertificate,
        const std::string& channelName,
        uint32_t uid,
        UserRole role,
        uint32_t privilegeExpiredTs = 0);
```

  | 参数               | 描述                                                         |
  | :----------------- | :----------------------------------------------------------- |
  | appId              | 你的项目 App ID。                                            |
  | appCertificate     | 你的项目 App 证书。                                          |
  | channelName        | 标识通话的频道名称，长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）：<li>26 个小写英文字母 a-z<li>26 个大写字母英文 A-Z<li>10 个数字 0-9<li>空格<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ","</li> |
  | uid                | 用户 ID，32 位无符号整形。                                   |
  | role               | 用户角色。暂时只支持一种角色，请使用默认值 `Role_Publisher = 1`。 |
  | privilegeExpiredTs | Token 有效时间戳，为自 1970 年 1 月 1 日零时起经过的秒数。比如，如果你将 `privilegeExpiredTs` 设为当前时间戳再加 600 秒，那么 Token 会在生成 10 分钟后过期。Token 的最大有效期为 24 小时。如果你设为 0，或超过 24 小时，则 Token 有效期依然是 24 小时。 |

  我们在 GitHub 上提供了一个开源的 [Agora Dynamic Key](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 仓库，并覆盖了 C++、C#、Go、Java、Node.js、Perl、PHP、Python、Ruby 等语言。你可以根据实际开发环境，选择一种语言，查看 `src` 文件夹中的源代码或 `sample` 文件夹中的示例代码。

**4. 更换和删除 App 证书**

开启主要证书后，你还可以启用次要证书，次要证书可以用于生成正式 Token。如果主要证书存在安全风险，你可以将次要证书转换为主要证书并删除原来的主要证书，从而规避风险。

参考如下步骤更换和删除 App 证书：

1. 在**编辑项目**页面，找到**次要证书**，点击**启用**。成功启用后，用户可以使用主要证书和次要证书生成的 Token 加入同一频道。
   ![](https://web-cdn.agora.io/docs-files/1592489766225)
2. 点击**切换为主证书**，次要证书就会和主要证书互换。切换后，使用原主要证书生成的临时 Token 会失效。
   ![](https://web-cdn.agora.io/docs-files/1592489781768)
3. 点击**删除**即可删除原主要证书。删除后，你无法恢复该证书，且使用该证书生成的 Token 会失效。你需要使用新的主要证书重新生成 Token 并鉴权。
4. 删除后，次要证书的状态会变成**未启用**。下次启用时会生成新的次要证书。

### 使用 Token

你可以参考如下步骤使用 Token：

1. app 客户端调用 Agora 的 API 接口实现功能，如加入频道时，需要填入生成的 Token、以及生成 Token 时使用的用户名、频道名等。
2. Agora 服务端接收到填入的 Token 等信息后，会验证用户是否有权限访问频道。如果验证通过，用户可以加入频道并使用相应的 Agora 服务。
3. Token 具有有效期。在 Token 失效后，你需要在 app 服务端重新生成 Token，并使用新的 Token 加入频道或使用相应的 Agora 服务。

<a name="appid_token"></a>
## 同时使用 App ID 和 Token 鉴权

<div class="alert warning">为提高项目的安全性，Agora 会逐步取消对 App ID 鉴权方案的支持，建议你将所有项目升级至使用 Token 鉴权。</div>

为保证不影响项目的运行，你可以同时使用 App ID 和 Token 鉴权进行灰度升级。参考如下步骤进行灰度升级：

1. [启用主要 App 证书](#appcertificate)。

2. 成功启用主要 App 证书后，你可以使用主要证书[生成 Token](https://docs.agora.io/cn/Audio%20Broadcast/token_server_cpp)。

3. 同时使用 App ID 和 Token 鉴权。例如，当现存用户都使用 App ID 鉴权时，你可以对新用户使用 Token 鉴权，保障新老用户可以加入相同频道。再逐步将老用户的鉴权方案升级为使用 Token 鉴权。

4. 在全量用户切换至 Token 鉴权后，Agora 建议你删除无证书。

	<div class="alert warning">一旦无证书被删除，你无法使用 App ID 鉴权，且项目无法重新开启无证书。</div>

   
