
---
title: Other Questions
description: 
platform: Other Questions
updatedAt: Mon Apr 08 2019 21:15:41 GMT+0000 (UTC)
---
# Other Questions
## Android

### Why does the Android suspension window not respond to clicking or dragging after integrating the Agora SDK?

Check whether the system suspension window permission is enabled. The app cannot open the window if it is disabled.

### Can I use the Agora Native SDK on 64-bit Android devices?

* Agora Native SDK v1.7.4+:

   The Agora Native SDK supports the 64-bit ARM architecture, and you only need to copy the files from the arm64-v8a folder (included in the SDK package) to the corresponding folder in your project.
Currently, only x86 compatibility mode is supported but not x86_64. Ensure that there is no x86_64 directory in the app of any x86 64-bit device.

* Agora Native SDK before v1.7.4:

   The Agora Native SDK provides a 32-bit native library (armeabi-v7a). On 64-bit devices, Android allows starting the app in the 32-bit processing mode. Ensure that the arm64 folder of the app is empty. Otherwise, the Android system loads the app in the 64-bit mode and the app boot fails.

Since most Android clients are 32-bit devices, manufacturers generally provide 32-bit libraries. Hence, it is common to start in the 32-bit processing mode on 64-bit Android devices.

If you see the following error on 64-bit devices, `java.lang.UnsatisfiedLinkError: dlopen failed: "libHDACEngine.so"`, possible reasons include:

- When installing the app, the system looks for the directory of the native library (the existing ABI:armeabi, armeabi-v7a, arm64-v8a, x86, x86_64, MIPS64, MIPs) under the lib directory of the app according to `Build.SUPPORTED_ABIS` lib.
- If the app has a 64-bit compatible directory with missing library files, do not replace them from the ABI directory. The library files cannot be mixed. The corresponding library files for each architecture must be used.
- On Android, if there is no arm64-v8a directory on a 64-bit system, it will try to find the libraries under armeabi-v7a.

Solutions:

* Method 1: When building the app, delete the arm64-v8a directory in the project. After the app is built, ensure that there is no arm64-v81directory under lib in the app package.
* Method 2: Set abiFilters in the gradle build file, and only include the 32-bit library.
 
```
 android {
  ...
  defaultConfig {
    ...
    ndk {
      abiFilters "armeabi-v7a","x86"
    }
  }
}
```

### Why did I receive this error: Failed to crunch file...?

This error may occur when using the Android Open Video Call Sample App:

```
Error: Failed to crunch file E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\exploded-aar\com.android.support\appcompat-v7\25.0.0\res\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
into
E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\res\merged\debug\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
```

This error occurs when the referenced file name is too long.

## Login

### Why do I automatically log out of a device?

You automatically log out of your current device if you log in another device.

## Channel

### In poor network conditions, does the SDK force users to leave a channel?

No, users do not automatically leave a channel unless they do so themselves. For example, the application calls the `leaveChannel` method.

### Does each channel/room need an administrator in a call?

No, an administrator is only implemented in the business management layer. Your signaling server sends commands and calls the SDK interfaces for call management.

### Does the client need to maintain the channel?

No, a channel is created and deleted automatically. When all users leave the channel, the channel is deleted automatically.

### What are the App ID and Dynamic Key? How can I use them?

See [Use Security Keys](../../en/voice/token.md) for details.

### How do I check who is speaking in the channel?

The following callbacks indicate who is speaking and the speakers' volume.

* For Android and Windows: `onAudioVolumeIndication`
* For iOS and macOS: `reportAudioVolumeIndicationOfSpeakers`

By default, these callbacks are disabled. You can use the `enableAudioVolumeIndication` method to enable them.

### What should I be aware of for initialization in Windows?

* Always ensure that the initialize method is called before joining a channel.
* If you want to enable the video function, call the `enableVideo` method before joining a channel.
* If you want to enable message transmission in the same channel, call the `enableVendorMessage` method before joining a channel.

