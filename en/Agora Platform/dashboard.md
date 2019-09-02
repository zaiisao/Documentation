
---
title: Dashboard
description: 
platform: All Platforms
updatedAt: Mon Sep 02 2019 10:39:12 GMT+0800 (CST)
---
# Dashboard
Agora Dashboard allows you to check your usage and the Quality of Experience (QoE), check your account balance, manage your projects and members, and connect with Agora customer support.

Before using Agora Dashboard, [create an Agora account](../../en/Agora%20Platform/sign_in_and_sign_up.md) at [www.agora.io](https://www.agora.io/en/).

> This page introduces all the functions of Dashboard. What functions a member can access depend on the role and permissions.

## Introduction

<table>
<tr>
<th>Function</th>
<th>Description</th>
</tr>
<tr>
<td>Check your usage</td>
<td>Check the usage of your voice and video calls in a channel during a specific time frame to calculate your charges.</td>
</tr>
<tr>
<td>Check the QoE</td>
<td>Check the performance of Agora's products from diagrams associated with your project to quickly identify problems.</td>
</tr>
<tr>
<td>Check your account balance</td>
<td>Check your account balance and transactions.</td>
</tr>
<tr>
<td>Manage projects</td>
<td>Create and manage projects, get the App ID, and enable the App Certificate.</td>
</tr>
<tr>
<td>Manage members and roles</td>
<td>Add and manage members, and set their roles and permissions.</td>
</tr>
<tr>
<td>Submit and track tickets</td>
<td>Search for solutions to problems or submit tickets to Agora's customer support.</td>
</tr>
<tr>
<td>Use RESTful APIs</td>
<td>Use RESTful APIs to ban users, check the usage, and inquire about online statistics at the server.</td>
</tr>
<tr>
<td>Other functions</td>
<td>Change the profile information and account settings, and connect with developer communities.</td>
</tr>
</table>

## Overview

![](https://web-cdn.agora.io/docs-files/1557816862427)

The **Overview** page provides entry points to commonly-used Dashboard features. On this page, you can:

- View your projects and usage.
- View your account balance and transactions, and add money to your account.
- Check your latest messages.
- Learn about Agora's products, SDKs, code samples, and API reference.
- Learn to integrate Agora's products.
- View the quality of your calls.

## Check your usage 

Click ![](https://web-cdn.agora.io/docs-files/1551260936285) in the left navigation menu to go to the **Usage Stats** page. Select a project, and you can check the usage of your voice and video calls and recordings, and analyze the QoE during a specific time frame. 

### Check the usage of your voice and video calls

![](https://web-cdn.agora.io/docs-files/1557817278622)

Click the following icons to check the usage of your voice and video calls during a specific time frame. You can calculate your charges based on the usage<sup>[1]</sup>.

- **Duration**: Check the duration (minutes) of the voice and video (SD, HD, and HDP<sup>[2]</sup>) calls during a specific time frame.
- **Mini App**: Check the duration (minutes) of the voice and video calls made on Mini Apps during a specific time frame.

> [1] See [Pricing and Billing](https://docs.agora.io/en/Agora%20Platform/billing_faq).
>
> [2] Video Quality Types
>
> | Type | Resolution              |
> |------|----------------------------------|
> | HD | 360p ≤ Resolution ≤ 720p |
> | HDP | Resolution > 720p             |

### Check the usage of your recordings

Click the **Recording** button in the left navigation menu to check the duration (minutes) of your recording files during a specific time frame.

## Check the QoE

Click ![](https://web-cdn.agora.io/docs-files/1557817492440) in the left navigation menu to go to the **Agora Analytics** page.

Select a **Project** and **Time Frame**, and enter a **Channel Name** or **UID** to check the QoE. For more information, see [Agora Analytics Tutorial](../../en/Agora%20Platform/aa_guide.md).

## Check your account balance

Click ![](https://web-cdn.agora.io/docs-files/1551350477096) to go to the **Finance** page. Here, you can check your account balance and transactions.

## Manage members and roles

Click ![](https://web-cdn.agora.io/docs-files/1551255228096) in the left navigation menu to manage members.

![](https://web-cdn.agora.io/docs-files/1551257470398)

You can:

- Click **Add** to add a new member and set the role and permissions. The invitee receives an email invitation.
- Click ![](https://web-cdn.agora.io/docs-files/1551255422216) to reset the role and permissions of a member.
- Click ![](https://web-cdn.agora.io/docs-files/1551255494008) to allow a member to reset the password. The member receives an email to reset the password.
- Click ![](https://web-cdn.agora.io/docs-files/1551255516590) to remove a member from your project.

### Role and permissions

You can set the role of members. Different roles have different permissions.

The default roles are as follows:

- **Administrator** can view the usage, Agora Analytics, the account balance and transactions, and manage projects and members.
- **Finance** can view the account balance and transactions.
- **Product Manager** and **Operations Manager** can view the usage.
- **Customer Support** and **Technical Support** can view Agora Analytics.
- **Engineer** can manage projects and view Agora Analytics.

You can also create custom roles and set their permissions under **Customized**.

## Manage projects

Click ![](https://web-cdn.agora.io/docs-files/1551254998344) **Project** in the left navigation menu to manage projects.

![](https://web-cdn.agora.io/docs-files/1558345346386)

**Create a New Project**

Click **Create**. A dialog box pops up for you to enter your project name and select your authentication mechanism (App ID or Token). 

> Agora provides two authentication mechanisms, App ID and Token. For details, see [User Security Keys](../../en/Interactive%20Broadcast/token.md). We recommend that you use a token for higher security:
> - In the testing stage, you can generate a temporary token on the Dashboard after enabling the App Certificate.
> - When you are preparing for releasing your project, you need to deploy a Token Generator on your server.

In the figure above, Project A uses an App ID for authentication and the App Certificate of this project is not enabled. Project B uses a token for authentication and the App Certificate of this project is enabled. For Project B, you can click ![](https://web-cdn.agora.io/docs-files/1558345848047) to generate a temporary token in the testing stage.

On this page, you can also: 

- Enter a project name or App ID in the input box and click ![](https://web-cdn.agora.io/docs-files/1551255111208) to search for the project.
- Click ![](https://web-cdn.agora.io/docs-files/1551255135678) to edit the project information and see the App ID.
- Click ![](https://web-cdn.agora.io/docs-files/1551255151708) to view the project usage.
- Check the project states according to the ![](https://web-cdn.agora.io/docs-files/1551255188685) and ![](https://web-cdn.agora.io/docs-files/1551255166718) icons.

## Submit and track tickets

If you have any questions about Agora's products, you can: 

1. Click **Support > Submit Ticket** in the top navigation menu to go to **Agora Customer Service Center**.
2. Type in your question or keywords to see if the question has been answered.
3. If you cannot find your answer, select a category and submit a ticket to Agora's customer support.
4. Click **Support > View Ticket** to track the status of your ticket.

## Use RESTful APIs

Agora Dashboard provides RESTful APIs for you to ban users, check your usage, and inquire about online statistics at the server. 

Click **User Name > RESTful API** in the top navigation menu to get the Customer ID and Customer Certificate for the RESTful API. See [RESTful API](../../en/Agora%20Platform/dashboard_restful_live.md).

## Other functions

### Change the profile information and account settings

By clicking **User Name** in the top navigation menu, you can:

- Modify personal information.
- Change the password.
- Change the language of Dashboard and Agora's newsletter.

### Reach out to developer communities

Click **Community** in the top navigation menu to connect with global developers in developer communities, such as Github, online forums, and WeChat public accounts.
