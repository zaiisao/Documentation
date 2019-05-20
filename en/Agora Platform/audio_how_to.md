
---
title: Audio-related Issues
description: 
platform: Audio-related Issues
updatedAt: Mon May 20 2019 07:32:15 GMT+0800 (CST)
---
# Audio-related Issues
### My H5 game integrates the Agora SDK v2.2.0 for iOS. When the host uses WKWebview with Layabox and joins the channel, why is the game volume very low?
The H5 game loaded by WKWebView plays audio by AVAudioSession instead of the SDK.

To fix this issue, call the `AudioProfile` method and set `scenario` to the `GameStreaming` mode:
`self.agoraEngine setAudioProfile:(AgoraRtc_AudioProfile_Default) scenario:(AgoraRtc_AudioScenario_GameStreaming)`.

Setting `scenario` to the `GameStreaming` mode may lead to echo issues. If you have any concerns, please contact Agora technical support.

### Why is no audio or video captured when an Android 9 device locks its screen or the app on the device runs in the background?

According to the Android Developer website, this is a system restriction:

> **Limited access to sensors in background**
> Android 9 limits the ability for background apps to access user input and sensor data. If your app is running in the background on a device running Android 9, the system applies the following restrictions to your app:
> * Your app cannot access the microphone or camera.
> * Sensors that use the continuous reporting mode, such as accelerometers and gyroscopes, don't receive events.
> * Sensors that use the on-change or one-shot reporting modes don't receive events.
> If your app needs to detect sensor events on devices running Android 9, use a foreground service.

See [Behavior changes: all apps](https://developer.android.com/about/versions/pie/android-9.0-changes-all).

Developers can use **Foreground Service** to work around this restriction.
If you need to use an Android 9 device to capture audio or video after the device locks its screen, you can start a foreground service before the screen locks, and stop the service before exiting the screen lock. On how to start a service, see https://developer.android.com/reference/android/app/Service.

### Why is no audio heard or the audio routing abnormal after the Android device joins the channel?

Some Android sample apps provided by Agora maintain a global RtcEngine instance in WorkerThread that keeps alive while the app is running and is destroyed when the app process is destroyed.

The problem of no audio or abnormal audio routing may occur when developers fail to manage WorkerThread appropriately.

In their design, developers tend to operate on WorkerThread to manage the life cycle of the RtcEngine instance, which is quite right for creating the engine and joining a channel. But when they quit the WorkerThread, they do not destroy the RtcEngine instance. This may cause problems, especially when the life cycle of the WorkerThread is not the same as the app process.

By calling `destroy`, the RtcEngine removes all registered system listeners (in this case, PhoneStateListener), some of which may reference to the Looper of the current Thread. If a system listener is not removed when WorkerThread quits, the listener still monitors but the Looper it references to is already invalid, leading to a dead binder error.

Agora recommends using one of the following solutions to solve this problem:

* Maintain one WorkerThreader.
* Call `destroy` to release the RtcEngine instance when exiting the channel.
