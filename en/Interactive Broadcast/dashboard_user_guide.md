
---
title: Use Dashboard
description: 
platform: All Platforms
updatedAt: Mon Dec 17 2018 18:12:52 GMT+0000 (UTC)
---
# Use Dashboard
Agora Dashboard allows you to check your usage and the QoE, manage your projects and team members, and connect with Agora customer support.

Before using Agora Dashboard, [create an Agora account](../../en/Interactive%20Broadcast/sign_in_and_sign_up.md) at [www.agora.io](https://www.agora.io/en/).

## Overview

<table>
<tr>
<th>Function</th>
<th>Description</th>
</tr>
<tr>
<td>Check Your Usage</td>
<td>Check the usage of your voice and video calls in a channel during a specific time frame to help you calculate your charges.</td>
</tr>
<tr>
<td>Check the QoE</td>
<td>Check the performance of Agora's products from diagrams associated with your project to quickly identify problems.</td>
</tr>
<tr>
<td>Manage Projects</td>
<td>Create and manage projects, and get the App ID and App Certificate.</td>
</tr>
<tr>
<td>Manage Team Members</td>
<td>Add and manage team members, and set the permissions for different roles.</td>
</tr>
<tr>
<td>Submit and Track Tickets</td>
<td>Search for solutions to problems or submit tickets to Agora customer support.</td>
</tr>
<tr>
<td>Use RESTful APIs</td>
<td>Use RESTful APIs to ban users, check the usage, and inquire about online statistics at the server.</td>
</tr>
<tr>
<td>Other Functions</td>
<td>Change the profile information and account settings, and connect with developer communities.</td>
</tr>
</table>

## Check Your Usage

Click on **Usage > Usage Statistics** on the left of the navigation menu to go to the **Usage Details** page where you can check the total usage of your voice and video calls in a specific channel during a specific time frame, and calculate your charges. 

The following figure shows an example.

![](https://web-cdn.agora.io/docs-files/1543989847923)

You can select a channel and time frame to view the usage (minutes) of your voice and video (SD, HD, and HD+) calls for all users.

* **Total Audio Usage**: The total usage of voice calls. In the example, between 16 and 22 November 2018, the total usage of voice calls for the channel is 618,958 minutes. 
* **Total SD Video Resolutions < 360P**: The total usage of SD (resolution < 360p) video calls. For billing purposes, the usage of SD video calls is incorporated into the usage of HD video calls.
* **Total HD Video Resolutions ≤ 720**: The total usage of HD (360p ≤ resolution ≤ 720p) video calls. In the example, between 16 and 22 November 2018, the total usage of HD video calls for the channel is 145,964 minutes. 
* **Total HD+ Video Resolutions > 720**: The total usage of HD+ (resolution > 720p) video calls. In the example, on 19 November 2018, the total usage of HD+ video calls for the channel is 1,328 minutes.

> See [Pricing and Billing](https://docs.agora.io/en/Agora%20Platform/billing_faq).

## Check the QoE

You can check the QoE in **Agora Analytics**, which provides data of the call process in diagrams showing the device status, user events, bitrates of the sent/received audio and video, the freeze time in rendering the audio and video and the packet loss rates. You can quickly identify your issues from the diagrams.

Click on **Analytics > Call Search** on the left of the navigation menu to go to **QoE Page**.

After using Agora's products, you can select a **Project** and **Time Frame**, and enter a **Channel Name** or **UID** to check the QoE. See [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).

![](https://web-cdn.agora.io/docs-files/1543913574811)

## Manage Projects

Click on **Project > Project List** on the left of the navigation menu to go to **Project Page**, where you can create and manage projects, and get App IDs and App Certificates.

#### Create a Project and Get an App ID

See [Getting an App ID](../../en/Interactive%20Broadcast/token.md).

> You can sort the existing projects by the creation time, project name, and project status, or look for a specific project by entering the project name.
>
> Click on **Edit** to modify the project name.

#### Get an App Certificate

See [Getting an App Certificate](../../en/Interactive%20Broadcast/token.md).

#### Ban Users

If you want to ban a user in the app, you can use the ban-user API at the server. See [Ban Users at the Server](https://docs.agora.io/en/Interactive%20Broadcast/dashboard_restful_live?platform=All_Platforms#5-api). You can also contact Agora customer support to directly ban users in your project.


## Manage Team Members

Click on **Member** on the left of the navigation menu to go to **Member Management**.

![](https://web-cdn.agora.io/docs-files/1543990035082)

You can:

- Click on **Add Member** to add a new team member and set the role permissions. The invitee will receive an email invitation.
- Click on **Edit** to reset the role and permissions of a team member.
- Click on **Reset Password** to allow a team member to reset the password. The team member will receive an email to reset the password.
- Click on **Remove** to remove a team member from your project.

### Role Permissions

Different roles have different permissions:

- **Administrator** can view the usage, Agora Analytics, projects, and manage the projects.
- **Product Manager** and **Operation Manager** can only view the usage.
- **Customer Support** and **Maintenance Manager** can only view Agora Analytics.
- **Engineer** can view Agora Analytics, projects, and manage projects.

You can also create custom roles under **Customized**.
## Submit and Track a Ticket

If you have any questions about Agora's products, you can: 

1. Click on **Support > Submit Ticket** at the top of the navigation menu to go to **Agora Customer Service Center**.
![](https://web-cdn.agora.io/docs-files/1543913838952)
2. Type in your question or keywords to see if the question has been answered.
3. If you cannot find your answer, select a category and submit a ticket to Agora customer support.
4. Click on **Support > View Ticket** to track the status of your ticket.

## Use RESTful APIs

Agora Dashboard provides RESTful APIs for you to ban users, check your usage, and inquire about online statistics at the server. 

Click on **User Name > RESTful API** at the top of the navigation menu to get the Customer ID and Customer Certificate for the RESTful API. See [RESTful API](../../en/Interactive%20Broadcast/dashboard_restful_live.md).

## Other Functions

### Change the Profile Information and Account Settings

By clicking on **User Name** at the top of the navigation menu, you can:
* Modify personal information.
* Change the password
* Change the language of the Dashboard and Agora newsletter.

### Reach out to Developer Communities

Click on **Community** at the top of the navigation menu to connect with global developers in developer communities such as Github, online forums, and WeChat public accounts.
