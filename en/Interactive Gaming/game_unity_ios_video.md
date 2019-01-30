
---
title: Implement Video for Gaming
description: 
platform: Unity_(iOS)
updatedAt: Wed Jan 30 2019 13:03:48 GMT+0000 (UTC)
---
# Implement Video for Gaming
## Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the latest Unity video kit. See the following structure:

![](https://web-cdn.agora.io/docs-files/1548831707751)

2.  Hardware and software requirements:

    -   Unity 5.5 or later

    -   Xcode 8.0 or later

    -   Two or more iOS 9.0 or later devices with video and audio functions

3.  [Getting an App ID](../../en/Agora%20Platform/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


## Step 2: Create a Project

1.  Open Unity to create a new project. Select **3D**.

    <img alt="../_images/AMG-Video-Unity3D_23.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_23.png" />

2.  Add **Game Object**: **Sphere** and one **Button** in the default scene.

    <img alt="../_images/AMG-Video-Unity3D_24.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_24.png" />

3.  Save the scene to `Assets/playscene.unity`.

For information on how to use the Unity, refer to the official Unity documentation.

## Step 3: Add the SDK

1. Open the root directory of your project and create a `Assets/Scripts` folder.

2. Copy the `Assets/Scripts/AgoraGamingSDK` folder from the sample to `Assets/Scripts` of your project.

3. Add `.framework` files:

- Copy `Assets/Plugins/iOS/AgoraRtcEngineKit.framework` from the sample to `Assets/Plugins/iOS` of your project.

- Copy `Assets/Plugins/iOS/libagoraSdkCWrapper.a` from the sample to `Assets/Plugins/iOS` of your project.

- Add the necessary system libraries: `MediaToolBox.framwork`, `VideoToolbox.framework`, `CoreTelephony.framework` and `libresolv.tbd`.

<img alt="../_images/AMG-Video-Unity3D_22.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_22.png"/>
 

## Step 4: Call the APIs

Follow [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to call the APIs to implement the required functions. The following figure shows how to create a C\# script `example.cs`:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using agora_gaming_rtc;
using UnityEngine.UI;

// this is an example of using Agora Unity SDK
// It demonstrates:
// How to enable video
// How to join/leave channel
// 
public class HelloUnityVideo : MonoBehaviour {

	// PLEASE KEEP THIS App ID IN SAFE PLACE
	// Get your own App ID at https://dashboard.agora.io/
	// After you entered the App ID, remove ## outside of Your App ID
	private static string appId = #YOUR APP ID#;

	// load agora engine
	public void loadEngine()
	{
		// start sdk
		Debug.Log ("initializeEngine");

		if (mRtcEngine != null) {
			Debug.Log ("Engine exists. Please unload it first!");
			return;
		}

		// init engine
		mRtcEngine = IRtcEngine.getEngine (appId);

		// enable log
		mRtcEngine.SetLogFilter (LOG_FILTER.DEBUG | LOG_FILTER.INFO | LOG_FILTER.WARNING | LOG_FILTER.ERROR | LOG_FILTER.CRITICAL);
	}

	public void join(string channel)
	{
		Debug.Log ("calling join (channel = " + channel + ")");

		if (mRtcEngine == null)
			return;

		// set callbacks (optional)
		mRtcEngine.OnJoinChannelSuccess = onJoinChannelSuccess;
		mRtcEngine.OnUserJoined = onUserJoined;
		mRtcEngine.OnUserOffline = onUserOffline;

		// enable video
		mRtcEngine.EnableVideo();

		// allow camera output callback
		mRtcEngine.EnableVideoObserver();

		// join channel
		mRtcEngine.JoinChannel(channel, null, 0);

		Debug.Log ("initializeEngine done");
	}

	public string getSdkVersion () {
		return IRtcEngine.GetSdkVersion ();
	}

	public void leave()
	{
		Debug.Log ("calling leave");

		if (mRtcEngine == null)
			return;

		// leave channel
		mRtcEngine.LeaveChannel();
		// deregister video frame observers in native-c code
		mRtcEngine.DisableVideoObserver();
	}

	// unload agora engine
	public void unloadEngine()
	{
		Debug.Log ("calling unloadEngine");

		// delete
		if (mRtcEngine != null) {
			IRtcEngine.Destroy ();
			mRtcEngine = null;
		}
	}

	// accessing GameObject in Scnene1
	// set video transform delegate for statically created GameObject
	public void onSceneHelloVideoLoaded()
	{
		GameObject go = GameObject.Find ("Cylinder");
		if (ReferenceEquals (go, null)) {
			Debug.Log ("BBBB: failed to find Cylinder");
			return;
		}
		VideoSurface o = go.GetComponent<VideoSurface> ();
		o.mAdjustTransfrom += onTransformDelegate;
	}

	// instance of agora engine
	public IRtcEngine mRtcEngine;

	// implement engine callbacks

	public uint mRemotePeer = 0; // insignificant. only record one peer

	private void onJoinChannelSuccess (string channelName, uint uid, int elapsed)
	{
		Debug.Log ("JoinChannelSuccessHandler: uid = " + uid);
		GameObject textVersionGameObject = GameObject.Find ("VersionText");
		textVersionGameObject.GetComponent<Text> ().text = "Version : " + getSdkVersion ();
	}

	// When a remote user joined, this delegate will be called. Typically
	// create a GameObject to render video on it
	private void onUserJoined(uint uid, int elapsed)
	{
		Debug.Log ("onUserJoined: uid = " + uid);
		// this is called in main thread

		// find a game object to render video stream from 'uid'
		GameObject go = GameObject.Find (uid.ToString ());
		if (!ReferenceEquals (go, null)) {
			return; // reuse
		}

		// create a GameObject and assigne to this new user
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

		mRemotePeer = uid;
	}

	// When remote user is offline, this delegate will be called. Typically
	// delete the GameObject for this user
	private void onUserOffline(uint uid, USER_OFFLINE_REASON reason)
	{
		// remove video stream
		Debug.Log ("onUserOffline: uid = " + uid);
		// this is called in main thread
		GameObject go = GameObject.Find (uid.ToString());
		if (!ReferenceEquals (go, null)) {
			Destroy (go);
		}
	}

	// delegate: adjust transfrom for game object 'objName' connected with user 'uid'
	// you could save information for 'uid' (e.g. which GameObject is attached)
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

2.  Sphere: Select `videoSurface.cs`.

3.  Connect your iOS devices.


## Step 6: Compile and Install

1.  Select **File/Build Settings…**, and the **Build Settings** dialog box pops up.

2.  Select the platform as **iOS**.

3.  Click **Player Settings…** and open the PlayerSettings panel:

    -   Other Settings/Rendering/Auto Graphics API: False

    -   Delete Metal

    -   Keep OpenGLES2

    -   Privacy: Camera and Microphone

    <img alt="../_images/AMG-Video-Unity3D_21.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_21.png" style="width: 840.0px;"/>

4.  Click **save** to save the settings.

5.  Click **Build And Run**, and a dialog box pops up to name the targeted folder of the exported projects, for example:

       <img alt="../_images/AMG-Video-Unity3D_14.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_14.png" />


## Step 7: Run the Application

To demonstrate video for gaming, you need two or more iOS devices.

Run **RollingVideo** on both devices and click **Join**.

<img alt="../_images/AMG-Video-Unity3D_20.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_20.png" style="width: 840.0px;"/>

You can now use video for gaming. If there is no video or voice, check the following:

-   If the App ID is set correctly.

-   If the network is in good condition.

-   If the permissions of network and camera are authorized.



