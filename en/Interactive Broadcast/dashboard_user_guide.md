
---
title: Dashboard User Guide
description: 
platform: All Platforms
updatedAt: Wed Dec 05 2018 07:05:19 GMT+0000 (UTC)
---
# Dashboard User Guide
Welcome to Agora Dashboard! Here, you can do things like checking your usage and the QoE, managing projects and team members, getting Agora technical support, and more.

Before using Agora Dashboard, please head on over to [https://www.agora.io/en/](https://www.agora.io/en/) to create an Agora account. Learn how to [create an Agora account](../../en/Interactive%20Broadcast/sign_in_and_sign_up.md) and how to [reset your password](../../en/Interactive%20Broadcast/sign_in_and_sign_up.md) if you forget it.

## Functions

<table>
<tr>
<th>Function</th>
<th>Description</th>
</tr>
<tr>
<td>Check Usage</td>
<td>Check the duration of each voice and video call during a specific time frame and calculate your fees.
T</td>
</tr>
<tr>
<td>Check Quality of Experience</td>
<td>Get an intuitive overview of the performance of our products by reading and analyzing the various charts based on the Agora products associated with your project and quickly identify problems.</td>
</tr>
<tr>
<td>Manage Projects</td>
<td>Create and manage projects and get the App ID and App Certificates.</td>
</tr>
<tr>
<td>Manage Team Members</td>
<td>在项目中添加并管理成员角色，设置角色对应的项目管理权限。</td>
</tr>
<tr>
<td>Submit and Check a Ticket</td>
<td>If you have any question when using the Agora products, search for solutions or submit a ticket for Agora technical support.</td>
</tr>
<tr>
<td>Use RESTful API</td>
<td>Use the RESTful APIs to ban users, check usage and inquire some online statistcis at the server.</td>
</tr>
<tr>
<td>Other Functions</td>
<td>Change the profile information and account settings, and reach out to developer communities.</td>
</tr>
</table>

## Check Usage

Click on ** Usage > Usage Statistics** in the left-side navigation menu and enter the page of **Usage Details**, where you can check the total usage of voice and video calls in a specific channel during a specific time frame and calculate your fees.charts 

![](https://web-cdn.agora.io/docs-files/1542965752366)

如上图所示，选择频道和周期后，可查看该频道在所选周期内的用量统计。该图展示频道中包括 [主播](../../cn/Agora%20Platform/terms.md) 和 [观众](../../cn/Agora%20Platform/terms.md) 在内的所有用户的通话用量，其中，
As shown in the figure above, you select the channel, enter the time frame, and then view the total usage (minute) of audio, SD Video, HD Video and HD+ Video respectively on a daily basis. These figures include the usage of all users, including the [hosts](../../cn/Agora%20Platform/terms.md) and [audience](../../cn/Agora%20Platform/terms.md) in the selected channel.

* **Audio Usage**: The duration of audio The total number of minutes of audio calls for this channel during the selected period. The total number of audio calls for this channel between November 16 and 22, 2018 is 618,958 minutes.该频道在所选周期内音频通话的总分钟数。如图所示该频道在 2018 年 11 月 16 日至 22 日期间的音频通话的总分钟为 618958 分钟。
* **SD Video Usage**: 该频道在所选周期内视频分辨率小于 360p 的标清视频通话的总分钟数，目前视频通话质量均按 HD 视频总量计费。Resolutions <360P
* **HD Video Usage**: 该频道在所选周期内视频分辨率介于 360p 和 720p 之间的高清视频通话的总分钟数。如图所示该频道在 2018 年 11 月 16 日至 22 日期间的 HD 视频通话的总分钟为 145964 分钟。360p < Resolutions < 720p
* **HD+ Video Usage**: 该频道在所选周期内视频分辨率大于 720p 的高清视频通话的总分钟数。如图所示该频道在 2018 年 11 月 19 日的 HD+ 视频通话的总分钟为 1328 分钟。720p < Resolutions

> 关于音视频对应的计费方式，详见 [计费](https://docs.agora.io/cn/Agora%20Platform/billing_faq)。

## Check the QoE

You can check the QoE in Agora Analytics, which provides data of the call process in diagrams, covering device status, user events, bitrates of the sent/received audio and video, the freeze time in rendering the audio and video and the packet loss rates of the audio and video. You can also quickly identify the issues according to the diagrams.

Click on **Analytics > Call Search** in the left-side navigation menu and enter the **QoE Page**.

After using our products, you can select a **Project** and **Time Frame**, and enter a **Channel Name** or **UID** to check the QoE. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).

![](https://web-cdn.agora.io/docs-files/1543913574811)

## Manage Projects

Click on **Project > Project List** in the left-side navigation menu and enter the **Project Page**, where you can create and manage projects, and get App IDs and App Certificates.

#### Create a Project and Get an App ID

See the detailed introduction and steps of [creating a project and getting an App ID](../../cn/Interactive%20Broadcast/token.md).

> You can sort the existing projects by the creation time, project name, and project status, or look for a specific project by entering the project name.
>
> 若需要修改项目名称，可点击**编辑**进行修改或添加录制服务器 IP 或域名。Click on Edit to modify the project name and add the recording server IP or domain name.

#### Get App Certificates

For the detailed introduction and steps of getting [App Certificates](../../cn/Interactive%20Broadcast/token.md).

#### Ban a User

If you want to ban a user in the app, you can use the [Dashboard RESTful API](https://docs.agora.io/cn/Interactive%20Broadcast/dashboard_restful_live?platform=All_Platforms#5-api) of banning user at the server, or contact the Agora technical support for the permissions ，也可以联系 Agora 技术支持开通权限后，直接在**项目**中进行配置。


## 管理成员

点击左侧导航栏**成员管理**，进入成员管理页面。Click on ** Member** in the left-side navigation menu and enter the page of **Member Management**.

![](https://web-cdn.agora.io/docs-files/1542624935837)

You can:

* Click on **Add**, 设置不同的用户权限，进行协同管理。受邀者将收到邀请邮件。
* Click on **Edit**, 根据需求重新设置该成员的角色和权限。
* Click on **重置成员密码Reset Member Password**，该成员会收到密码重置邮件。
* Click on **Delete**，将不需要继续参与该项目的成员移出。

#### Role Permission 

不同角色对应的不同功能权限，其中，

* 管理员：拥有最大权限，能够查看用量、水晶球及项目详情，并负责项目管理和成员管理。View usage, Agora Analytics, projects	Manage Project
* 产品/运营：仅能查看用量，其他功能对其不开放。
* 技术支持/运维：仅能使用水晶球，其他功能对其不开放。
* 工程师：能够使用水晶球，有权查看项目和管理项目。

你也可以根据所需自定义团队成员的角色和权限。

## Submit and Check a Ticket

If you have any question when using the Agora products, take the following steps to get seek for an answer: 

1. Click on **Support > Submit Ticket** at the top navigation bar, enter your question or keywords to see if your question has been answered.
2. If the existing contents do not solve your problem, select a category, and submit a ticket for Agora technical support.
3. Click on **Support > View Ticket** to check if your ticket has been processed.

![](https://web-cdn.agora.io/docs-files/1543913838952)

## Use RESTful API

Agora Dashboard provides the RESTful APIs that enable you to ban users, check usage and inquire some online statistcis at the server. 

Click on the **User Name > RESTful API** at the top navigation bar to get the Customer ID and Customer Certificate for the RESTful API. For mor information, see [RESTful API](../../en/Interactive%20Broadcast/dashboard_restful_live.md).

## Other Functions

### Change Profile Information and Account Settings

Click on the **User Name** at the top navigation bar, and then you can:
* Modify personal information;
* Change password;
* Change the language of the Dashborad and Agora newsletter.

### Reach out to Developer Communities

Click on the **Community** button at the top navigation bar to reach out to global developers in the developer communities, such as Github, the online forum, and the WeChat public account.
