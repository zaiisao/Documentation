
---
title: Implement Video for Gaming
description: 
platform: Unity_(Android)
updatedAt: Mon Apr 13 2020 15:40:09 GMT+0800 (CST)
---
# Implement Video for Gaming
<div class="alert note">Agora no longer maintains the Agora Unity SDK for Interactive Gaming, use the Agora RTC Unity SDK instead. See more details in <a href="https://docs.agora.io/en/Video/start_call_unity?platform=Unity">Start a Video Call</a >.</div>

## Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the latest Unity video kit. See the following structure:

    ![](https://web-cdn.agora.io/docs-files/1548828163644)


2.  Hardware and software requirements:

    -   Unity 5.5 or later
    -   Android Studio 2.0 or later
    -   Two or more Android 4.0 or later devices with video and audio functions

3.  [Getting an App ID](../../en/Interactive%20Gaming/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


## Step 2: Create a Project

1. Open Unity to create a new project. Select **3D**.

    <img alt="../_images/AMG-Video-Unity3D_23.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_23.png" />

2. Add **Game Object: Sphere** and one **Button**in the default scene.

    <img alt="../_images/AMG-Video-Unity3D_24.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_24.png"/>

3.  Save the scene to `Assets/playscene.unity`.

For information on how to use the Unity, refer to the [offical Unity documentation](https://docs.unity3d.com/2018.2/Documentation/Manual).

## Step 3: Add the SDK

1. Open the root directory of your project and create:

	- `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/libs`
	- `Assets/Scripts`

2. Copy the `libs/Scripts/AgoraGamingSDK` from the downloaded SDK to the `Assets/Scripts` directory of your project.
3. Copy the `libs/agora-rtc-sdk.jar` file in the SDK to `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/libs` path of your project.
4. Copy the `Assets/Plugins/Android/mainTemplate.gradle` file from the Sample in the SDK to the `Assets/Plugins/Android` path of your project.
5. Copy the `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/AndroidManifest.xml` file from the Sample in the SDK to the `Assets/Plugins/Android/AgoraRtcEngineKit.plugin` directory of your project.
6. Copy the **armeabi-v7a** and **x86** files in `libs/Android` of the SDK to the `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/libs` path of your project.

## Step 4: Add Permissions

Add the following permissions to the `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/AndroidManifest.xml` file:

```C#
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

## Step 5: Prevent Code Obfuscation

Add the following line in the `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/project.properties` to obfuscate the code:

```C#
-keep class io.agora.**{*;}
```


## Step 6: Call the APIs

Follow [Interactive Gaming API](../../en/Interactive%20Gaming/game_unity.md) to call the APIs to implement the required functions. The following figure shows how to create a C\# script `example.cs`:

<img alt="../_images/AMG-Video-Unity3D_25.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_25.png" />

```
using UnityEngine;
using System.Collections;
using UnityEngine.UI;
using agora_gaming_rtc;

public class example : MonoBehaviour
{
    private IRtcEngine mRtcEngine;
    private string mVendorKey = <your app id>;

    // Use this for initialization
    void Start ()
    {
        GameObject g = GameObject.Find (“Join”);
        Text text = g.GetComponentInChildren<Text>(true);
        text.text = “Join”;
    }

    // Update is called once per frame
    void Update ()
    {

    }

    public void onButtonClicked() {
        GameObject g = GameObject.Find (“Join”);
        Text text = g.GetComponentInChildren<Text>(true);
        if (ReferenceEquals (mRtcEngine, null)) {
            startCall ();
            text.text = “Leave”;
        } else {
            endCall ();
            text.text = “Join”;
        }
    }

    void startCall()
    {
        // init engine
        mRtcEngine = IRtcEngine.getEngine (mVendorKey);
        // enable log
        mRtcEngine.SetLogFilter (LOG_FILTER.DEBUG | LOG_FILTER.INFO | LOG_FILTER.WARNING | LOG_FILTER.ERROR | LOG_FILTER.CRITICAL);

        // set callbacks (optional)
        mRtcEngine.OnJoinChannelSuccess = onJoinChannelSuccess;
        mRtcEngine.OnUserJoined = onUserJoined;
        mRtcEngine.OnUserOffline = onUserOffline;

        // enable video
        mRtcEngine.EnableVideo();
        // allow camera output callback
        mRtcEngine.EnableVideoObserver();

        // join channel
        mRtcEngine.JoinChannel(“exampleChannel”, null, 0);
    }

    void endCall()
    {
        // leave channel
        mRtcEngine.LeaveChannel();
        // deregister video frame observers in native-c code
        mRtcEngine.DisableVideoObserver();

        IRtcEngine.Destroy ();
        mRtcEngine = null;
    }

    // Callbacks
    private void onJoinChannelSuccess (string channelName, uint uid, int elapsed)
    {
        Debug.Log (“JoinChannelSuccessHandler: uid = “ + uid);
    }

    // When a remote user joined, this delegate will be called. Typically
    // create a GameObject to render video on it
    private void onUserJoined(uint uid, int elapsed)
    {
        Debug.Log (“onUserJoined: uid = “ + uid);
        // this is called in the main thread

        // find a game object to render the video stream from ‘uid’
        GameObject go = GameObject.Find (uid.ToString ());
        if (!ReferenceEquals (go, null)) {
            return; // reuse
        }

        // create a GameObject and assign it to this new user
        go = GameObject.CreatePrimitive (PrimitiveType.Plane);
        if (!ReferenceEquals (go, null)) {
            go.name = uid.ToString ();

            // configure videoSurface
            VideoSurface o = go.AddComponent<VideoSurface> ();
            o.SetForUser (uid);
            o.mAdjustTransfrom += onTransformDelegate;
            o.SetEnable (true);
            o.transform.Rotate (-90.0f, 0.0f, 0.0f);
            float r = Random.Range (-5.0f, 5.0f);
            o.transform.position = new Vector3 (0f, r, 0f);
            o.transform.localScale = new Vector3 (0.5f, 0.5f, 1.0f);
        }
    }

    // When a remote user is offline, this delegate will be called. Typically
    // delete the GameObject for this user
    private void onUserOffline(uint uid, USER_OFFLINE_REASON reason)
    {
        // remove the video stream
        Debug.Log (“onUserOffline: uid = “ + uid);
        // this is called in the main thread
        GameObject go = GameObject.Find (uid.ToString());
        if (!ReferenceEquals (go, null)) {
            Destroy (go);
        }
    }

    // Delegate: Adjust the transform for the game object ‘objName’ connected with the user ‘uid’
    // You can save information for ‘uid’ (e.g. which GameObject is attached)
    private void onTransformDelegate (uint uid, string objName, ref Transform transform)
    {
        if (uid == 0) {
            transform.position = new Vector3 (0f, 2f, 0f);
            transform.localScale = new Vector3 (2.0f, 2.0f, 1.0f);
            transform.Rotate (0f, 1f, 0f);
        } else {
            transform.Rotate (0.0f, 1.0f, 0.0f);
        }
    }
}
```



## Step 7: Set the GameObject Script File

1.  Click **Join** and select `example.cs`.

2.  Sphere: Select `VideoSurface.cs`.

3.  Connect your Android devices.

## Step 8: Compile and Install

1.  Select **File\> Build Settings…**, and the **Build Settings** dialog box pops up.

2.  Open playscene and click **Add Open Scenes** to load the playscene to the build.

3.  Select the platform as **Android** and set the **Build System** as **Internal**:

    ![](https://web-cdn.agora.io/docs-files/1548830012546)


4.  Click **Player Settings…**, and open the **Other Settings**:

    -   **Other Settings**/**Rendering**/**Auto Graphics API**: **False**
    -   Delete **OpenGLES3** and **Vulkan**
    -   Ensure that you keep **OpenGLES2** which applies for video functions
    -   Ensure that **Multithreaded Rendering** is set to **False**

5.  Click **Save** to save the settings.

6.  Click **Build** to build the app.


## Step 9: Run the Application

To demonstrate the video for gaming functions, you will need two or more Android devices.

Click **Join** on both devices to join a channel.

<img alt="../_images/AMG-Video-Unity3D_20.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_20.png" style="width: 840.0px;"/>


You can now use video for gaming. If there is no video or voice, check the following:

-   If the App ID is set correctly.
-   If the network is in good condition.
-   If the permissions of network and camera are authorized.



