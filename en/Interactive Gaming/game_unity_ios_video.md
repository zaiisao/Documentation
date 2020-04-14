
---
title: Implement Video for Gaming
description: 
platform: Unity_(iOS)
updatedAt: Mon Apr 13 2020 15:40:23 GMT+0800 (CST)
---
# Implement Video for Gaming
<div class="alert note">Agora no longer maintains the Agora Unity SDK for Interactive Gaming, use the Agora RTC Unity SDK instead. See more details in <a href="https://docs.agora.io/en/Video/start_call_unity?platform=Unity">Start a Video Call</a >.</div>

## Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the latest Unity video kit. See the following structure:

![](https://web-cdn.agora.io/docs-files/1548831707751)

2.  Hardware and software requirements:

    -   Unity 5.5 or later

    -   Xcode 8.0 or later

    -   Two or more iOS 9.0 or later devices with video and audio functions

3.  [Getting an App ID](../../en/Interactive%20Gaming/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


## Step 2: Create a Project

1.  Open Unity to create a new project. Select **3D**.

    <img alt="../_images/AMG-Video-Unity3D_23.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_23.png" />

2.  Add **Game Object**: **Sphere** and one **Button** in the default scene.

    <img alt="../_images/AMG-Video-Unity3D_24.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_24.png" />

3.  Save the scene to `Assets/playscene.unity`.

For information on how to use the Unity, refer to the [official Unity documentation](https://docs.unity3d.com/2018.2/Documentation/Manual).

## Step 3: Add the SDK

1. Open the root directory of your project and create a `Assets/Scripts` folder.

2. Copy the `Assets/Scripts/AgoraGamingSDK` folder from the sample to `Assets/Scripts` of your project.

3. Add `.framework` files:

- Copy `Assets/Plugins/iOS/AgoraRtcEngineKit.framework` from the sample to `Assets/Plugins/iOS` of your project.

- Copy `Assets/Plugins/iOS/libagoraSdkCWrapper.a` from the sample to `Assets/Plugins/iOS` of your project.

- Add the necessary system libraries: `MediaToolBox.framwork`, `VideoToolbox.framework`, `CoreTelephony.framework` and `libresolv.tbd`.

<img alt="../_images/AMG-Video-Unity3D_22.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_22.png"/>
 

## Step 4: Call the APIs

Follow [Interactive Gaming API](../../en/Interactive%20Gaming/game_unity.md) to call the APIs to implement the required functions. The following figure shows how to create a C\# script `example.cs`:

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

## Step 5: Set the GameObject Script File

1.  Click **Join** and select `example.cs`.

2.  Sphere: Select `VideoSurface.cs`.

3.  Connect your iOS devices.


## Step 6: Compile and Install

1.  Select **File/Build Settings…**, and the **Build Settings** dialog box pops up.

2.  Select the platform as **iOS**.

3.  Click **Player Settings…** and open the **Other Settings**:

    -   **Other Settings**/**Rendering**/**Auto Graphics API**: **False**
    -   Delete **Metal**
    -   Ensure that you keep **OpenGLES2** which applies for the video function
    -   Ensure that **Multithreaded Rendering** is set to **False**
    
		![](https://web-cdn.agora.io/docs-files/1553767348175)

4.  Click **Save** to save the settings.

5.  Click **Build And Run**, and a dialog box pops up to name the targeted folder of the exported projects, for example:

       <img alt="../_images/AMG-Video-Unity3D_14.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_14.png" />

6. In the generated XCode project after clicking **Build And Run**，ensure that you have configured the following permission:

	  -   **Privacy**: `Camera and Microphone`


## Step 7: Run the Application

To demonstrate video for gaming, you need two or more iOS devices. Click **Join** on both devices to join a channel.

<img alt="../_images/AMG-Video-Unity3D_20.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_20.png" style="width: 840.0px;"/>

You can now use video for gaming. If there is no video or voice, check the following:

-   If the App ID is set correctly.

-   If the network is in good condition.

-   If the permissions of network and camera are authorized.



