
---
title: Audio-related Issues
description: 
platform: Audio-related Issues
updatedAt: Fri Jun 14 2019 01:56:44 GMT+0800 (CST)
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

### When a third-party recording app is being used on the Android device, the local user cannot send the local audio stream. Why is there no warning or error message from the SDK?

We recommed referring to the following logic when implementing your code:

Before the user joins the channel, use the Android native methods to get the status of the audio recorder. When the audio recorder is available, if, within 6 seconds after the user joins the channel, the app receives the following codes, the SDK  decides that the recording device is occupied:

- The warning code WARN_ADM_RECORD_AUDIO_LOWLEVEL(1031), triggered multiple times.
-  The error code ERR_ADM_RECORD_AUDIO_IS_ACTIVE(1033).

You can remind your user to quit the third-party recording app before using yours.

<a id="audioscenario"></a>
### Wht can't I set the volume to 0?

When you set the volume of a device, you set either the in-call volume or the media volume.

- In-call volume: During a audio or video call, you set the in-call volume.
- Media volume: When you play background music, video or games, you set the media volume.

The differences between the two are as follows:

- In-call volume features acoustic echo cancelling, and should always be above 0.
- Media volume features quality sound performance, and can be set 0.

The SDK provides six audio scenarios with the `setAudioProfile` method: `CHATROOM_ENTERTAINMENT`, `EDUCATION`, `GAME_STREAMING`, `SHOWROOM`, `CHATROOM_GAMING` and `DEFAULT`.

- If you set `scenario` to `GAME_STREAMING`, all users use the media volume.
- If you set `scenario` to `EDUCATION`, `SHOWROOM` or `DEFAULT`, audience users in the Live-broadcast profile use the media volume, while host users in the Live-broadcast profile and all users in the Communication profile use the in-call volume.
- If you set `scenario` to `CHATROOM_ENTERTAINMENT` and `CHATROOM_GAMING`, all users use the in-call volume.

Given the system restrictions, the media volume can be set to 0, but the in-call volume cannot. If you want to adjust the volume to 0, ensure that you set an audio scenario where the system uses the media volume.

