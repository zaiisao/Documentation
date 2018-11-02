
---
title: Integrate SDK
description: 
platform: macOS
updatedAt: Fri Nov 02 2018 04:06:12 GMT+0000 (UTC)
---
# Integrate SDK
This page contains information on how to prepare the development environment before enabling a video call with the Agora Video SDK.

## Prerequisites

- Xcode 9.0+.
- Physical OS X 10.0+.
- Ensure that your project has a validated provisioning profile certificate.
- Before accessing Agora’s services, ensure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).

> Use a physical device to run the sample. Simulators may lack the functionality or the performance needed to run the sample.

## Creating an Agora Account and Getting an App ID

1. Sign up for a developer account at [https://dashboard.agora.io/](https://dashboard.agora.io/).

2. Click **Add New Project** on the **Projects** page of the dashboard.

   <img alt="../_images/appid_1.jpg" src="https://web-cdn.agora.io/docs-files/en/appid_1.jpg" />

3. Fill in the **Project Name** and click **Submit**. You have created your first project at Agora.

4. Find the **App ID** under the created project.

   <img alt="../_images/appid_2.jpg" src="https://web-cdn.agora.io/docs-files/en/appid_2.jpg" />

## Adding the Agora SDK to Your Project

Choose one of the following methods to add the Agora SDK libraries to your project:

- [Adding the Libraries Automatically](#auto-add): Add the libraries automatically using CocoaPods. You do not need to download the SDK.
- [Adding the Libraries Manually](#man-add): Download the SDK and add the libraries manually.

### <a name = "auto-add"></a>Adding the Libraries Automatically

1. Install CocoaPods by running the following command in Terminal:

   ```
   brew install cocoapods
   ```

 > - Skip this step if you have preconfigured **CocoaPods** and **Homebrew** on your system.
 > - If Terminal says `-bash: brew: command not found`, install Homebrew before running the command. See [Homebrew Installation Method](http://brew.sh/index.html).

1. Create a Podfile in your project. In the root directory of your project, run the following command in Terminal. This creates a Podfile in the same directory.

   ```
   pod init
   ```

2. Add the Agora SDK reference in the Podfile. Open the Podfile, and fill it with the following content. Fill **“Your App”** with the name of your Target.

   ```
   platform :macOS, '10.11'
   use_frameworks!
   
   target 'Your App' do
     pod 'AgoraRtcEngine_macOS'
   end
   ```

3. Update the local Cocoapods library. Run the following command in Terminal:

   ```
   pod update
   ```

4. Install the related libraries.

   ```
   pod install
   ```

   If Terminal says `Pod installation complete!`, you have successfully added the libraries. Click open the `YourApp.xcworkspace` file, or run the following command to open it. Fill **“YourApp”** with the name of your Target.

   ```
   open YourApp.xcworkspace
   ```

### <a name = "man-add"></a>Adding the Libraries Manually

1. Download the [Agora Video SDK for macOS](https://docs.agora.io/en/Agora%20Platform/downloads) and unzip the downloaded SDK package.

2. Open your project with Xcode and select the current Target.

   <img alt="../_images/mac_video_1.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_1.jpg" />

3. Navigate to the **Build Phases** tab.

   <img alt="../_images/mac_video_2.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_2.jpg" />

4. Expand the **Link Binary with Libraries** section to add the following libraries. To begin adding new libraries, click the **+** button.

   - `libresolv.tbd`
   - `libc++.1.dylib`
   - `Accelerate.framework`
   - `SystemConfiguration.framework`
   - `CoreWLAN.framework`
   - `Foundation.framework`
   - `QTKit.framework`
   - `CoreAudio.framework`
   - `CoreMedia.framework`
   - `AVFoudation.framework`
   - `VideoToolbox.framework`
   - `AudioToolbox.framework`
   - `AgoraRtcEngineKit.framework`

   **Before:**

   <img alt="../_images/mac_video_3.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_3.jpg" />

   **After:**

   <img alt="../_images/mac_video_4.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_4.jpg" />

   `AgoraRtcEngineKit.framework` is in the **libs** folder of the downloaded SDK. Click **+** \> **Add Other…**, go to the downloaded SDK and add `AgoraRtcEngineKit.framework`.

   <img alt="../_images/mac_video_5.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_5.jpg" />

## Authorizing the Use of the Agora SDK

Before enabling a video call, you need to enable camera and microphone access to the SDK on your device. Open `info.plist` and click **+** to add:

- **Privacy - Microphone Usage Description**, and add a note in the **Value** colume.
- **Privacy - Camera Usage Description**, and add a note in the **Value** colume.

**Before:**

<img alt="../_images/mac_video_6.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_6.jpg" />

**After:**

<img alt="../_images/mac_video_7.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_7.jpg" />

## Accessing the Library

You can access the added library using [Objective-C](#oc) or [Swift](#swift).

### <a name = "oc"></a>Objective-C

In the main file that uses Agora APIs, add `#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>`.

> The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.

### <a name = "swift"></a>Swift

In the main file that uses Agora APIs, add `import AgoraRtcEngineKit`.

<img alt="../_images/mac_video_8.jpg" src="https://web-cdn.agora.io/docs-files/en/mac_video_8.jpg" />

The macOS environment is now set to use the Agora SDK.
