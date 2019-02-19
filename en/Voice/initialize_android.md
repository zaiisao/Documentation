
---
title: Create and Initialize an Agora Instance
description: 
platform: Android
updatedAt: Thu Dec 13 2018 23:25:58 GMT+0000 (UTC)
---
# Create and Initialize an Agora Instance
Before creating an RtcEngine instance, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Voice/android_audio.md).

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

## Next Steps
You have created the RtcEngine instance and can start a voice call with the following steps:
* [Join a Channel](../../en/Voice/join_communication_android.md)
* [Publish and Subscribe to Streams](../../en/Voice/publish_android_audio.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:
* [Conduct a Last Mile Test](../../en/Voice/lastmile_android.md)
* [Set the Stereo/High-fidelity Audio Profile](../../en/Voice/audio_profile_android_audio.md)
