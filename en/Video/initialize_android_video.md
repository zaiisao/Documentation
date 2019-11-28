
---
title: Create and Initialize an Agora Instance
description: 
platform: Android
updatedAt: Tue Oct 22 2019 06:19:07 GMT+0800 (CST)
---
# Create and Initialize an Agora Instance
## Prerequisites

Before creating an RtcEngine instance, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/android_video.md).

Create a project in Agora Console and get the App ID of the project. You need to pass in the App ID during initialization.

1. Sign up for a developer account at [Agora Console](https://dashboard.agora.io/). See [Sign in and Sign up](../../en/Video/sign_in_and_sign_up.md).

2. Click ![](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to enter the [**Project Management**](https://dashboard.agora.io/projects) page.

3. Click **Create**. 

![](https://web-cdn.agora.io/docs-files/1574924327108)

4.  Enter your project name and select your authentication mechanism ("App ID") in the dialog box.

![](https://web-cdn.agora.io/docs-files/1574924446798)
	
5. Click **Submit** and you can find the **App ID** of your newly created project.

![](https://web-cdn.agora.io/docs-files/1574924570426)


## Implementation
The following imports define the interface of the Agora API that provides  communication functionality:

-   `io.agora.rtc.Constants`
-   `io.agora.rtc.IRtcEngineEventHandler`
-   `io.agora.rtc.RtcEngine`
-   `io.agora.rtc.video.VideoCanvas`

Create a singleton instance by invoking the `create` method during initialization. In the `create` method:

-  Pass the Agora App ID. Only apps with the same App ID can join the same channel.
-  Specify a reference to the activity’s event handler. The Agora API uses callbacks to inform the app about Agora engine runtime events, such as joining or leaving a channel and adding users.

```
import io.agora.rtc.Constants;
import io.agora.rtc.IRtcEngineEventHandler;
import io.agora.rtc.RtcEngine;

...

private void initializeAgoraEngine() {
    try {
        mRtcEngine = RtcEngine.create(getBaseContext(), getString(R.string.agora_app_id), mRtcEventHandler);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));

        throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```

> Ensure that you call the `create` method to intiialize the AgoraRtcEngine before calling any other API. 

## Next Steps
You have created the RtcEngine instance and can start a video call with the following steps:
* [Join a Channel](../../en/Video/join_video_android.md)
* [Publish and Subscribe to Streams](../../en/Video/publish_android.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:
* [Conduct a Last Mile Test](../../en/Video/lastmile_android.md)
* [Set the Stereo/High-fidelity Audio Profile](../../en/Video/audio_profile_android.md)
