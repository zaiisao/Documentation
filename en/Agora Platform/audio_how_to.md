
---
title: Audio-related Issues
description: 
platform: Audio-related Issues
updatedAt: Tue Jan 15 2019 09:45:40 GMT+0000 (UTC)
---
# Audio-related Issues
### My H5 game integrates the Agora SDK v2.2.0 for iOS. When the host uses WKWebview with Layabox and joins the channel, why is the game volume very low?
The H5 game loaded by WKWebView plays audio by AVAudioSession instead of the SDK.

To fix this issue, call the `AudioProfile` method and set `scenario` to the `GameStreaming` mode:
`self.agoraEngine setAudioProfile:(AgoraRtc_AudioProfile_Default) scenario:(AgoraRtc_AudioScenario_GameStreaming)`.

Setting `scenario` to the `GameStreaming` mode may lead to echo issues. If you have any concerns, please contact Agora technical support.

### Why is no audio captured when an Android 9 device locks its screen or the app on the device runs in the background?

According to the Android Developer website, this is a system restriction:

> **Limited access to sensors in background**
> Android 9 limits the ability for background apps to access user input and sensor data. If your app is running in the background on a device running Android 9, the system applies the following restrictions to your app:
> * Your app cannot access the microphone or camera.
> * Sensors that use the continuous reporting mode, such as accelerometers and gyroscopes, don't receive events.
> * Sensors that use the on-change or one-shot reporting modes don't receive events.
> If your app needs to detect sensor events on devices running Android 9, use a foreground service.

See [Behavior changes: all apps](https://developer.android.com/about/versions/pie/android-9.0-changes-all).

Developers can use **Foreground Service** to work around this restriction.
If you need to use an Android 9 device to capture audio after the device locks its screen, you can start a foreground service before the screen locks, and stop the service before exiting the screen lock. On how to start a service, see https://developer.android.com/reference/android/app/Service.

