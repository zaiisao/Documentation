
---
title: Reseller Console Overview
description: 
platform: All Platforms
updatedAt: Thu Oct 17 2019 07:52:52 GMT+0800 (CST)
---
# Reseller Console Overview
Agora Reseller Console is a tool for Agora's resellers to manage their companies and projects. Agora Reseller Console provides a set of tools to check usage and manage the company.

This page shows how to use Agora Reseller Console.

## Sign up

### Sign up for an Agora Reseller Console account

Before using Agora Reseller Console, you contact sales@agora.io or [submit a ticket](https://dashboard.agora.io/support) in Agora Dashboard to apply for an account. If you forget your password, you can reset it.

After creating the account, you can log in to [Agora Reseller Console](http://reseller.agora.io/) and check the usage of a company or a project.

### Change your account settings & password

Log in to [Agora Reseller Console](http://reseller.agora.io/), and click **Account Name > Setting** in the top navigation menu to enter the **Setting** interface.
![](https://web-cdn.agora.io/docs-files/1571297503483)

- In **User Info**, you can check the user information, change the language of the user interface, and reset your password.
  ![](https://web-cdn.agora.io/docs-files/1571297521568)

- In **API Key**, you can generate the **Access Key ID** and **Access Key Secret** for [RESTful API signature](https://confluence.agoralab.co/display/TEKP/Reseller+Console+RESTful+API#ResellerConsoleRESTfulAPI-2Signature). Access Key ID is the identification of the user, and Access Key Secret is a key to encrypt the signature string and the server authentication.
  ![](https://web-cdn.agora.io/docs-files/1571297533312)
  Input the necessary information for this key then click **Generate Key**. The API Key appears under the input box. A reseller can have many activated API key at the same time. 
 
 <div class="alert note">To strengthen the security of the API key, you cannot see the API key again once the page refreshes. Copy and save the API key in time.</div>

  You can also click **Delete** to remove the existing API key. When you have successfully removed the API key, the RESTful API signature, which uses the removed API key, becomes invalid.

### Reset your password

If you forget your password, follow these steps to reset it:

1. Go to [Login page](https://reseller.agora.io/).
2. Click **Forget password**.
  ![](https://web-cdn.agora.io/docs-files/1571297590270)
3. In the **Email** field, add your email address. Then, Agora sends a reset password email to you.
4. Sign in to your email and follow the steps indicated to reset your password.

## Create and manage your company

### Create a company

Follow these steps to create a company:

1. Log in to [Agora Reseller Console](http://reseller.agora.io/), click **Account > Company** in the left navigation menu to find the register link. The company created through this link automatically belongs to your Agora Reseller Console account.
   ![](https://web-cdn.agora.io/docs-files/1571297614481)
2. Go to the register link and follow the steps in [Sign up for an Agora Dashboard account](https://docs.agora.io/en/Agora Platform/sign_in_and_sign_up?platform=All Platforms&_ga=2.252293897.123770364.1570515745-569329450.1562664463) to create an Agora Dashboard account for the company. If your company has an Agora Dashboard account, contact sales@agora.io and provide the company name to add the company to your Agora Reseller Console account.
3. After you have successfully created a company, you can begin to manage it.

### Manage your company

In **Company List**, you can manage the existed company.

![](https://web-cdn.agora.io/docs-files/1571297695930)

#### Search

1. At the top-right corner of the **Company List**, choose the search type: CID (Unique company ID), user, or email.

2. Input the CID, user or email, then click **Search**.
   
	 <div class="alert note">Input the exact CID value to search for a target company.</div>

3. The target company appears in the list.

#### Check the status

In the **Status** of **Company List**, you can check the status of your company.

- Normal: The company is authorized to use Agora products.
- Suspended Manually: The company account is frozen, and the company is unauthorized to use Agora products.

#### Freeze or remove your company

Click **Detail** in the **Company List** to check detailed company and user information. You can also do the following operations:

![](https://web-cdn.agora.io/docs-files/1571297756552)

- Freeze the account. Click **Freeze Account** , choose the reason and whether to block the dashboard account or not, then click **Freeze**.

  - After you have successfully frozen the account, the financial status of the company changes to **Suspended Manually**. At the same time, Agora sends an email to notify the user.
  - When you choose to block the dashboard account at the same time, the account status of the company changes to **Blocked**, and the user cannot log in to Agora Dashboard.

- Remove the company form your account. Click **Remove this company from your account** and confirm, the company disappears from your company list.
 
  <div class="alert note">After you have successfully removed the company account, this company account can no longer be worked on.</div>

- Click **Visit Dashboard of this company** to enter and visit the Agora Dashboard for the selected company.

#### Check the usage

Click **Usage** in the **Company List** to quickly check the usage of this company. See **Check the usage** in detail.

## Check and download the usage

### Check the usage

Log in to [Agora Reseller Console](http://reseller.agora.io/), click **Usage > Chart** in the left navigation menu to access the following functions:

![](https://web-cdn.agora.io/docs-files/1571297936135)

- Choose the query interval and display type (Daily or Monthly).
- Input the company name to check the usage of the target company. When no company name is inputted, total usage for all of your companies are displayed in the chart by default.
- Check the usage of different products, such as Video & Audio, On-premise Recording and Cloud Recording. You can see the detailed usage data by moving your mouse on the chart or scrolling down to see the table.

#### Video & Audio

Choose **Video & Audio** to check the usage of the video and the audio products. Daily usage is an aggregate of all company's project usage for each day. Monthly usage is an aggregate value of the daily usage.

![](https://web-cdn.agora.io/docs-files/1571297955502)

- Unit: minute

- Chart description: It shows the total audio duration and the total video duration by default. Click the legend below the chart to see the usage chart of SD video, HD video and HDP video.
   
	<div class="alert note"> The total video duration is the sum of the SD video duration, HD video duration and HDP video duration.<ul><li>SD: Standard definition, with the value range: 16p × 16p, 640p × 360p.<br><li>HD: High definition, with the value range: 640p × 360p, 1280p × 720p.<br><li>HDP: Ultra high-definition, with the value more than 1280p × 720p.</ul></div>
 

  

#### On-premise Recording

Choose **On-premise Recording** to check the usage of the on-premise recording product. Daily usage is an aggregate of all company's project usage for each day. Monthly usage is an aggregate value of the daily usage.

![](https://web-cdn.agora.io/docs-files/1571298148799)

- Unit: minute
- Chart description: It shows the audio duration, SD video duration, HD video duration and HDP video duration by default. To hide a chart, click the legend below the chart.

#### Cloud Recording

Choose **Cloud recording** to check the usage of the cloud recording product. Daily usage is an aggregate of all company's project usage for each day. Monthly usage is an aggregate value of the daily usage.

![](https://web-cdn.agora.io/docs-files/1571298160017)

- Unit: minute
- Chart description: It shows the audio duration, SD video duration, HD video duration and HDP video duration by default. To hide a chart, click the legend below the chart.

#### Mini App

Choose **Mini App** to check the usage of the mini app product. Daily usage is an aggregate of all company's project usage for each day. Monthly usage is an aggregate value of the daily usage.

![](https://web-cdn.agora.io/docs-files/1571298172741)

- Unit: minute
- Chart description: It shows the audio duration and the video duration by default. To hide a chart, click the legend below the chart.

#### Transcoding

Choose **Transcoding** to check the usage of the transcoding product.

- Choose query by daily to show usage as an aggregate value of the peak number of each project during the day.
- Choose query by monthly to show usage as an aggregate value of the peak number of each project during the month, instead of the aggregate value of the daily usage.

![](https://web-cdn.agora.io/docs-files/1571298184817)

- Unit: number

- Chart description: It shows the following types by default:

  - H264 audio hosting-in duration
  - Total H264 audio duration
  - H264 video duration of the single host
  - H264 SD video hosting-in duration
  - H264 HD video hosting-in duration
  - H264 HDP video hosting-in duration

  Click the legend below the chart to see the usage chart of H265 audio and video hosting-in duration.

  <div class="alert note">Key terms:<ul><li>Transcoding: Transcoding is used in CDN live streaming when multiple hosts are in the channel. In CDN live streaming, the audio and video streams sent to the SD-RTN™ are transferred into RTMP (Real-Time Messaging Protocol) protocol and pushed to the CDN.<br><li>Hosting-in: An audience can apply to become a host to interact directly with the existing hosts.<br><li>H264: H.264, also referred to as MPEG-4 Part 10 or Advanced Video Coding (MPEG-4 AVC), is a block-oriented motion-compensation-based video compression standard.<br><li>H265: H.265, also referred to as MPEG-H Part 2 or High Efficiency Video Coding (HEVC), is a video compression standard.</ul></div>
  

#### Concurrent channel

Choose **Concurrent Channel** to check the usage of the transcoding concurrent channel.

- Choose query by daily to show usage as an aggregate value of the peak number of each project during the day.
- Choose query by monthly to show usage as an aggregate value of the peak number of each project during the month, instead of the aggregate value of the daily usage.

![](https://web-cdn.agora.io/docs-files/1571298230585)

- Unit: number
- Chart description: This shows the number of total H264 concurrent channels. Click the legend below the chart to see more usage charts for the specific concurrent channel.

#### Real-time Messaging (RTM)

Choose **Real-time Messaging (RTM)** to check the usage of the RTM product.

- Choose query by daily to show usage as an aggregate value of the peak number of each project during the day.
- Choose query by monthly to show usage as an aggregate value of the peak number of each project during the month, instead of the aggregate value of the daily usage.!

  ![](https://web-cdn.agora.io/docs-files/1571298347908)

- Unit: number
- Chart description: It shows the number of active users.

### Download the usage sheet

1. Log in to [Agora Reseller Console](http://reseller.agora.io/), then click **Usage > Download** in the left navigation menu.

2. Choose the query interval and type (Daily or Monthly).

3. Check the content you need, such as the information of the company, project and products. Click **Download**, and you can check the usage in the Excel file (The default file name is **usage**).
   
	 <div class="alert note"> To reduce the data amount, the downloaded usage sheet does not include the company which doesn't consume the usage.</div>
  
## Related documentation

About how to use the RESTful API, see [Reseller Console RESTful API](https://docs.agora.io/en/Agora%20Platform/reseller_console_restful?platform=All%20Platforms) for details.
