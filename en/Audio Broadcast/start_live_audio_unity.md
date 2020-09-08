
---
title: Start Live Interactive Audio Streaming
description: 
platform: Unity
updatedAt: Wed Jul 29 2020 07:29:38 GMT+0800 (CST)
---
# Start Live Interactive Audio Streaming
Use this guide to quickly start the live interactive audio streaming with the Agora Voice SDK for Unity.

## Prerequisites

- Unity 2017 or later

- Operating system and IDE requirements:

  | Target Platform | Operating system version | IDE version                 |
  | :-------------- | :----------------------- | :-------------------------- |
  | Android         | Android 4.1 or later     | Android Studio 2.0 or later |
  | iOS             | iOS 8.0 or later         | Xcode 8.0 or later          |
  | macOS           | macOS 10.0 or later      | Xcode 8.0 or later          |
  | Windows         | Windows XP SP3 or later  | Visual Studio 2013 or later |

- A valid [Agora account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up) and an [App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-an-app-id)

<div class="alert note">Open the specified ports in <a href="https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms#agora-rtc-sdk">Firewall Requirements</a > if your network has a firewall.</div>

## Set up the development environment

In this section, we create a Unity project and integrate the SDK into the project.

### Create a Unity project

Use the following steps or follow the[ Unity official manual](https://docs.unity3d.com/Manual/GettingStarted.html) to build a project from scratch. Skip to [Integrate the SDK](#Integrate) if you have already created a Unity project.

<details>
	<summary><font color="#3ab7f8">Create a Unity Project</font></summary>
	
1. Ensure that you have downloaded and installed **Unity** before the following steps. If not, click <a href="https://store.unity.com/?_ga=2.210473373.969150697.1571977051-1808388573.1570601452#plans-individual">here</a > to download.
	
2. Open **Unity** and click **New**.
	
3. Fill in and select the options in the following fields, and click **Create project**.
	 - Project name
	 - Location
	 - Organization
	 - Template: Choose **3D**
</details>

<a name="Integrate"></a>
### Integrate the SDK

Choose either of the following approaches to integrate the Agora Unity SDK into your project.

**Approach 1: Automatically integrate the SDK with Unity Asset Store**

1. Open **Asset Store** in **Unity**, search for **Agora Voice SDK for Unity** and click it.
   ![](https://web-cdn.agora.io/docs-files/1576206914656)
2. Click **Download** to download the SDK.
   ![](https://web-cdn.agora.io/docs-files/1576223710590)
3. When the download completes, the button becomes **Import**. Click **Import** to show the packages of the downloaded SDK.
   ![](https://web-cdn.agora.io/docs-files/1576223720730)
4. All packages of the SDK are chosen by default. Uncheck the packages you do not need and click **Import**.
   ![](https://web-cdn.agora.io/docs-files/1576223735922)

**Approach 2: Manually add the SDK files**

1. Go to [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads), download the Agora SDK for Unity under **Voice SDK**, and unzip the downloaded SDK package.
2. Create a **Plugins** folder under the **Assets** folder in your project.
3. Copy the following file or subfolder from the **libs** folder of the downloaded SDK to the corresponding directory of your project.

  | Target Platform | File or subfolder                 | Directory of your project |
  | :-------------- | :-------------------------------- | :------------------------ |
  | Android         | /Android/AgoraRtcEngineKit.plugin | /Assets/Plugins/Android   |
  | iOS             | /iOS/AgoraAudioKit.framework  | /Assets/Plugins/iOS       |
  | macOS           | /macOS/agoraSdkCWrapper.bundle    | /Assets/Plugins/macOS     |
  | Windows         | /x86                              | /Assets/Plugins/x86       |
  | Windows  | /x86_64  | /Assets/Plugins/x86_64            |

   <div class="alert note"><ul><li>Android or iOS developers using Unity Editor for macOS or Windows must also copy the SDK file for macOS or the subfolder for Windows X86_64 to the specified directory.<li>Android developers also need to copy the AndroidManifest.xml and project.properties  from the directory of the sample project (./Assets/Plugins/Android/AgoraRtcEngineKit.plugin) to the directory of the Unity project (./Assets/Plugins/Android). The former is for adding project permissions; the latter is for setting project properties.<li>iOS developers also need to do the following steps: (You can find the sample code logic in <a href="https://github.com/AgoraIO/Agora-Unity-Quickstart/blob/master/audio/Hello-Unity3D-Agora/Assets/Editor/BL_BuildPostProcess.cs">BL_BuildPostProcess.cs</a>, or copy  this file to directory of your Unity project)<ul><li>Link the following libraries: <ul><li>CoreTelephony.framework<li>libresolv.tbd<li>libiPhone-lib.a<li>CoreText.framework<li>CoreML.framework<li>Accelerate.framework</li></ul><li>Ask for the following permissios: NSMicrophoneUsageDescription</div>

## Implement a basic live interactive streaming

This section provides instructions on using the Agora Voice SDK for Unity to implement the live interactive streaming, as well as an API call sequence diagram.

![](https://web-cdn.agora.io/docs-files/1576224606319)

### 1. Create the UI

Create the user interface (UI) for the live interactive audio streaming in your project. If you already have one UI in your project, skip to [Get the device permission (Android only)](#permission) or [Initialize IRtcEngine](#initialize).

We recommend adding the following elements to the UI:

- A switch role button
- An exit button

<a name="permission"></a>
### 2. Get the device permission (Android only)

If you build for Android, you should add in APIs to check and request the device permission. For all other platforms, this is handled by the engine, and you can skip to [Initialize IRtcEngine](#initialize).

Unity versions later than **UNITY_2018_3_OR_NEWER** do not automatically request microphone permissions from your Android device. If you are using one of these versions, call the `CheckPermission` method to request access to the microphone of your Android device.

```C#
#if(UNITY_2018_3_OR_NEWER) 
using UnityEngine.Android; 
#endif 
void Start () 
{  
#if(UNITY_2018_3_OR_NEWER) 
permissionList.Add(Permission.Microphone);  
#endif  
} 
private void CheckPermission()
{ 
#if(UNITY_2018_3_OR_NEWER) 
foreach(string permission in permissionList) 
{ 
if (Permission.HasUserAuthorizedPermission(permission)) 
{ 
} 
else 
{  
Permission.RequestUserPermission(permission); 
} 
} 
#endif 
} 
void Update () 
{  
#if(UNITY_2018_3_OR_NEWER) 
// Ask for your Android device's permissions.
CheckPermission(); 
#endif  
}
```

<a name="initialize"></a>
### 3. Initialize IRtcEngine

Initialize the `IRtcEngine` object before calling any other Agora APIs.

Call the `GetEngine` method and pass in the App ID to initialize the IRtcEngine object.

<div class="alert note">If you want to exit the app or release the memory of <tt>IRtcEngine</tt>, call <tt>Destroy</tt> to destroy the <tt>IRtcEngine</tt> object. See details in <a href="#destroy">Destroy the IRtcEngine object</a >.</div>

Listen for callback events, such as when the local user joins the channel, and when the first audio frame of a remote user is decoded.

```C#
// Pass an App ID to create and initialize an IRtcEngine object.
mRtcEngine = IRtcEngine.GetEngine (appId); 
// Listen for the OnJoinChannelSuccessHandler callback.
// This callback occurs when the local user successfully joins the channel.
mRtcEngine.OnJoinChannelSuccessHandler = OnJoinChannelSuccessHandler; 
// Listen for the OnUserJoinedHandler callback.
// This callback occurs when the first audio frame of a remote user is received and decoded after the remote user successfully joins the channel.
mRtcEngine.OnUserJoinedHandler = OnUserJoinedHandler; 
// Listen for the OnUserOfflineHandler callback.
// This callback occurs when the remote user leaves the channel or drops offline.
mRtcEngine.OnUserOfflineHandler = OnUserOfflineHandler;
```


### 4. Set the channnel profile

After initializing the `IRtcEngine` object, call the `SetChannelProfile` method to set the channel profile as `LIVE_BROADCASTING`.

One `IRtcEngine` object uses one profile only. If you want to switch to another profile, destroy the current `IRtcEngine` object with the `Destroy` method and create a new one before calling the `SetChannelProfile` method.

```C#
// Set the channel profile as LIVE_BROADCASTING.
mRtcEngine.SetChannelProfile(CHANNEL_PROFILE.CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### 5. Set the client role

An interactive streaming channel has two client roles: `BROADCASTER` and `AUDIENCE`, and the default role is `AUDIENCE`. After setting the channel profile to `LIVE_BROADCASTING`, your app may use the following steps to set the client role:

1. Allow the user to set the role as BROADCASTER or AUDIENCE.
2. Call the `SetClientRole` method and pass in the client role set by the user.

Note that in an live interactive streaming, only the host can be heard. If you want to switch the client role after joining the channel, call the `SetClientRole` method.

```C#
// Set the client role as the host.
mRtcEngine.SetClientRole(CLIENT_ROLE.BROADCASTER);
```

### 6. Join a channel

After setting the client role, you can call `JoinChannelByKey` to join a channel. Set the following parameters when calling this method::

- `channelKey`: The token for identifying the role and privileges of a user. Set it as one of the following values:

  - `NULL`.
  - A temporary token generated in Agora Console. A temporary token is valid for 24 hours. For details, see [Get a temporary token](https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token).
  - A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](https://docs.agora.io/en/Video/token_server_cpp).

  <div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>

- ·`channelName`: The unique name of the channel to join. Users that input the same channel name join the same channel.

- `uid`: Integer. The unique ID of the local user. If you set `uid` as `0`, the SDK automatically assigns one user ID and returns it in the `OnJoinChannelSuccessHandler` callback.

If an interactive streaming channel uses both the RTC Native SDK and the RTC Web SDK, ensure that you call the `EnableWebSdkInteroperability` method before joining the channel.

```C#
// Call this method to enable interoperability between the RTC Native SDK and the RTC Web SDK if the RTC Web SDK is in the channel.
mRtcEngine.EnableWebSdkInteroperability(true);
// Join a channel. 
mRtcEngine.JoinChannelByKey(null, channel, null, 0);
```

### 7. Leave the channel

According to your scenario, such as when the live interactive streaming ends and when you need to close the app, call `LeaveChannel` to leave the current channel.

```C#
public void leave()
	{
		Debug.Log ("calling leave");
		if (mRtcEngine == null)
			return;
		// Leave the channel.
		mRtcEngine.LeaveChannel();
	}
```

<a name="destroy"></a>
### 8. Destroy the IRtcEngine object

After leaving the channel, if you want to exit the app or release the memory of `IRtcEngine`, call `Destroy` to destroy the `IRtcEngine` object.

```C#
void OnApplicationQuit()
{
	if (mRtcEngine != null)
	{
	// Destroy the IRtcEngine object.
	IRtcEngine.Destroy();
    mRtcEngine = null;
	}
}
```

## Run the project

Run the project in Unity. When you set the role as the host and successfully start the live interactive audio streaming, you can hear the voice of yourself in the app. When you set the role as the audience and successfully join the live interactive audio streaming, you can hear the voice of the host in the app.

## Reference

[How can I listen for an audience joining or leaving a live interactive streaming channel?](https://docs.agora.io/en/faq/audience_event)
