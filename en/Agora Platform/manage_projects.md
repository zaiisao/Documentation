
---
title: Manage Projects
description: 
platform: All Platforms
updatedAt: Thu Nov 28 2019 07:22:47 GMT+0800 (CST)
---
# Manage Projects
This page shows how to create and manage projects in Console.

> The administrator and engineers have access to the **Project Management** page.

## Create a new project

> You can create up to 10 projects with the deleted ones included. If you need to use more projects, please contact us by submitting a ticket.

Follow these steps to create a new project:

1. Log in Console, click ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to enter the [**Project Management**](https://dashboard.agora.io/projects) page.

2. Click **Create**. 

![](https://web-cdn.agora.io/docs-files/1574662791845)

3. Enter your project name and select your authentication mechanism ("App ID" or "App ID + App Certificate + Token") in the dialog box.

![](https://web-cdn.agora.io/docs-files/1574662907483)

> We provide two authentication mechanisms: "App ID" and "App ID + App Certificate + Token". For details, see [User Security Keys](https://docs.agora.io/en/Interactive%20Broadcast/token#agoras-authentication-mechanisms). 
>
> We recommend that you use a token for more security:
>
> - In the testing stage, you can generate a temporary token in Console after enabling the App Certificate.
> - In the production stage, you need to deploy a Token Generator on your server. See [Generate a Token](https://docs.agora.io/en/Interactive&20Broadcast/token_server?platform=C++).

4. Click **Submit** and you can see the project on the **Project Management** page. 

## Manage your projects

![](https://web-cdn.agora.io/docs-files/1574663170053)

On the **Project Management** page, you can also do the following:

- Enter a project name or App ID in the input box and click ![img](https://web-cdn.agora.io/docs-files/1551255111208) to search for the project.
- Click ![img](https://web-cdn.agora.io/docs-files/1551255135678) to edit the project information and get the App ID.
- Click ![](https://web-cdn.agora.io/docs-files/1564048876293) to view the project usage.
- Click ![](https://web-cdn.agora.io/docs-files/1564048991389) to generate a temporary token.
- Check the project states according to the ![img](https://web-cdn.agora.io/docs-files/1551255188685) and ![img](https://web-cdn.agora.io/docs-files/1551255166718) icons.

- Check the basic information of projects, including: 

  - Stage: Testing, Live, and Not Specified
  - Project Name
  - Creation Date
  - App ID

- Click ![](https://web-cdn.agora.io/docs-files/1574156449172) to try real-time communication on the webpage.

- Click ![](https://web-cdn.agora.io/docs-files/1564048991389) to generate a **temporary token** for testing. 

- View the project usage.

- Edit the project information, including: 

![](https://web-cdn.agora.io/docs-files/1574664691375)

### Enable the App Certificate

If you use the App ID for authentication when creating the project and want to switch to the "App ID + App Certificate + Token" mechanism, you need to enable the App Certificate first. 

Follow these steps to enable the App Certificate:

1. Click the **edit** button of the targeted project.

![](https://web-cdn.agora.io/docs-files/1574925402348)

2. Click **Enable** in the "Basic Info" page. 

![](https://web-cdn.agora.io/docs-files/1574664820135)

3. Read **About App Certificate**.

![](https://web-cdn.agora.io/docs-files/1574664881593)

4. We will send you an email. Follow the steps in the email to confirm about enabling the App Certificate. 

5. Go back to the **Edit project** page to check the enabled App Certificate.

>Note: If you do not find the confirmation email in your inbox, check your spam or junk email folder.


