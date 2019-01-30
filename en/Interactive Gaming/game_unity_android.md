
---
title: Implement Voice for Gaming
description: 
platform: Unity
updatedAt: Wed Jan 30 2019 08:11:44 GMT+0000 (UTC)
---
# Implement Voice for Gaming
With the `Hello-Unity-Agora` Sample App provided by Agora, you can:

- Create and join a channel
- Talk in the channel
- Leave the channel

This page shows how to use the Unity3D SDK to implement these functions on the Android and iOS platforms.

## Implementations on Android
### Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Unity** Voice. See the following structure:

	![](https://web-cdn.agora.io/docs-files/1548830935872)

-   **include**: header files. Typically not used in Unity3D projects except for modifying raw data.

-   **libs**: library files. Only the `.jar` and `.so` files and the C\# API library files encapsulated in the **Scripts** folder are used on this page.

-   **samples**: sample code

2.  Hardware and software requirements:

    -   Unity3D 5.5 or later
    -   Android Studio 2.0 or later
    -   Two or more Android 4.0 or later devices with audio support

3.  [Getting an App ID](../../en/Agora%20Platform/token.md).

4.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a Unity3D project. Refer to [here](https://docs.unity3d.com/2018.2/Documentation/Manual/GettingStarted.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Open the root directory of the project and create:

    -  `Assets/Plugins/Android/AgoraAudioKit.plugin/libs`
    -  `Assets/Scripts`

2.  Copy the `libs/Scripts/AgoraGamingSDK` from the downloaded SDK to the `Assets/Scripts` directory of your project.

3.  Copy the `libs/Android/agora-rtc-sdk.jar` file in the SDK to `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` path of your project.

4.  Copy the `Assets/Plugins/Android/mainTemplate.gradle` file from the Sample in the SDK to the `Assets/Plugins/Android` path of your project.

5.  Copy the `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` from the Sample in the SDK to the`Assets/Plugins/Android/AgoraAudioKit.plugin` directory of your project.

6.  Copy the **armeabi-v7a** and **x86** files in **libs/Android** of the SDK to the `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` path your project.


### Step 4: Add Permissions

Add the following permissions to the `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` file:

```C#
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.READ_LOGS" />
```

### Step 5: Prevent Code Obfuscation

Add the following line in the `Assets/Plugins/Android/AgoraAudioKit.plugin/project.properties` to obfuscate the code:

```
-keep class io.agora.**{*;}
```

### Step 6: Call the APIs

Call the APIs in [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to implement the required functions. Voice for gaming includes two modes:

-   Free talk mode
-   Command mode

You can choose which mode to use by calling `setChannelProfile`.

## Implementations on iOS

### Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the Game SDK for **Unity** Voice. See the following structure:

![](https://web-cdn.agora.io/docs-files/1548829843264)

-   **include**: header files. Typically not used in Unity3D projects except for when modifying the raw data.

-   **libs**: library files. Only the framework files and C\# API library files encapsulated in the in the **Scripts** folder are used in this document.

-   **samples**: sample code

1.  Hardware and software requirements:

    -   Unity3D 5.5 or later

    -   Xcode 8.0 or later

    -   Two or more iOS 9.0 or later devices with audio support

2.  [Getting an App ID](../../en/Agora%20Platform/token.md).

3.  Before accessing Agora’s services, make sure that you have opened the ports and whitelisted the domains as specified in [Firewall Requirements](../../en/Agora%20Platform/firewall.md).


### Step 2: Create a New Project

Create a Unity3D project. Refer to [here](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/Setup/Setup.html) if necessary. Skip this step if a project already exists.

### Step 3: Add the SDK

1.  Open the root directory of your project and create a `Assets/Scripts` folder.

2.  Copy the `Assets/Scripts/AgoraGamingSDK` folder from the sample to `Assets/Scripts` of your project.

3.  Add `.framework` files:

    -   Copy `Assets/Plugins/iOS/AgoraAudioKit.framework` from the sample to `Assets/Plugins/iOS` of your project.

    -   Copy `Assets/Plugins/iOS/AgoraGamingRtcSDKWrapper.a` from the sample to `Assets/Plugins/iOS` of your project.

    -   Add the necessary system libraries: `AgoraAudioKit.framework`, `AudioToolBox.framework`, `CoreTelephony.framework`, `AVFoundation.framework` and `libresolv.tbd`.

    ![](https://web-cdn.agora.io/docs-files/1543568568723)


### Step 4: Add Permissions

Add any necessary permission, such as access permission to the microphone:

![](https://web-cdn.agora.io/docs-files/1543568604885)


### Step 5: Call the APIs

Use the following sample code or refer to [Interactive Gaming API](../../en/API%20Reference/game_unity.md) to implement the required functions. 

```
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

using agora_gaming_rtc;

public class HelloUnity3D : MonoBehaviour
{
	public InputField mChannelNameInputField;
	public Text mShownMessage;
	public Text versionText;
	public Button joinChannel;
	public Button leaveChannel;
	private IRtcEngine mRtcEngine = null;

	// PLEASE KEEP THIS App ID IN SAFE PLACE
	// Get your own App ID at https://dashboard.agora.io/
	// After you entered the App ID, remove ## outside of Your App ID
	private string appId = #YOUR APP ID#;

	void Awake ()
	{
		QualitySettings.vSyncCount = 0;
		Application.targetFrameRate = 30;
	}

	// Use this for initialization
	void Start ()
	{
			joinChannel.onClick.AddListener (JoinChannel);	
			leaveChannel.onClick.AddListener (LeaveChannel);

			mRtcEngine = IRtcEngine.GetEngine (appId);
			versionText.GetComponent<Text> ().text ="Version : " + IRtcEngine.GetSdkVersion ();

			mRtcEngine.OnJoinChannelSuccess += (string channelName, uint uid, int elapsed) => {
				string joinSuccessMessage = string.Format ("joinChannel callback uid: {0}, channel: {1}, version: {2}", uid, channelName, IRtcEngine.GetSdkVersion ());
				Debug.Log (joinSuccessMessage);
				mShownMessage.GetComponent<Text> ().text = (joinSuccessMessage);
			};

			mRtcEngine.OnLeaveChannel += (RtcStats stats) => {
				string leaveChannelMessage = string.Format ("onLeaveChannel callback duration {0}, tx: {1}, rx: {2}, tx kbps: {3}, rx kbps: {4}", stats.duration, stats.txBytes, stats.rxBytes, stats.txKBitRate, stats.rxKBitRate);
				Debug.Log (leaveChannelMessage);
				mShownMessage.GetComponent<Text> ().text = (leaveChannelMessage);
			};

			mRtcEngine.OnUserJoined += (uint uid, int elapsed) => {
				string userJoinedMessage = string.Format ("onUserJoined callback uid {0} {1}", uid, elapsed);
				Debug.Log (userJoinedMessage);
			};

			mRtcEngine.OnUserOffline += (uint uid, USER_OFFLINE_REASON reason) => {
				string userOfflineMessage = string.Format ("onUserOffline callback uid {0} {1}", uid, reason);
				Debug.Log (userOfflineMessage);
			};

			mRtcEngine.OnVolumeIndication += (AudioVolumeInfo[] speakers, int speakerNumber, int totalVolume) => {
				if (speakerNumber == 0 || speakers == null) {
					Debug.Log (string.Format("onVolumeIndication only local {0}", totalVolume));
				}

				for (int idx = 0; idx < speakerNumber; idx++) {
					string volumeIndicationMessage = string.Format ("{0} onVolumeIndication {1} {2}", speakerNumber, speakers[idx].uid, speakers[idx].volume);
					Debug.Log (volumeIndicationMessage);
				}
			};

			mRtcEngine.OnUserMuted += (uint uid, bool muted) => {
				string userMutedMessage = string.Format ("onUserMuted callback uid {0} {1}", uid, muted);
				Debug.Log (userMutedMessage);
			};

			mRtcEngine.OnWarning += (int warn, string msg) => {
				string description = IRtcEngine.GetErrorDescription(warn);
				string warningMessage = string.Format ("onWarning callback {0} {1} {2}", warn, msg, description);
				Debug.Log (warningMessage);
			};

			mRtcEngine.OnError += (int error, string msg) => {
				string description = IRtcEngine.GetErrorDescription(error);
				string errorMessage = string.Format ("onError callback {0} {1} {2}", error, msg, description);
				Debug.Log (errorMessage);
			};

			mRtcEngine.OnRtcStats += (RtcStats stats) => {
				string rtcStatsMessage = string.Format ("onRtcStats callback duration {0}, tx: {1}, rx: {2}, tx kbps: {3}, rx kbps: {4}, tx(a) kbps: {5}, rx(a) kbps: {6} users {7}",
					stats.duration, stats.txBytes, stats.rxBytes, stats.txKBitRate, stats.rxKBitRate, stats.txAudioKBitRate, stats.rxAudioKBitRate, stats.users);
				Debug.Log (rtcStatsMessage);

				int lengthOfMixingFile = mRtcEngine.GetAudioMixingDuration();
				int currentTs = mRtcEngine.GetAudioMixingCurrentPosition();

				string mixingMessage = string.Format ("Mixing File Meta {0}, {1}", lengthOfMixingFile, currentTs);
				Debug.Log (mixingMessage);
			};

			mRtcEngine.OnAudioRouteChanged += (AUDIO_ROUTE route) => {
				string routeMessage = string.Format ("onAudioRouteChanged {0}", route);
				Debug.Log (routeMessage);
			};

			mRtcEngine.OnRequestToken += () => {
				string requestKeyMessage = string.Format ("OnRequestToken");
				Debug.Log (requestKeyMessage);
			};

			mRtcEngine.OnConnectionInterrupted += () => {
				string interruptedMessage = string.Format ("OnConnectionInterrupted");
				Debug.Log (interruptedMessage);
			};

			mRtcEngine.OnConnectionLost += () => {
				string lostMessage = string.Format ("OnConnectionLost");
				Debug.Log (lostMessage);
			};

			mRtcEngine.SetLogFilter (LOG_FILTER.INFO);

			// mRtcEngine.setLogFile("path_to_file_unity.log");

			mRtcEngine.SetChannelProfile (CHANNEL_PROFILE.GAME_FREE_MODE);

			// mRtcEngine.SetChannelProfile (CHANNEL_PROFILE.GAME_COMMAND_MODE);
			// mRtcEngine.SetClientRole (CLIENT_ROLE.BROADCASTER);
	}

	// Update is called once per frame
	void Update ()
	{
		
	}

	public void JoinChannel ()
	{
		string channelName = mChannelNameInputField.text.Trim ();

		Debug.Log (string.Format ("tap joinChannel with channel name {0}", channelName));

		if (string.IsNullOrEmpty (channelName)) {
			return;
		}

		mRtcEngine.JoinChannel (channelName, "extra", 0);
		// mRtcEngine.JoinChannelByKey ("YOUR_CHANNEL_KEY", channelName, "extra", 9527);
	}

	public void LeaveChannel ()
	{
		// int duration = mRtcEngine.GetAudioMixingDuration ();
		// int current_duration = mRtcEngine.GetAudioMixingCurrentPosition ();

		// IAudioEffectManager effect = mRtcEngine.GetAudioEffectManager();
		// effect.StopAllEffects ();

		mRtcEngine.LeaveChannel ();
	}
}

```



