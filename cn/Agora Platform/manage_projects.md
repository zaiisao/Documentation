
---
title: 创建和管理项目
description: 
platform: All Platforms
updatedAt: Mon Oct 21 2019 07:50:49 GMT+0800 (CST)
---
# 创建和管理项目
本页介绍如何在 Agora 控制台创建和管理项目。

> 角色为管理员/工程师的账户拥有查看项目管理页面的权限。

## 创建新项目

创建新项目的步骤如下：

1. 登录 Agora Dashboard，点击左侧导航栏 ![](https://web-cdn.agora.io/docs-files/1551254998344) **项目管理**按钮进入[**项目管理**页面](https://dashboard.agora.io/projects)。

2. 在**项目管理**页面，点击**创建**按钮。

![](https://web-cdn.agora.io/docs-files/1558344557924)

3. 在弹出的对话框内输入**项目名称**，选择一种**鉴权机制**（App ID 或 App ID + App 证书 + Token）。

![](https://web-cdn.agora.io/docs-files/1563785012162)

> Agora 提供两种鉴权机制：App ID 和 App ID + App 证书 + Token。鉴权机制的介绍详见[校验用户权限](../../cn/Agora%20Platform/token.md)。我们推荐使用安全性更高的 Token 机制：
>
> - 在项目测试阶段，启用 App 证书后可以直接在 Dashboard 生成一个临时 token 进行测试。
> - 项目准备正式上线时，你需要在 Server 端部署一个 Token Generator 来生成正式 token。

4. 点击**提交**后，新建的项目就会显示在**项目管理**页中。

在最上方的图片中，项目 A 是鉴权机制为 Token 的项目，App 证书已开启，点击 ![](https://web-cdn.agora.io/docs-files/1558344584474) 可以生成**临时 Token**，用于项目测试阶段。项目 B 是鉴权机制为 App ID 的项目，App 证书未启用。

> 最多可创建 10 个项目，包括已删除的项目。如果需要创建更多项目，请通过工单系统申请。

### 启用 App 证书

如果你在创建项目时，选择 App ID 为鉴权机制，之后又想要更换为 App ID + App 证书 + Token，这时候你就需要先启用 App 证书，然后才能获取 token。

启用 App 证书的步骤如下：

1. 在**项目管理**页面，点击目标项目的![](https://web-cdn.agora.io/docs-files/1551255135678)**编辑**按钮，进入**编辑项目**页面。
2. 找到 App 证书一栏，点击**启用**按钮，仔细阅读关于 App 证书的提示。声网会给你发一封邮件，按照邮件中的提示进行确认，即可启用 App 证书。

![](https://web-cdn.agora.io/docs-files/1563785138740)

3. 成功启用后， App 证书会显示在**编辑项目**页面。

## 管理已创建的项目

![](https://web-cdn.agora.io/docs-files/1563785157670)

对于已创建的项目，你还可以在该页面进行以下操作：

- 输入项目名称或 App ID，点击 ![](https://web-cdn.agora.io/docs-files/1551255111208) 搜索项目。
- 点击 ![](https://web-cdn.agora.io/docs-files/1564049108155)，获取 App ID，编辑项目信息。
- 点击 ![](https://web-cdn.agora.io/docs-files/1564048876293)，查看项目用量。
- 点击 ![](https://web-cdn.agora.io/docs-files/1564048991389)，生成临时token。
- 根据![](https://web-cdn.agora.io/docs-files/1551255188685)和![](https://web-cdn.agora.io/docs-files/1551258332165)判断项目状态为禁用还是开启。
