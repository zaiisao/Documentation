
---
title: Dashboard User Guide
description: 
platform: All Platforms
updatedAt: Wed Dec 05 2018 07:40:57 GMT+0000 (UTC)
---
# Dashboard User Guide
Welcome to Agora Dashboard! Here, you can do things like checking your usage and the QoE, managing projects and team members, getting Agora customer support, and more.

Before using Agora Dashboard, please head on over to [https://www.agora.io/en/](https://www.agora.io/en/) to create an Agora account. Learn how to [create an Agora account](../../en/Interactive%20Broadcast/sign_in_and_sign_up.md) and how to [reset your password](../../en/Interactive%20Broadcast/sign_in_and_sign_up.md) if you forget it.

## Functions

<table>
<tr>
<th>Function</th>
<th>Description</th>
</tr>
<tr>
<td>Check Usage</td>
<td>Check the usage of voice and video calls in a channel during a specific time frame to help you calculate fees.</td>
</tr>
<tr>
<td>Check Quality of Experience</td>
<td>Get an intuitive overview of the performance of our products by reading and analyzing the various charts based on the Agora products associated with your project and quickly identify problems.</td>
</tr>
<tr>
<td>Manage Projects</td>
<td>Create and manage projects, and get the App ID and App Certificates.</td>
</tr>
<tr>
<td>Manage Team Members</td>
<td>Add and manage team members, and set permissions for different roles.</td>
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

Click on **Usage > Usage Statistics** in the left-side navigation menu and enter the page of **Usage Details**, where you can check the total usage of voice and video calls in a specific channel during a specific time frame and calculate your fees. 

![](https://web-cdn.agora.io/docs-files/1543989847923)

As shown in the figure above, you can select the channel and time frame, and then view the usage (minutes) of audio, SD Video, HD Video and HD+ Video. These figures show the usage of all users, including the [host](../../en/Agora%20Platform/terms.md) and [audience](../../en/Agora%20Platform/terms.md).

* **Audio Usage**: The total usage of audio calls for the selected channel during the selected time frame. As shown in the figure, the total usage of audio calls for this channel between November 16 and 22, 2018 is 618,958 minutes. 
* **SD Video Usage**: The total usage of SD video calls (resolutions < 360p) for the selected channel during the selected time frame. Currently, the usage of SD video calls are incorporated into the usage of HD video call for charging.
* **HD Video Usage**: The total usage of HD video calls (360p ≤ resolutions ≤ 720p) for the selected channel during the selected time frame. As shown in the figure, the total usage of HD video calls for this channel between November 16 and 22, 2018 is 145,964 minutes. 
* **HD+ Video Usage**: The total usage of HD+ video calls (resolutions > 720p) for the selected channel during the selected time frame. As shown in the figure, the total usage of HD+ video calls for this channel on November 19, 2018 is 1,328 minutes.

> For more information about fees, see [Pricing and Billing](https://docs.agora.io/en/Agora%20Platform/billing_faq).

## Check the QoE

You can check the QoE in Agora Analytics, which provides data of the call process in diagrams, covering device status, user events, bitrates of the sent/received audio and video, the freeze time in rendering the audio and video and the packet loss rates of the audio and video. You can also quickly identify the issues according to the diagrams.

Click on **Analytics > Call Search** in the left-side navigation menu and enter the **QoE Page**.

After using our products, you can select a **Project** and **Time Frame**, and enter a **Channel Name** or **UID** to check the QoE. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).

![](https://web-cdn.agora.io/docs-files/1543913574811)

## Manage Projects

Click on **Project > Project List** in the left-side navigation menu and enter the **Project Page**, where you can create and manage projects, and get App IDs and App Certificates.

#### Create a Project and Get an App ID

For the detailed introduction and steps,  see  [Getting an App ID](../../en/Interactive%20Broadcast/token.md).

> You can sort the existing projects by the creation time, project name, and project status, or look for a specific project by entering the project name.
>
> Click on **Edit** to modify the project name.

#### Get App Certificates

For the detailed introduction and steps,  see  [Getting an App Certificate](../../en/Interactive%20Broadcast/token.md).

#### Ban a User

If you want to ban a user in the app, you can use the ban-user API at the server. See [Ban Users at the Server](https://docs.agora.io/en/Interactive%20Broadcast/dashboard_restful_live?platform=All_Platforms#5-api) for details. Or you can contact the Agora customer support to acquire permissions to directly ban users in your project.


## Manage Team Members

Click on **Member** in the left-side navigation menu and enter the page of **Member Management**.

![](https://web-cdn.agora.io/docs-files/1543990035082)

You can:

- Click on **Add Member** to add a new team member and set his role permissions and the invitee will receive an email invitation.
- Click on **Edit** to reset the role and permissions of the team member.
- Click on **Reset Password** to help the team member reset the password and he or she will receive a password reset email.
- Click on **Remove** to remove the member out of your project.

### Right Instructions

Different permissions for different roles.

- The **Administrator** can view usage, Agora Analytics and projects and manage projects.
- The **Product Manager** or **Operation Manager** can only view usage.
- The **Customer Support** or **Maintenance Manager** can only view Agora Analytics.
- The **Engineer** can view Agora Analytics and projects and manage projects.

You can also customize the roles by select the role as customised and select the check boxes to set permissions of team members as needed.
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
