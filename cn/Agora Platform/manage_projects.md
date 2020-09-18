
---
title: 创建和管理项目
description: 
platform: All Platforms
updatedAt: Fri Sep 18 2020 04:38:04 GMT+0800 (CST)
---
# 创建和管理项目
本页介绍如何在 Agora 控制台创建和管理项目。
<div class="alert info">角色为管理员和工程师的账户拥有查看<b>项目管理</b>页面的权限。</div>


## 创建新项目

<div class="alert info"> 一个账户最多可创建 10 个项目，包括已删除的项目。如果需要创建更多项目，请通过<a href="https://agora-ticket.agora.io/">工单系统</a >申请。</div>

创建新项目的步骤如下：

1. 登录控制台，点击左侧导航栏 ![](https://web-cdn.agora.io/docs-files/1594283671161) **项目管理**按钮进入[**项目管理**](https://dashboard.agora.io/projects)页面。


2. 在**项目管理**页面，点击**创建**按钮。

 ![](https://web-cdn.agora.io/docs-files/1594287028966)

3. 在弹出的对话框内输入**项目名称**，选择一种**鉴权机制**。
<div class="alert info">Agora 提供两种鉴权机制：<b>APP ID + APP 证书 + Token（推荐）</b>和 <b>APP ID</b>。我们推荐使用安全性更高的 <b>APP ID + APP 证书 + Token</b> 鉴权机制：<ul><li>在项目测试阶段，<a href="#primary">启用主要证书</a >后可以直接在控制台生成一个临时 Token 进行测试。详见<a href="https://docs.agora.io/cn/Agora%20Platform/token#get-a-temporary-token">获取临时 Token</a >。</li><li>项目准备正式上线时，你需要在你的服务端部署一个 Token Generator 来生成正式 Token。详见<a href="https://docs.agora.io/cn/Interactive%20Broadcast/token_server?platform=All%20Platforms">生成正式 Token</a >。</li></ul></div>
 
  ![](https://web-cdn.agora.io/docs-files/1594283833299)

4. 点击**提交**后，新建的项目就会显示在**项目管理**页中。Agora 会给每个项目自动分配一个 App ID 作为项目唯一标识。

## 管理已创建的项目

对于已创建的项目，你还可以在**项目管理**页面进行以下操作：

![](https://web-cdn.agora.io/docs-files/1594287052269)

- 查看项目基本信息，包括：

  - 项目阶段：**测试中**、**已上线**、**未表明**
  - 项目名称
  - 创建日期
  - App ID

- 点击 ![](https://web-cdn.agora.io/docs-files/1594284750215)，可在网页端即刻体验实时音视频通信。

- 点击 ![](https://web-cdn.agora.io/docs-files/1594284775010)，可生成**临时 Token**，用于在项目测试阶段进行鉴权。

  <div class="alert note">只有主要证书可以用来生成临时 Token，详见<a href="#primary">启用主要证书</a >。</div>

- 点击**用量**，查看项目用量。

- 点击**编辑**，进入**编辑项目**页面，编辑项目信息，包括项目阶段、项目名称、App 证书、临时 Token、项目状态等。

### 管理 App 证书

App 证书是 Agora 控制台为开发项目生成的字符串，用于开启 Token 鉴权。根据不同的安全需求，Agora 在项目中设置了两种 App 证书，区别如下：

- 主要证书：用于生成临时 Token 和正式 Token。你不能删除主要证书。
- 次要证书：只可用于生成正式 Token。启用次要证书后，你可以将次要证书与主要证书互换或删除次要证书。

<div class="alert note"><ul><li> <b>无证书</b>表示仅使用 App ID 鉴权。只有在创建项目时选择 <b>APP ID</b> 为鉴权机制，你才会看到<b>无证书</b>。</li>
	<li><b>次要证书</b>不适用于 RESTful API。</li></ul></div>


#### 启用主要证书<a name="primary"></a>

在对安全要求较高的场景中，你可以通过以下方法启用主要证书：

- 如果你在创建项目时，选择 **APP ID + APP 证书 + Token** 为鉴权机制，Agora 会默认为你启用主要证书。主要证书会显示在**编辑项目**页面，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592468375475) 查看并复制主要证书。
  ![](https://web-cdn.agora.io/docs-files/1594953313427)
- 如果你在创建项目时，选择 **APP ID** 为鉴权机制，那么你需要手动启用主要证书。在**编辑项目**页面，找到**主要证书**，点击**启用**。成功启用后，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592468410982) 查看并复制主要证书。此时，App ID 和主要证书生成的 Token 均可用于鉴权。
 ![](https://web-cdn.agora.io/docs-files/1594953349103)

#### 启用次要证书<a name="secondary"></a>

开启主要证书后，在需要更换 App 证书的场景中，你还可以启用次要证书。

在**编辑项目**页面，找到**次要证书**，点击**启用**。成功启用后，你可以点击 ![](https://web-cdn.agora.io/docs-files/1592468484328) 查看并复制主要证书。此时，主要证书和次要证书生成的 Token 均可用于鉴权。
![](https://web-cdn.agora.io/docs-files/1594953370867)

#### 更换和删除主要证书

启用主要证书和次要证书后，如果主要证书存在安全风险，你可以将次要证书转换为主要证书并删除原来的主要证书，从而规避风险。

参考如下步骤更换和删除 App 证书：

1. 找到次要证书，点击**切换为主证书**，次要证书就会和主要证书互换。切换后，使用原主要证书生成的临时 Token 会失效。
   ![](https://web-cdn.agora.io/docs-files/1594953435240)
2. 点击**删除**即可删除原主要证书。删除后，你无法恢复该证书，且使用该证书生成的 Token 会失效。你需要使用新的主要证书重新生成 Token 并鉴权。
3. 删除后，次要证书的状态会变成**未启用**。下次启用时会生成新的次要证书。

#### 删除无证书

只有在创建项目时选择 **APP ID** 为鉴权机制，Agora 才会为你启用**无证书**。**无证书**表示用户仅使用 App ID 鉴权，安全性较低。
![](https://web-cdn.agora.io/docs-files/1594953452229)
如果你需要提高安全性，你可以启用主要证书，并使用 Token 鉴权。启用主要证书后，你还可以删除**无证书**。
<div class="alert warning">一旦无证书被删除，你无法仅使用 App ID 鉴权，且项目无法重新开启无证书。</div>
 




