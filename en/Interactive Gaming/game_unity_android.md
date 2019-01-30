
---
title: Implement Voice for Gaming
description: 
platform: Unity
updatedAt: Wed Jan 30 2019 07:31:42 GMT+0000 (UTC)
---
# Implement Voice for Gaming
With the `Hello-Unity-Agora` Sample App provided by Agora, you can:

- Create and join a channel
- Talk in the channel
- Leave the channel

This page shows how to use the Unity3D SDK to implement these functions on the Android and iOS platforms.

## Implementations on Android
### Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Unity** Voice. See the following structure:

    <img alt="../_images/AMG-SDK-structure-full-Unity3D.png" src="https://web-cdn.agora.io/docs-files/en/AMG-SDK-structure-full-Unity3D.png" style="width: 840.0px;"/>

-   **include**: header files. Typically not used in Unity3D projects except for modifying raw data.

-   **libs**: library files. Only the `.jar` and `.so` files and the C\# API library files encapsulated in the **Scripts** folder are used on this page.

-   **samples**: sample code

2.  Hardware and software requirements:

    -   Unity3D 5.5 or later

    -   Android Studio 2.0 or later

    -   Two or more Android 4.0 or later devices with audio support

3.  [Getting an App ID](../../en/Agora%20Platform/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a Unity3D project. Refer to [here](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/Setup/Setup.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Open the root directory of the project and create:

    -  `Plugins/Android/AgoraAudioKit.plugin/libs`

    -   **Plugins/Scripts**

2.  Copy the **libs/Scripts/AgoraGamingSDK** folder to **Plugins/Scripts**.

3.  Add `.jar` files:

    -   Copy `.jar` files to `Assets/Plugins/Android/AgoraAudioKit.plugin/libs`.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/Android/mainTemplate.gradle `to **Assets/Plugins/Android**.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/Android/AgoraAudioKit.plugin/libs/libagora-unity3d-wrapper.jar` to `Assets/Plugins/Android/AgoraAudioKit.plugin/libs`.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/Android/AgoraGamingRtcSDKWrapper/AndroidManifest.xml` to `Assets/Plugins/Android/AgoraAudioKit.plugin`.

> `libagora-unity3d-wrapper.jar` is compiled from **Hello-Unity3D-Agora/Assets/Plugins/Android/AgoraGamingRtcSDKWrapper (./gradlew clean makeAndCopySDKWrapper)**.

4.  Add `.so` files:

    Copy **arm64-v8a/armeabi-v7a/x86** from **libs/Android** to `Assets/Plugins/Android/AgoraAudioKit.plugin/libs`.


### Step 4: Add Permissions

Add any necessary permission to `AndroidManifest.xml`. **AgoraGamingRtcSDKWrapper** already includes the required permissions for voice calls, for example:

-   android.permission.INTERNET

-   android.permission.RECORD\_AUDIO

-   android.permission.MODIFY\_AUDIO\_SETTINGS

-   android.permission.ACCESS\_NETWORK\_STATE

-   android.permission.BLUETOOTH

### Step 5: Obfuscate the Code

Add the following line to obfuscate the code:

```
-keep class io.agora.**{*;}
```

### Step 6: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode

-   Command mode


You can choose which mode to use by calling `setChannelProfile`.

## Implementations on iOS

### Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Unity** Voice. See the following structure:

    <img alt="../_images/AMG-SDK-structure-full-Unity3D.png" src="https://web-cdn.agora.io/docs-files/en/AMG-SDK-structure-full-Unity3D.png" style="width: 840.0px;"/>

-   **include**: header files. Typically not used in Unity3D projects except for when modifying the raw data.

-   **libs**: library files. Only the framework files and C\# API library files encapsulated in the in the **Scripts** folder are used in this document.

-   **samples**: sample code

1.  Hardware and software requirements:

    -   Unity3D 5.5 or later

    -   Xcode 8.0 or later

    -   Two or more iOS 9.0 or later devices with audio support

2.  [Getting an App ID](../../en/Agora%20Platform/token.md).

3.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a Unity3D project. Refer to [here](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/Setup/Setup.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Open the root directory of the project and create the following:

    -   `Plugins/Android/AgoraAudioKit.plugin/libs`

    -   **Plugins/Scripts**

2.  Copy the **libs/Scripts/AgoraGamingSDK** folder to **Plugins/Scripts**.

3.  Add `.framework` files:

    -   Copy `AgoraAudioKit.framework` to **Plugins/iOS**.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/iOS/AgoraGamingRtcSDKWrapper.h` to **Plugins/iOS**.

    -   Copy `Hello-Unity3D-Agora/Assets/Plugins/iOS/AgoraGamingRtcSDKWrapper.mm` to **Plugins/iOS**.

    -   Add the necessary system libraries:

    ![](https://web-cdn.agora.io/docs-files/1543568568723)



### Step 4: Disable bitcode

a.  Select the current **Target**.

b.  Select **Build Settings**.

c.  Select **Enable Bitcode**, and set it to **No**.


![](https://web-cdn.agora.io/docs-files/1543568593668)


### Step 5: Add Permissions

Add any necessary permission, such as access permission to the microphone:

![](https://web-cdn.agora.io/docs-files/1543568604885)


### Step 6: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode

-   Command mode


You can choose which mode to use by calling `setChannelProfile`.


