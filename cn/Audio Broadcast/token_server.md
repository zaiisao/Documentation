
---
title: 在服务端生成 Token
description: Guide on how to generate tokens on the server side
platform: C++
updatedAt: Fri Jul 19 2019 03:02:12 GMT+0800 (CST)
---
# 在服务端生成 Token
## Token 代码仓库说明

你需要在业务服务器自行部署 Token 生成器生成 Token。我们的 [Github 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 为你提供了 Token 生成源码以及使用这些源码生成 Token 的简单示例。我们目前支持以下几种语言：

- CPP
- Java
- Python
- Ruby
- Node.js
- Go

[GitHub 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 的 **./\<language\>/src** 文件夹下包含生成各种版本的 Dynamic key 和 Token 的源码。其中：**AccessToken** 和 **SimpleTokenBuilder** 用于为以下 SDK 生成 Token：

- Agora RTC SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 

我们推荐使用 **SimpleTokenBuilder** 而不是 **AccessToken** 生成 Token。**AccessToken** 实现了底层的核心算法，**SimpleTokenBuilder** 实际上对 **AccessToken** 又进行了一层封装，提供了更为简化易懂的 Token 生成接口。

开源仓库的 **./\<language\>/sample** 文件夹下包含用门用于演示 Token 生成的示例代码。其中， **Sample_builder** 是我们基于 **SimpleTokenBuilder** 编写的一个简单的 Token 生成器示例程序。你可以根据自己的业务逻辑对我们的示例程序做相应调整。

本页为 Agora RTC SDK v2.1+、Agora Web SDK v2.4+ 以及 Agora Recording SDK v2.1+  的用户演示如何使用我们提供的 Demo 快速生成一个伪 Token，并提供 Token 生成相关的 C++ API 参考。

## 快速生成 Token

下面我们以 **sample_builder.cpp** 为例演示 Token 生成的过程：

1. 由于我们的实例代码依赖 openssl, 请确保已安装 **openssl** 库。macOS 平台可通过 `brew install openssl` 安装。
2. 将 GitHub 仓库同步到本地。
3. 打开 **/cpp/sample/sample_builder.cpp** 。
4. 用你自己的 App ID、App Certificate 以及 Channel Name 替换实例代码中的伪码。关于如何获取 App ID 和 App Certificate，详见 [校验用户权限](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id)
    - 如果你使用 int 型 uid 加入频道，请注释掉以下代码段：
```C++
  result = SimpleTokenBuilder::buildTokenWithUserAccount(
      appID, appCertificate, channelName, userAccount, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With UserAccount:" << result << std::endl;
```
    - 如果你使用 string 型 userAccount 加入频道，请注释掉以下代码段：
```C++
  result = SimpleTokenBuilder::buildTokenWithUid(
      appID, appCertificate, channelName, uid, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With Int Uid:" << result << std::endl;
```
> 如果只需了解 Token 的生成过程，可略过第四步。
5. 打开你的本地终端，cd 进入到 **sample_builder.cpp** 所在文件夹。
6. 运行指令 `g++ -std=c++0x -O0 -I../../ sample_builder.cpp -lz -lcrypto -o sample_builder` 。
    *相同文件夹下会生成一个可执行文件 <b>sample_builder</b>* 。
7. 在你的本地终端运行 **./sample_builder** 生成 Token。
     *新生成的伪 Token 会在你的本地终端显示。*
		 
> 假设你用的是 macOS 系统并遇到以下提示：fatal error: 'openssl/hmac.h' file not found ，你的 openssl 相关环境变量可能设置有误。可以在你的终端参考如下步骤排查问题：
>  1. 在本地终端运行指令： `which openssl`
>     *终端显示 `/usr/bin/openssl` 。*
>  2. `cd /usr/local/include`
>  3. `ln -s ../opt/openssl/include/openssl`


## API 参考

源码： [../cpp/src/SimpleTokenBuilder.h](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/src/SimpleTokenBuilder.h)

你可以通过调用 `SimpleTokenBuilder.h` 提供的公开方法创建自己的 Token 生成器。请注意，`SimpleTokenBuilder.h` 既支持 int uid 也支持 string userAccount，请根据需要选择合适的生成方法。


### buildTokenWithUid

```c++
   static std::string buildTokenWithUid(
       const std::string& appId,
       const std::string& appCertificate,
       const std::string& channelName,
       uint32_t uid,
       UserRole role,
       uint32_t privilegeExpiredTs = 0);
```

该方法支持用 int 型 uid 生成 Token。

| **参数**    | **描述**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token/#app-id). | 
| `appCertificate` | Agora 为应用程序开发者签发的 App Certificate。启用 App Certificate 后你必须使用 Token 才能加入频道，详见 [获取 App Certificate](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-certificate). |
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~ |
| `uid`            | 用户ID，32位无符号整数。建议设置范围：1到 (2<sup>32</sup>-1)，并保证唯一性。 |
| `role`           | <li>`Role_Attendee = 0`: 已废弃。通信模式的通话方。享有权限与角色 `Role_Publisher` 相同。<li> `Role_Publisher = 1` ：直播模式下的主播（BROADCASTER）。<li>`Role_Subscriber = 2`: (默认) 直播模式下的观众（AUDIENCE）。<li>`Role_Admin = 101` ：已废弃。享有权限与角色 `Role_Publisher` 相同。 |
| `privilegeExpiredTs`      | 时间戳。自 1970 年 1 月 1 日零时起经过的秒数。比如你希望将权限设为 Token 生成后 10 分钟，那么你要在这里把 privilegeExpiredTs 设为当前 timestamp 再加 600 (秒)。如果权限始终不过期，请填 0。 |

### buildTokenWithUserAccount

```c++
  static std::string buildTokenWithUserAccount(
      const std::string& appId,
      const std::string& appCertificate,
      const std::string& channelName,
      const std::string& userAccount,
      UserRole role,
      uint32_t privilegeExpiredTs = 0);
```

该方法支持用 string 型 userAccount 生成 Token。

| **参数**    | **描述**                                             |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id). |
| `appCertificate` | Agora 为应用程序开发者签发的 App Certificate。启用 App Certificate 后你必须使用 Token 才能加入频道，详见 [获取 App Certificate](https://docs.agora.io/cn/Agora%20Platform/token/#app-certificate). |
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~ |
|* `userAccount`* | 用户 User Account。该参数为必需，最大不超过 255 字节，不可为 null。请确保加入频道的 User Account 的唯一性。 以下为支持的字符集范围（共 89 个字符）：26 个小写英文字母 a-z；26 个大写英文字母 A-Z；10 个数字 0-9；空格；"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `role`           | <li>`Role_Attendee = 0`: 已废弃。通信模式的通话方。享有权限与角色 `Role_Publisher` 相同。<li> `Role_Publisher = 1` ：直播模式下的主播（BROADCASTER）。<li>`Role_Subscriber = 2`: (默认) 直播模式下的观众（AUDIENCE）。<li>`Role_Admin = 101` ：已废弃。享有权限与角色 `Role_Publisher` 相同。 |
| `privilegeExpiredTs`      | 时间戳。自 1970 年 1 月 1 日零时起经过的秒数。比如你希望将权限设为 Token 生成后 10 分钟，那么你要在这里把 privilegeExpiredTs 设为当前 timestamp 再加 600 (秒)。如果权限始终不过期，请填 0。|
