
---
title: Android 平台常见问题
description: 
platform: Android 平台常见问题
updatedAt: Thu Nov 01 2018 08:10:39 GMT+0000 (UTC)
---
# Android 平台常见问题
# Android 平台常见问题

### 安卓平台上，可以将 SDK 代码与我自己的代码混用吗？

不能，否则无法回调 `.so` 文件。

### 集成 SDK 后，点击 App 中视频小窗口没有反应，也无法拖动，我该怎么办?

检查系统上有没有开启悬浮窗权限。如果没有开启该权限，App 是无法启动小窗口的。

### 可以在 Android 设备上做 64 位兼容吗？

取决于你使用的 Agora Native SDK 版本：

* 如果你使用的是 1.7.4 版 (或更高版本) Agora Native SDK：
  Agora Native SDK 目前支持 64 位的 ARM 架构，只需将 arm64-v8a 里面的文件(位于SDK包内)拷贝至项目对应的 arm64-v8a 路径下。目前不支持 x86_64 架构直接运行，不过可以以 x86 方式兼容运行。具体做法就是在 64 位的 x86 设备上保证 APK 里面没有   x86_64 目录。

* 如果你使用的是 1.7.4 版之前的 Agora Native SDK:
   Agora Native SDK 目前只提供 32 位 native 库（armeabi-v7a），在 64 位设备上，Android 支持以 32 位进程模式启动 APK，因此 Agora Native SDK 也可以在 64 位 Android 设备上使用，但必须保证 APK 的 arm64 目录为空。否则 Android 系统将以 64 位进程模式加载 APK，由于 Agora Native SDK 没有提供 64 位库，APK 启动会失败。

由于目前 Android 的主流还是 32 位设备，一般各厂商都会提供 32 位库，因此一般来说，在 64 位 Android 设备以 32 位进程模式启动一般不会有问题。但是如果在某些 64 位平台上出现了这样的错误: java.lang.UnsatisfiedLinkError: dlopen failed: ”libHDACEngine.so”

但是如果 64 位平台上出现以下报错: java.lang.UnsatisfiedLinkError: dlopen failed: "libHDACEngine.so"

可能的原因：

在安装 APK 的时候，系统会按照 `Build.SUPPORTED_ABIS` 去查找 APK 的 lib 目录下的 native 库的目录（现有的 ABI: armeabi, armeabi-v7a,arm64-v8a, x86, x86_64, mips64, mips）。如果在 app 中有兼容 64-bit 的目录但是又缺少库文件的话，并不会使用其他 ABI 目录下的库文件替换所缺少的库文件进行安装，这些库不混合使用，也就是说需要为每个架构提供对应的库文件。Android 在加载 native 库的时候有回退（fallback）机制，在64位系统上如果 app 并不存在 arm64-v8a 的目录，则会尝试寻找armeabi-v7a下面的库进行加载，一般来说是向下兼容的。

解决方案:
* 方法 1: 在构建应用程序的时候，在工程 (project) 里删除所有 arm64-v8a 下面的库以及该目录；在生成 app 后，确认 apk 的包内 lib 下没有 arm64-v8a 的目录。
* 方法 2: 在 gradle 构建文件中设置 abiFilters，只打包 32 位架构的库:
          android {
          ...
          defaultConfig {
          ...
          ndk {
          abiFilters "armeabi-v7a","x86"
          }
          }
          }

### 为什么我会收到到错误消息: Failed to crunch file?

当您在使用 Android Open Video Call demo 时，如果遇到以下错误消息，例如:
Error: Failed to crunch file E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\exploded-aar\com.android.support\appcompat-v7\25.0.0\res\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png
into
E:\Rock\videoIM\Agora_Native_SDK_for_Android_v1_7_4_FULL\Agora_Native_SDK_for_Android_FULL\samples\OpenVideoCall_Android\app\build\intermediates\res\merged\debug\drawable-xhdpi-v4\abc_ab_share_pack_mtrl_alpha.9.png

该问题是由于 demo 引用的文件名过长造成的。
