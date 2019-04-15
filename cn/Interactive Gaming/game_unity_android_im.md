
---
title: 实现游戏 IM 功能
description: 
platform: Unity_(Android)
updatedAt: Fri Nov 02 2018 04:11:52 GMT+0800 (CST)
---
# 实现游戏 IM 功能
使用 Agora 的 `Hello-IM-Unity-Agora` 代码示例可以实现以下功能:

-   发送 / 接收点对点消息

-   发送 / 接收聊天室消息

-   发送 / 接收语音消息

-   发送 / 接收讨论组消息


## 步骤 1: 准备环境

1.  [下载](https://docs.agora.io/cn/Agora%20Platform/downloads)最新的 Unity 视频软件包。软件包结构如下:

    <img alt="../_images/AMG-Video-Unity3D_0.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_0.png" style="width: 370.0px;"/>

> `Hello-IM-Unity-Agora` 即为本文需要使用的代码示例。

   1.  请确保已满足以下环境要求:

       -   Unity 5.5 或更高版本

       -   Android Studio 2.0 或更高版本

       -   两部支持语音和视频的 Android 真机设备

       -   准备一个 App ID。详见 [获取 App ID](../../cn/Agora%20Platform/token.md)。

       -   准备一个 App Key。请联系 [sales@agora.io](mailto:sales@agora.io) 获取您的 App Key。


## 步骤 2: 编译代码示例

1.  使用 Unity 打开项目` Hello-IM-Unity-Agora`。

    <img alt="../_images/AMG-Video-Unity3D_1.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_1.png "/>

2.  填写 App ID 和 App Key。

    Unity 会提示 error，点击状态栏的红色惊叹号，自动打开 MonoDevelop 并定位到出错的脚本处填写 App ID 和 App Key。

	![AMG-IM-Unity_fill_in_id.jpeg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412885625)

3.  设置编译选项。

    1.  选择 **File\> Build Settings…** 。

        <img alt="../_images/AMG-Video-Unity3D_7.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_7.png"/>

    2.  在弹出的对话框里:

        -   选择 **Android** 平台；

            <img alt="../_images/AMG-Video-Unity3D_8.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_8.png" />

        -   Build System 选择 **Gradle\(New\)** ;

        -   勾选 **Export Project** ;

        -   点击按钮 **Export**，在文件对话框里选择导出到某个文件夹\(需要对新文件夹进行命名\)，例如:

            <img alt="../_images/AMG-Video-Unity3D_9.png" src="https://web-cdn.agora.io/docs-files/cn/AMG-Video-Unity3D_9.png" />

        -   点击 **create** 进行确认。

4.  打开命令行终端\(Terminal\), 进入到 Android/RollingVideo 目录下，执行命令编译和安装:

    ```
    ./gradlew assembleDebug
    adb install build/outputs/apk/Hello Gaming IM Agora-debug.apk
    ```


## 步骤 3: 演示游戏 IM

演示游戏 IM 至少需要两部或多部 Android 真机，本文仅以两部手机为例进行演示。

1.  两部手机上都运行 `Hello-Gaming-IM-Agora`。

2.  两部手机加入相同的频道名，默认频道名为 `unity3d`, 你可以自行修改。只有加入相同的频道才能互通。

	![AMG-IM-Unity-channel-name.jpeg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412000531)

3.  进入测试页面。

	![AMG-IM-Unity-main-scene.jpeg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412027030)


   1.  测试发送点对点消息。

       1.  在 **SenderId** 框中输入对方ID;

       2.  在 **MessageText** 框中输入要发送的文字；

       3.  点击 **SendPrivate** 发送点对点消息。

   2.  测试发送聊天室消息。

       1.  在 **MessageText** 框中输入要发送的内容；

       2.  点击 **SendChannel** 发送消息给频道名对应的聊天室。

   3.  测试发送讨论组相关信息。

       1.  点击 **CreateDiscussion** 创建并加入聊天室;

       2.  点击 **SendDiscussion** 在讨论组发送消息。

   4.  测试发送语音消息。

       1.  点击 **StartRecord** 开始录音；

       2.  点击 **StopRecord** 停止录音；

       3.  点击 **SendVoice** 发送刚刚录制的音频。

   5.  测试接收文字消息。

       接收到的点对点消息，聊天室消息，或讨论组消息会出现在 **message** 处。

   6.  测试接收语音消息。

       收到语音消息时，会有消息提示。点击 **PlayAudio** 播放收到的语音消息。
>
> 有的系统会弹出授权对话框（麦克风，网络等），需要同意。如果没有声音，或者不通，请检查权限是否被禁掉了。


