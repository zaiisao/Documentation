
---
title: Implement Voice for Gaming
description: 
platform: Cocos
updatedAt: Mon Apr 20 2020 03:05:49 GMT+0800 (CST)
---
# Implement Voice for Gaming
With the `Hello-Cocos2D-Agora` Sample App provided by Agora, you can:

- Create and join a channel
- Talk in the channel
- Leave the channel

This page shows how to use the Cocos2d SDK to implement these functions on the Android and iOS platforms.

## Implementations on Android

### Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for Cocos. See the following structure:

    <img alt="../_images/AMG-SDK-structure-full-Cocos2d.png" src="https://web-cdn.agora.io/docs-files/en/AMG-SDK-structure-full-Cocos2d.png" style="width: 840.0px;"/>



-   **include**:  header files. Used in Cocos2d projects when modifying raw data.

-   **libs**: library files. Only `.jar` and `.so` files in the Android folder are used on this page.

-   **samples**: sample code. You can also view the source code in [GitHub](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-Cocos2d-Agora).

2.  Hardware and software requirements:

    -   Cocos2d 3.0 or later

    -   Android Studio 2.0 or later

    -   Two or more Android 4.0 or later devices with audio support

3.  [Getting an App ID](../../en/Video/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a new Cocos2d project. Refer to [here](http://www.cocos2d-x.org/docs/cocos2d-x/en/index.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Create a new folder, such as **AgoraGamingSDK**, under the root directory of the project.

2.  Copy the **include** and **libs** folders to **AgoraGamingSDK**.

3.  Add `.jar` files. Add the following statement to **dependencies** in `proj.android-studio/app/build.gradle`:

    ```
    compile fileTree(include: ['*.jar'], dir: '../../AgoraGamingSDK/libs/Android')
    ```


4.  Add the `.so` files

 a.  Add the following statement to `sourceSets.main` in `proj.android-studio/app/build.gradle`:

     ```
     jniLibs.srcDirs = ["libs", "../../AgoraGamingSDK/libs/Android"]
     ```

 b.  Add a LOCAL\_MODULE from agora-rtc, and then reference it.

     ```
     LOCAL_SHARED_LIBRARIES := agora-rtc
     ```

 c.  Add the `.h` files. Put the following to the end of *LOCAL\_C\_INCLUDES*:

   	```
	  $(LOCAL_PATH)/../../../AgoraGamingSDK/include
	  ```


### Step 4: Add Permissions

Add the necessary permissions, such as:

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

Call the APIs in [Interactive Gaming API](../../en/Interactive%20Gaming/game_cocos_cpp.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode

-   Command mode


You can choose which mode to use by calling `setChannelProfile`.

## Implementations on iOS

### Step 1: Prepare the Environment

1. [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Cocos**. See the following structure:

    <img alt="../_images/AMG-SDK-structure-full-Cocos2d.png" src="https://web-cdn.agora.io/docs-files/en/AMG-SDK-structure-full-Cocos2d.png" style="width: 840.0px;"/>

 - **include**: header files. Used in Cocos2d projects when modifying raw data.
 - **libs**: library files. Only the framework in the **iOS** folder is used on this page.
 - **samples**: sample code. You can also view the source code in [GitHub](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-Cocos2d-Agora).

2. Hardware and software requirements:
 - Cocos2D 3.0 or later
 - Xcode 8.0 or later
 - Two or more iOS 9.0 or later devices with audio support

3. [Getting an App ID](../../en/Video/token.md).

4. Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a new Cocos2d project. Refer to [here](http://www.cocos2d-x.org/docs/cocos2d-x/en/index.html) if necessary. Skip this step if a project already exists.


### Step 3: Add the SDK

1.  Create a new folder, such as **AgoraGamingSDK**, under the root directory of the project.

2.  Copy the **include** and **libs** folders to **AgoraGamingSDK**.

3.  Add `.framework` files:

    a.  Select the current **Target**.

    b.  Select **Build Phases** > **Link Binary With Libraries**.

    c.  Click **+**, and then **Add Other…**:

    d.  Select `AgoraAudioKit.framework`.

    e.  Add the following required system libraries:

       ![](https://web-cdn.agora.io/docs-files/1543568726412)



### Step 4: Disable bitcode

1.  Select the current **Target**.

2.  Select **Build Settings**.

3.  Select **Enable Bitcode**, and set it to **No**.

 ![](https://web-cdn.agora.io/docs-files/1543568747162)

### Step 5: Include Header Files

#### Objective-C

The default programming language in Cocos2d projects is Objective-C, so include the header files only:

```
#import <AgoraAudioKit/AgoraRtcEngineKit.h>
```

#### Swift

1.  Create a bridging file and name it `MyAgora-Bridging-Header.h`.

2.  Include **Agora Interactive Gaming SDK** in the bridging file:

    ```
    #import <AgoraAudioKit/AgoraRtcEngineKit.h>
    ```

3.  Set the bridging file to the Objective-C Bridging Header:

    a.  Select the current **Target**.

    b.  Select **Building Settings** > **Swift Compiler-General**; set **Objective-C Bridging Header** to `<Target_Name>/MyAgora-Bridging-Header.h`.


### Step 6: Add Permissions

Add any necessary permission, such as access permission to the microphone:

![](https://web-cdn.agora.io/docs-files/1543568795521)

### Step 7: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/Interactive%20Gaming/game_cocos_cpp.md) to implement the required functions.

Voice for gaming includes two modes:

-   Free talk mode

-   Command mode


You can choose which mode to use by calling `setChannelProfile`.


