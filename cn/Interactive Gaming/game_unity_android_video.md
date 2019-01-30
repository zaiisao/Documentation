
---
title: 实现游戏视频功能
description: 
platform: Unity_(Android)
updatedAt: Wed Jan 30 2019 08:34:09 GMT+0000 (UTC)
---
# 实现游戏视频功能
使用 Agora 的 `Hello-Video-Unity-Agora` 代码示例可以实现以下功能:

-   创建/加入频道

-   自由发言

-   离开频道


## 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads)最新的 Unity 视频软件包。软件包结构如下:

    <img alt="../_images/AMG-Video-Unity3D_0.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_0.png" style="width: 840.0px;"/>

> `Hello-Video-Unity-Agora` 即为本文需要使用的代码示例。

   1.  请确保已满足以下环境要求:

       -   Unity 5.5 或更高版本

       -   Android Studio 2.0 或更高版本

       -   两部支持语音和视频的 Android 真机设备

       -   准备一个 App ID，详见 [获取 App ID](../../cn/Agora%20Platform/token.md)

   2.  请确保在使用 Agora 相关功能及服务前，已打开特定端口，详见 [防火墙说明](../../cn/Agora%20Platform/firewall.md)。


## 步骤 2: 编译代码示例

1.  使用 Unity 打开项目 `Hello-Video-Unity-Agora` 。

    <img alt="../_images/AMG-Video-Unity3D_1.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_1.png" />


2.  填写 App ID。Unity 会提示 error，点击状态栏的红色惊叹号，自动打开 MonoDevelop 并定位到出错的脚本处填写 App ID。

    <img alt="../_images/AMG-Video-Unity3D_2.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_2.png "/>


3.  检查 Script 文件和 GameObject 的绑定状态。**关于如何绑定，请参考 Unity 官方文档** 。

    1.  打开 SceneHome, 选择 **JoinButton**，检查对应的 Script 是否正常（如果有黄色的惊叹号，则需要重新选择 `ButtonClick.cs` 文件\)。

        <img alt="../_images/AMG-Video-Unity3D_3.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_3.png" style="width: 840.0px;" />

    2.  打开 SceneHelloVideo，选择 **LeaveButton**，和检查 Scene0 中的 **JoinButton** 一样，检查对应的 Script 是否正常（如果有黄色的惊叹号，则需要重新选择 `ButtonClick.cs` 文件）。

    3.  检查 SceneHelloVideo 中的 Cube 和 Cylinder，是否显示 script 文件丢失。如果显示这样的标记:

         <img alt="../_images/AMG-Video-Unity3D_4.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_4.png" style="width: 840.0px;"/>

         则需要重新绑定 Script 文件。

         -   点击上图被红框标记的圆形按钮，弹出下面的脚本对话框:
					
			![](https://web-cdn.agora.io/docs-files/1543570455320)

         -   选择 `videoSurface` 。

4.  选择 **File\> Build Settings…** 。

     <img alt="../_images/AMG-Video-Unity3D_7.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_7.png" style="width: 1000.0px;"/>

    1.  在弹出的对话框里:

      -   选择 **Android** 平台；

          <img alt="../_images/AMG-Video-Unity3D_8.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_8.png" />

     -   Build System 选择 **Gradle\(New\)** ;

     -   勾选 **Export Project** ;

     -   选择 **Player Settings\> Other Settings\>Graphics API\> OpenGLES2**;

     -   点击按钮 **Export**，在文件对话框里选择导出到某个文件夹\(需要对新文件夹进行命名\)，例如:

         <img alt="../_images/AMG-Video-Unity3D_9.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_9.png" />

     -   点击 **create** 进行确认。

   2.  打开命令行终端\(Terminal\), 进入到 Android/RollingVideo 目录下，执行命令编译和安装:

    ```
    ./gradlew assembleDebug
    adb install build/outputs/apk/Hello Gaming Video Agora-debug.apk
    ```


## 步骤 3: 演示游戏视频

演示游戏视频至少需要两部或多部 Android 真机，本文仅以两部手机为例进行演示。

1.  两部手机上都运行 `Hello Gaming Video Agora `。

2.  两部手机加入相同的频道名，默认频道名为 unit3d，你可以自行修改。只有加入相同的频道才能互通。

你现在可以在频道内自由发言了！请观察该页面的文字提示消息来确定演示程序的运行状况。

> 有的系统会弹出授权对话框（麦克风，网络等），需要同意。如果没有声音，或者不通，请检查权限是否被禁掉了。


