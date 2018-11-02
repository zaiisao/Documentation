
---
title: Implementing Voice for Gaming
description: 
platform: Unity_(iOS)
updatedAt: Fri Nov 02 2018 04:20:39 GMT+0000 (UTC)
---
# Implementing Voice for Gaming
## Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Unity** Voice. See the following structure:

    <img alt="../_images/AMG-SDK-structure-full-Unity3D.png" src="https://web-cdn.agora.io/docs-files/en/AMG-SDK-structure-full-Unity3D.png" style="width: 370.0px;"/>

-   **include**: header files. Typically not used in Unity3D projects except for when modifying the raw data.

-   **libs**: library files. Only the framework files and C\# API library files encapsulated in the in the **Scripts** folder are used in this document.

-   **samples**: sample code

1.  Hardware and software requirements:

    -   Unity3D 5.5 or later

    -   Xcode 8.0 or later

    -   Two or more iOS 9.0 or later devices with audio support

2.  [Getting an App ID](../../en/Agora%20Platform/token.md).

3.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


## Step 2: Create a New Project

Create a Unity3D project. Refer to [here](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/Setup/Setup.html) if necessary. Skip this step if a project already exists.

## Step 3: Add the SDK

1.  Open the root directory of the project and create the following:

    -   `Plugins/Android/AgoraAudioKit.plugin/libs`

    -   **Plugins/Scripts**

2.  Copy the **libs/Scripts/AgoraGamingSDK** folder to **Plugins/Scripts**.

3.  Add `.framework` files:

    -   Copy `AgoraAudioKit.framework` to **Plugins/iOS**.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/iOS/AgoraGamingRtcSDKWrapper.h` to **Plugins/iOS**.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/iOS/AgoraGamingRtcSDKWrapper.mm` to **Plugins/iOS**.

    -   Add the necessary system libraries:

    <img alt="../_images/Intergration-Libraries-iOS.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Libraries-iOS.png" />



## Step 4: Disable bitcode

1.  Select the current **Target**.

2.  Select **Build Settings**.

3.  Select **Enable Bitcode**, and set it to **No**.


<img alt="../_images/Intergration-Bitcode-iOS.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Bitcode-iOS.png" />


## Step 5: Add Permissions

Add any necessary permission, such as access permission to the microphone:

<img alt="../_images/Intergration-Privileges-Microphone-iOS.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Privileges-Microphone-iOS.png" />


## Step 6: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode

-   Command mode


You can choose which mode to use by calling `setChannelProfile`.


