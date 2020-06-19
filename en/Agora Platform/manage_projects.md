
---
title: Manage Projects
description: 
platform: All Platforms
updatedAt: Fri Jun 19 2020 14:28:26 GMT+0800 (CST)
---
# Manage Projects
This article shows how to create and manage projects in Agora Console.


 <div class="alert info">Project members with the role of <b>Admin</b> or <b>Engineer</b> have access to the <b>Project Management</b> page.</div>

## Create a new project

 <div class="alert info">You can create up to 10 projects, including the deleted ones. If you need to create more projects, please contact us by <a href="https://agora-ticket.agora.io/">submitting a ticket</a >.
</div>

To create a new project, follow these steps:

1. Log in to Agora Console, and click ![img](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to enter the [Project Management](https://dashboard.agora.io/projects) page.

2. Click **Create**.
   ![img](https://web-cdn.agora.io/docs-files/1574662791845)

3. Enter your project name and select your authentication mechanism in the dialog box.

   <div class="alert info">We provide two authentication mechanisms: <ul><li>APP ID + APP Certificate + Token (Recommended)</li><li>APP ID</li></ul>
We recommend that you use the <b>APP ID + APP Certificate + Token</b> authentication mechanism which is more secure:<ul><li>In the testing stage, you can generate a temporary token in Agora Console after <a href="#primary">enabling the primary certificate</a >. See <a href="https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token">Get a temporary token</a >.</li><li>In the production stage, you need to deploy a token generator on your server. See <a href="https://docs.agora.io/en/Interactive%20Broadcast/token_server_cpp?platform=CPP">Generate a Token</a >.</li></ul></div>

  ![](https://web-cdn.agora.io/docs-files/1592467781248)
4. Click **Submit**, and you can see the project on the **Project Management** page. Agora automatically assigns each project an App ID as a unique identifier.

## Manage your projects

For created projects, you can also do the following on the **Project Management** page:

![img](https://web-cdn.agora.io/docs-files/1574663170053)

- Search: Enter a project name or App ID, and click ![](https://web-cdn.agora.io/docs-files/1592466538637) to search for a project.

- View basic project information, including:

  - Stage: **Testing**, **Live**, and **Not Specified**
  - Project name
  - Creation date
  - App ID

- Click ![img](https://web-cdn.agora.io/docs-files/1574156449172) to enable real-time communication on the web page.

- Click ![img](https://web-cdn.agora.io/docs-files/1564048991389) to generate a temporary token for authentication during the project testing stage.
   <div class="alert note">You can only use the primary app certificate to generate a temporary token. See <a href="#primary">Enable the primary certificate</a >.</div>

- Click **Usage** to view the amount of Agora services the project uses.

- Click **Edit** to view the **Edit Project** page. You can edit your project information, including the following:

  - Project stage

  - Project name

  - App certificate

  - Temporary token

  - Project status

### Manage your app certificates

An app certificate is a string generated from Agora Console, and it enables token authentication. For different security requirements, Agora provides two types of app certificates. The differences are as follows:

- Primary certificate: You can use a primary certificate to generate tokens, including temporary tokens. You cannot delete a primary certificate.
- Secondary certificate: You can use a secondary certificate to generate tokens, except for temporary tokens. After enabling a secondary certificate, you can switch it and the primary certificate, or delete it.
<div class="alert note"><ul><li><b>No certificate</b> means that your project uses only the App ID for authentication. <b>No certificate</b> appears only if you choose the <b>APP ID</b> authentication mechanism when creating a project.</li><li>The secondary certificate does not applies to Agora RESTful API.</li></ul></div>


#### Enable a primary certificate<a name="primary"></a>

For scenarios requiring high security, you can enable a primary certificate as follows:

- If you choose the **APP ID + APP Certificate + Token** authentication mechanism when creating a project, Agora enables the primary certificate for you by default. On the **Edit Project** page, you can click ![](https://web-cdn.agora.io/docs-files/1592469028908) to view and copy the primary certificate.
![](https://web-cdn.agora.io/docs-files/1592469047303)

- If you choose the **APP ID** authentication mechanism when creating a project, you need to enable the primary certificate manually. On the **Edit Project** page, find **Primary certificate** and click **Enable**. Once the primary certificate is enabled, you can click ![](https://web-cdn.agora.io/docs-files/1592469070485) to view and copy the primary certificate, and use either an App ID or the token generated by the primary certificate for authentication. 
![](https://web-cdn.agora.io/docs-files/1592469088556)

#### Enable a secondary certificate<a name="secondary"></a>

After successfully enabling the primary certificate, you can enable the secondary certificate when you need to change app certificates.

On the **Edit Project** page, find **Secondary certificate**, and click **Enable**. Once the secondary certificate is enabled, you can click ![](https://web-cdn.agora.io/docs-files/1592469104861) to view and copy the secondary certificate, and use either the primary certificate or the secondary certificate to generate tokens for authentication. 

![](https://web-cdn.agora.io/docs-files/1592469125002)

#### Switch and delete the primary certificate

After enabling both the primary certificate and the secondary certificate, if the primary certificate is exposed to security risks, you can switch it to **Secondary certificate** and delete it.

To switch and delete the primary app certificate, do the following:

1. Find **Secondary certificate**, click **Set as primary** to switch the secondary certificate and the primary certificate. Once you switch the primary certificate to the secondary certificate, all temporary tokens become invalid.
![](https://web-cdn.agora.io/docs-files/1592469150352)
2. Click **Delete** to delete the original primary certificate. You cannot restore the deleted certificate, and all tokens generated by the original primary certificate become invalid. You need to use a new primary certificate to generate tokens for authentication.
3. After deletion, the status of the secondary certificate becomes **Disabled**. A new secondary certificate is generated when you click **Enable** next time.

#### Delete No certificate

**No certificate** is enabled only if you choose the **APP ID** authentication mechanism when creating a project. **No certificate** means that your project uses only the App ID for authentication, which is less secure.
![](https://web-cdn.agora.io/docs-files/1592469181051)

If you need to raise the security level, you can click **Enable** to enable the primary certificate, and generate a token for authentication. After enabling the primary certificate, you can delete **No certificate**. 
<div class="alert warning">Once you delete <b>No certificate</b>, you can no longer use only the App ID for authentication, and the project cannot enable <b>No certificate</b> again.</div>

