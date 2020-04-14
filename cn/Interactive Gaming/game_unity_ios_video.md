
---
title: 实现游戏视频功能
description: 
platform: Unity_(iOS)
updatedAt: Mon Apr 13 2020 15:28:36 GMT+0800 (CST)
---
# 实现游戏视频功能
<div class="alert note">互动游戏 Unity SDK 将不再维护，请使用 RTC Unity SDK 替代。详情见<a href="https://docs.agora.io/cn/Video/start_call_unity?platform=Unity">实现视频通话</a >。</div>

## 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads) 最新的 Unity 视频软件包。软件包结构如下:

![](https://web-cdn.agora.io/docs-files/1548831707751)

2. 请确保已满足以下环境要求:
- Unity 5.5 或更高版本
- Xcode 8.0 或更高版本
- 两部或多部支持音频功能的 iOS 真机(9.0 或更高版本)

3. [获取 App ID](../../cn/Interactive%20Gaming/token.md)

4. 请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。

## 步骤 2: 创建项目

1.  在 Unity 中新建一个项目，选择 **3D**。

    <img alt="../_images/AMG-Video-Unity3D_23.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_23.png" />

2.  在默认场景中添加 **Game Object**: **Sphere** 和一个 **Button**。

    <img alt="../_images/AMG-Video-Unity3D_24.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_24.png" />

3.  把该场景保存于 `Assets/playscene.unity`。

更多 Unity 使用相关问题，请参考 [Unity 官方文档](https://docs.unity3d.com/2018.2/Documentation/Manual)。

## 步骤 3：添加 SDK

1. 在你的项目的根目录下，创建 `Assets/Scripts` 文件夹。

2. 把示例代码中的 `Assets/Scripts/AgoraGamingSDK` 复制到你的项目中的 `Assets/Scripts` 文件夹下。

3. 添加 `.framework` 文件：

- 把示例代码中的 `Assets/Plugins/iOS/AgoraRtcEngineKit.framework` 文件复制到你的项目中的 `Assets/Plugins/iOS` 文件夹下。
- 把示例代码中的 `Assets/Plugins/iOS/libagoraSdkCWrapper.a` 文件复制到你的项目中的 `Assets/Plugins/iOS` 文件夹下。
- 添加如下图所示系统文件：`MediaToolBox.framwork`，`VideoToolbox.framework`，`CoreTelephony.framework` 和 `libresolv.tbd`。

	<img alt="../_images/AMG-Video-Unity3D_22.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_22.png"/>

## 步骤 4：调用 API

参考 [互动游戏 API](../../en/Interactive%20Gaming/game_unity.md) 或以下实例代码，实现基本 API 的调用。

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

## 步骤 5：设置 GameObject Script 文件

1. 点击 **Join**，选择 `example.cs`。

2.  **Sphere** 选择 `VideoSurface.cs`。

3.  将你的 iOS 设备连接到电脑。


## 步骤 6: 编译与安装

1.  选择 **File/Build Settings…**，出现 **Build Settings** 弹窗。

2.  选择 **iOS** 平台。

3.  点击 **Player Settings…**，打开 **Other Settings** 面板：

    -   **Other Settings/Rendering/Auto Graphics API**，设置为 **False**。
    -   删除 **Metal**
    -   请确认保留了 **OpenGLES2**，使用视频功能需要使用
    -   请确认 **Multithread Rendering** 为 **False**
    
    ![](https://web-cdn.agora.io/docs-files/1553767152433)

4.  点击 **Save** 保存设置。

5.  点击 **Build And Run**，在对话框中选择导出到某个文件夹（需要对新文件夹进行命名），例如：

     <img alt="../_images/AMG-Video-Unity3D_14.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_14.png" />
		 
6. 在 **Build And Run** 后生成的 XCode Project 中，确保进行如下权限配置：

    -   **Privacy**：`Camera and Microphone`


## 步骤 7：运行程序

演示游戏视频至少需要两部或多部 iOS 真机。在两台设备上点击 **Join** 加入频道。

<img alt="../_images/AMG-Video-Unity3D_20.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_20.png" style="width: 840.0px;"/>

现在你可以在游戏中使用视频功能了。

如果没有声音或影像，请检查以下事项：

- App ID 是否设置正确。
- 网络连接是否正常。
- 是否开启网络与摄像头权限。





