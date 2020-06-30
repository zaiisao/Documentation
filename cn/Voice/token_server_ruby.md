
---
title: 在服务端生成 Token
description: 
platform: Ruby
updatedAt: Sun Jun 28 2020 02:28:41 GMT+0800 (CST)
---
# 在服务端生成 Token
本页为 Agora Native SDK v2.1+、Agora Web SDK v2.4+、Agora Recording SDK v2.1+ 以及 Agora RTSA SDK  的用户演示如何使用我们提供的 Demo 快速生成一个 RTC token，并提供 RTC token 生成相关的 Ruby API 参考。

## Token 代码仓库说明

你需要在业务服务器自行部署 Token 生成器生成 Token。我们的 [GitHub 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 为你提供了 Token 生成源码以及使用这些源码生成 Token 的简单示例。我们目前支持以下几种语言：

- CPP
- Java
- Python
- PHP
- Ruby
- Node.js
- Go

[GitHub 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 的 <b>./ruby/lib</b> 文件夹下包含生成各种版本的 Dynamic key 和 Token 的源码。其中：**AccessToken** 和 **RtcTokenBuilder** 用于为以下 SDK 生成 Token：

- Agora Native SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 
- Agora RTSA SDK



我们推荐使用 **RtcTokenBuilder** 而不是 **AccessToken** 生成 Token。**AccessToken** 实现了底层的核心算法，**RtcTokenBuilder** 实际上对 **AccessToken** 又进行了一层封装，提供了更为简化易懂的 Token 生成接口。

开源仓库的 **./\<language\>/sample** 文件夹下包含用于演示 Token 生成的示例代码。其中， **RtcTokenBuilderSample** 是我们基于 **RtcTokenBuilder** 编写的一个简单的 Token 生成器示例程序。你可以根据自己的业务逻辑对我们的示例程序做相应调整。

## 快速生成 Token

下面我们以 **rtc_token_builder_sample.rb** 为例演示 RTC token 生成的过程：

1. 请确保已安装 **Ruby** 且版本号大于 1.9。
     `ruby --version` 
3. 将 GitHub 仓库同步到本地。
4. 打开 **/ruby/sample/rtc_token_builder_sample.rb** 。
5. 用你自己的 App ID、App 证书以及 Channel Name 替换实例代码中的伪码。关于如何获取 App ID 和 App 证书，详见 [校验用户权限](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id)
    - 如果你使用 int 型 uid 加入频道，请注释掉以下代码段：
```Ruby
   result = AgoraDynamicKey::RtcTokenBuilder.build_token_with_account params_with_account
   puts "Token With UserAccount: #{result}"
```
    - 如果你使用 string 型 userAccount 加入频道，请注释掉以下代码段：
```Ruby
  result = AgoraDynamicKey::RtcTokenBuilder.build_token_with_uid params
  puts "Token With Int Uid: #{result}"
```
5. 打开你的本地终端，cd 进入到 **rtc_token_builder_sample.rb** 所在文件夹。
6. 运行指令： 
    `ruby rtc_token_builder_sample.rb` 
     *新生成的 RTC token 会在你的本地终端显示。*
		 


## API 参考

源码： [../ruby/lib/dynamic_key/rtc_token_builder.rb](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/ruby/lib/dynamic_key/rtc_token_builder.rb)

你可以通过调用 **Rrtc_token_builder.rb** 提供的公开方法创建自己的 Token 生成器。请注意，**rtc_token_builder.rb** 既支持 int 型 uid 也支持 string 型 userAccount，请根据需要选择合适的生成方法。


### build_token_with_uid

```Ruby
   def build_token_with_uid payload
        check! payload, %i[app_id app_certificate channel_name role uid privilege_expired_ts]
        build_token_with_account @params.merge(:account => @params[:uid])
      end
```

该方法支持用 int 型 uid 生成 RTC token。

| **参数**    | **描述**                                              |
| ---------------- | ------------------------------------------------------------ |
| `app_id`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token/#app-id). | 
| `app_certificate` | Agora 为应用程序开发者签发的 App 证书。启用 App 证书后你必须使用 Token 才能加入频道，详见 [开启 App 证书](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-certificate). |
| `channel_name`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>a-z,<li>A-Z,<li>0-9,<li>空格,<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `uid`            | 用户ID，32位无符号整数。建议设置范围：1到 (2<sup>32</sup>-1)，并保证唯一性。 |
| `role`          | 用户角色。不论设置何种角色， 用户的权限都相同。<li> `Role_Publisher = 1` ：（推荐）主播</li><li>`Role_Subscriber = 2`：观众</li> |
| `privilege_expired_ts`     | Token 过期时间戳，格式为自 1970 年 1 月 1 日零时起经过的秒数。比如，如果你将 `privilegeExpiredTs` 设为当前时间戳再加 600（秒），那么 Token 会在生成 10 分钟后过期。Token 的最大有效期为 24 小时。如果你设为 0，或超过 24 小时，Token 有效期依然是 24 小时。 |

### build_token_with_account

```Ruby
  def build_token_with_account payload
        check! payload, %i[app_id app_certificate channel_name role account privilege_expired_ts]
        @params.merge!(:uid => @params[:account])
        generate_access_token!
      end
```

该方法支持用 string 型 userAccount 生成 RTC token。

| **参数**    | **描述**                                             |
| ---------------- | ------------------------------------------------------------ |
| `app_id`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id). |
| `app_certificate` | Agora 为应用程序开发者签发的 App 证书。启用 App 证书后你必须使用 Token 才能加入频道，详见 [开启 App 证书](https://docs.agora.io/cn/Agora%20Platform/token/#app-certificate). |
| `channel_name`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>a-z,<li>A-Z,<li>0-9,<li>空格,<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
|`account` | 用户 User Account。该参数为必需，最大不超过 255 字节，不可为 null。请确保加入频道的 User Account 的唯一性。 以下为支持的字符集范围（共 89 个字符）：<li>26 个小写英文字母 a-z；<li>26 个大写英文字母 A-Z；<li>10 个数字 0-9；<li>空格；<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `role`          | 用户角色。不论设置何种角色， 用户的权限都相同。<li> `Role_Publisher = 1` ：（推荐）主播</li><li>`Role_Subscriber = 2`：观众</li> |
| `privilege_expired_ts`     | Token 过期时间戳，格式为自 1970 年 1 月 1 日零时起经过的秒数。比如，如果你将 `privilegeExpiredTs` 设为当前时间戳再加 600（秒），那么 Token 会在生成 10 分钟后过期。Token 的最大有效期为 24 小时。如果你设为 0，或超过 24 小时，Token 有效期依然是 24 小时。 |
	
## 相关链接
	
生成 Token 过程中，你也可以参考如下文档：

- [如何处理 RTC SDK 中 Token 相关错误码？](https://docs.agora.io/cn/faq/token_error)
- [如何处理云端录制 101 错误码？](https://docs.agora.io/cn/faq/101_error)
