
---
title: 实现游戏视频功能
description: 
platform: Unity_(Android)
updatedAt: Wed Jan 30 2019 12:45:04 GMT+0000 (UTC)
---
# 实现游戏视频功能
使用 Agora 的 `Hello-Video-Unity-Agora` 代码示例可以实现以下功能:

-   创建/加入频道

-   自由发言

-   离开频道


## 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads)最新的 Unity 视频软件包。软件包结构如下:

    ![](https://web-cdn.agora.io/docs-files/1548828163644)

> `Hello-Video-Unity-Agora` 即为本文需要使用的代码示例。

2.  请确保已满足以下环境要求:

    -   Unity 5.5 或更高版本
    -   Android Studio 2.0 或更高版本
    -   两部支持语音和视频的 Android 真机设备
    -   准备一个 App ID，详见 [获取 App ID](../../cn/Agora%20Platform/token.md)

3.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


## 步骤 2: 创建新项目

1. 打开 Unity 创建一个新的项目。选择 **3D**：

	<img alt="../_images/AMG-Video-Unity3D_23.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_23.png" />

2. 在默认的界面上添加 **Game Object: Sphere** 和一个 **Button** 按钮：

	<img alt="../_images/AMG-Video-Unity3D_24.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_24.png"/>
	
3. 将界面保存至 `Assets/playscene.unity`。

更多 Unity 使用相关问题，请参考 [Unity 官方文档](https://docs.unity3d.com/2018.2/Documentation/Manual)。


## 步骤 3: 添加 SDK

1. 在项目的根目录下，创建如下文件夹或路径：

	- `Assets/Plugins/Android/AgoraRtcEngineKit.plugin/libs`
	- `Assets/Scripts`

2. 将下载下来的 SDK 包中的 `libs/Scripts/AgoraGamingSDK` 文件夹复制到你项目的 `Assets/Scripts` 路径下
3. 将 SDK 包中的 `libs/Android/agora-rtc-sdk.jar` 文件复制到你项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/libs` 路径下
4. 将示例代码中的 `Assets/Plugins/Android/mainTemplate.gradle` 文件复制到你项目的 `Assets/Plugins/Android` 路径下
5. 将示例代码中的 `Assets/Plugins/Android/AgoraRtcEngine.plugin/AndroidManifest.xml` 文件复制到你项目的 `Assets/Plugins/Android/AgoraRtcEnginet.plugin` 路径下
6. 将 SDK 包 `libs/Android` 路径下的 **armeabi-v7a** 和 **x86** 文件夹复制到你项目的 `Assets/Plugins/Android/AgoraRtcEngine.plugin/libs` 路径下

## 步骤 4：添加权限

在你的项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/AndroidManifest.xml` 文件中，添加如下权限：

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
    <uses-permission android:name="android.permission.READ_LOGS" />
```

## 步骤 5：防止混淆代码

在你的项目的 `Assets/Plugins/Android/AgoraAudioKit.plugin/project.properties` 文件中添加如下行，防止混淆代码：

```
-keep class io.agora.**{*;}
```

## 步骤 6：调用 API 实现游戏视频

调用 [互动游戏 API](../../cn/API%20Reference/game_unity.md) 中的 API，实现你想要的功能。下图展示如何创建一个 C# `example.cs` 文件：

<img alt="../_images/AMG-Video-Unity3D_25.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_25.png" />

```
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

```

## 步骤 7：设置 GameObject 脚本文件

1. 点击 **Join**，选择 `example.cs`
2. 在 **Sphere** 中选择 `videoSource.cs`
3. 连接 Android 设备

## 步骤 8：编译及安装

1. 选择 **File > Build Settings**，会弹出一个 **Build Settings** 的弹框
2. 打开游戏界面，点击 **Add Open Scenes**，加载游戏界面
3. 将平台选择为 **Android**，并将 **Build System** 设置为 **Internal**：

	![](https://web-cdn.agora.io/docs-files/1548830012546)
	
4. 点击 **Play Settings**，打开 **PlayerSettingspanel**：

  - **Other Settings**/**Rendering**/**Auto Graphics API**：选择 **False**
  - 删除 **OpenGLES3** 和 **Vulkan**
  - 保留 **OpenGLES2**

5. 点击 **Save** 保存相关设置
6. 点击 **Build** 编译项目

## 步骤 9：运行项目

你需要两台 Android 设备来实现游戏视频功能。在两台设备上点击 **RollingVideo**，并点击 **Join**。

<img alt="../_images/AMG-Video-Unity3D_20.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_20.png" style="width: 840.0px;"/>

现在你可以使用游戏视频功能了。如果没有看到视频，请检查如下项：

- App ID 是否正确有效
- 网络状态是否良好
- 是否授权使用网络及摄像头