## Others

### Which callbacks are important to apps?

Apps need information about user status-related callbacks, such as `userJoined` and `userOffline`. 

### What error codes are important?

All error codes are important, and warning codes can be ignored.

### What error codes mean that I should end the call?

When an error code is received, you should end the call.

### Which callback informs users that the server is disconnected?

The `rtcEngineConnectionDidLost` or `connectionLostBlock` callback notifies users that the server is disconnected. When a server is disconnected, the SDK tries to reconnect automatically.

### What happens after calling initWithAppId multiple times?

Calling `initWithAppId` multiple times may result in abnormal camera behaviors and other problems.

### How do encryption and decryption impact the system?

Encryption and decryption may affect CPU performance and network latency. This requires testing in actual situations.

### Why does the phone screen rotate with the phone in a call?

If the screen rotation function is enabled in your phone settings, the screen rotates with the phone.

### Why does the screen display a frozen image of the other user after the network reconnects?

It takes some time for the video communication to resume after the network reconnects. Latency is a common problem for all video communications and worsens in poor network conditions.

### What should I do when the user ID is a string?

The user ID in the SDK only accepts 32-bit unsigned integers. Agora recommends mapping the strings into integers on your client server.

### How do I retrieve a successful volume callback message?

The volume indicator is off by default. Call the `enableAudioVolumeIndication` method to enable the callback before or after joining a channel.

### Why does the initialization fail on Windows XP?

Starting from v1.1, the Agora SDK uses Visual C++ 2013. Since Windows XP does not include the Visual C++ Redistributable Packages by default, download and install them from the Microsoft website: https://www.microsoft.com/en-us/download/details.aspx?id=40784

### What is a callback?

There are two programming categories:

* System Programming: Developing libraries related to the system.
* Application Programming: Developing applications with functions that use the libraries related to the system.

A system programmer develops interfaces (Application Programming Interfaces) for the application programmers to use.

When an application is running, it calls functions included in the APIs. Callback functions require the application to send them a function to call first in order to execute the task.

The following figure shows how callback functions work:

![](https://web-cdn.agora.io/docs-files/1543545706059)

### When debugging in Windows, it says that agorartc.pdb is required. Where can I get it?

You can ignore this message as this file is not necessary.

### Why can't I join a channel?

1. Check whether there is an error code.
2. If the error code indicates that you did not leave the last channel, call the `leaveChannel` method and rejoin the channel.

### Why can't I authenticate the dynamic key?

1. Check the algorithm to generate the Dynamic Key.
2. Check the App ID and App Certificate; note that they are case-sensitive.
3. Check whether `expireTime` expired.

### Why does the iOS app crash after a user joins a channel and switches to Background Mode?

This issue is caused if you did not select the Audio, AirPlay, and Picture-in-Picture options in Background Mode when integrating the Agora SDK in Xcode.

### Why is the call interrupted after the app switches to Background Mode on iOS?

To fix this issue, enable Background Mode in Xcode:

1. Open your project in Xcode.
2. Select **Project** > **Capabilities**.
3. Enable **Background Mode** and select the **Audio**, **AirPlay**, **Picture-in-Picture**, and **Voice over IP** options.

### Why does the log contain “Failed to decode frame xxx, return error code is -1”?

This is a decoding failure, most likely due to a bad network connection causing the device not to receive a full frame. Check the network conditions.

### Why did an exception message occur when starting the app?

After integrating the Agora SDK, the following exception message occurs when you start the app:

dalvik.system.PathClassLoader[DexPathList[[zip file xxxxx.apk],nativeLibraryDirectories=[/data/app/xxxxx/lib/arm,/vendor/lib/,system/lib]]] couldn’t find "agoraxxx.so" java.lang.Runtime.loadLibrary(Runtime.java:366)…

During integration, the .so file is not in the correct path. Therefore, the app cannot find it after boot. Ensure that everything is in the correct path before integrating the package.



