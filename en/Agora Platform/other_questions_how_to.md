
---
title: Other Questions
description: 
platform: Other Questions
updatedAt: Thu Nov 29 2018 02:56:15 GMT+0000 (UTC)
---
# Other Questions
## Android

### Can I mix my code with the SDK code on Android?

No, the `.so` files cannot be called if the codes are mixed.

### Why does the Android suspension window not respond to clicking or dragging after integrating the Agora SDK?

Check whether the system suspension window permission is enabled. The app cannot open the window if it is disabled.

### Can I use the Agora Native SDK on 64-bit Android devices?

* If you are using the Agora Native SDK v1.7.4+:

   The Agora Native SDK supports the 64-bit ARM architecture, and you only need to copy the files from the arm64-v8a folder (included in the SDK package) to the corresponding folder in your project.
Currently, only x86 compatibility mode is supported but not x86_64. Ensure that there is no x86_64 directory in the APK of any x86 64-bit device.

* If you are using an Agora Native SDK before v1.7.4:

   The Agora Native SDK provides a 32-bit native library (armeabi-v7a). On 64-bit devices, Android allows starting the APK in the 32-bit processing mode. Ensure that the arm64 folder of the APK is empty. Otherwise, the Android system loads the APK in the 64-bit mode and the APK boot will fail.

Since most Android clients are 32-bit devices, manufacturers generally provide 32-bit libraries. Hence, it is common to start in the 32-bit processing mode on 64-bit Android devices.

If you see the following error on 64-bit devices: `java.lang.UnsatisfiedLinkError: dlopen failed: "libHDACEngine.so"`

Possible reasons:

When installing the APK, the system will look for the directory of the native library (the existing ABI:armeabi, armeabi-v7a, arm64-v8a, x86, x86_64, MIPS64, MIPs) under the lib directory of the APK according to `Build.SUPPORTED_ABIS` lib.

If the app has a 64-bit compatible directory with missing library files, do not replace them from the ABI directory. The library files cannot be mixed. The corresponding library files for each architecture must be used.

On Android, if there is no arm64-v8a directory on a 64-bit system, it will try to find the libraries under armeabi-v7a.

Solution:

* Method 1: When building the application, delete the arm64-v8a directory in the project. After the application is built, ensure that there is no arm64-v81directory under lib in the APK package.
* Method 2: Set abiFilters in the gradle build file, and only pack the 32-bit library.
 
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

This error may occur when using the Android Open Video Call demo:

```
Error: Failed to crunch file E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\exploded-aar\com.android.support\appcompat-v7\25.0.0\res\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
into
E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\res\merged\debug\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
```

This error occurs when the referenced file name is too long.

## Login

### Why am I automatically logged out of a device?

You will be automatically logged out of your current device if you log in another device.

### Does the Agora SDK reconnect when a user drops offline or a process is killed?

#### Case 1: User Drops Offline

User A and user B are in the same channel and user A loses network connection:

1. User A loses connection with the server (no data was received from the server within four seconds):  
 * Android, Windows, or Linux: User A receives the `onConnectionInterrupted` callback.
 * iOS or macOS: User A receives the `rtcEngineConnectionDidInterrupted` callback.
 * The Web: User A does not receive any callback.
2. After being disconnected, user A tries to connect to other servers until a successful connection.
 * If user A does not reconnect to the server successfully within 10 seconds (you can set the timeout value. For more information, contact support@agora.io):
    * Android, Windows, or Linux: User A receives the `onConnectionLost` callback.
    * iOS or macOS: User A receives the `rtcEngineConnectionDidLost` callback.
    * The Web: User A does not receive any callback.  
 * User B receives a callback if no packet was received from user A within a specified time frame:
    * Android, Windows, or Linux: If user B did not receive any packet from user A within 20 seconds, user B receives the `onUserOffline` callback.
    * iOS or macOS: If user B did not receive any packet from user A within 20 seconds, user B receives the `didOfflineOfUid` callback.
    * The Web: If user B did not receive any packet from user A within 10 seconds, user B receives the `client.on('stream-removed')` callback.
 * If user A successfully reconnects to the server:
    * Android, Windows, or Linux: User A receives the `didRejoinChannel` callback.
    * iOS or macOS: User A receives the `didRejoinChannel` callback.
    * The Web: User A does not receive any callback.
 * If user B does not receive any callback indicating that user A has failed to reconnect, then user B will not receive a callback even if user A has successfully reconnected to the server.
 * If user B previously received a callback indicating that user A has failed to reconnect to the server, then:
    * Android, Windows, or Linux: User B receives the `onUserJoined` callback.
    * iOS or macOS: User B receives the `didJoinedOfUid` callback.
    * The Web: User B receives the `client.on('stream-added')` callback.

