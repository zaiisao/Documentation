
---
title: Other Issues
description: 
platform: Other Issues
updatedAt: Thu Nov 01 2018 09:23:52 GMT+0000 (UTC)
---
# Other Issues
# Other Issues

### Unable to Join a Channel

1. Check whether there is any error code.

2. If the error code indicates that you did not leave the last channel, call leaveChannel and rejoin the channel.

### Unable to Authenticate the Dynamic Key

1. Check the algorithm to generate the Dynamic Key.

2. Check the App ID and App Certificate; note that they are case-sensitive.

3. Check whether expireTime has expired.

### The iOS App Crashes After the User Joined a Channel, then Switched to Background Mode

This issue is caused if you did not select Audio, AirPlay, and Picture in Picture in Background Modes when integrating the Agora SDK in Xcode.

### The Call Succeeded With No Audio

1. Confirm that the user has initialized the media when initializing signaling, for example with getInstanceWithMediaKey.

2. You must join a channel after a successful call.

### Unable to Call Through

1. You must be signed in to initiate a call.

2. The call fails if the other user is not online.

### Call Interruption After the App Enters into Background Mode on iOS

In your application:

1. Select project>Capabilities.

2. Open Background Mode and select the Audio, AirPlay and Picture in Picture, and Voice over IP options.

### The log contains “Failed to decode frame xxx, return error code is -1”

This is a decoding failure, most likely due to a bad network connection causing the device not to receive a full frame. Check the network environment.

### Initialization on Windows XP Fails

Starting from v1.1, the Agora SDK uses Visual C++ 2013. Since Windows XP does not include the Visual C++ Redistributable Packages by default, download and install them from the Microsoft website：https://www.microsoft.com/en-us/download/details.aspx?id=40784.

### An Exception Message Occurs when Starting the App

After integrating the Agora SDK, the following exception message occurs when you start the app:

```
dalvik.system.PathClassLoader[DexPathList[[zip file xxxxx.apk],nativeLibraryDirectories=[/data/app/xxxxx/lib/arm,/vendor/lib/,system/lib]]] couldn’t find "libHDACEngine.so" java.lang.Runtime.loadLibrary(Runtime.java:366)…
```

During integration, the .so file is not in the correct path. Therefore, the app cannot find it after booting. Make sure everything is in the correct path before integrating the package.








