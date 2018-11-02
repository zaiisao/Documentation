
---
title:  Android Specific Questions
description: 
platform:  Android Specific Questions
updatedAt: Fri Nov 02 2018 04:18:37 GMT+0000 (UTC)
---
#  Android Specific Questions
### Can I mix my code with the SDK code on Android?

No, the .so files cannot be called if the codes are mixed.

### Why does the Android suspension window not respond to clicking or dragging after integrating the Agora SDK?

Check whether the system suspension window permission is enabled. If it is disabled, the app cannot open the window.

### Can I use the Agora Native SDK on 64-bit Android devices?

* If you are using the Agora Native SDK v1.7.4 or later:
    The Agora Native SDK supports the 64-bit ARM architecture, and you only need to copy the files in the arm64-v8a folder (included in the SDK package) to the corresponding folder in your project.
    Currently, only x86 compatibility mode is supported but not x86_64. Ensure there is no x86_64 directory in the APK of any x86 64-bit device.

* If you are using Agora Native SDK versions before v1.7.4:
     The Agora Native SDK provides a 32-bit native library (armeabi-v7a). On 64-bit devices, Android allows starting the APK in the 32-bit processing mode. Ensure the arm64 folder of the APK is empty, otherwise, the Android system loads the APK in 64-bit mode and the APK boot will fail.
     Since most Android clients are 32-bit devices, manufacturers generally provide 32-bit libraries, which means that it is common to start in the 32-bit processing mode on 64-bit Android devices.

* If you see the following error on 64-bit devices: java.lang.UnsatisfiedLinkError: dlopen failed: "libHDACEngine.so"

Possible Reasons:

When installing the APK, the system will look for the directory of the native library (the existing ABI:armeabi, armeabi-v7a, arm64-v8a, x86, x86_64, MIPS64, MIPs) under the lib directory of APK according to Build.SUPPORTED_ABIS lib.

If the app has a 64-bit compatible directory with missing library files, do not replace them from the ABI directory. The library files cannot mixed. The corresponding library files for each architecture must be used.

On Android, if there is no arm64-v8a directory on a 64-bit system, it will try to find the libraries under armeabi-v7a.

Solution:

Method 1: When building the application, delete the arm64-v8a directory in the project. After the application is built, ensure there is no arm64-v81directory under lib in the apk package.

Method 2: Set abiFilters in the gradle build file, and only pack the 32-bit library.

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

#### Why did I receive this error: Failed to crunch file?

This error may occur when using the Android Open Video Call demo:


```
Error: Failed to crunch file E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\exploded-aar\com.android.support\appcompat-v7\25.0.0\res\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
into
E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\res\merged\debug\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
```


This error occurs when the referenced file name is too long.