#### Case 2: Process is Killed

This scenario involves the following situations:

* Enabled or disabled VoIP mode.
* A process is killed.
* Closed a web page (the Web).

User A and user B are in the same channel.

When the process of user A gets killed:

* iOS or macOS: User A calls the `leaveChannel` method and user B will receive a callback:
   * Android, Windows, or Linux: User B receives the `onUserOffline` callback.
   * iOS or macOS: User B receives the `didOfflineOfUid` callback.
   * The Web: User B receives the `client.on('peer-leave')` callback.
* If user A is in Android, Windows, or Linux and user B uses the Native SDK:
   * If user A has not restarted the app and re-joined the original channel within 20 seconds (you can set the timeout value. For more information, contact support@agora.io), user B will receive a callback:
      * Android, Windows, or Linux: User B receives the `onUserOffline` callback.
      * iOS or macOS: User B receives the `didOfflineOfUid` callback.
   * If user A restarts the app and rejoins the original channel within 20 seconds, user B will not receive any callback.
* If user A is in Android, Windows, or Linux and user B uses the Web SDK:
   * If user A has not restarted the app and re-joined the original channel within 10 seconds, user B will receive the `client.on('stream-removed')` callback.
   * If user A restarts the app and rejoins the original channel, user B will not receive any callback.
* For the Web SDK, killing a process is equivalent to a user dropping offline.
* If user A is the last user in the channel, the server will destroy the channel in 10 seconds.

## Channel

### In poor network conditions, does the SDK force users to leave a channel?

No, users will not automatically leave a channel unless they do so themselves. For example, the application calls the `leaveChannel` method.

### Does each channel/room need an administrator in a call?

No, an administrator can only be implemented in the signaling layer. Your signaling server sends commands and calls the SDK interfaces for call management.

### Does the client need to maintain the channel?

No, a channel is created and deleted automatically. When all users have left the channel, the channel will be deleted automatically.

### What are the App ID and Dynamic Key? How can I use them?

See [Use Security Keys](../../en/voice/token.md) for details.

### If both the App ID and Dynamic Key are used at the same time, which one prevails?

The Dynamic Key.

### How do I check who is talking in the channel?

The following callbacks indicate who is talking and the speaker’s volume.

* For Android and Windows: `onAudioVolumeIndication`
* For iOS and macOS: `reportAudioVolumeIndicationOfSpeakers`

By default, these callbacks are disabled. You can use the `enableAudioVolumeIndication` method to enable them.

### What should I be aware of for initialization-related API methods in Windows?

* Always ensure that the initialize method is called before joining a channel.
* If you want to enable the video function, call the `enableVideo` method before joining a channel.
* If you want to enable message transmission in the same channel, call the `enableVendorMessage` method before joining a channel.

## Others

### Which callbacks are important to apps?

Apps need information about user status-related callbacks, such as `userJoined` and `userOffline`. All user-related information and remote user callbacks are transmitted over the UDP, which is not reliable.

### What error codes are important?

All error codes are important, and warning codes can be ignored.

### What error codes mean that I should end the call?

When an error code is received, you should end the call.

### Which callback informs users that the server is disconnected?

The `rtcEngineConnectionDidLost` or `connectionLostBlock` callback notifies users that the server is disconnected. When a server is disconnected, the SDK will try to reconnect automatically.

### What happens after calling initWithAppId multiple times?

Calling `initWithAppId` multiple times may result in abnormal camera behaviors and other problems.

### How do encryption and decryption impact the system?

Encryption and decryption may affect CPU performance and network latency. This requires testing in actual situations.

### Why does the phone screen rotate with the phone in a call?

If the screen rotation function is enabled in your phone settings, the screen will rotate with the phone.

### Why does the screen display a frozen image of the other user after the network is reconnected?

It takes some time for the video communication to resume after the network is reconnected. Latency is a common problem for all video communications and worsens in poor network conditions.

