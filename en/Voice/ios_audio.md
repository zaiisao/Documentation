
---
title: Integrate the SDK
description: 
platform: iOS
updatedAt: Fri May 17 2019 06:56:26 GMT+0800 (CST)
---
# Integrate the SDK
This page contains information on how to prepare the development environment before enabling a voice call with the Agora SDK for iOS.

## Prerequisites

Development environment:
- Xcode 9.0+.
- Physical iOS device 8.0+ \(iPhone or iPad\).
- Ensure that your project has a validated provisioning profile certificate.
- Before accessing Agora’s services, ensure that you open the ports and whitelist the domains specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).

> Use a physical device to run the sample. Emulators may lack the functionality or performance needed to run the sample.

## Create an Agora Account and Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io/).

2. Click the **Project Management** icon ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu and click **Create**.

3. Fill in the **Project Name** and click **Submit**. You have created your first project at Agora.

4. Find the **App ID** under the created project.

## Add the Agora SDK to Your Project

Choose one of the following methods to add the Agora SDK libraries to your project:

- [Adding the Libraries Automatically](#auto-add): Add the libraries automatically using *CocoaPods*. You do not need to download the SDK.
- [Adding the Libraries Manually](#man-add): Download the SDK and add the libraries manually.

### <a name = "auto-add"></a>Add the Libraries Automatically

1. Install *CocoaPods* by running the following command in Terminal:

   ```
   brew install cocoapods
   ```
	 
	> - Skip this step if you have preconfigured **CocoaPods** and **Homebrew** on your system.
	> - If you see `-bash: brew: command not found` in Terminal, install Homebrew before running the command. See [Homebrew Installation Method](http://brew.sh/index.html).

1. Create a Podfile in your project. In the root directory of your project, run the following command in Terminal. This creates a Podfile in the same directory.

   ```
   pod init
   ```

2. Add the Agora SDK reference in the Podfile. Open the Podfile, and put in the following content. Fill **“Your App”** with the name of your Target.

   ```
   platform :ios, '9.0'
   use_frameworks!
   
   target 'Your App' do
     pod 'AgoraRtcEngine_iOS'
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

   If you see `Pod installation complete!` in Terminal, you have successfully added the libraries. Click to open the `YourApp.xcworkspace` file, or run the following command to open it. Fill **“YourApp”** with the name of your Target.

   ```
   open YourApp.xcworkspace
   ```

### <a name = "man-add"></a>Add the Libraries Manually

1. Download the [Agora Voice SDK for iOS](https://docs.agora.io/en/Agora%20Platform/downloads) and unzip the downloaded SDK package.

2. Open your project with Xcode and select the current Target.

   <img alt="../_images/ios_voice_1.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_1.jpg" />

3. Navigate to the **Build Phases** tab. Expand the **Link Binary with Libraries** section to add the following libraries. To begin adding the new libraries, click the **+** button.

	<img alt="../_images/ios_voice_3.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_3.jpg" />

   - `AgoraAudioKit.framework`
   - `Accelerate.framework`
   - `SystemConfiguration.framework`
   - `libc++.tbd`
   - `libresolv.tbd`
   - `CoreMedia.framework`
   - `AudioToolbox.framework`
   - `CoreTelephony.framework`
   - `AVFoundation.framework`
   - `CoreML.framework`

 `AgoraAudioKit.framework` is in the **libs** folder of the downloaded SDK. Click **+** \> **Add Other…**, go to the downloaded SDK, and add `AgoraAudioKit.framework`.

   <img alt="../_images/ios_voice_5.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_5.jpg" />

## Authorize the Use of the Agora SDK

Before enabling a voice call, you need to enable microphone access to the SDK on your device. Open `info.plist` and click **+** to add:

**Privacy - Microphone Usage Description**, and add a note in the **Value** column.

**Before:**

<img alt="../_images/ios_voice_6.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_6.jpg" />

**After:**

<img alt="../_images/ios_voice_7.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_7.jpg" />

## Access the Library

You can access the added library using [Objective-C](#oc) or [Swift](#swift).

### <a name = "oc"></a>Objective-C

In the main file that uses the Agora APIs, add **\#import <AgoraAudioKit/AgoraRtcEngineKit.h\>**.

> The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.

### <a name = "swift"></a>Swift

In the main file that uses the Agora APIs, add `import AgoraAudioKit`.

<img alt="../_images/ios_voice_8.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_8.jpg" />

## Additional Settings and Permissions

The Agora SDK provides the following additional settings and permissions for you to optimize your project:

- Set the Background Modes. When the background mode is enabled, your application can still run the voice call when it is switched to the background. Select the current Target, click the **Capabilities** tab, enable **Background Modes**, and check **Audio, AirPlay, and Picture in Picture**.

  <img alt="../_images/ios_voice_9.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_9.jpg" />

- Enable or disable Bitcode. Applications developed with Bitcode can be optimized once it is uploaded to the App Store. Select the current Target, click the **Build Settings** tab, and enable or disable Bitcode according to your needs.

  <img alt="../_images/ios_voice_10.jpg" src="https://web-cdn.agora.io/docs-files/en/ios_voice_10.jpg" />
	
## Next Steps
You have set up the iOS environment and can start a call/live broadcast following the steps under **Quickstart Guide**:
* Initialize the SDK
* Join a Channel
* Publish and Subscribe to Streams
