
---
title: 服务端 RESTful API
description: Agora RESTful 形式的服务端 API 概览。
platform: All Platforms
updatedAt: Tue Sep 29 2020 10:11:47 GMT+0800 (CST)
---
# 服务端 RESTful API
除客户端 API 外，Agora 还提供 RESTful 形式的服务端 API，通过发送 HTTPS 请求就可以获取 （GET），更新（PUT）, 创建 （POST），和删除 （DELETE） 项目、用量等相关数据。

点击查看我们的[服务端 RESTful API](https://docs.agora.io/cn/rtc/restfulapi/) ![](https://web-cdn.agora.io/docs-files/1583736328279) 文档。该文档包含方法和参数的详细解释，并提供 **Try it out** 功能，使你在文档页内就能进行 RESTful API 的调用。

所有请求都发送给 BaseUrl：`https://api.agora.io`。

<div class="alert note">要查看各参数的详细解释，请点击 <b>Schema</b>。
	<img src="https://web-cdn.agora.io/docs-files/1601374019968"/> </div>




## API 概览

### 项目管理

| 请求 URL                                                     | 方法   | 功能                    |
| :----------------------------------------------------------- | :----- | :---------------------- |
| [BaseUrl/dev/v1/project](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/createProject) | POST   | 创建新项目              |
| [BaseUrl/dev/v1/project](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/getProject) | GET    | 获取指定项目            |
| [BaseUrl/dev/v1/project](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/deleteProject) | DELETE | 删除指定项目            |
| [BaseUrl/dev/v1/projects](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/getProjects) | GET    | 获取所有项目            |
| [BaseUrl/dev/v1/project_status](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/changeProjectStatus) | POST   | 禁用或启用项目          |
| [BaseUrl/dev/v1/recording_config](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/setRecordingServer) | POST   | 设置录制服务器 IP       |
| [BaseUrl/dev/v1/signkey](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/changeSignKey) | POST   | 启用或禁用主要 App 证书 |
| [BaseUrl/dev/v1/reset_signkey](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E9%A1%B9%E7%9B%AE%E7%AE%A1%E7%90%86/resetSignKey) | POST   | 重置主要 App 证书       |



### 查询项目用量

| 请求 URL                                                     | 方法 | 功能                   |
| :----------------------------------------------------------- | :--- | :--------------------- |
| [BaseUrl/dev/v3/usage](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E6%9F%A5%E8%AF%A2%E9%A1%B9%E7%9B%AE%E7%94%A8%E9%87%8F/getProjectUsagesV3) | GET  | 获取单个项目的用量数据 |



### 踢人规则管理

| 请求 URL                                                     | 方法   | 功能                   |
| :----------------------------------------------------------- | :----- | :--------------------- |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E8%B8%A2%E4%BA%BA%E8%A7%84%E5%88%99%E7%AE%A1%E7%90%86/createKickingRule) | POST   | 创建踢人规则           |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E8%B8%A2%E4%BA%BA%E8%A7%84%E5%88%99%E7%AE%A1%E7%90%86/listKickingRule) | GET    | 获取踢人规则列表       |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E8%B8%A2%E4%BA%BA%E8%A7%84%E5%88%99%E7%AE%A1%E7%90%86/updateKickingRule) | PUT    | 更新踢人规则的生效时间 |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E8%B8%A2%E4%BA%BA%E8%A7%84%E5%88%99%E7%AE%A1%E7%90%86/deleteKickingRule) | DELETE | 删除踢人规则           |



### 查询在线频道信息

| 请求 URL                                                     | 方法 | 功能                 |
| :----------------------------------------------------------- | :--- | :------------------- |
| [BaseUrl/dev/v1/channel/user/property/{appid}/{uid}/{channelName}](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E6%9F%A5%E8%AF%A2%E5%9C%A8%E7%BA%BF%E9%A2%91%E9%81%93%E4%BF%A1%E6%81%AF/userProperty) | GET  | 查询用户状态         |
| [BaseUrl/dev/v1/channel/user/{appid}/{channelName}](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E6%9F%A5%E8%AF%A2%E5%9C%A8%E7%BA%BF%E9%A2%91%E9%81%93%E4%BF%A1%E6%81%AF/userList) | GET  | 获取用户列表         |
| [BaseUrl/dev/v1/channel/{appid}](https://docs.agora.io/cn/rtc/restfulapi/?_ga=2.218864153.1695148571.1593515861-1969480941.1589793536#/%E6%9F%A5%E8%AF%A2%E5%9C%A8%E7%BA%BF%E9%A2%91%E9%81%93%E4%BF%A1%E6%81%AF/channelList) | GET  | 分页查询项目的频道列表 |
