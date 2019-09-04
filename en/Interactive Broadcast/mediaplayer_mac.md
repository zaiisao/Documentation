
---
title: MediaPlayer Kit
description: 
platform: macOS
updatedAt: Wed Sep 04 2019 07:19:16 GMT+0800 (CST)
---
# MediaPlayer Kit
## Description

During an interactive broadcast, the host can broadcast a separate video stream to the audience through Agora’s SD-RTN™ by using MediaPlayer Kit. The audience will see two video windows; one for the live broadcast and one for the separate video stream. The host can adjust the playback of the separate video stream in real time by using MediaPlayer Kit.

<a name="format"></a>
### Supported formats

- Local video: AVI, MP4, MKV, and FLV file formats.
- Online video: RTMP and RTSP streams.

<div class="alert note">Whether it is local video or online video, only single/dual video with a sampling rate of 32 kHz, 44100 Hz or 48 kHz can be played normally in MediaPlayer Kit.</div>

### Usage

Using MediaPlayer Kit, you can start/pause the playback video, adjust the playback progress, adjust the playback volume, and publish the playback video to the remote user:

- If you want to join the channel as the host and publish the playback video to the remote user, that is, let the audience see two video windows at the same time, then you need to join the channel with a dual process. In a process, you can capture and publish the live broadcast video; while in another process, you can publish the playback video stream. If you join the channel with only a process, the remote user can only see the video window corresponding to that process.

<div class="alert note"> Because the AgoraRtcEngine is a single instance, so you need to use a dual process. In a process, you create a AgoraRtcEngine to capture and publish the live broadcast video; while in another process, you create a AgoraRtcEngine and a MediaPlayerKit to publish the separate video stream. In this documentation, we only discuss the usage of the latter process.</div>

- If you do not need to publish the playback video to the remote user, you can use MediaPlayer Kit as your local media player. We will not discuss this scenario here.

## Quickly experience MediaPlayer Kit

### Prerequisites

- Xcode 10.12 or later.
- Physical OS X 10.12 or later.
- Ensure that your project has a validated provisioning profile certificate.

