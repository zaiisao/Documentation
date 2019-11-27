
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Mon Nov 18 2019 06:42:20 GMT+0800 (CST)
---
# 校验用户权限
## 简介

针对用户的项目开发需求，Agora SDK 提供了两种鉴权机制：[App ID](#APPID) 和 [SignalingToken](#SignalingToken) 。下图描述这两种鉴权机制的关系以及适用场景：

<img alt="../_images/key_relation_signaling.jpg" src="https://web-cdn.agora.io/docs-files/cn/key_relation_signaling.jpg" style="width: 840px;"/>


其中：

-   App ID 易于获取，适用于对安全要求不高的场景

-   SignalingToken 安全性高，更适用于对安全要求较高的生产环境

-   App 证书仅用于生成 SignalingToken，不可单独使用。一旦启用了 App 证书，则必须使用 SignalingToken


<a name = "APPID"></a>

## App ID

在 Agora 官网注册后，你就可以创建项目，每一个项目的唯一标识就叫做 App ID。 一旦有人非法获取了你的 App ID，他就可以在 Agora 提供的 SDK 中使用你的 App ID，因此 Agora 建议你仅在测试阶段或对安全性要求不高的场景里使用 App ID。


> 用户若需更换 App ID， 需要先调用 destroy\(\) 方法销毁实例。

<a name = "GetAnAppID"></a>

### 获取 App ID

每个 Agora 账户下可创建多个项目，且每个项目有独立的 App ID。

1.  访问 [https://console.agora.io/](https://console.agora.io/)，按照屏幕提示依次填入您的邮箱、密码等信息。

2.  在 **项目列表** 页面点击 **添加新项目** 。按照屏幕提示填写您的手机号码，完成手机号验证。

3.  填写您想要的 **项目名**， 然后点击 **提交** 。

4.  在所创建的项目下查看 App ID。下图中红框标出的就是对应项目的App ID。获取 App ID 后即可使用对应的 Agora.io 服务功能。


### 使用 App ID

在调用 Agora SDK 的 API 接口实现功能时，如创建对象、加入频道，SDK 会需要你填入 App ID。将你获取到的 App ID 直接填入即可。


<a name = "SignalingToken"></a>

## SignalingToken

### 获取 SignalingToken

1.  获取 App ID。详见上文的[获取 App ID](#GetAnAppID) 。

2.  获取 App 证书。在上文项目列表中查看 App ID 的地方，启用该项目的 App 证书:

	1. 点击激活项目右上方的 **编辑** 按钮。

     <img alt="../_images/project_edit.png" src="https://web-cdn.agora.io/docs-files/cn/project_edit.png" />

 2. 点击 App 证书右方的 **启用** 按钮。仔细阅读关于 App 证书介绍后，根据屏幕提示，确认启用 App 证书。

     <img alt="../_images/enable_app_cert.png" src="https://web-cdn.agora.io/docs-files/cn/enable_app_cert.png" />

 3. 点击 App 证书后面的“眼睛”图标，显示完整的 App 证书。如需隐藏 App 证书，再次点击“眼睛”图标。

     <img alt="../_images/view_app_certificate.png" src="https://web-cdn.agora.io/docs-files/cn/view_app_certificate.png" />
		 
	>  -   将你的 App 证书保存在服务器端，且对任何客户端均不可见。
	>  -   通常 App 证书在启用一小时后生效。
	>  -   当项目的 App 证书被启用后，你必须使用 Token。例如: 在启用 App 证书前，你可以使用 App ID 加入频道。但启用了 App 证书后，你就必须使用 Token 加入频道。

3.  集成算法。使用以下方法生成令牌 (SignalingToken):
    **输入**:
    ```
    appId             = "C5D15F8FD394285DA5227B533302A518" //App ID
    appCertificate    = "fe1a0437bf217bdd34cd65053fb0fe1d" //App 证书
    expiredTime       = "1546271999" // 授权时间戳
    account           = "test@agora.io" //客户端定义的用户 ID
    ```

  下表中所有字段从前往后拼接:

 <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>字段</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>SDK 版本</td>
<td>SignalingToken 版本号，固定为 1 的字符串</td>
</tr>
<tr><td>App ID</td>
<td>App ID, 长度为 32 的字符串</td>
</tr>
<tr><td>授权时间戳</td>
<td>服务到期的 UTC 时间戳，用户在服务到期后，无法再登录 Agora 信令系统和使用其功能。长度为 10 的数字串。</td>
</tr>
<tr><td>签名</td>
<td><p>根据以下字段的输入，通过 MD5 算法和 hex 编码而成的 32 位字符串:</p>
<ul>
<li>account: 用户登录厂商 Agora 信令系统的账户</li>
<li>appId: 32 位 App ID 字符串</li>
<li>appCertificate: 32 位 App 证书字符串</li>
<li>expiredTime: 服务到期的 UTC 时间戳，用户在服务到期后，无法再登录 Agora 信令系统和使用其功能</li>
</ul>
</td>
</tr>
</tbody>
</table>
    
**输出**:
    
```
    token       = "1:appId:expiredTime:md5(account + appId + appCertificate + expiredTime)"
                = "1:C5D15F8FD394285DA5227B533302A518:1546271999:md5(test@agora.ioC5D15F8FD394285DA5227B533302A518fe1a0437bf217bdd34cd65053fb0fe1d1546271999)"
                = "1:C5D15F8FD394285DA5227B533302A518:1546271999:2d572d6e3a75ebde5a06626f40fe9684"
 ```


 生成的 token 即为登录 Agora 信令系统所需的 SignalingToken，详见[使用 SignalingToken](#SignalingToken) 。



> Agora 已在 GitHub 上提供了生成 SignalingToken 的示例代码：
> -   [C++](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/src/generatorSignalToken.h)
> -   [Go](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/go/src/SignalingToken/SignalingToken.go)
> -   [Java](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/java/src/io/agora/signal/SignalingToken.java)
> -   [Node.js](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/nodejs/src/SignalingToken.js)
> -   [PHP](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/php/src/SignalingToken.php)
> -   [Python](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/python/src/SignalingToken.py)


### 使用 SignalingToken

在用户请求登录 Agora 信令系统时:

1.  客户端向企业自己的服务器申请授权。

2.  服务器端收到请求后，通过 Agora 提供的算法生成 SignalingToken，返回给授权的客户端应用程序。

3.  客户端应用程序调用 login，参数 token 要求设为 SignalingToken。

4.  Agora 的服务器接收到 SignalingToken 信息，验证该通话是来自于合法用户，并允许访问 Agora 信令系统。




