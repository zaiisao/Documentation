
---
title: Cocos Creator Quickstart
description: 
platform: Cocos Creator
updatedAt: Thu Mar 21 2019 08:15:33 GMT+0000 (UTC)
---
# Cocos Creator Quickstart
## Prerequisites

Development environment:

- Cocos Creator v2.0.7 or later (except v2.1.0)
- An Android or iOS device for mobile end
- No VPN connection
- Ensure the browser for Web end meet the [requirements](https://docs.agora.io/en/Audio%20Broadcast/web_prepare?platform=Web).


## Create a New Project

If you already have a project, skip this step.

1. Open Cocos Creator and choose **New Project**。
   ![](https://web-cdn.agora.io/docs-files/1552018036690)
   

2. On the **New Project** tab, choose **Empty Project**, select the path file for your project and choose **Create**.
   ![](https://web-cdn.agora.io/docs-files/1552018176389)


Now you have created your project as follows:
![](https://web-cdn.agora.io/docs-files/1552018232037)

## Enable Agora Service

1. Open your project with Cocos Creator. Choose **Panel** and **Service** in the drop-down menu. 
![](https://web-cdn.agora.io/docs-files/1552018316864)

The **Service** panel appears on the right.
   ![](https://web-cdn.agora.io/docs-files/1552269837812)

2. Select Agora service and enable the switch button.
   ![](https://web-cdn.agora.io/docs-files/1552017332653)


    Cocos Creator automatically downloads and configures all the resources necessary for Agora service. The Agora object provides you with [major APIs](../../en/Interactive%20Gaming/game_coco.md).

   > The JavaScript APIs provided by Cocos Creator only covers major functions. For more advanced functions, see the full API reference documents on other platforms.

3. In your project, call the APIs for voice call in gaming. For detailed information on the core API methods see the `HelloAgora` Demo。
   ![](https://web-cdn.agora.io/docs-files/1551929077432)
