
---
title: Token
description: 
platform: All Platforms
updatedAt: Fri Sep 18 2020 04:38:34 GMT+0800 (CST)
---
# Token
Token，也称动态密钥，是 app 用户在加入 RTC 频道或登录 RTM 系统时，Agora 采用的一种鉴权方式。

Token 签发服务器需要开发者自行部署。Token 有使用期限，因此能更好地保证通信安全。

生成 Token 时，开发者需要传入 App ID、App 证书、用户名和服务过期时间戳等参数。

Token 一旦过期，开发者会无法继续使用 Agora 的服务，app 用户会被踢出所在频道，或无法登录相关的服务系统。在 Token 即将或已经过期时，SDK 均会通过回调提醒 app 更新 Token。开发者需要在服务端重新生成 Token，然后将新的 Token 传给 SDK。 

<div class="alert info">相关链接：
	<li><a href="https://docs.agora.io/cn/Interactive%20Broadcast/token_server?platform=All%20Platforms">生成 RTC 产品的 Token</a></li>
	<li><a href="https://docs.agora.io/cn/Real-time-Messaging/rtm_token">生成 RTM 产品的 Token</a></li>
	<li><a href="https://docs.agora.io/cn/faq/token_error">如何处理 Token 相关错误码？</a></li>
</div>

<a href="../../cn/Agora%20Platform/terms.md"><button>返回术语库</button></a>
