
---
title: Cocos Creator Quickstart
description: 
platform: Cocos Creator
updatedAt: Fri Jul 05 2019 03:38:28 GMT+0800 (CST)
---
# Cocos Creator Quickstart
## Prerequisites

Development environment:

- Cocos Creator v2.0.9
- A certified Cocos Creator account (After the sign-up, it takes 1 to 3 days for Cocos Creator to certify your account.)
- An Android or iOS device for mobile end
- No VPN connection
- Ensure the browser for Web end meet the [requirements](https://docs.agora.io/en/Audio%20Broadcast/web_prepare?platform=Web).


## Create a new project

If you already have a project, skip this step.

1. Open Cocos Creator and choose **New Project**。
   ![](https://web-cdn.agora.io/docs-files/1552018036690)
   

2. On the **New Project** tab, choose **Empty Project**, select the path file for your project and choose **Create**.
   ![](https://web-cdn.agora.io/docs-files/1552018176389)


Now you have created your project as follows:
![](https://web-cdn.agora.io/docs-files/1552018232037)

## Enable the Agora Service

1. Open your project with Cocos Creator. Choose **Panel** and **Service** in the drop-down menu. The **Service** panel appears on the right.

	 ![](https://web-cdn.agora.io/docs-files/1552018316864)
   

2. On the **Service** panel, select the game your just created and click **Association**.

	 ![](https://web-cdn.agora.io/docs-files/1555038788749)

	When the association completes, the **Agora Service** panel appears in the **Service** panel.
	 
   ![](https://web-cdn.agora.io/docs-files/1555041482406)

3. Enable the **Agora Service** panel, and click **OK** in the pop-up window.

   ![](https://web-cdn.agora.io/docs-files/1555039396375)
	 
4. Follow the on-screen instructions and click **open service**.

   ![](https://web-cdn.agora.io/docs-files/1555039476153)
	 
5. Upon opening the service, the following window appears. Click **console** to go to Agora **Dashboard**. The Dashboard automatically creates an Agora project. Click open the project and get your Agora App ID.

   ![](https://web-cdn.agora.io/docs-files/1555039933767)
	 
   Now you successfully enable the **Agora Service** for your Cocos Creator project and get the Agora App ID. You need the App ID to build and run the sample code. Cocos Creator automatically downloads and configures all the Agora's depenencies.


6. In your project, call the APIs for voice call in gaming. For detailed information on the core API methods see the [`HelloAgora` Demo](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-Cocos-Creator-Voice-Agora)。
   ![](https://web-cdn.agora.io/docs-files/1551929077432)
	 
	 > The [JavaScript APIs](../../en/Interactive%20Gaming/game_coco.md) provided by Cocos Creator only covers major functions. For more advanced functions, see the full API reference documents on other platforms.
