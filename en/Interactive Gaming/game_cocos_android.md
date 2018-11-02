
---
title: Implementing Voice for Gaming
description: 
platform: Cocos_(Android)
updatedAt: Thu Nov 01 2018 10:01:42 GMT+0000 (UTC)
---
# Implementing Voice for Gaming
# Implementing Voice for Gaming

## Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for Cocos. See the following structure:

    <img alt="../_images/AMG-SDK-structure-full-Cocos2d.png" src="https://web-cdn.agora.io/docs-files/en/AMG-SDK-structure-full-Cocos2d.png" style="width: 352.2px; height: 322.2px;"/>



-   **include**:  header files. Used in Cocos2d projects when modifying raw data.

-   **libs**: library files. Only `.jar` and `.so` files in the Android folder are used on this page.

-   **samples**: sample code.


1.  Hardware and software requirements:

    -   Cocos2D 3.0 or later

    -   Android Studio 2.0 or later

    -   Two or more Android 4.0 or later devices with audio support

2.  [Getting an App ID](../../en/Agora%20Platform/token.md).

3.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


## Step 2: Create a New Project

Create a new Cocos2d project. Refer to [here](http://www.cocos2d-x.org/docs/cocos2d-x/en/index.html) if necessary. Skip this step if a project already exists.

## Step 3: Add the SDK

1.  Create a new folder, such as **AgoraGamingSDK**, under the root directory of the project.

2.  Copy the **include** and **libs** folders to **AgoraGamingSDK**.

3.  Add `.jar` files. Add the following statement to **dependencies** in `proj.android-studio/app/build.gradle`:

    ```
    compile fileTree(include: ['*.jar'], dir: '../../AgoraGamingSDK/libs/Android')
    ```

    Aternatively, use the GUI Project Structure of Android Studio:

    <img alt="../_images/Intergration-Libraries-jar-Android.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Libraries-jar-Android.png" style="width: 480.0px; height: 84.6px;"/>


4.  Add the `.so` files

    1.  Add the following statement to `sourceSets.main` in `proj.android-studio/app/build.gradle`:

        ```
        jniLibs.srcDirs = ["libs", "../../AgoraGamingSDK/libs/Android"]
        ```

        Aternatively, use the GUI Project Structure of Android Studio:

        <img alt="../_images/Intergration-Libraries-so-Android.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Libraries-so-Android.png" style="width: 438.0px; height: 112.2px;"/>



1.  Add a LOCAL\_MODULE from agora-rtc, and then reference it.

    ```
    LOCAL_SHARED_LIBRARIES := agora-rtc
    ```


1.  Add the `.h` files. Put the following to the end of *LOCAL\_C\_INCLUDES*:


```
$(LOCAL_PATH)/../../../AgoraGamingSDK/include
```

<img alt="../_images/Intergration-Libraries-h-Android.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Libraries-h-Android.png" style="width: 417.0px; height: 55.2px;"/>


## Step 4: Add Permissions

Add the necessary permissions, such as:

-   android.permission.INTERNET

-   android.permission.RECORD\_AUDIO

-   android.permission.MODIFY\_AUDIO\_SETTINGS

-   android.permission.ACCESS\_NETWORK\_STATE

-   android.permission.BLUETOOTH


<img alt="../_images/Intergration-Privileges-Android.png" src="https://web-cdn.agora.io/docs-files/en/Intergration-Privileges-Android.png" style="width: 524.4px; height: 207.0px;"/>


## Step 5: Obfuscate the Code

Add the following line to obfuscate the code:

```
-keep class io.agora.**{*;}
```

### Step 6: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/API%20Reference/game_cocos.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode

-   Command mode


You can choose which mode to use by calling `setChannelProfile`.


