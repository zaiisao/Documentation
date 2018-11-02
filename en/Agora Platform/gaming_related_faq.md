
---
title: Gaming-related Issues
description: 
platform: Gaming-related Issues
updatedAt: Thu Nov 01 2018 09:22:24 GMT+0000 (UTC)
---
# Gaming-related Issues

# Gaming-related Issues

### Why are there errors when I compile the sample code with Unity3D 5.4?

The Agora sample code is written with Unity3D 5.5. When you compile it with Unity3D 5.4, you may encounter the following errors:

**Scenario 1**: CheckConsistency: GameObject does not reference component MonoBehaviour

When this error occurs, do the following:

1. Delete the HelloUnity3D scene in the project.
2. Create a new scene with the same name.
3. Add HelloUnity3D.cs to the scene.

**Scenario 2**: It crashes during runtime when the compilation is completed after exporting:

When this error occurs, do the following:

1. Check whether all the permissions are normal.
2.  Copy the AndroidManifest.xml of the submodule AgoraAudioKit.plugin to your project.

### Why is there a signing error when I compile the sample code on an iOS platform?

When the error displayed on the left of the following figure occurs, fix it according to the right side of the following figure:

![](https://web-cdn.agora.io/docs-files/1539338995380)

### Why is there a bitcode error when I compile the sample code on an iOS platform?

Disable the bitcode if the following error messages occur during the compilation:

![](https://web-cdn.agora.io/docs-files/1539339030927)

Do the following:

1. Select the current Target.
2. Select Build Settings.
3. Select Enable Bitcode and set it to No.

![](https://web-cdn.agora.io/docs-files/1539339116352)

### Why is there a core-telephony error when I compile the sample code on an iOS platform?

When an error displayed on the left of the following figure occurs, add the framework according to the right side of the following figure:

![](https://web-cdn.agora.io/docs-files/1539339192986)

### Why is there a libresolv error when I compile the sample code on an iOS platform?

When an error displayed on the left of the following figure occurs, add the library according to the right side of the following figure:

![](https://web-cdn.agora.io/docs-files/1539339238057)

### How can I export the gradle project when compiling the Unity sample code?

After finishing the compilation according to game-android, modify the following sample command to compile the code according to your actual environment.

```
/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/bin/java -classpath "/Applications/Unity/PlaybackEngines/AndroidPlayer/Tools/gradle/lib/gradle-launcher-2.14.jar" org.gradle.launcher.GradleMain "clean" "assembleDebug"
```
