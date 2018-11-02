
---
title: Integrate SDK
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:06:08 GMT+0000 (UTC)
---
# Integrate SDK
## Prerequisites

-   A device with audio running Android 4.1 or higher
-   Android SDK for API level 16 or higher
-   [Android Studio 2.0](https://developer.android.com/studio/)
-   Agora [Voice SDK for Android](https://docs.agora.io/en/Agora%20Platform/downloads)


Before getting started, connect ports and whitelist domains as described in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).

## Creating an Agora Account and App ID

1.  Create a developer account at [www.agora.io](https://dashboard.agora.io/signin/). When you finish the sign-up, the website displays your [Dashboard](https://dashboard.agora.io/) .

2.  Click **Add New Project** on the **Projects** page of the dashboard.

3.  Fill in the **Project Name** and click **Submit** .

4.  Copy the value of your **App ID** .

5.  In the Android Studio project, locate the `app/src/main/res/values/strings.xml` file and add the APP ID:

	<img alt="../_images/video_appid.png" src="https://web-cdn.agora.io/docs-files/en/video_appid.png" style="width: 500px;"/>

## Adding the Agora SDK to Your Project

1. Open the `build.gradle` file under the **app** folder from the **Project Files** view in Android Studio. Make a note of the path of the libs folder to the right of compile `fileTree`. This is where you will put the Agora SDK for Android files later on.

   <img alt="../_images/video_buildgradle.png" src="https://web-cdn.agora.io/docs-files/en/video_buildgradle.png" />

> -   The libs path directory is relative to the application’s app directory.
> -   Ensure the path name contains no Chinese characters. Otherwise, if the path contains Chinese characters, compiling the code fails and displays an error message that contains random ASCII characters.

2.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Agora Voice SDK for Android.

3.  Copy the contents of the **libs** folder from the SDK to the **libs** folder whose path you found earlier in the `build.gradle` file. The following table lists the contents of the libs folder.

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>File/Folder Name</strong></td>
<td><strong>Notes</strong></td>
</tr>
<tr><td>agora-rtc-sdk.jar</td>
<td>.jar file (required)</td>
</tr>
<tr><td>armeabi-v7a</td>
<td>folder</td>
</tr>
<tr><td>x86</td>
<td>folder</td>
</tr>
<tr><td>arm64-v8a</td>
<td>folder</td>
</tr>
<tr><td>include</td>
<td>folder</td>
</tr>
</tbody>
</table>

## Adding the sourceSets

In the `build.gradle` file, add the `sourceSets` to the Android JSON object. Set the path for **libs** to the relative path for the **libs** app directory.

```
android {

...

sourceSets{
 main {
   jniLibs.srcDirs = ['../../../libs']
 }
}

}
```

## Synchronizing the Project

Click **Sync Project With Gradle Files** until the sync is complete.

<img alt="../_images/android9.png" src="https://web-cdn.agora.io/docs-files/en/android9.png" style="width: 500px;"/>


## Configuring the Android NDK

If you see the following “NDK not configured” error message, download and install the [Android NDK](https://developer.android.com/ndk/).

<img alt="../_images/android6.png" src="https://web-cdn.agora.io/docs-files/en/android6.png" style="width: 500px;"/>


1. Click the **Configure** button menu and select **Project Defaults \> Project Structure** as shown below:

	<img alt="../_images/project_structure.png" src="https://web-cdn.agora.io/docs-files/en/project_structure.png" style="width: 708.8px; height: 549.6px;"/>

2. Copy the [Android NDK](https://developer.android.com/ndk/) into the Android NDK location listed in the Android Studio's Project Structure window.

	<img alt="../_images/android7.png" src="https://web-cdn.agora.io/docs-files/en/android7.png" />

3. Re-sync the Android project with the NDK files by clicking **Sync Project With Gradle Files** .

	<img alt="../_images/android9.png" src="https://web-cdn.agora.io/docs-files/en/android9.png" style="width: 500px;"/>


## Adding Device Permissions

1. Open the `AndroidManifest.xml` file located under **app \> src \> main** and Add the required device permissions to the file.

    ```
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     package="io.agora.openvideocall">
    
     <uses-permission android:name="android.permission.INTERNET" />
     <uses-permission android:name="android.permission.RECORD_AUDIO" />
     <uses-permission android:name="android.permission.CAMERA" />
     <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
     <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    
     ...
    
     </manifest>
    ```

2. Re-sync the Android project with the NDK files by clicking **Sync Project With Gradle Files** .

	<img alt="../_images/android9.png" src="https://web-cdn.agora.io/docs-files/en/android9.png" style="width: 500px;"/>


## Preventing Obfuscation of the Agora Classes

In the `proguard-rules.pro` file, add a `-keep` class configuration for the Agora SDK. This prevents obfuscation of the Agora SDK public class names.

```
-keep class io.agora.**{*;}
```

The Android environment is now set up to use the Agora SDK.

