
---
title: Why can't I answer calls through a Bluetooth device after connecting an iOS device to it?
description: iOS 蓝牙耳机相关问题
platform: iOS
updatedAt: Fri Feb 28 2020 22:28:37 GMT+0800 (CST)
---
# Why can't I answer calls through a Bluetooth device after connecting an iOS device to it?
## Issue description

After connecting an iOS device to a Bluetooth device, you may encounter the following issues:

- Failing to answer calls through the Bluetooth headset even though you have connected the headset to your iOS device.
- Being unable to record and play back audio through the Bluetooth speaker after connecting an iOS device to a Bluetooth speaker.

## Reason

1. The iOS system selects audio routes for phone and VoIP calls, and the default audio routes may be different from what you expect:

   **The default audio route for a phone call (Including Facetime and CallKit)**
   - After connecting an iOS device to a Bluetooth device and tapping the answer button on iPhone, the default audio route is the iPhone speaker.
   - After connecting an iOS device to a Bluetooth device and tapping the answer button on a Bluetooth device, the default audio route is the Bluetooth device.

   **The default audio route for a VoIP call**

   - If you have answered phone calls after connecting an iOS device to a Bluetooth device, the default audio route is the one used by the last phone or VoIP call.
   - If you have not answered any phone call after connecting an iOS device to a Bluetooth device, the default audio route is the Bluetooth device.

   If the default settings mentioned above are in conflict with the audio routes you want, you need to change them when connecting your iOS device to Bluetooth. For details, see [Solution](#solution).

2. In an iOS device, the audio route for audio inputs and outputs must be the same. If you set the Bluetooth device as the audio route for recording, the system switches the audio route for playback to the Bluetooth device accordingly.

3. A Bluetooth speaker can only record audio during a system phone call, which includes calls made through Facetime and CallKit. If the App does not use the CallKit, all users who send audio streams cannot record or play back audio through the Bluetooth speaker, and all users who only receive audio streams can only play back audio through the Bluetooth speaker.

<a name="solution"></a>
## Solution

Depending on which type of call you have an issue with, choose one of the following solutions to set the audio routes:

**Phone call (Including Facetime and CallKit)**

- Before answering a phone call, change the audio route setting in **Settings** > **General** > **Accessibility** > **Call Audio Routing**. When you select the **Bluetooth Headset** option, all incoming calls will be answered through the Bluetooth device even if you press the answer button on the iPhone.
- During a phone call, you can switch between the **Bluetooth Headset**, **Handset**, or **Speaker** options in the call interface.
- If you connect an iOS device to a Bluetooth speaker and answer calls in an App, ensure that the App uses the CallKit, otherwise, the above settings do not work.

**VoIP call**

- Before making a VoIP call, you need to switch to the **Bluetooth Headset** mode in the **Control Center** which you can call out by swiping up from the bottom of the screen. Apps can call the `setPreferredInput` method to change the audio route.
- When a VoIP call through the Bluetooth device is interrupted by a phone call, tap the answer button on the Bluetooth device to answer the phone call, after which you can continue the VoIP call through the Bluetooth device once the phone call ends.
