
---
title: 水晶球内嵌 (Beta)
description: 
platform: All Platforms
updatedAt: Tue Aug 11 2020 03:14:41 GMT+0800 (CST)
---
# 水晶球内嵌 (Beta)
水晶球的**水晶球内嵌**功能帮助你快速在你的内部系统中嵌入水晶球功能，仅需少量开发成本即可实现以下功能：

- 直接在内部系统中查看[水晶球功能列表页面](#featureList)，例如通话调查列表页面。
- 直接在内部系统中查看[通话调查详情页面](#callSearchDetails)，包含通话体验质量页面和端到端详情页面。
- 通过设置水晶球内嵌页面属性，在内部系统中实现你的业务功能。例如，结合内部系统的分级权限配置，为不同等级的员工设置不同水晶球内嵌页面的访问权限。

<div class="alert note">目前仅支持内嵌水晶球<a href="../../cn/Agora%20Platform/aa_call_search.md">通话调查</a >页面。</div>

## 前提条件

1. 已有安全可靠且有账号控制权限的内部系统。
2. 项目中已开启需内嵌的功能，例如通话调查。
3. 联系 [sales@agora.io](mailto:sales@agora.io) 为你的项目开通水晶球内嵌功能，并获取水晶球内嵌的客户 ID 与客户密钥。

<div class="alert note"><li>水晶球内嵌的客户 ID 与客户密钥与水晶球 RESTful API 的客户 ID 和客户证书不同。<li>为保证数据安全，使用该功能期间，开发者需配合声网在约定时间内完成产品的改造或升级。</li></div>

## 使用步骤

### 第一步：访问内嵌设置

1. 登录[控制台](https://console.agora.io/)，点击左侧菜单栏中的水晶球。

2. 在页面左上方的下拉菜单中选择你要查看的项目。

3. 进入需内嵌的功能页面，例如通话调查。

4. 点击页面右侧**内嵌**悬浮按钮，访问**内嵌设置**。

  ![](https://web-cdn.agora.io/docs-files/1591584948493)

### 第二步：获取水晶球功能页面 URL

参考下图代码，在服务端发送 HTTP 请求获取水晶球功能页面的 URL。

 ![](https://web-cdn.agora.io/docs-files/1597115664563)

#### 1. HTTP 基本认证

发送请求时，你需要通过 `clientId` 和 `clientSecret` 获取 `Authorization` 字段并填入 HTTP 请求头部。

- `clientId`: 客户 ID
- `clientSecret`: 客户密钥

<div class="alert note">水晶球内嵌的客户 ID 与客户密钥与水晶球 RESTful API 的客户 ID 和客户证书不同，你需要联系 <a href="mailto:sales@agora.io">sales@agora.io</a > 获取水晶球内嵌的客户 ID 与客户密钥。</div>

#### <a name="featureList"></a>2. 获取水晶球功能列表页面 URL

调用此方法，你可以获取水晶球功能列表页 URL，例如通话调查页面 URL。

**数据格式**
请求和响应的格式为 JSON。

**基本信息**

| 请求基本信息 | 描述                                                |
| :----------- | :-------------------------------------------------- |
| 方法         | POST                                                |
| 请求 URL     | https://analytics-lab.agora.io/api/getEmbedLocation |

**请求参数**

包体参数

| 参数    | 描述                                                        |
| :------ | :---------------------------------------------------------- |
| feature | 内嵌的水晶球功能。当前仅支持 `callSearch`，即通话调查功能。 |

**请求示例**

```
{
"feature": "callSearch"
}
```

**响应参数**

| 参数  | 描述                                                     |
| :---- | :------------------------------------------------------- |
| token | 动态密钥。有效期为 2 小时，你需要每 2 小时更新动态密钥。 |

**响应示例**

```
https://analytics-lab.agora.io/analytics/call/search?token=xxxxxxxxxxxxxxxxxxxxxx
```

#### <a name="callSearchDetails"></a>3. 获取通话调查详情页面 URL

将以下参数与 URL 拼接，即可获取通话调查详情页面 URL：

URL: https://analytics-lab.agora.io/api/analytics/research

| 参数    | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| token   | 动态密钥，从上一步获取的水晶球功能列表 URL 中获得。          |
| cname   | 频道名称。<br>**Note**<br>需填写正确的频道名称。如果频道名称不存在，你会获取通话搜索页面，但搜索结果为空。</br> |
| fromUid | （选填）发送端用户 ID。                                      |
| toUid   | （选填）接收端用户 ID。                                      |
| fromTs | （选填）查询开始时间，Unix 时间戳 （秒）。设置后，你可以查询该时间点（包含）之后的通话。                                      |
| toTs   | （选填）查询结束时间，Unix 时间戳 （秒）。设置后，你可以查询该时间点（包含）之前的通话。                                      |

<div class="alert note"><li>根据查询参数的设置，你会获取以下两种通话详情页面：<ul><ul><li>如果只填写了正确的 <tt>cname</tt>，你会获取通话体验质量页面。<li>如果填写了正确的 <tt>cname</tt>、<tt>fromUid</tt> 和 <tt>toUid</tt>，你会获取通话的端到端详情页面。</ul></ul><li>如果多个通话有相同的 <tt>cname</tt>，获取的是通话开始时间距离当前时刻最近的通话详情页面。<li>如果不设置 <tt>fromTs</tt> 和 <tt>toTs</tt>，则默认显示最近 14 天的搜索结果。</li></div>

拼接示例如下： 

```
https://analytics-lab.agora.io/api/analytics/research?token=xxxxxxxxxxxxxxxxxxxxxx&cname=xxxxxxxxxxxxxxxxxxxxxxxx&fromUid=xxxxxx&toUid=xxxxxx
```

拼接完成后，你需要内嵌通话调查详情页面 URL 至客户端。详见[内嵌水晶球页面至客户端](#embed)。

### <a name="featureListWithAttributes"></a>第三步：添加属性至水晶球功能列表页面 URL

在**页面属性**菜单，你可以设置水晶球内嵌页面的如下属性：

- 语言：选择**中文**或**English**。
- 时区：选择**本地时区**或 **UTC 时区**。
- 项目权限：选择**任意项目**或**指定项目**。
  - 任意项目：员工可访问该账号下开通了对应功能的任意项目。
  - 指定项目：员工仅可访问该账号下开通了对应功能的指定项目。选择指定项目后，你需要在下方**默认项目**列表中指定一个可访问的项目。
- 默认项目：Agora 默认将当前项目设置为内嵌页面的默认项目，你可以修改默认项目。

设置页面属性时，下图中代码会实时更新。

<div class="alert note">内嵌设置无法保存，页面刷新后会恢复默认设置。完成设置后，请及时拷贝代码。</div>

![](https://web-cdn.agora.io/docs-files/1591696559627)

页面属性代码中包含如下参数：

| 参数                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| locale              | 语言。<li>`zh`: 中文。<li>`en`: 英文。</li>                          |
| timezone            | 时区。<li>`UTC`: UTC 时区。<li>`Local`: 本地时区。</li>                   |
| showProjectSelector | 是否在页面上显示项目选择下拉框：<li>`true`: 显示。<li>`false`: 不显示。</li> |
| projectId           | （选填）项目 ID。<br>**Note**<br>只有对指定项目设置访问权限时需要填写此字段。</br> |

将上述参数与第二步获取的[水晶球功能列表页面 URL](#featureList) 拼接，即可获取含页面属性的水晶球功能列表页面 URL。拼接示例如下：

```
https://analytics-lab.agora.io/analytics/call/search?token=xxxxxxxxxxxxxxxxxxxxxx&locale=zh&timezone=UTC&showProjectSelector=true&projectId=xxxxxxxxx
```

### <a name="embed"></a>第四步：内嵌水晶球页面至客户端

选择客户端开发平台，复制下图代码片段至客户端。

![](https://web-cdn.agora.io/docs-files/1591696570915)

将第二步[获取的通话调查详情页面 URL](#callSearchDetails) 或第三步[拼接的水晶球功能列表页面 URL](#featureListWithAttributes) 传入 `iframeUrl`，即可在客户端查看对应页面。
