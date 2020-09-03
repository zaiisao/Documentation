
---
title: 如何开通连麦鉴权？
description: 
platform: All Platforms
updatedAt: Thu Sep 03 2020 18:27:38 GMT+0800 (CST)
---
# 如何开通连麦鉴权？
## 功能介绍

连麦鉴权，主要用于控制当前用户是否有发布流的权限，需要开发者通过自己的业务服务端来校验 Token 实现。

## 开通流程

连麦鉴权功能默认不开启。你可以联系 sales@agora.io，或[提交工单](https://agora-ticket.agora.io/)，并提供项目的 App ID，申请开启连麦鉴权服务。请确保你提供的 App ID 对应的项目已开启 App 证书。

## App 层实现逻辑

一旦你的项目开通了连麦鉴权服务，则用户在频道中发流，需要同时满足两个条件：

- 在 `setClientRole` 中设置的 `role` 参数为 `BROADCASTER`。
- 在生成 Token 的代码中设置的 `role` 参数为 `Publisher`。

你可以参考如下步骤在业务层对连麦用户的发流权限进行校验：

1. 加入频道前，客户端向业务服务器申请角色为 `Subscriber` 的 Token。业务服务器将生成的角色为 `Subscriber` 的 Token 回传给客户端。
2. 客户端在调用 `joinChannel` 方法时，传入以 `Subscriber` 角色生成的 Token。
3. 客户端由观众切换为主播前，向业务服务器申请角色为 `Publisher` 的 Token。业务服务器将生成的角色为 `Publisher` 的 Token 回传给客户端。
4. 客户端调用 `renewToken` 方法将新的 Token 同步给 Agora 服务器。
5. 客户端调用 `setClientRole` 将用户角色切换为主播。

Agora 服务器会在调用 `setClientRole` 方法的同时校验用户权限，如果 Token 角色为 `Publisher`，则客户端成功获得发布流的权限。
