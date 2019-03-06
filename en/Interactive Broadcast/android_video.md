
---
title: Integrate the SDK
description: 
platform: Android
updatedAt: Wed Mar 06 2019 07:44:07 GMT+0000 (UTC)
---
# Integrate the SDK
This page contains information on how to prepare the development environment before enabling a call/live broadcast with the Agora SDK for Android.

## Prerequisites

Development environment:

- A device with audio support running Android 4.1 or later.
- Android SDK for API level 16 or later.
- Android Studio 3.0 or later.
- Before accessing Agora’s services, ensure that you open the ports and whitelist the domains specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).

Download the Agora SDK：

The Agora [Video SDK for Android](https://docs.agora.io/en/Agora%20Platform/downloads)

Downloaded files include the libs folder and the sample folder. The following table lists the contents of the libs folder.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>File/Folder Name</strong></td>
<td><strong>File Type</strong></td>
</tr>
<tr><td>agora-rtc-sdk.jar</td>
<td>Java JAR file</td>
</tr>
<tr><td>arm64-v8a</td>
<td>folder</td>
</tr>
<tr><td>armeabi-v7a</td>
<td>folder</td>
</tr>
<tr><td>include</td>
<td>folder</td>
</tr>
<tr><td>x86</td>
<td>folder</td>
</tr>
</tbody>
</table></strong></td>

> Before integrating the SDK into your project, you can try to integrate the SDK into the sample first.



## Create an Agora Account and Get an App ID

1. Sign up for a developer account at <https://dashboard.agora.io/>.

2.  Click **Add New Project** on the **Projects** page in [Dashboard](https://dashboard.agora.io/).

3. Fill in the **Project Name** and click **Submit**. You have created your first project at Agora.

4.  Find the **App ID** under the created project.

    ![](https://web-cdn.agora.io/docs-files/1543388532968)

## Add the Agora SDK to Your Project

1. Set the storage directory of the libs folder. Open your project in Android Studio (this article takes the sample as an example), select the *app/src/main/build.gradle* file, and add the preset storage directory to the `fileTree` code line.

   ![](https://web-cdn.agora.io/docs-files/1549865890416)

> Ensure that the path name contains no Chinese characters. If the path contains Chinese characters, compiling the code fails and displays an error message that contains random ASCII characters.

2. Add the libs folder according to the storage directory preset in step 1.

3. Add `sourceSets`. In the `build.gradle` file, set the same storage directory as the libs folder.

    ```
    android {
     ...
     sourceSets {
            main {
                jniLibs.srcDirs = ['../../../libs']
            }
        }
    }
    ```

4.  Add the App ID in the *app/src/main/res/values/strings.xml* file.

    ```
    <resources>
        <string name="app_name">Agora-Android-Voice-Tutorial</string>
        ...
        <string name="agora_app_id">YOUR APP ID</string>
    </resources>
    ```

5. Click **Sync Project With Gradle Files** until the synchronization is complete.


## Configure the Android NDK

To call the plug-ins in the include files under the libs folder, you need to configure the Android NDK: 

1. Click the **Configure** button and select **Project Defaults \> Project Structure**. Click to download the Android NDK.
   
	 ![](https://web-cdn.agora.io/docs-files/1543388575943)

2. Click **Finish** when the download is complete and Android Studio automatically adds the NDK path.
   
	 ![](https://web-cdn.agora.io/docs-files/1543388586395)
   
	 If the path is not automatically added, add it manually and check it in the `local.properties` file.
   
	 ![](https://web-cdn.agora.io/docs-files/1543388615750)
	 
3. Re-synchronize the Android project by clicking **Sync Project With Gradle Files**.


## Add the Device Permissions

1. Open the *app/src/main/AndroidManifest.xml* file and add the required device permissions to the file.

	```
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="io.agora.tutorials1v1acall">
      
  <uses-permission android:name="android.permission.READ_PHONE_STATE” />	
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.RECORD_AUDIO" />
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <!-- If the app uses Bluetooth, please add Bluetooth permissions.-->
  <uses-permission android:name="android.permission.BLUETOOTH" />
  
  ...
  </manifest>
	```

2. Re-synchronize the Android project by clicking **Sync Project With Gradle Files**.



## Prevent Obfuscation of the Agora Classes

In the `proguard-rules.pro` file, add a `-keep` class configuration for the Agora SDK. This prevents obfuscation of the Agora SDK public class names.

```
-keep class io.agora.**{*;}
```

## Next Steps
You have set up the Android environment and can start a call/live broadcast following the steps under **Quickstart Guide**:

- Initialize the SDK
- Join a Channel
- Publish and Subscribe to Streams