### What should I do when the user ID is a string?

The user ID in the SDK only accepts 32-bit unsigned integers. Agora recommends mapping the strings into integers on your client server.

### How do I retrieve a successful volume callback message?

The volume indicator is off by default. Call the `enableAudioVolumeIndication` method to enable the callback before or after joining a channel.

### Why did the initialization fail on Windows XP?

Starting from v1.1, the Agora SDK uses Visual C++ 2013. Since Windows XP does not include the Visual C++ Redistributable Packages by default, download and install them from the Microsoft website: https://www.microsoft.com/en-us/download/details.aspx?id=40784

### When setting the video profile by calling the setVideoProfile method, why do some profiles have the same frame rate but different bitrates?

Different users set different video parameters:

<table>
  <tr>
    <th>Video Profile</th>
    <th>Enumeration Value</th>
    <th>Resolution (Width * Height)</th>
    <th>Frame Rate</th>
    <th>Bitrate (kbps)</th>
  </tr>
  <tr>
    <td>360P</td>
    <td>30</td>
    <td>640 x 360</td>
    <td>15</td>
    <td>400</td>
  </tr>
  <tr>
    <td>360P_9</td>
    <td>38</td>
    <td>640 x 360</td>
    <td>15</td>
    <td>800</td>
  </tr>
</table>

Users who set the bitrate as 800 Kbps prefer a higher image quality.

### What is a callback?

There are two programming categories:

* System Programming: Developing libraries related to the system.
* Application Programming: Developing applications with functions that use the libraries related to the system.

A system programmer develops interfaces (Application Programming Interfaces) for the application programmers to use.

When an application is running, it calls functions included in the APIs. Callback functions require the application to send them a function to call first in order to execute the task.

The following figure shows how callback functions work:

![](https://web-cdn.agora.io/docs-files/1539331143464)

### When debugging in Windows, it says that agorartc.pdb is required. Where can I get it?

You can ignore this message as this file is not necessary.

### Why can't I join a channel?

1. Check whether there is an error code.
2. If the error code indicates that you did not leave the last channel, call the `leaveChannel` method and rejoin the channel.

### Why can't I authenticate the dynamic key?

1. Check the algorithm to generate the Dynamic Key.
2. Check the App ID and App Certificate; note that they are case-sensitive.
3. Check whether `expireTime` has expired.

### Why did the iOS app crash after the user joined a channel and switched to Background Mode?

This issue is caused if you did not select the Audio, AirPlay, and Picture-in-Picture options in Background Mode when integrating the Agora SDK in Xcode.

### Why can't I hear anything after the call is established?

Ensure that the user has initialized the media when initializing signaling. For example, with `getInstanceWithMediaKey`.

By "establishing a call", we mean that A has sent a call invitation, and B has received it. They both need to join the channel to hear each other.

### Why can't I put a call through?

* You must be signed in to start a call.
* The call fails if the other user is not online.

### The call is interrupted after the app switches to Background Mode on iOS?

To fix this issue, enable Background Mode in Xcode:

1. Open your project in Xcode.
2. Select **Project** > **Capabilities**.
3. Enable **Background Mode** and select the **Audio**, **AirPlay**, **Picture-in-Picture**, and **Voice over IP** options.

### Why does the log contain “Failed to decode frame xxx, return error code is -1”?

This is a decoding failure, most likely due to a bad network connection causing the device not to receive a full frame. Check the network conditions.

### Why did initialization fail on Windows XP?

Starting from v1.1, the Agora SDK uses Visual C++ 2013. Since Windows XP does not include the Visual C++ Redistributable Packages by default, download and install them from the Microsoft website：https://www.microsoft.com/en-us/download/details.aspx?id=40784.

### Why did an exception message occur when starting the app?

After integrating the Agora SDK, the following exception message occurs when you start the app:

dalvik.system.PathClassLoader[DexPathList[[zip file xxxxx.apk],nativeLibraryDirectories=[/data/app/xxxxx/lib/arm,/vendor/lib/,system/lib]]] couldn’t find "libHDACEngine.so" java.lang.Runtime.loadLibrary(Runtime.java:366)…

During integration, the .so file is not in the correct path. Therefore, the app cannot find it after boot. Ensure that everything is in the correct path before integrating the package.



