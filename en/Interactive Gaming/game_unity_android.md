
---
title: Implement Voice for Gaming
description: 
platform: Unity
updatedAt: Mon Apr 13 2020 15:40:40 GMT+0800 (CST)
---
# Implement Voice for Gaming
<div class="alert note">Agora no longer maintains the Agora Unity SDK for Interactive Gaming, use the Agora RTC Unity SDK instead. See more details in <a href="https://docs.agora.io/en/Voice/start_call_audio_unity?platform=Unity">Start a Voice Call</a >.</div>

With the `Hello-Unity-Agora` Sample App provided by Agora, you can:

- Create and join a channel
- Talk in the channel
- Leave the channel

This page shows how to use the Unity3D SDK to implement these functions on the Android and iOS platforms.

## Implementations on Android
### Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Unity** Voice. See the following structure:

	![](https://web-cdn.agora.io/docs-files/1548830935872)

-   **include**: header files. Typically not used in Unity3D projects except for modifying raw data.

-   **libs**: library files. Only the `.jar` and `.so` files and the C\# API library files encapsulated in the **Scripts** folder are used on this page.

-   **samples**: sample code

2.  Hardware and software requirements:

    -   Unity3D 5.5 or later
    -   Android Studio 2.0 or later
    -   Two or more Android 4.0 or later devices with audio support

3.  [Getting an App ID](../../en/Interactive%20Gaming/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a Unity3D project. Refer to [Unity Quick Start](https://docs.unity3d.com/2018.2/Documentation/Manual/GettingStarted.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Open the root directory of the project and create:

    -  `Assets/Plugins/Android/AgoraAudioKit.plugin/libs`
    -  `Assets/Scripts`

2.  Copy the `libs/Scripts/AgoraGamingSDK` from the downloaded SDK to the `Assets/Scripts` directory of your project.

3.  Copy the `libs/Android/agora-rtc-sdk.jar` file in the SDK to `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` path of your project.

4.  Copy the `Assets/Plugins/Android/mainTemplate.gradle` file from the Sample in the SDK to the `Assets/Plugins/Android` path of your project.

5.  Copy the `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` from the Sample in the SDK to the `Assets/Plugins/Android/AgoraAudioKit.plugin` directory of your project.

6.  Copy the **armeabi-v7a** and **x86** files in `libs/Android` of the SDK to the `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` path your project.


### Step 4: Add Permissions

Add the following permissions to the `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` file:

```C#
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

### Step 5: Prevent Code Obfuscation

Add the following line in the `Assets/Plugins/Android/AgoraAudioKit.plugin/project.properties` to prevent obfuscating the code:

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

	![](https://web-cdn.agora.io/docs-files/1548829843264)

-   **include**: header files. Typically not used in Unity3D projects except for when modifying the raw data.

-   **libs**: library files. Only the framework files and C\# API library files encapsulated in the in the **Scripts** folder are used in this document.

-   **samples**: sample code

1.  Hardware and software requirements:

    -   Unity3D 5.5 or later

    -   Xcode 8.0 or later

    -   Two or more iOS 9.0 or later devices with audio support

2.  [Getting an App ID](../../en/Interactive%20Gaming/token.md).

3.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a Unity3D project. Refer to [Unity Quick Start](https://docs.unity3d.com/2018.2/Documentation/Manual/GettingStarted.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Open the root directory of your project and create a `Assets/Scripts` folder.

2.  Copy the `Assets/Scripts/AgoraGamingSDK` folder from the sample to `Assets/Scripts` of your project.

3.  Add `.framework` files:

    -   Copy `Assets/Plugins/iOS/AgoraAudioKit.framework` from the sample to `Assets/Plugins/iOS` of your project.

    -   Copy `Assets/Plugins/iOS/libagoraSdkCWrapper.a` from the sample to `Assets/Plugins/iOS` of your project.

    -   Add the necessary system libraries: `AgoraAudioKit.framework`, `AudioToolBox.framework`, `CoreTelephony.framework`, `AVFoundation.framework` and `libresolv.tbd`.

    ![](https://web-cdn.agora.io/docs-files/1543568568723)


### Step 4: Add Permissions

Add any necessary permission, such as access permission to the microphone:

![](https://web-cdn.agora.io/docs-files/1543568604885)


### Step 5: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode
-   Command mode

You can choose which mode to use by calling `setChannelProfile`.


