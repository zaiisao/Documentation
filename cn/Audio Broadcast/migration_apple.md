
---
title: 升级至 3.0.1 版本 (iOS/macOS)
description: 
platform: All Platforms
updatedAt: Mon Jun 01 2020 07:25:04 GMT+0800 (CST)
---
# 升级至 3.0.1 版本 (iOS/macOS)
本文描述 Agora iOS SDK 和 Agora macOS SDK 中库的变更，及升级到 3.0.1 版本时用户需要注意的集成方法。

<div class="alert note">如果你使用的是 3.0.0 动态库版本 SDK，则无需重新集成 SDK。</div>

## 库变更

自 3.0.1 版本起，SDK 包内仅包含动态库 `AgoraRtcKit.framework`，你可以通过 [SDK 下载](https://docs.agora.io/cn/Agora%20Platform/downloads)获取 3.0.1 版本 SDK。下表列出了各版本库文件的区别：

| SDK 版本 | 库名 | 库类型 |
| ---------------- | ---------------- | ---------------- |
| 3.0.1 及以上      | `AgoraRtcKit`      | 动态库      |
| 3.0.0      | `AgoraRtcKit`      | 动态库、静态库      |
| 3.0.0 以下      | `AgoraRtcEngineKit`     | 静态库      |

## 从 3.0.0 静态库版本升级到 3.0.1 版本

1. 复制 3.0.1 版本 SDK 的 `AgoraRtcKit.framework` 至项目路径下，并替换 3.0.0 版本 SDK 的静态库文件。
2. 打开 Xcode（以 Xcode 11.0 为例），进入 **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content** 菜单，并点击 **-** 移除以下库文件：

<table>
     <tr>
         <td>操作系统</td>
         <td>库文件</td>
      </tr>
     <tr>
         <td>iOS</td>
         <td><li><tt>Accelerate.framework</tt><li><tt>AudioToolbox.framework</tt><li><tt>AVFoundation.framework</tt><li><tt>CoreMedia.framework</tt><li><tt>libc++.tbd</tt><li><tt>libresolv.tbd</tt><li><tt>SystemConfiguration.framework</tt><li><tt>CoreTelephony.framework</tt><li><tt>CoreML.framework</tt><li><tt>VideoToolbox.framework</tt></li></td>
     </tr>
     <tr>
         <td>macOS</td>
         <td><li><tt>Accelerate.framework</tt><li><tt>CoreWLAN.framework</tt><li><tt>libc++.tbd</tt><li><tt>libresolv.9.tbd</tt>
<li><tt>SystemConfiguration.framework</tt><li><tt>VideoToolbox.framework</tt></li></td>
     </tr>
 </table>

  <div class="alert note"><li>在 iOS 平台上，<tt>CoreTelephony.framework</tt> 仅适用于 Agora 语音 SDK。<tt>CoreML.framework</tt> 和 <tt>VideoToolbox.framework</tt> 仅适用于 Agora 视频 SDK。<li>在 macOS 平台上，<tt>VideoToolbox.framework</tt> 仅适用于 Agora 视频 SDK。</li></div>

3. 将 `AgoraRtcKit.framework` 的 **Embed** 属性改为 **Embed & Sign**。

## 从 3.0.0 之前版本升级到 3.0.1 版本

1. 打开 Xcode（以 Xcode 11.0 为例），在项目导航栏中移除 `AgoraRtcEngineKit.framework` 库文件。
2. 进入 **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content** 菜单，点击 **-** 移除以下库文件：

<table>
     <tr>
         <td>操作系统</td>
         <td>库文件</td>
      </tr>
     <tr>
         <td>iOS</td>
         <td><li><tt>Accelerate.framework</tt><li><tt>AudioToolbox.framework</tt><li><tt>AVFoundation.framework</tt><li><tt>CoreMedia.framework</tt><li><tt>libc++.tbd</tt><li><tt>libresolv.tbd</tt><li><tt>SystemConfiguration.framework</tt><li><tt>CoreTelephony.framework</tt><li><tt>CoreML.framework</tt><li><tt>VideoToolbox.framework</tt></li></td>
     </tr>
     <tr>
         <td>macOS</td>
         <td><li><tt>Accelerate.framework</tt><li><tt>CoreWLAN.framework</tt><li><tt>libc++.tbd</tt><li><tt>libresolv.9.tbd</tt>
<li><tt>SystemConfiguration.framework</tt><li><tt>VideoToolbox.framework</tt></li></td>
     </tr>
 </table>

  <div class="alert note"><li>在 iOS 平台上，<tt>CoreTelephony.framework</tt> 仅适用于 Agora 语音 SDK。<tt>CoreML.framework</tt> 和 <tt>VideoToolbox.framework</tt> 仅适用于 Agora 视频 SDK。<li>在 macOS 平台上，<tt>VideoToolbox.framework</tt> 仅适用于 Agora 视频 SDK。</li></div>

3. 点击 **+**，再点击 **Add Other…** 添加 3.0.1 版本 SDK 的 `AgoraRtcKit.framework` 库文件。
4. 修改 `AgoraRtcKit.framework` 的 **Embed** 属性为 **Embed & Sign**。