### Use MediaPlayer Kit
1. Download and unzip the [MediaPlayerKitQuickstart](https://github.com/AgoraIO/Advanced-Video/tree/master/MediaPlayer/Mediaplayer-Mac) folder.
2. Open the `MediaPlayerKitQuickstart.xcodeproj` file with Xcode.
3. Click on the upper left triangle symbol in the Xcode interface to build the project.
4. After a successful compilation, a graphical interface pops up.
> If the compilation fails, check whether a validated provisioning profile certificate is set.

5. Click **Settings** on the interface to select the video quality seen by the remote user, then click **Confirm**.
6. Fill in **Channel Name**, select **Join as Broadcaster**, then you can see the playback interface of MediaPlayer Kit.
![](https://web-cdn.agora.io/docs-files/1567481228573)
7. At the bottom of the interface, you can see the following icons from left to right: Start/Pause the playback video, the progress bar, the volume bar, enables (default)/disables **PUBLISH STREAMING** and the settings.
8. Click on the text box under the download icon on the playback interface. You can select the local path to import the video file and **PUBLISH STREAMING** (default).

### Considerations

MediaPlayerKitQuickstart only supports importing the local video files.

## Implement MediaPlayer Kit

### Prerequisites

- Xcode 10.12 or later.
- Physical OS X 10.12 or later.
- Ensure that your project has a validated provisioning profile certificate.

### Integrate MediaPlayer Kit

**1. Preparation**

- Download and unzip Agora Native SDK. See the **Video SDK** in [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads).
- Make sure you have completed the integration of the Agora Native SDK, as described in [Integrated Client](../../en/Interactive%20Broadcast/mac_video.md).
- Download and unzip the MediaPlayerKit folder.

**2. Create a project**

<div class="alert warning">
	<li>In this step, you should continue to integrate MediaPlayerKit projects based on existing project that has integrated the Agora Native SDK, rather than creating a new project from scratch. 
<li> This step shows you how to create a new project, you can skip it if you don't need this reference.</div>

- Open Xcode, **Create a new Xcode project**.
- On the **Choose a template for your new project** page, select **macOS** and **cocoa App** under Application, and click **Next**.
- Fill in your **Product Name**, such as "MediaPlayer", select **Language** for Objective-C, and click **Next**.
- Select the local path where the project file is stored, such as `/Users/xxx/Desktop`, click **Create**, and then you can see your project page.

<a name="import"></a>
**3. Import files**

- Right click on the "MediaPlayer" **project file** and click on **Add Files to "MediaPlayer"**.
	- Import the `AgoraRtcEngineKit.framework` file:
		- Select the local path of this file;
		- Select **Copy items if needed** for **Destination**;
		- Select **Create groups** for **Added folder**;
		- Click on **Add**.
	- Import the `MediaPlayerKit.framework` file:
		- The import method is the same as that of the `AgoraRtcEngineKit.framework` file.
- Right click on the "MediaPlayer" **file** and click on **Add Files to "MediaPlayer"**.
	- Import the `Agora_MediaPlayer_Kit` file:
		- The import method is the same as that of the `AgoraRtcEngineKit.framework` file.

**4. Import libraries**

- Click **Build Phases**, expand **Link Binary With Libraries**, and you can see the libraries that have been filled in after importing the files:
    - `MediaPlayerKit.framework`
    - `iblibyuv.a`
    - `AgoraRtcEngineKit.framework`

> Make sure that these three libraries are added, otherwise go back to [Step 3](#import).

- Click on the "**+**" icon to add the following 21 libraries:
    - `Carbon.framework`
    - `ForceFeedback.framework`
    - `IOKit.framework`
    - `libbz2.tbd`
    - `libiconv.2.4.0.tbd`
    - `QuartzCore.framework`
    - `SecurityFoundation.framework`
    - `CoreImage.framework`
    - `OpenGL.framework`
    - `OpenCL.framework`
    - `CoreVideo.framework`
    - `SystemConfiguration.framework`
    - `libresolv.tbd`
    - `libc++.tbd`
    - `libz.tbd`
    - `CoreAudio.framework`
    - `CoreWLAN.framework`
    - `CoreMedia.framework`
    - `AudioToolbox.framework`
    - `VideoToolbox.framework`
    - `AVFoundation.framework`

**5. Configure the library path**

- Find **Header Search Paths**:
	- Click **Build Settings**;
	- Select **All** and **Levels**;
	- Fill in the search bar with **Header Search Paths**.

- Click **Header Search Paths** and add the path of the `libyuv.h` file:
	- Expand on the left side of the project page and find **Agora_MediaPlayer_Kit** > **third_party** > **libyuv** > **include** > **libyuv.h** file under the "MediaPlayer" **file**;
		- Right click here to select **Show in Finder**, you can see that the local absolute path of the `libyuv.h` file is /Users/xxx/Desktop/MediaPlayer/MediaPlayer/Agora_MediaPlayer_Kit/third_party_libyuv/include;
    > `/Users/xxx/Desktop/MediaPlayer` is your project path, `$(PROJECT_DIR)`.	
    
		- Fill in the local relative path of the `libyuv.h` file: `$(PROJECT_DIR)/MediaPlayer/Agora_MediaPlayer_Kit/third_party_libyuv/include`.


**6. Compile the project**

- Click on the upper left triangle symbol in the Xcode interface to build the project.
- After a successful compilation, a window pops up.

You can refer to [Call the interfaces](#1) or [API documentation](#2) to complete your project.

<a name="1"></a>
### Call the interfaces

**API sequence diagram**
![](https://web-cdn.agora.io/docs-files/1567498466302)

> - The diagram shows only the basic interfaces. If you have more complex requirements, you can also call more interfaces in the Agora Native SDK, such as [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:).
> - Because `setVideoView` in the MediaPlayer Kit interface can set the local video window, you don't need to call [`setupLocalVideo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupLocalVideo:) in the Agora Native SDK interface beforehand.

1. Call the Agora Native SDK interface to complete initialization and the basic video setup.

	- Call the `shareEngineWithAppId` method to create and initialize an AgoraRtcEngineKit instance. See [Implementation](https://docs.agora.io/en/Interactive%20Broadcast/initialize_mac_live?platform=macOS#implementation).
	- Call the `setChannelProfile` method to set the channel profile as Live Broadcast. See [Set the channel profile as Live Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/join_live_mac?platform=macOS#set-the-channel-profile-as-live-broadcast).
	- Call the `setClientRole` method to set the role as the host.
```Objective-c
	[self.agoraKit setClientRole:AgoraClientRoleBroadcaster]
```
	- Call the `enableVideo` method to enable the video mode. See [Enable the video mode](https://docs.agora.io/en/Interactive%20Broadcast/publish_mac_live?platform=macOS#enable-the-video-mode).

2. Call the MediaPlayer Kit interface to complete initialization and other preparations.

	- Call the `shareInstance` and `createMediaPlayerKitWithRtcEngine()` methods to create and initialize a MediaPlayer Kit instance. 
```Objective-c
[[MediaPlayerKit shareInstance] createMediaPlayerKitWithRtcEngine:agoraKit withSampleRate:44100];
```
	- Call the `MediaPlayerKitDelegate` method to set the delegate method. See [API documentation](#2) for details.
  ```Objective-c
	 [MediaPlayerKit shareInstance].delegate = self;
	```   
	- Call the `setVideoView` method to set the local view.
	```Objective-c
	 [[MediaPlayerKit shareInstance] setVideoView:self.view];
	```
    
3. Call the Agora Native SDK interface to complete the channel management.

	- Call the `joinChannelByToken` method to join the channel. See [Join a live broadcast channel](https://docs.agora.io/en/Interactive%20Broadcast/join_live_mac?platform=macOS#join-a-live-broadcast-channel).
> Call this method after the `setVideoView` method.

	- Call the [`setupRemoteVideo`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupRemoteVideo:) method to set the view of the remote user.

4. Call the MediaPlayer Kit interface to load the video.

	- Call the `load` method to load the video into the memory.
> You can load the video from the local or URL path to meet your requirements. See [Supported format](#format).
>
   ```Objective-c
	[[MediaPlayerKit shareInstance] load:url isAutoPlay:true/false];
	```

<a name="3"></a>
5. Call the MediaPlayer Kit interface to complete the media player functions.

	- Call the `play`, `adjustPlaybackSignalVolume`, `seekTo`, `getCurrentPosition`, `pause`, and `resume` methods to adjust the media player in real time.
> You can learn about these methods by referring to [API documentation](#2).
>
  ```Objective-c
 [[MediaPlayerKit shareInstance] play];
 [[MediaPlayerKit shareInstance] adjustPublishSignalVolume:volume];
 [[MediaPlayerKit shareInstance] seekTo:msec];
 [[MediaPlayerKit shareInstance] getCurrentPosition];
...
```

6. Call the MediaPlayer Kit interface to publish the audio/video to the remote user.

    - If you only want to publish the audio to the remote user:
        - Call the `publishAudio` method to publish the audio.
        ```Objective-c
	      [[MediaPlayerKit shareInstance] publishAudio];
	      ```

        - Adjust the media player in real time by calling the interfaces in [Step 5](#3).

        - Call the `adjustPublishSignalVolume` method to adjust the volume received by the remote user.
        ```Objective-c
	     [[MediaPlayerKit shareInstance] adjustPublishSignalVolume:<#(int)#>];
	     ```

        - Call the `unpublishAudio` method to stop publishing the audio to the remote user.
        ```Objective-c
	      [[MediaPlayerKit shareInstance] unpublishAudio];
	      ```

    - If you want to publish the the video to the remote user:
        - Call the `publishVideo` method to publish the video.
        ```Objective-c
	      [[MediaPlayerKit shareInstance] publishVideo];
	      ```

        - Adjust the media player in real time by calling the interfaces in [Step 5](#3).

        - Call the `adjustPublishSignalVolume` method to adjust the volume received by the the remote user.
        ```Objective-c
	      [[MediaPlayerKit shareInstance] adjustPublishSignalVolume:<#(int)#>];
	      ```

        - Call the `unpublishVideo` method to stop publishing the video to the remote user.
        ```Objective-c
	      [[MediaPlayerKit shareInstance] unPublishVideo];
	      ```


7. Call the MediaPlayer Kit interface to stop playback and quit MediaPlayer Kit.
	- Call the `stop` method to stop the playback.
```Objective-c
 [[MediaPlayerKit shareInstance] stop];
```
	- Call the `unload` method to release the video loaded into the memory.
```Objective-c
 [[MediaPlayerKit shareInstance] unload];
```
	- Call the `destroy` method to destroy the MediaPlayerKit instance and quit MediaPlayer Kit.
> After calling the `destroy` method, the binding of the display view of the local video stream is not valid.
>
```Objective-c
[[MediaPlayerKit shareInstance] destroy];
```


<a name="2"></a>
## API documentation
See [API documentation](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/mediaplayer_oc/docs/headers/MediaPlayer-Kit-Objective-C-API-Overview.html).

## Consideration

If you get an error in MediaPlayer Kit when playing video from the URL path due to network problems, you need to call the `load` method again to reload the video after the network conditions improve.
