
---
title: Interactive Gaming API
description: 
platform: Unity
updatedAt: Thu Aug 15 2019 10:24:41 GMT+0800 (CST)
---
# Interactive Gaming API
This topic provides API reference of Agora AMG SDK for Unity for iOS, Android, and Windows.

-   For Android platform, you need to apply for the following system permissions:

    -   <uses-permission android:name=”android.permission.INTERNET” /\>
    -   <uses-permission android:name=”android.permission.RECORD\_AUDIO” /\>
    -   <uses-permission android:name=”android.permission.BLUETOOTH” /\>
    -   <uses-permission android:name=”android.permission.MODIFY\_AUDIO\_SETTINGS” /\>
    -   <uses-permission android:name=”android.permission.ACCESS\_NETWORK\_STATE” /\>
    -   <uses-permission android:name=”android.permission.ACCESS\_WIFI\_STATE” /\>
    -   <uses-permission android:name=”android.permission.WAKE\_LOCK” /\>
    -   <uses-permission android:name=”android.permission.WRITE\_EXTERNAL\_STORAGE” /\>
    -   <uses-permission android:name=”android.permission.READ\_PHONE\_STATE” /\>
    -   <uses-permission android:name=”android.permission.READ\_LOGS” /\>

-   For the iOS platform, you need to include the following libraries/frameworks:

    -   `CoreTelephony.framework`
    -   `VideoToolbox.framework`
    -   `libresolv.tbd`
    -   `AgoraAudioKit.framework`
    -   `libagoraSdkCWrapper.a`
    -   `libopencore-amrnb.a`


This document is provided for the C# programming language with the following classes:

-   [IRtcEngine Abstract Class](#irtcengine): includes the main methods for the application to call and the corresponding callbacks.

-   [IAudioEffectManager Interface Class](#iaudioeffectmanager): includes the methods for the application to manage the audio effects.

-   [IM Interface Class](#im): includes the methods for the application to send and receive IM messages.

-   [IM RESTful Interface Class](#im-restful): includes the methods for the application to get IM token and service for sensitive words.

-   [IM Callback Class](#im-callback): includes the callbacks of IM Interface Class.

<a id = "irtcengine"></a>
## IRtcEngine Abstract Class

IRtcEngine Abstract Class includes the following methods:

-   [Engine Instance related](#engine)
-   [Channel related](#channel)
-   [Audio related](#audio)
-   [Video related](#video)
-   [Mute related](#mute)
-   [Mixing related](#mixing)
-   [Recording related](#recording)
-   [Encryption related](#encryption)
-   [Others](#other)
-   [Callbacks](#callback)

<a name = "engine"></a>
### Get an Engine Instance (GetEngine)

```
public static IRtcEngine GetEngine (string appId);
```

This method gets an engine instance. If none exists, it creates an instance. Once the API is executed successfully, it returns the IRtcEngine instance (not null).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>appId</code></td>
<td>App ID, see <a href="../../en/Agora%20Platform/token.md"><span>Getting an App ID</span></a> for details</td>
</tr>
</tbody>
</table>



### Query an Engine Instance (QueryEngine)

```
puiblic static IRtcEngine QueryEngine();
```

This method queries an engine instance. Unlike `GetEngine`, `QueryEngine` will not create a new instance if none exists.

### Destroy an Engine Instance (Destroy)

```
public static void Destroy()
```
	
This method releases all the resources used by the Agora SDK.  This is useful for applications that occassionaly make voice or video calls, to free up resources for other operations when not making calls.

Once the application has called `destroy` to destroy the created RtcEngine instance,
no other methods in the SDK can be used and no callbacks occur.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "channel"></a>
### Set the Channel Profile (SetChannelProfile)

```
public abstract int SetChannelProfile(CHANNEL_PROFILE profile);
```

This method configures the channel profile. The Agora RtcEngine needs to know what scenario the application is in to apply different methods for optimization.

> -   Only one profile can be used at the same time in the same channel.
> -   This method must be called and configured before a user joins a channel because the channel profile cannot be configured when the channel is in use.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>profile</code></td>
<td><p>The channel profile. Choose from the following:</p>
<ul>
<li>CHANNEL_PROFILE_GAME_FREEMODE=2: Free Mode. In this mode, everyone in the channel talks freely.</li>
<li>CHANNEL_PROFILE _GAME_COMMANDMODE=3: Command Mode. In this mode, there are hosts and audiences the channel. Only the hosts can speak in the channel.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Client Role (SetClientRole)

```
public abstract int SetClientRole(CLIENT_ROLE role, string permissionKey);
```

This method sets the user role before joining a channel, and allows you to switch the user role after joining a channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
	<tr><td><code>role</code></td>
<td><p>User role in a command-mode channel:</p>
<ul>
<li>CLIENT_ROLE_BROADCASTER = 1; Host</li>
<li>CLIENT_ROLE_AUDIENCE = 2; Audience( default)</li>
</ul>
<p>Once set, only the host in the channel can talk, not the audience.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>permissionKey</code></td>
<td>Set as NULL</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Join a Channel (JoinChannel)

```
public abstract int JoinChannel (string channelName, string info, uint uid);
```

This method allows the user to join a channel. If a channel is not created, this method creates a channel automatically.

Users in the same channel can talk to each other; and multiple users in the same channel can start a group chat.

> The UID must not be the same as that of the other users in the same channel, otherwise it forces the voice/video services to be stopped for the other users with the same UID in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>channelName</code></td>
<td><p>A string providing the unique channel name for the AgoraRTC session. The length must be within 64 bytes.</p>
<p>Supported characters: a-z,A-Z,0-9,space,!</p>
<p>#$%&amp;,()+,</p>
<p>-,:;&lt;=.,&gt;?</p>
<p>@[],^_,{|},~</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td><code>info</code></td>
<td>(Optional) Additional information about the channel. Other users in the channel will not receive this information.</td>
</tr>
<tr><td><code>uid</code></td>
<td>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If it is not assigned (or set to 0), the SDK will assign one and return it in the <code>onJoinChannelSuccess</code> callback. The app needs to record and maintain the returned value as the SDK does not maintain it.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


### Join a Channel With a Key (JoinChannelByKey)

```
public abstract int joinChannelByKey (string channelKey, string channelName, string info, uint uid);
```

This method provides the same function as the `joinChannel` method, and the only difference is that it requires a Channel Key. In most cases, the method `joinChannel` will suffice. For added security, use this method with a Channel Key.

### Renew the Channel Key (RenewChannelKey)

```
public override int RenewChannelKey (string channelKey);
```

The Channel Key expires after a certain period of time once the Channel Key schema is enabled. When the `RequestChannelKeyHandler` callback is triggered, call this method to retrieve a new key. Failure to do so will result in the SDK disconnecting with the server.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>channelKey</code></td>
<td>Specifies the Channel key to be renewed.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Leave a Channel (LeaveChannel)

```
public abstract int LeaveChannel();
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "audio"></a>
### Enable the Audio Mode (EnableAudio)

```
public abstract int EnableAudio();
```

This method enables the audio mode, which is enabled by default.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Disable the Audio Mode (DisableAudio)

```
public abstract int DisableAudio();
```

This method disables the audio mode.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Default Audio Route to Speakerphone (setDefaultAudioRouteToSpeakerphone)

```
public int SetDefaultAudioRouteToSpeakerphone(bool speakerphone)
```

This method sets the default audio route to speakerphone or earpiece.

Once a headset is plugged in or bluetooth is connected, the audio route will be changed accordingly. Once removing the headset or disconnecting bluetooth, the audio route will be the default value.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>defaultToSpeaker</code></td>
<td><ul>
<li>True: sets the default audio route to speakerphone</li>
<li>False: sets the default audio route to earpiece</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>


> This method only works in audio mode.

The original default value for audio route is as follows:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Channel Mode</strong></td>
<td><strong>Original Default Audio Route</strong></td>
</tr>
<tr><td>Free Talk</td>
<td>Earpiece</td>
</tr>
<tr><td>Command</td>
<td>Speakerphone</td>
</tr>
</tbody>
</table>



### Set the Audio Route to Speakerphone (setEnableSpeakerphone)

```
public int SetEnableSpeakerphone (bool speakerphone)
```

This method sets the audio route to speakerphone. The action, that a headset is plugged in or bluetooth is connected, does not affect the audio route.

After calling this method, the SDK will return the `onAudioRouteChanged` callback to indicate the changes.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>enabled</code></td>
<td><ul>
<li>True:<ul>
<li>If this API is called after a user joins a channel, the audio route of the user will be the speakerphone.</li>
<li>If this API is called before a user joins a channel, when the user joins a channel, the audio route of the user will be the speakerphone.</li>
</ul>
</li>
<li>False: use the default audio route value. See `setDefaultAudioRouteToSpeakerphone` for details.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### Check Whether Audio Route is Set to Speakerphone (isSpeakerphoneEnabled)

```
public bool IsSpeakerphoneEnabled()
```

This method checks whether the audio route is set to speakerphone.

> The method is only valid on Unity for iOS and Unity for Android.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Speakerphone Volume (SetSpeakerphoneVolume)

```
public int SetSpeakerphoneVolume(int volume)
```

This method sets the speakerphone volume.

> The method is only valid on Unity for iOS and Unity for Android.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>volume</code></td>
<td>Sets the speakerphone volume between 0 (lowest volume) to 255 (highest volume).</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Enable the Audio Volume Indication (EnableAudioVolumeIndication)

```
public abstract int EnableAudioVolumeIndication (int interval, int smooth);
```

This method enables or disables the audio volume indication.

This method enables the SDK to regularly report to the application on which user is talking and the volume of the speaker.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>interval</code></td>
<td><p>Time interval between two consecutive volume indications.</p>
<ul>
<li>&lt;=0 : Disables the volume indication.</li>
<li>&gt;0: Volume indication interval (ms). Recommend setting it to a minimum of 200 ms.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>smooth</code></td>
<td>Smoothing factor. Recommended default value is 3.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Pause the Audio (Pause)

```
public void Pause ()
```

This method pauses the audio. For example, when using background mode, you can call this API to pause the audio.

### Resume the Audio (Resume)

```
public void Resume ()
```

This method resumes the paused audio.

> -   Before you implement the video function, read [Audio related](#audio). The basic video function is based on the APIs listed in [Audio related](#audio).
> -   To implement any video-related functions on iOS or Android platform, select **Player Settings> Other Settings> Graphics API> OpenGLES2**.

<a name = "video"></a>
### Enable the Video Mode (EnableVideo)

```
public int EnableVideo ()
```

This method enables the video mode. The application can call this method either before entering a channel or during a call. If called before entering the channel, the service starts in the video mode; if it is called during a call, it switches from the audio to video mode. To disable the video mode, call the `Disablevideo` method.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Disable the Video Mode (DisableVideo)

```
public int DisableVideo ()
```

This method disables the video mode. The application can call this method either before entering a channel or during a call. If it is called before entering the channel, the service starts in the audio mode; if it is called during a call, it switches from the video to audio mode. To enable the video mode, call the `EnableVideo` method.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Video Profile (SetVideoProfile)

```
public int SetVideoProfile(int profile, bool swapWidthAndHeight)
```

This method sets the video encoding profile. Each profile includes a set of parameters such as resolution, frame rate, and bitrate. When the camera does not support the specified resolution, the SDK chooses a suitable camera resolution, but the encoder resolution still uses the one specified by `setVideoProfile`.

The method only sets the attributes of the streams encoded by the encoder which may be inconsistent with the final display. For example, when the resolution of the encoded stream is 640 x 480, and the rotation attribute of the stream is 90 degrees, then the resolution of the final display will be in portrait mode.

> -   Always set the video profile after calling the enableVideo method.
> -   Always set the video profile before calling the `joinChannel`/`startPreview` method.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>profile</code></td>
<td>Video profile. See Video Profile Definition for more details.</td>
</tr>
<tr><td><code>swapWidthAndHeight</code></td>
<td><p>Whether to swap the width and height of the stream:</p>
<ul>
<li>true: Swap</li>
<li>false: No swap (default)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

**Video Profile Definition**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Video Profile</strong></td>
<td><strong>Enum Value</strong></td>
<td><strong>Resolution (Width x Height)</strong></td>
<td><strong>Frame Rate (fps)</strong></td>
<td><strong>Bitrate (kbit/s)</strong></td>
</tr>
<tr><td>120P</td>
<td>0</td>
<td>160 x 120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_3</td>
<td>2</td>
<td>120 x 120</td>
<td>15</td>
<td>50</td>
</tr>
<tr><td>180P</td>
<td>10</td>
<td>320 x 180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_3</td>
<td>12</td>
<td>180 x 180</td>
<td>15</td>
<td>100</td>
</tr>
<tr><td>180P_4</td>
<td>13</td>
<td>240 x 180</td>
<td>15</td>
<td>120</td>
</tr>
<tr><td>240P</td>
<td>20</td>
<td>320 x 240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_3</td>
<td>22</td>
<td>240 x 240</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>240P_4</td>
<td>24</td>
<td>424 x 240</td>
<td>15</td>
<td>220</td>
</tr>
<tr><td>360P</td>
<td>30</td>
<td>640 x 360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_3</td>
<td>32</td>
<td>360 x 360</td>
<td>15</td>
<td>260</td>
</tr>
<tr><td>360P_4</td>
<td>33</td>
<td>640 x 360</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>360P_6</td>
<td>35</td>
<td>360 x 360</td>
<td>30</td>
<td>400</td>
</tr>
<tr><td>360P_7</td>
<td>36</td>
<td>480 x 360</td>
<td>15</td>
<td>320</td>
</tr>
<tr><td>360P_8</td>
<td>37</td>
<td>480 x 360</td>
<td>30</td>
<td>490</td>
</tr>
<tr><td>360P_9</td>
<td>38</td>
<td>640 x 360</td>
<td>15</td>
<td>800</td>
</tr>
<tr><td>360P_10</td>
<td>39</td>
<td>640 x 360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>360P_11</td>
<td>100</td>
<td>640 x 360</td>
<td>24</td>
<td>1000</td>
</tr>
<tr><td>480P</td>
<td>40</td>
<td>640 x 480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_3</td>
<td>42</td>
<td>480 x 480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>43</td>
<td>640 x 480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>45</td>
<td>480 x 480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>47</td>
<td>848 x 480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>48</td>
<td>848 x 480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>720P</td>
<td>50</td>
<td>1280 x 720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_3</td>
<td>52</td>
<td>1280 x 720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>54</td>
<td>960 x 720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>55</td>
<td>960 x 720</td>
<td>30</td>
<td>1380</td>
</tr>
<tr><td>1080P</td>
<td>60</td>
<td>1920 x 1080  <sup>[1]</sup></td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_3</td>
<td>62</td>
<td>1920 x 1080 <sup>[1]</sup></td>
<td>30</td>
<td>3150</td>
</tr>
<tr><td>1080P_5</td>
<td>64</td>
<td>1920 x 1080 <sup>[1]</sup></td>
<td>60</td>
<td>4780</td>
</tr>
</tbody>
</table>


> [1] Whether the video can achieve 1080p resolution depends on the performance and capabilities of the device. Devices with lower performance may experience low frame rates when using 1080p. Agora will optimize the video for lower-end devices in future releases. Contact [support@agora.io](mailto:support@agora.io) for details.

### Enable Dual-stream Mode (EnableDualStreamMode)

```
public int EnableDualStreamMode(bool enabled)
```

This method sets the stream mode: Single- (default) or dual-stream, which is only applicable when you set the channel profile as command mode when calling `setChannelProfile`.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>enabled</code></td>
<td><p>The mode is in single stream or dual stream:</p>
<ul>
<li>true: Dual stream</li>
<li>false: Single stream</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Video Stream Type (SetRemoteVideoStreamType)

```
public int SetRemoteVideoStreamType(uint uid, int streamType)
```

This method specifies the video stream type of the remote user to be received by the local user when the remote user sends dual streams. This method allows the application to adjust the corresponding video stream type according to the size of the video windows to save bandwidth and resources.

The Agora SDK receives the high-video stream by default. If needed, users can switch to the low-video stream using this method.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID</td>
</tr>
<tr><td><code>streamType</code></td>
<td><p>Set the video stream size.</p>
<p>The following lists the video stream types:</p>
<ul>
<li>REMOTE_VIDEO_STREAM_HIGH(0): High-video Stream</li>
<li>REMOTE_VIDEO_STREAM_LOW(1): Low-video Stream</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

The resolutions of the high-video stream are 1:1, 4:3, and 16:9. The low-video stream has the same aspect ratio:

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Resolution</th>
<th>Frame Rate</th>
<th>Keyframe Interval</th>
<th>Bitrate (kbit/s)</th>
</tr>
</thead>
<tbody>
<tr><td>160 x 160</td>
<td>5</td>
<td>5</td>
<td>45</td>
</tr>
<tr><td>160 x 120</td>
<td>5</td>
<td>5</td>
<td>32</td>
</tr>
<tr><td>160 x 90</td>
<td>5</td>
<td>5</td>
<td>28</td>
</tr>
</tbody>
</table>

### Set High-quality Video Preferences (SetVideoQualityParameters)

```
public int SetVideoQualityParameters(bool preferFrameRateOverImageQuality)
```

This method allows the users to set the video preferences.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>preferFrameRateOverImageQuality</code></td>
<td><p>Video preference to be set:</p>
<ul>
<li>True: Frame rate over image quality.</li>
<li>False: Image quality over frame rate (default).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Disable the Local Video (EnableLocalVideo)

```
public int EnableLocalVideo (bool enabled)
```

This method disables the local video, which is only applicable to the scenario when the user only wants to watch the remote video without sending any video stream to the other user. This method does not require a local camera.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>enabled</code></td>
<td><ul>
<li>true: Enable the local video (default)</li>
<li>false: Disable the local video</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Start the Video Preview (StartPreview)

```
public int StartPreview ()
```

This method starts the local video preview.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Stop the Video Preview (StopPreview)

```
public int StopPreview ()
```

This method stops the local video preview and closes the video.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Switch Between Front and Back Cameras (SwitchCamera)

```
public int SwitchCamera()
```

This method switches between front and back cameras.

> The method is only valid on Unity for iOS and Unity for Android.

### Enable the Video Observer (EnableVideoObserver)

```
public int EnableVideoObserver ()
```

This method sends the video pictures directly to the app instead of to the traditional view renderer.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



Disable the Video Observer (DisableVideoObserver) 

```
public int DisableVideoObserver ()
```

This method disables sending video directly to the app.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "mute"></a>
### Mute the Local Audio Stream (MuteLocalAudioStream)

```
public abstract int MuteLocalAudioStream (bool mute);
```

This method enables/disables sending local audio streams to the network.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: Stops sending local audio streams to the network.</li>
<li>False: Allows sending local audio streams to the network.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Mute all Remote Audio Streams (MuteAllRemoteAudioStreams)

```
public abstract int MuteAllRemoteAudioStreams (bool mute);
```

This method mutes/unmutes all remote users’ audio streams.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: Mutes all received audio streams.</li>
<li>False: Unmutes all received audio streams.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Mute a Remote Audio Stream (MuteRemoteAudioStream)

```
public abstract int MuteRemoteAudioStream (uint uid, bool mute);
```

This method mutes/unmutes a specified remote user’s audio stream.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID of the user whose audio stream is to be muted.</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: Mutes the user’s audio stream.</li>
<li>False: Unmutes the user’s audio stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Mute the Local Video Stream (MuteLocalVideoStream)

```
public int MuteLocalVideoStream(bool mute)
```

This method enables/disables sending local video streams to the network. When set to True, this method does not disable the camera and thus does not affect getting local video streams.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: Stops sending local video streams to the network.</li>
<li>False: Allows sending local video streams to the network.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Mute all Remote Video Streams (MuteAllRemoteVideoStreams)

```
public int MuteAllRemoteVideoStreams(bool mute)
```

This method enables/disables playing all remote users’ video streams. When set to True, this method stops playing video streams without affecting the video stream receiving process.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: Stops playing all the received video streams.</li>
<li>False: Allows playing all the received video streams.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Mute a Certain Remote Video Stream (MuteRemoteVideoStream)

```
public int MuteRemoteVideoStream(uint uid, bool mute)
```

This method pauses/resumes playing a specified user’s video, that is, enables/disables playing a specified user’s video stream.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID of the specified user.</td>
</tr>
<tr><td><code>mute</code></td>
<td><ul>
<li>True: Stops playing a specified user’s video stream.</li>
<li>False: Allows playing a specified user’s video stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "mixing"></a>
### Start Audio Mixing (StartAudioMixing)

```
public abstract int StartAudioMixing (string filePath, bool loopback, bool replace, int cycle, int playTime = 0);
```

This method mixes the specified local audio file with the audio stream from the microphone; or, it replaces the microphone’s audio stream with the specified local audio file. You can choose whether the other user can hear the local audio playback and specify the number of loop playbacks. This API also supports online music playback.

> Call this API when you are in the channel, otherwise it may cause issues.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>filePath</code></td>
<td><p>Name and path of the local audio file to be mixed.</p>
<p>Supported audio formats: mp3, aac, m4a, 3gp, and wav.</p>
</td>
</tr>
<tr/>
<tr><td><code>loopback</code></td>
<td><ul>
<li>True: Only the local user can hear the remix or the replaced audio stream.</li>
<li>False: Both users can hear the remix or the replaced audio stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td><code>replace</code></td>
<td><ul>
<li>True: The content of the local audio file replaces the audio stream from the microphone.</li>
<li>False: Mix the content of the local audio file with the audio stream from the microphone.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td><code>cycle</code></td>
<td><p>Number of loop playbacks:</p>
<ul>
<li>Positive integer: Number of loop playbacks.</li>
<li>-1: Infinite loop.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>playTime</code></td>
<td><p>Start time (ms) of the audio file to play.</p>
<p>0 means from the beginning.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Stop Audio Mixing (StopAudioMixing)

```
public abstract int StopAudioMixing();
```

This method stops mixing the specified local audio with the microphone input. Call this API when you are in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Pause Audio Mixing (PauseAudioMixing)

```
public abstract int PauseAudioMixing();
```

This method pauses mixing the specified local audio with the microphone input. Call this API when you are in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Resume Audio Mixing (ResumeAudioMixing)

```
public abstract int ResumeAudioMixing();
```

This method resumes mixing the specified local audio with the microphone input. Call this API when you are in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Adjust the Audio Mixing Volume (AdjustAudioMixingVolume)

```
public abstract int AdjustAudioMixingVolume (int volume);
```

This method adjusts the volume during audio mixing. Call this API when you are in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>volume</code></td>
<td>Volume ranging from 0 to 100. By default, 100 is the original volume.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Get the Audio Mixing Duration (GetAudioMixingDuration)

```
public abstract int GetAudioMixingDuration();
```

This method gets the duration (ms) of the audio mixing. Call this API when you are in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Get the Current Audio Position (GetAudioMixingCurrentPosition)

```
public abstract int GetAudioMixingCurrentPosition();
```

This method gets the playback position (ms) of the audio. Call this API when you are in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "recording"></a>
### Start an Audio Recording (StartAudioRecording)

```
public abstract int StartAudioRecording(string filePath);
```

This method starts an audio recording. The SDK allows recording during a call, which supports either one of the following two formats:

- `.wav`: Large file size with high sound fidelity 
- `.aac`: Small file size with low sound fidelity

Ensure that the directory to save the recording file exists and is writable. This method is usually called after joining a channel. The recording automatically stops when the `LeaveChannel` method is called.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>filePath</code></td>
<td>Full file path of the recording file. The string of the file name is in UTF-8 code.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded</li>
<li>&lt;0: Method call failed</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Stop an Audio Recording (StopAudioRecording)

```
public abstract int StopAudioRecording();
```

This method stops recording on the client.

Call this method before calling `LeaveChannel`, otherwise the recording automatically stops when the `LeaveChannel` method is called.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Adjust the Recording Volume (AdjustRecordingSignalVolume)

```
public abstract int AdjustRecordingSignalVolume (int volume);
```

This method adjusts the recording volume.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>volume</code></td>
<td><p>Recording volume, ranges from 0~400:</p>
<ul>
<li>0: Mute</li>
<li>100: Original volume</li>
<li>400: (Maximum) Four times the original volume with heap overflow protection</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Adjust the Playback Volume (AdjustPlaybackSignalVolume)

```
public abstract int AdjustPlaybackSignalVolume (int volume);
```

This method adjusts the playback volume.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>volume</code></td>
<td><p>Playback volume, ranges from 0 to 400:</p>
<ul>
<li>0: Mute</li>
<li>100: Original volume</li>
<li>400: (Maximum) Four times the original volume with heap overflow protection</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "encryption"></a>
### Enable Built-in Encryption (setEncryptionSecret)

```
public int SetEncryptionSecret(string secret)
```

Call `setEncryptionSecret` to specify an encryption password to enable built-in encryption before joining a channel, otherwise the communication will be unencrypted. All users in a channel must set the same encryption password. The encryption password is automatically cleared once the user has left the channel. If the encryption password is not specified or set to empty, the encryption function will be disabled.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>secret</code></td>
<td>Encryption Password</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Built-in Encryption Mode (SetEncryptionMode)

```
public int SetEncryptionMode(string encryptionMode)
```

The Agora SDK supports built-in encryption in AES-128-XTS mode by default. To use other modes, call this API to set the encryption mode.

All users in the same channel must use the same encryption mode and password. Refer to information related to the AES encryption algorithm on the differences between encryption modes.

Call `setEncryptionSecret` to enable the built-in encryption function before calling this API.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>encryptionMode</code></td>
<td><p>Encryption mode. The following modes are supported:</p>
<ul>
	<li><code>“aes-128-xts”</code>: 128-bit AES encryption, XTS mode</li>
	<li><code>“aes-128-ecb”</code>: 128-bit AES encryption, ECB mode</li>
	<li><code>“aes-256-xts”</code>: 256-bit AES encryption, XTS mode</li>
	<li><code>“”</code>: When set to a NUL string, the encryption is in “aes-128-xts” by default.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a name = "other"></a>
### Set the Log Filter (SetLogFilter)

```
public abstract int SetLogFilter (LOG_FILTER filter);
```

This method sets the SDK output log filter.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>filter</code></td>
<td><p>Sets the levels of the filters.</p>
<ul>
<li>1: INFO</li>
<li>2: WARNING</li>
<li>4: ERROR</li>
<li>8: FATAL</li>
<li>0x800: DEBUG</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set a Log File (SetLogFile)

```
public abstract int SetLogFile (string filePath);
```

This method sets the path to save the log file.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>filePath</code></td>
<td>Path to save the log file.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Trigger an SDK Event (Poll)

```
public abstract void Poll ();
```

This method triggers an SDK event according to vertical synchronization, such as `Update` (Unity3D).

### Start an Audio Call Test (startEchoTest)

```
public int StartEchoTest()
```

This method launches an audio call test to determine whether the audio devices (for example, headset and speaker) and the network connection work properly. In the test, the user speaks first, and the recording is played back in 10 seconds. If the user can hear what he or she says in 10 seconds, it indicates that the audio devices and network connection work properly.

> After calling the `startEchoTest method`, call `stopEchoTest` to end the test; otherwise the application cannot run the next echo test, nor can it call the joinChannel method to start a new call.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<ul>
<li>ERR_REFUSED (-5): Failed to launch the echo test, for example, initialization failed.</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



### Stop an Audio Call Test (StopEchoTest)

```
public int StopEchoTest()
```

This method stops an audio call test.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<ul>
<li>ERR_REFUSED(-5): Failed to stop the echo test. It could be that the echo test is not running.</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



### Get the SDK Version (GetSdkVersion)

```
public static string GetSdkVersion ()
```

This method returns the string of the version number in character format.

### Get the Error Description (GetErrorDescription)

```
public static string GetErrorDescription (int code)
```

This method returns the error code when an error is detected during SDK runtime.

### Retrieve the Current Call ID (GetCallId)

```
public string GetCallId()
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td>Current call ID.</td>
</tr>
</tbody>
</table>



### Rate the Call (Rate)

```
public int Rate(string callId, int rating, string desc)
```

This method lets the user rate the call. It is usually called after the call ends.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>callId</code></td>
<td>Call ID retrieved from the getCallId method.</td>
</tr>
<tr><td><code>rating</code></td>
<td>Rating for the call between 1 (lowest score) to 10 (highest score).</td>
</tr>
<tr><td><code>desc</code></td>
<td><p>A given description for the call with a length less than 800 bytes.</p>
<p>This parameter is optional.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid, for example, callId invalid.</li>
<li>ERR_NOT_READY (-3): The SDK status is incorrect, for example, initialization failed.</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Complain about the Call Quality (Complain)

```
public int Complain(string callId, string desc)
```

This method allows the user to complain about the call quality. It is usually called after the call ends.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>callId</code></td>
<td>Call ID retrieved from the <code>getCallId</code> method.</td>
</tr>
<tr><td><code>desc</code></td>
<td><p>A given description for the call with a length less than 800 bytes.</p>
<p>This parameter is optional.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid, for example, callId invalid.</li>
<li>ERR_NOT_READY (-3): The SDK status is incorrect, for example, initialization failed.</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Get the Message Count (getMessageCount)

```
public int GetMessageCount ()
```

This method gets the number of messages in the message queue.

<a name = "callback"></a>
### Join Channel Callback (JoinChannelSuccessHandler)

```
public delegate void JoinChannelSuccessHandler (string channelName, uint uid, int elapsed);
```

This callback is triggered when the user has joined the channel successfully.

The channel ID is assigned based on the channel name specified in the `JoinChannel` API. If the User ID is not specified when `JoinChannel` is called, the server assigns one automatically.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>channel</code></td>
<td>Channel Name.</td>
</tr>
<tr><td><code>uid</code></td>
<td><p>User ID.</p>
<p>If the uid is specified in the <code>joinChannel</code> method, returns the specified ID; if not, returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td><code>elapsed</code></td>
<td>Time elapsed in milliseconds from calling <code>joinChannel</code> until this event occurs.</td>
</tr>
</tbody>
</table>



### Rejoin Channel Callback (ReJoinChannelSuccessHandler)

```
public delegate void ReJoinChannelSuccessHandler (string channelName, uint uid, int elapsed);
```

This callback is triggered when the user has re-joined the channel successfully.

In times when the client loses connection with the server because of network problems, the SDK automatically attempts to reconnect, and then triggers this callback method upon reconnection.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
	<tr><td><code>channel</code></td>
<td>Channel Name.</td>
</tr>
<tr><td><code>uid</code></td>
<td><p>User ID.</p>
<p>If the UID is specified in the <code>JoinChannel</code> method, it returns the specified ID;</p>
<p>If not, it returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling <code>JoinChannel</code> until this event occurs.</td>
</tr>
</tbody>
</table>



### Connection Interrupted Callback (ConnectionInterruptedHandler)

```
public delegate void ConnectionInterruptedHandler ();
```

This callback indicates that the connection is lost between the SDK and the server. Once the connection is lost, the SDK tries to reconnect it repeatedly until the application calls `LeaveChannel`.

### Connection Lost Callback (ConnectionLostHandler)

```
public delegate void ConnectionLostHandler ();
```

When the connection between the SDK and server is lost, the `ConnectionInterruptedHandler` callback is triggered and the SDK reconnects automatically. If the reconnection fails within a certain period (10 seconds by default), the callback `ConnectionLostHandler` is triggered. The SDK continues to reconnect until the application calls `leaveChannel`.

### Other User Joined Channel Callback (UserJoinedHandler)

```
public delegate void UserJoinedHandler (uint uid, int elapsed);
```

This callback is triggered when the other user has joined the channel.

This callback method notifies the application that another user has joined the channel. If there are other users in the channel when that user joins, the SDK also reports to the application on the existing users who are already in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID.</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>Time interval (ms) from calling <code>JoinChannel()</code> until this callback is triggered.</td>
</tr>
</tbody>
</table>



### Other User Offline Callback (UserOfflineHandler)

```
public delegate void UserOfflineHandler (uint uid, USER_OFFLINE_REASON reason);
```

This callback notifies the application that a user has left the channel or gone offline.

The SDK reads the timeout data to determine if a user has left the channel (or has gone offline). If no data package is received from the user in 15 seconds, the SDK assumes the user is offline. Sometimes a weak network connection may lead to false detections; therefore, it is recommend to use signaling for reliable offline detection.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID.</td>
</tr>
<tr><td><code>reason</code></td>
<td><p>This callback is triggered when:</p>
<ul>
<li><strong>USER_OFFLINE_QUIT</strong>: User has quit the call.</li>
<li><strong>USER_OFFLINE_DROPPED</strong>: The SDK timed out and has dropped offline because it has not received a data package within a certain period.</li>
</ul>
<p>If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the event has timed out.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Leave Channel Callback (LeaveChannelHandler)

```
public delegate void LeaveChannelHandler (RtcStats stats);
```

This callback is triggered when the user left the channel.

When the application calls the `LeaveChannel` method, the SDK uses this callback to notify the application that the user has successfully left the channel. With this callback function, the application retrieves information such as the call duration and the statistics of data received/transmitted by the SDK.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>stat</code></td>
<td><p>The statistics about the call.</p>
<ul>
	<li><code>duration</code>: Call duration (s).</li>
	<li><code>txBytes</code>: Total number of bytes transmitted.</li>
	<li><code>rxBytes</code>: Total number of bytes received.</li>
	<li><code>txKBitRate</code>: Transmission bitrate (kbit/s).</li>
	<li><code>rxKBitRate</code>: Receive bitrate (kbit/s).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



```
struct RtcStats {
    unsigned int duration;
    unsigned int txBytes;
    unsigned int rxBytes;
    unsigned short txKBitRate;
    unsigned short rxKBitRate;
  };
```

### Audio Volume Indication Callback (VolumeIndicationHandler)

```
public delegate void VolumeIndicationHandler (AudioVolumeInfo[] speakers, int speakerNumber, int totalVolume);
```

This callback indicates who is talking and the speaker’s volume. By default the indication is disabled. If needed, call the `enableAudioVolumeIndication` method to trigger this indication.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>speakers</td>
<td><p>The speaker (array). Each speaker () includes:</p>
<ul>
<li><code>uid</code>: User ID of the speaker.</li>
<li><code>volume</code>: Volume of the speaker, between 0 (lowest volume) to 255 (highest volume).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>speakerNumber</code></td>
<td>Number of the speakers</td>
</tr>
<tr><td><code>totalVolume</code></td>
<td>Total volume after audio mixing between 0 (lowest volume) to 255 (highest volume).</td>
</tr>
</tbody>
</table>



### Other User Muted Audio Callback (UserMutedHandler)

```
public delegate void UserMutedHandler (uint uid, bool muted);
```

This callback indicates that some other user has muted/unmuted his/her audio stream.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID.</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: User has muted his/her audio.</li>
<li>False: User has unmuted his/her audio.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Warning Occurred Callback (SDKWarningHandler)

```
public delegate void SDKWarningHandler (int warn, string msg);
```

This callback method indicates that some warning occurred during SDK runtime. In most cases, the application can ignore the warnings reported by the SDK, because the SDK can usually fix the issue and resume running. For instance, the SDK may report an ERR_OPEN_CHANNEL_TIMEOUT warning upon disconnection with the server and attempts to reconnect.

> For detailed warning codes, refer to [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>warn</code></td>
<td>The warning code.</td>
</tr>
<tr><td><code>msg</code></td>
<td>The warning message.</td>
</tr>
</tbody>
</table>



### Error Occurred Callback (SDKErrorHandler)

```
public delegate void SDKErrorHandler (int error, string msg);
```

This callback indicates that a network or media error occurred during SDK runtime. In most cases, reporting an error means that the SDK cannot fix the issue and resume running, and therefore requires actions from the application or simply informs the user on the issue. For instance, the SDK reports an ERR_START_CALL error when failing to initialize a call. In this case, the application informs the user that the call initialization failed and calls the `LeaveChannel` method to exit the channel.

> For detailed error codes, see [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>err</code></td>
<td>The error code.</td>
</tr>
<tr><td><code>msg</code></td>
<td>The error message.</td>
</tr>
</tbody>
</table>


### RtcEngine Statistics Callback (RtcStatsHandler)

```
public delegate void RtcStatsHandler (RtcStats stats);
```

The SDK updates the application on the statistics of the RtcEngine runtime status once every two seconds.

### Audio Mixing Playback Finished (AudioMixingFinishedHandler)

```
public delegate void AudioMixingFinishedHandler ();
```

This callback is triggered when the playback of the audio mixing file is finished.

### Audio Route Changed Callback (AudioRouteChangedHandler)

```
public delegate void AudioRouteChangedHandler (AUDIO_ROUTE route);
```

The SDK notifies the application that the audio route is changed, and is switched to an earpiece, speakerphone, headset, or bluetooth device.

### Channel Key Expired Callback (RequestChannelKeyHandler)

```
public delegate void RequestChannelKeyHandler ();
```

When a Channel Key is specified by calling `joinChannel`, if the SDK lost connection with the Agora server due to network issues, the Channel Key may expire after a certain period of time and a new Channel Key may be required to reconnect to the server.

This callback notifies the application the need to generate a new Channel Key, and calls `RenewChannelKey` to renew the Channel Key.

### First Remote Video Frame Received and Decoded Callback (OnFirstRemoteVideoDecodedHandler)

```
public delegate void OnFirstRemoteVideoDecodedHandler (uint uid, int width, int height, int elapsed);
```

This callback is triggered upon receiving and successfully decoding the first frame of the remote video. The application can configure the user view settings in this callback.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID of the user whose video streams are received.</td>
</tr>
<tr><td><code>width</code></td>
<td>Width (in pixels) of the video stream.</td>
</tr>
<tr><td><code>height</code></td>
<td>Height (in pixels) of the video stream.</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling <code>joinChannel</code> until this callback is triggered.</td>
</tr>
</tbody>
</table>



### Video Size Changed Callback (OnVideoSizeChangedHandler)

```
public delegate void OnVideoSizeChangedHandler (uint uid, int width, int height, int elapsed);
```

This method is triggered when the video size of the specific user is changed.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID of the specific user</td>
</tr>
<tr><td><code>width</code></td>
<td>Width (in pixels) of the video stream.</td>
</tr>
<tr><td><code>height</code></td>
<td>Height (in pixels) of the video stream.</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling <code>joinChannel</code> until this callback is triggered.</td>
</tr>
</tbody>
</table>



### Client Role Changed Callback (onClientRoleChangedHandler)

```
public delegate void onClientRoleChangedHandler(int oldRole, int newRole)
```

When you set the channel profile as Live Broadcast when calling `setChannelProfile`,this callback is triggered when there is any role change in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>oldRole</code></td>
<td>The role that you switched from</td>
</tr>
<tr><td><code>newRole</code></td>
<td>The role that you are now in</td>
</tr>
</tbody>
</table>



### User Video Muted Callback (OnUserMuteVideoHandler)

```
public delegate void OnUserMuteVideoHandler (uint uid, bool muted);
```

This callback indicates that some other user has paused/resumed his/her video streams.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>uid</td>
<td>User ID.</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>True: The remote user mute the video</li>
<li>False: The remote user unmute the video</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a id = "iaudioeffectmanager"></a>
## IAudioEffectManager Interface Class

### Get teh Audio Effect Volume (GetEffectsVolume)

```
double GetEffectsVolume();
```

This method gets the volume of the effects from 0.0 to 100.0.

### Set the Audio Effect Volume (SetEffectsVolume)

```
int SetEffectsVolume(double volume)
```

This method sets the volume of the audio effects.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>volume</code></td>
<td>The value ranges from 0.0 to 100.0. 100.0 is the default value.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Play the Audio Effect (PlayEffect)

```
int PlayEffect (int soundId, String filePath,
              bool loop = false,
              double pitch = 1.0D,
              double pan = 0.0D,
              double gain = 100.0D
);
```

This method plays the audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>soundId</code></td>
<td>ID of the audio effect. Each audio effect has a unique ID. <sup>[2]</sup></td>
</tr>
<tr><td><code>filePath</code></td>
<td>Absolute path of the audio effect file.</td>
</tr>
<tr><td><code>loop</code></td>
<td><p>Sets whether to play the audio effect in loops:</p>
<ul>
<li>True: Yes.</li>
<li>False: No (default).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pitch</code></td>
<td><p>Sets whether to change the frequency of the audio effect.</p>
<p>The range is [0.5, 2] with 1.0 as the default value, which means no need to change the frequency.</p>
<p>The smaller the value, the lower the frequency.</p>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pan</code></td>
<td><p>Sets whether to change the spatial position of the audio effect. The range is [-1, 1]:</p>
<ul>
<li>0: Audio effect shows right ahead; stereo effect.</li>
<li>-1: Audio effect shows on the left.</li>
<li>1: Audio effect shows on the right.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>gain</code></td>
<td><p>Sets whether to change the volume of a single audio effect. The range is [0.0, 100.0]</p>
<p>The default value is 100.0. The smaller the value, and lower the volume.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

> [2] If the audio effect is loaded into the memory through *preloadEffect*, ensure that the soundId set is the same as in `preloadEffect`.

### Stop Playing the Audio Effect (StopEffect)

```
int StopEffect(int soundId);
```

This method stops playing a specific audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>soundId</code></td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Stop Playing all Audio Effects (stopAllEffects)

```
int stopAllEffects();
```

This method stops playing all the audio effects.

### Preload an Audio Effect (preloadEffect)

```
int PreloadEffect(int soundId, String filePath);
```

This method preloads a specific audio effect file (compressed audio file) to the memory.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>soundId</code></td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td><code>filePath</code></td>
<td>Path and file name of the effect audio file to be preloaded.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Release an Audio Effect (unloadEffect)

```
int unloadEffect(int soundId);
```

This method releases a specific preloaded audio effect from the memory.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>soundId</code></td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Pause an Audio Effect (pauseEffect)

```
virtual int pauseEffect(int soundId)
```

This method pauses a specific audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>soundId</code></td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Pause all Audio Effects (pauseAllEffects)

```
int pauseAllEffects();
```

This method pauses all audio effects.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Resume an Audio Effect (resumeEffect)

```
int resumeEffect(int soundId);
```

This method resumes playing a specific audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>soundId</code></td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Resume all Audio Effects (resumeAllEffects)

```
int resumeAllEffects();
```

This method resumes all audio effects.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set Voice-only Mode (setVoiceOnlyMode)

```
int setVoiceOnlyMode(bool enable);
```

This method sets to voice-only mode (transmit the audio stream only), and the other streams will be ignored; for example the sound of the keyboard strokes.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>enable</code></td>
<td><p>True: Enables voice-only mode.</p>
<p>False: Disables voice-only mode.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Voice Position of the Remote User (SetRemoteVoicePosition)

```
int SetRemoteVoicePosition(uint uid, double pan, double gain);
```

Sets the voice position of the remote user.

> This API is valid only for earphones, and is invalid when speakerphone is enabled.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
	<tr><td><code>uid</code></td>
<td>User ID of the remote user.</td>
</tr>
<tr><td><code>pan</code></td>
<td><p>Sets whether to change the spatial position of the audio effect. The range is [-1, 1]:</p>
<ul>
<li>0: Audio effect shows right ahead; stereo effect.</li>
<li>-1: Audio effect shows on the left.</li>
<li>1: Audio effect shows on the right.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><p><code>gain</code></p>
<p>Return Value</p>
</td>
<td><p>Sets whether to change the volume of a single audio effect. The range is [0.0, 100.0]</p>
<p>Default value is 100.0. The smaller the number, the lower the volume.</p>
<ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Set the Local Voice Pitch (SetLocalVoicePitch)

```
int SetLocalVoicePitch (double pitch);
```

This method changes the voice pitch of the local speaker.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>uid</code></td>
<td>User ID of the remote user.</td>
</tr>
<tr><td><code>pitch</code></td>
<td>Voice frequency, which is in the range of [0.5, 2.0]. The default value is 1.0.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


<a id = "im"></a>
## IM Interface Class

The class includes the methods for the application to send and receive IM messages.

### Create an IRtcEngine Object (GetEngine)

```
mRtcEngine = IRtcEngine.GetEngine (appId);
```

The method creates an IRtcEngine Object.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>appId</code></td>
<td>App ID for your application, see <a href="../../en/Agora%20Platform/token.md"><span>Getting an App ID</span></a> .</td>
</tr>
</tbody>
</table>



### Initialize IM resources

```
AgoraIMInit(appKey);
```

The method initializes IM-related resources with App Key.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>appKey</code></td>
<td>App Key for your application, like <em>“82hegw5u8y3dx”</em> . Contact <a href="mailto:sales%40agora.io">sales-us@agora.io</a> for your App key.</td>
</tr>
</tbody>
</table>



### Connect to IM Server

```
AgoraIMConnect(token);
```

The method connects IM server with IM token.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>token<code></td>
<td>Token for IM service, like “j9UjgwnHPVLCeh5sc/27VR0pyBBs8RAFmk/S9ysb6uMoJKDc/Mv5qwjsPNb/nqiSP/msWr6S3EqVWLSM5oxiuA==”. To get IM Token, you must use <a href="#gettoken">Get IM Token (getToken)</a> .</td>
</tr>
</tbody>
</table>


<a id = "agoraimsendmessage"></a>
### Send text message (AgoraIMSendMessage)

```
public void AgoraIMSendMessage(String targetId,string message,int whichConversationType){
    agoraIMSendMessage(targetId,message,conversationType);
}
```

The method sends text message to a specific user.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>targetId</code></td>
<td>The ID of a receiver</td>
</tr>
<tr><td><code>message</code></td>
<td>The text message</td>
</tr>
<tr><td><code>conversationType</code></td>
<td><p>Type of the messages, including the following types:</p>
<div><ul>
<li>public static int NONE = 0: no message</li>
<li>public static int PRIVATE = 1: point-to-point message</li>
<li>public static int DISCUSSION = 2: discussion message</li>
<li>public static int GROUP = 3: group message</li>
<li>public static int CHATROOM = 4: chatroom message</li>
<li>public static int CUSTOMER_SERVICE = 5: customer service message</li>
<li>public static int SYSTEM = 6: system message</li>
<li>public static int APP_PUBLIC_SERVICE = 7: app public service message</li>
<li>public static int PUBLIC_SERVICE = 8: public service message</li>
<li>public static int PUSH_SERVICE = 9: push service</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

> Make sure you have connected the server successfully before you send a text message.

### Initialize Message Receive Listener (AgoraIMInitMessageReceiveListener)

```
public void AgoraIMInitMessageReceiveListener(){
    agoraIMInitMessageReceiveListener();
}
```

The method initializes message receive listener.

### Send Voice Message (AgoraIMSendVoiceMessage)

```
public void AgoraIMSendVoiceMessage(string targetId, int conversationType, string filepath. int lengthOfVoiceMessage){
    agoraIMSendMessage(targetId,conversationType,filePath,lengthOfVoiceMessage);
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>targetId</code></td>
<td>User ID of the voice message receiver</td>
</tr>
<tr><td><code>conversationType</code></td>
<td><p>Type of the voice messages, including the following types:</p>
<div><ul>
<li>public static int NONE = 0: no message</li>
<li>public static int PRIVATE = 1: private message</li>
<li>public static int DISCUSSION = 2: discussion message</li>
<li>public static int GROUP = 3: group message</li>
<li>public static int CHATROOM = 4: chatroom message</li>
<li>public static int CUSTOMER_SERVICE = 5: customer service message</li>
<li>public static int SYSTEM = 6: system message</li>
<li>public static int APP_PUBLIC_SERVICE = 7: app public service message</li>
<li>public static int PUBLIC_SERVICE = 8: public service message</li>
<li>public static int PUSH_SERVICE = 9: push service</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>filePath</code></td>
<td>File path of the voice message</td>
</tr>
<tr><td><code>lengthOfVoiceMessage</code></td>
<td>Length of the voice message (Unit: second)</td>
</tr>
</tbody>
</table>
 
> -   Make sure you have connected the server successfully before you send a point-to-point message.
> -   You must keep the voice message under 128K , or you will fail when you send the message.
> -   The format of the voice file for iOS is `.wav`; while the format of the voice file for Android is `.amr`.


### Start Recording Audio (AgoraIMStartRecordAudio)

```
public void AgoraIMStartRecordAudio(){
    AgoraIMStartRecordAudio();
}
```

The method starts recording audio for voice message.

### Stop Recording Audio (AgoraIMStopRecordAudio)

```
public void AgoraIMStopRecordAudio(){
    AgoraIMStopRecordAudio();
}
```

This method stops recording audio.

### Get File Path of Recorded Audio \(AgoraIMGetRecordAudioFilePath\)

```
public string AgoraIMGetRecordAudioFilePath(){
    string result = AgoraIMGetRecordAudioFilePath();
    return result;
}
```

This method gets the file path of the recorded audio for voice message.

### Get Length of Recorded Audio (AgoraIMGetRecordAudioLength)

```
public int AgoraIMGetRecordAudioLength(){
    int result = AgoraIMGetRecordAudioLength();
    return result;
}
```

This method gets the length of the recorded audio.

> Please call the methods in the following sequence:
 
> 1.  `AgoraIMStartRecordAudio`;
> 2.  `AgoraIMStopRecordAudio` ;
> 3.  `AgoraIMGetRecordAudioFilePath` ;
> 4. `AgoraIMGetRecordAudioLength`.

If you do not follow the sequence, you may encounter errors.

#### Get History Messages (AgoraIMGetHistoryMessages)

```
public void AgoraIMGetHistoryMessages(int conversationType, string targetId, int latestMessageId, int count){
   agoraIMGetHistoryMessages(conversationType, targetId, latestMessageId, count);
}
```

This method gets the messages right before the specific history message. The type of the message is specified by `conversationType`. The target which the message locates in is specified by `targetId`.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>conversationType</code></td>
<td>Type of the messages. See the description about <em>conversationType</em> in <a href="#agoraimsendmessage">IMSendMessage</a> .</td>
</tr>
<tr><td><code>targetID</code></td>
<td>ID of the target where the message locates. The target depends on <code>conversationType</code>. It could be the ID of a private conversation, discussion, group or chatroom.</td>
</tr>
<tr><td><code>latestMessageId</code></td>
<td>ID of the latest message that you want.</td>
</tr>
<tr><td><code>count</code></td>
<td>The number of the messages that you want.</td>
</tr>
</tbody>
</table>



### Join IM Chatroom (AgoraIMJoinChatRoom)

```
public void AgoraIMJoinChatRoom(string chatRoomId, int defMessageCount){
    AgoraIMJoinChatRoom(chatRoomId, defMessageCount);
}
```

This method joins IM chatroom.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>chatRoomId</code></td>
<td>ID of the IM chatroom</td>
</tr>
<tr><td><code>defMessageCount</code></td>
<td>The default number of messages received earlier</td>
</tr>
</tbody>
</table>

> Make sure you have connected the server successfully before you send a voice message.

### Create Discussion (AgoraIMCreateDiscussion)

```
public void AgoraIMCreateDiscussion(string name, string userIdList){
    AgoraIMCreateDiscussion(name, userIdList);
}
```

This method creates a discussion.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>name</code></td>
<td>Name of the discussion</td>
</tr>
<tr><td><code>userIdList</code></td>
<td>List of the IDs of users in the discussion. Seperated by “,” .</td>
</tr>
</tbody>
</table>

> To call the discussion-related APIs, you need the ID of a specific discussion. Get the ID in `OnCreateDiscussionSuccessHandler` .

### Add Member to Discussion (AgoraIMAddMemberToDiscussion)

```
public void AgoraIMAddMemberToDiscussion(string name, string userIdList){
    AgoraIMAddMemberToDiscussion(name, userIdList);
}
```

This method adds a specific member to the existing discussion.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>name</code></td>
<td>Name of the discussion</td>
</tr>
<tr><td><code>userIdList</code></td>
<td>List of the IDs of the newly-added members. Seperated by “,” .</td>
</tr>
</tbody>
</table>



### Remove Member from Discussion (AgoraIMRemoveMemberFromDiscussion)

```
public void AgoraIMRemoveMemberFromDiscussion(string name, string userIdList){
    agoraIMRemoveMemberFromDiscussion(name, userIdList);
}
```

This method removes a specific member from the existing discussion.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>name</code></td>
<td>Name of the discussion</td>
</tr>
<tr><td><code>userIdList</code></td>
<td>List of the IDs of the removed members. Seperated by “,” .</td>
</tr>
</tbody>
</table>



### Quit Discussion (AgoraIMQuitDiscussion)

```
public void AgoraIMQuitDiscussion(string discussionId){
    agoraIMQuitDiscussion(discussionId);
}
```

This method quits the discussion.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>discussionId</code></td>
<td>ID of the discussion</td>
</tr>
</tbody>
</table>



### Set Discussion Name (AgoraIMSetDiscussionName)

```
public void AgoraIMSetDiscussionName(string discussionName,string discussionId){
    agoraIMSetDiscussionName(discussionName,discussionId);
}
```

This method sets the name of the existing discussion.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td><code>discussionName</code></td>
<td>Name of the discussion</td>
</tr>
<tr><td><code>discussionId</code></td>
<td>ID of the discussion. It is immutable.</td>
</tr>
</tbody>
</table>



### Translate IM Message (GetTranslate)

This method translates IM messages.

```
public void GetTranslate(string message, string original_language, string target_language){
    GetTranslate(message,original_language,target_language);
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
	<tr><td><code>message</code></td>
<td>The content of the message to translate</td>
</tr>
<tr><td><code>original_language</code></td>
<td>The abbreviation of the original language of the message</td>
</tr>
<tr><td><code>target_language</code></td>
<td>The abbreviation of the target language of the message</td>
</tr>
</tbody>
</table>



The mapping from the abbreviation to the name of the language:

> -   When you are unable to decide the original language of the message, set **original_language** to **auto**;
> -   Never set **target_language** to **auto**.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Abbreviation</strong></td>
<td><strong>The name of the language</strong></td>
</tr>
<tr><td>auto</td>
<td>Auto detection</td>
</tr>
<tr><td>Zh</td>
<td>Chinese</td>
</tr>
<tr><td>En</td>
<td>English</td>
</tr>
<tr><td>Yue</td>
<td>Cantonese</td>
</tr>
<tr><td>Wyw</td>
<td>Classical</td>
</tr>
<tr><td>Jp</td>
<td>Japanese</td>
</tr>
<tr><td>Kor</td>
<td>Korean</td>
</tr>
<tr><td>Fra</td>
<td>French</td>
</tr>
<tr><td>Spa</td>
<td>Spanish</td>
</tr>
<tr><td>Th</td>
<td>Thai</td>
</tr>
<tr><td>Ara</td>
<td>Arabic</td>
</tr>
<tr><td>Ru</td>
<td>Russian</td>
</tr>
<tr><td>Pt</td>
<td>Portuguese</td>
</tr>
<tr><td>De</td>
<td>German</td>
</tr>
<tr><td>It</td>
<td>Italian</td>
</tr>
<tr><td>El</td>
<td>Greek</td>
</tr>
<tr><td>Nl</td>
<td>Dutch</td>
</tr>
<tr><td>Pl</td>
<td>Polish</td>
</tr>
<tr><td>Bul</td>
<td>Bulgarian</td>
</tr>
<tr><td>Est</td>
<td>Estonian</td>
</tr>
<tr><td>Dan</td>
<td>Danish</td>
</tr>
<tr><td>Fin</td>
<td>Finnish</td>
</tr>
<tr><td>Cs</td>
<td>Czech</td>
</tr>
<tr><td>Rom</td>
<td>Romanian</td>
</tr>
<tr><td>Slo</td>
<td>Slovenian</td>
</tr>
<tr><td>Swe</td>
<td>Swedish</td>
</tr>
<tr><td>Hu</td>
<td>Hungarian</td>
</tr>
<tr><td>Cht</td>
<td>Traditional Chinese</td>
</tr>
<tr><td>Vie</td>
<td>Vietnamese</td>
</tr>
</tbody>
</table>



### Voice to Text (voiceToText)

This method transfers voice messages to text.

```
public void voiceToText(string filepath){
    voiceToText(filepath);
}
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>filepath</code></td>
<td>The filepath of the voice file</td>
</tr>
</tbody>
</table>


> Conditions must be met for the voice file:
 
> -   Format of the voice file must be pcm; （For iOS, the default format of the recording file is pcm, while for Android, the default format of the recording file is AMR. You must transform the Android recording file to pcm before you converting it to text by using ` imEngineManager.MAMRToPCM (filePath);`）
> -   The size of the pcm file cannot exceed 2M;
> -   The length of the pcm file cannot exceed 60 seconds.


### Disconnect IM Server (agoraIMDisconnect)

The method disconnects IM server. But you keep receiving new IM message notifications.

```
private static extern void agoraIMDisconnect();
```

### Log Out of IM Server (agoraIMLogout)

The method logs out of the IM server. And you’ll not receive any IM notification.

```
private static extern void agoraIMLogout();
```

<a id = "im-restful"></a>
## IM RESTful Interface Class

The class includes the methods for the application to get IM token and service for sensitive words.

### Rules to Call IM RESTful Interface

When the server calls the interface, the following 3 GET parameters will be added to the URL:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td><code>nonce</code></td>
<td>String</td>
<td>Random number, no length limit</td>
</tr>
<tr><td><code>timestamp</code></td>
<td>String</td>
<td>Time stamp. The time from Jan.1, 1970 to now. (Unit: Millisecond)</td>
</tr>
<tr><td><code>signature</code></td>
<td>String</td>
<td>Signature. The SHA 1 hash result of the string combining App Secret, Nonce and Timestamp in sequence. To get App Secret, please contact <a href="mailto:sales%40agora.io">sales-us@agora.io.</a></td>
</tr>
</tbody>
</table>


### IM Token

#### How to generate IM Token

When the client connects server using Agora Gaming SDK, you must send IM Token to the server for authentication. For the detailed process, refer to the following diagram:

![app_id_im_1.jpg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412170149)

In the subsequent login process, the App Server uses the previously saved IM Token. You do not need to request a new IM Token.

![app_id_im_2.jpg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412202895)

If your app is login-free, you can save the Token locally and log in directly.

![app_id_im_3.jpg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412220651)


If the Token fails, you must get the Token from the server again. The Token is permanetly valid by default.

<a id = "gettoken"></a>
#### Get IM Token (getToken)

-   Name: /user/getToken
-   URL: [http://api.cn.ronghub.com/user/getToken.format](http://api.cn.ronghub.com/user/getToken.format). `format` defines the format of the return value. It can be json or xml.
-   HTTP method: POST
-   Parameters: userId, name, portrait Uri

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>userId</td>
<td>String</td>
<td>(Required) User ID. No more than 64 bytes. It is the unique identifier of the user in the app and must not be duplicated in the same app. You can use user ID which the system provides. You can also define your own set of IDs.</td>
</tr>
<tr><td>name</td>
<td>String</td>
<td>(Required) User name. No more than 128 bits.</td>
</tr>
<tr><td>portraitUri</td>
<td>String</td>
<td>(Required) URI for the user’s portrait. No more than 1024 bytes.</td>
</tr>
</tbody>
</table>



-   Return value:

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>Return code. 200 : normal.</td>
</tr>
<tr><td>token</td>
<td>String</td>
<td>User Token. No more than 256 bytes.</td>
</tr>
<tr><td>userId</td>
<td>String</td>
<td>User ID. The same as the input user ID.</td>
</tr>
</tbody>
</table>

> A user can get multiple Tokens, all of which will be working without expiration.

-   Example:

  - HTTP request:

    ```
    POST /user/getToken.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    userId=jlk456j5&name=Ironman&portraitUri=http%3A%2F%2Fabc.com%2Fmyportrait.jpg
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200, "userId":"jlk456j5", "token":"sfd9823ihufi"}
    ```


### Service for sensitive words

There are two ways to filter sensitive words: mask or replace sensitive words in messages.

Currently, sensitive word filtering is supported in the built-in text message. To filter the messages sent through the Server API, you must contact [sales-us@agora.io](mailto:sales-us@agora.io). By default, you can set up to 50 sensitive words, and the setting takes effect within 2 hours.

#### Rules for sensitive word filtering

-   Supports auto smart filtering for simplified and traditional Chinese. Simplified and traditional Chinese share the same list of sensitive words.
-   Auto recognize the sensitive words split by punctuation.
-   Support full-width, half-width, large, and small-case matching for English and digital sensitive words.
-   Support intelligent identification and filtering for English words. For example, set the sensitive word “AV”. ‘av’ will not be filtered for a chat message containing “Java”.


#### Add Sensitive words (add)

-   Name: /sensitiveword/add
-   URL: http://api.cn.ronghub.com/user/add.format , `format` defines the format of the return value. It can be json or xml.
-   HTTP method: POST
-   Parameters: word, replaceWord

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>word</td>
<td>String</td>
<td>(Required) Sensitive word. No more than 32 bits. Can be Chinese characters, numbers, or English characters.</td>
</tr>
<tr><td>replaceWord</td>
<td>String</td>
<td><dl>
<dt>(Optional) Replacement for the sensitive word. No more than 32 bits.</dt>
<dd><ul>
<li>If <code>replaceWord</code> is not set, the messages containing <code>word</code> will be blocked.</li>
<li>If <code>replaceWord</code> is set, <code>word</code> conatined in the messages will be replaced to <code>replaceWord</code>. The filtering is supported for point-to-point messages, group messages and chatroom messages.</li>
</ul>
</dd>
</dl>
</td>
</tr>
</tbody>
</table>



-   Return value:

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>Return value. 200 : normal.</td>
</tr>
</tbody>
</table>

For more return values, see [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md) .

-   Example:

  - HTTP request:

    ```
    POST /conversation/notification/set.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    conversationType=1&requestId=b5NwvIrW8&targetId=UAhIaLkR0&isMuted=0
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200}
    ```


#### Delete a sensitive word (delete)

Remove a word from the list of sensitive words. The setting takes effect within 2 hours.

-   Method: /sensitiveword/delete
-   URL: http://api.cn.ronghub.com/sensitiveword/delete.format, `format` defines the format of the return value. It can be json or xml.
-   HTTP method: POST
-   Parameter: word

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>word</td>
<td>String</td>
<td>(Required) Sensitive word. No more than 32 bits.</td>
</tr>
</tbody>
</table>



-   Return value:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>Return value. 200 : normal.</td>
</tr>
</tbody>
</table>

For more return values, see [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md) .

-   Example

  - HTTP request:

    ```
    POST /sensitiveword/delete.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    word=money
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200}
    ```


#### Delete sensitive words (delete)

Delete words from the list of sensitive words. The setting takes effect within 2 hours.

-   Method: /sensitiveword/batch/delete
-   URL: http://api.cn.ronghub.com/sensitiveword/batch/delete.format,  `format` defines the format of the return value. It can be json or xml.
-   HTTP method: POST
-   Parameter: words

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>words</td>
<td>String[]</td>
<td>(Required) list of sensative words. The length is no more than 50.</td>
</tr>
</tbody>
</table>



-   Return value:

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>Return value. 200 : normal.</td>
</tr>
</tbody>
</table>

For more return values, see [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md) .

-   Example:

  - HTTP request:

    ```
    POST /sensitiveword/batch/delete.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    words=money&words=aaa&words=bbb
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200}
    ```


#### List Sensitive Words (list)

-   Method: /sensitiveword/list
-   URL: http://api.cn.ronghub.com/sensitiveword/list.format, `format` defines the format of the return value. It can be json or xml.
-   HTTP method: POST
-   Parameter: type

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>type</td>
<td>String</td>
<td><p>(Optional) Query type:</p>
<div><ul>
<li>0: Sensitive words to be replaced</li>
<li>1: (Default) sensitive words to be blocked</li>
<li>2: All sensitive words</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



-   Return value:

    <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Type</td>
<td>Description</td>
</tr>
<tr><td>code</td>
<td>Int</td>
<td>Return value. 200 : normal.</td>
</tr>
<tr><td>word</td>
<td>String</td>
<td>Sensitive word</td>
</tr>
<tr><td>replaceWord</td>
<td>String</td>
<td>Replacement for the sensitive word</td>
</tr>
</tbody>
</table>

For more return values, see [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md) .

-   Example to list the blocked sensitive words

  - HTTP request:

    ```
    POST /sensitiveword/list.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    type=1
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200,"words":[{"word":"screwed"},{"word":"bad"}]}
    ```

-   Example to list the replaced sensitive words

  - HTTP request:

    ```
    POST /sensitiveword/list.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    type=0
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200,"words":[{"type":"0","word":"lottery","replaceWord":"***"},{"type":"0","word":"qqq","replaceWord":"ddd"}]}
    ```

-   Example to list all the sensitive words

  - HTTP request:

    ```
    POST /sensitiveword/list.json HTTP/1.1
    Host: api.cn.ronghub.com
    App-Key: uwd1c0sxdlx2
    Nonce: 14314
    Timestamp: 1408710653491
    Signature: 45beb7cc7307889a8e711219a47b7cf6a5b000e8
    Content-Type: application/x-www-form-urlencoded
    
    type=2
    ```

  - HTTP response:

    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    
    {"code":200,"words":[{"type":"0","word":"lottery","replaceWord":"***"},{"type":"1","word":"qqq","replaceWord":""}]}
    ```

<a id = "im-callback"></a>
## IM Callback Class

The class includes the callbacks of IM Interface Class.

### Token error callback (OnTokenIncorrectHandler)

```
public delegate void OnTokenIncorrectHandler ();
public OnTokenIncorrectHandler OnTokenIncorrect;
```

Token error triggers the callback.

### Connect Server Success Callback (OnConnectSuccessHandler)

```
public delegate void OnConnectSuccessHandler (string s);
public OnConnectSuccessHandler OnConnectSuccess;
```

Success to connect server triggers the callback.

### Connect Server Error Callback (OnConnectErrorHandler)

```
public delegate void OnConnectErrorHander (string s);
public OnConnectErrorHander OnConnectError;
```

Failure to connect server triggers the callback.

### Sending Message Callback (OnSendMessageAttachedHandler)

```
public delegate void OnSendMessageAttachedHandler (Message message);
public OnSendMessageAttachedHandler OnSendMessageAttached;
```

Sending message triggers the callback.

> You cannot decide from the callback if sending the message succeeds or not.

### Send Message Success Callback (OnSendMessageSuccessHandler)

```
public delegate void OnSendMessageSuccessHandler (Message message);
public OnSendMessageSuccessHandler OnSendMessageSuccess;
```

Success to send the message triggers the callback.

### Send Message Error Callback (OnSendMessageErrorHandler)

```
public delegate void OnSendMessageErrorHandler (string errCode);
public OnSendMessageErrorHandler OnSendMessageError;
```

Failure to send the message triggers the callback.

### Receiving message Callback (OnMessageReceivedHandler)

```
public delegate void OnMessageReceivedHandler (Message message);
public OnMessageReceivedHandler OnMessageReceived;
```

Receiving the message triggers the callback.


> Receiving a point-to-point message, chatroom message, or discussion message triggers the callback. To decide the type of the received message, refer to the objectName field:
 
> -   Voice message: “RC:VcMsg”;
> -   Text message: “RC:TxtMsg”.


### Join Chatroom Success Callback (OnJoinChatRoomSuccessHandler)

```
public delegate void OnJoinChatRoomSuccessHandler ();
public OnJoinChatRoomSuccessHandler OnJoinChatRoomSuccess;
```

Success to join the chatroom triggers the callback.

### Join Chatroom Error Callback (OnJoinChatRoomErrorHandler)

```
public delegate void OnJoinChatRoomErrorHandler (string errCode);
public OnJoinChatRoomErrorHandler OnJoinChatRoomError;
```

Failure to join the chatroom triggers the callback.

### Get History Message Success Callback (OnGetHistoryMessageSuccessHandler)

```
public delegate void OnGetHistoryMessageSuccessHandler (Message[] messageList);
public OnGetHistoryMessageSuccessHandler OnGetHistoryMessageSuccess;
```

Success to get history message triggers the callback.

### Get History Message Error Callback (OnGetHistoryMessageErrorHandler)

```
public delegate void OnGetHistoryMessageErrorHandler (string errCode);
public OnGetHistoryMessageErrorHandler OnGetHistoryMessageError;
```

Failure to get history message triggers the callback.

### Clear Message Success Callback (OnClearMessageSuccessHandler)

```
public delegate void OnClearMessageSuccessHandler (bool success);
public OnClearMessageSuccessHandler OnClearMessageSuccess;
```

Success to clear the message triggers the callback.

### Clear Message Error Callback (OnClearMessageErrorHandler)

```
public delegate void OnClearMessageErrorHandler (string errCode);
public OnClearMessageErrorHandler OnClearMessageError;
```

Failure to clear message triggers the callback.

### Send Voice Message Callback (OnSendVoiceMessageAttachedHandler)

```
public delegate void OnSendVoiceMessageAttachedHandler (Message message);
public OnSendVoiceMessageAttachedHandler OnSendVoiceMessageAttached;
```

Sending voice message triggers the callback.

> You cannot decide from the callback if sending the voice message succeeds or not.

### Send Voice Message Success Callback (OnSendVoiceMessageSuccessHandler)

```
public delegate void OnSendVoiceMessageSuccessHandler ();
public OnSendVoiceMessageSuccessHandler OnSendVoiceMessageSuccess;
```

Success to send voice message triggers the callback.

### Send Voice Message Error Callback (OnSendVoiceMessageErrorHandler)

```
public delegate void OnSendVoiceMessageErrorHandler (string errMessage);
public OnSendVoiceMessageErrorHandler OnSendVoiceMessageError;
```

Failure to send voice message triggers the callback.

### Join Existing Chatroom Success Callback (OnJoinExistChatRoomSuccessHandler)

```
public delegate void OnJoinExistChatRoomSuccessHandler ();
public OnJoinExistChatRoomSuccessHandler OnJoinExistChatRoomSuccess;
```

Join the chatroom. If the chatroom exists, and joining chatroom succeeds, the callback is triggered.

### Join Existing Chatroom Error Callback (OnJoinExistChatRoomErrorHandler)

```
public delegate void OnJoinExistChatRoomErrorHandler (string errMessage);
public OnJoinExistChatRoomErrorHandler OnJoinExistChatRoomError;
```

Join the chatroom. If the chatroom exists, and joining chatroom fails, the callback is triggered.

### Quit Chatroom Success Callback (OnQuitChatRoomSuccessHandler)

```
public delegate void OnQuitChatRoomSuccessHandler ();
public OnQuitChatRoomSuccessHandler OnQuitChatRoomSuccess;
```

Success to quit the chatroom triggers the callback.

### Quit Chatroom Error Callback (OnQuitChatRoomErrorHandler)

```
public delegate void OnQuitChatRoomErrorHandler (string errMessage);
public OnQuitChatRoomErrorHandler OnQuitChatRoomError;
```

Failure the quit the chatroom triggers the callback.

### Create Discussion Success Callback (OnCreateDiscussionSuccessHandler)

```
public delegate void OnCreateDiscussionSuccessHandler (string s);
public OnCreateDiscussionSuccessHandler OnCreateDiscussionSuccess;
```

Success to create the discussion triggers the callback.

### Create Discussion Error Callback (OnCreateDiscussionErrorHandler)

```
public delegate void OnCreateDiscussionErrorHandler (string s);
public OnCreateDiscussionErrorHandler OnCreateDiscussionError;
```

Failure to create the discussion triggers the callback.

### Add Member to Discussion Success Callback (OnAddMemberToDiscussionSuccessHandler)

```
public delegate void OnAddMemberToDiscussionSuccessHandler ();
public OnAddMemberToDiscussionSuccessHandler OnAddMemberToDiscussionSuccess;
```

Success to add member to discussion triggers the callback.

### Add Member to Discussion Error Callback (OnAddMemberToDiscussionErrorHandler)

```
public delegate void OnAddMemberToDiscussionErrorHandler (string s);
public OnAddMemberToDiscussionErrorHandler OnAddMemberToDiscussionError;
```

Failure to add member to the discussion triggers the callback.

### Remove Member from Discussion Success Callback (OnRemoveMemberFromDiscussionSuccessHandler)

```
public delegate void OnRemoveMemberFromDiscussionSuccessHandler ();
public OnRemoveMemberFromDiscussionSuccessHandler OnRemoveMemberFromDiscussionSuccess;
```

Success to remove a member from the discussion triggers the callback.

### Remove Member from Discussion Error Callback (OnRemoveMemberFromDiscussionErrorHandler)

```
public delegate void OnRemoveMemberFromDiscussionErrorHandler (string s);
public OnRemoveMemberFromDiscussionErrorHandler OnRemoveMemberFromDiscussionError;
```

Failure to remove a member from the discussion triggers the callback.

### Quit Discussion Success Callback (OnQuitDiscussionSuccessHandler)

```
public delegate void OnQuitDiscussionSuccessHandler ();
public OnQuitDiscussionSuccessHandler OnQuitDiscussionSuccess;
```

Success to quit the discussion triggers the callback.

### Quit Discussion Error Callback (OnQuitDiscussionErrorHandler)

```
public delegate void OnQuitDiscussionErrorHandler (string s);
public OnQuitDiscussionErrorHandler OnQuitDiscussionError;
```

Failure to quit the discussion triggers the callback.

### Get Discussion-Related Information Success Callback (OnGetDiscussionSuccessHandler)

```
public delegate void OnGetDiscussionSuccessHandler (Discussion s);
public OnGetDiscussionSuccessHandler OnGetDiscussionSuccess;
```

Success to get discussion-related information triggers the callback.

### Get Discussion-Related Information Error Callback (OnGetDiscussionErrorHandler)

```
public delegate void OnGetDiscussionErrorHandler (string s);
public OnGetDiscussionErrorHandler OnGetDiscussionError;
```

Failure to get discussion-related information triggers the callback.

### Set Discussion Name Success Callback (OnSetDiscussionNameSuccessHandler)

```
public delegate void OnSetDiscussionNameSuccessHandler ();
public OnSetDiscussionNameSuccessHandler OnSetDiscussionNameSuccess;
```

Success to set the discussion name triggers the callback.

### Set Discussion Name Error Callback (OnSetDiscussionNameErrorHandler)

```
public delegate void OnSetDiscussionNameErrorHandler (string s);
public OnSetDiscussionNameErrorHandler OnSetDiscussionNameError;
```

Failure to set the discussion name triggers the callback.

### Set Discussion Invite Status Success Callback (OnSetDiscussionInviteStatusSuccessHandler)

```
public delegate void OnSetDiscussionInviteStatusSuccessHandler ();
public OnSetDiscussionInviteStatusSuccessHandler OnSetDiscussionInviteStatusSuccess;
```

Success to set the invite status of the discussion triggers the callback.

### Set Discussion Invite Status Error Callback (OnSetDiscussionInviteStatusErrorHandler)

```
public delegate void OnSetDiscussionInviteStatusErrorHandler ();
public OnSetDiscussionInviteStatusErrorHandler OnSetDiscussionInviteStatusError;
```

Failure to set the invite status of the discussion triggers the callback.

### Translate IM message Success Callback (OnTranslateCallBackSuccess)

```
public delegate void OnTranslateCallBackSuccessHandler ();
public OnTranslateCallBackSuccessHandler OnTranslateCallBackSuccess;
```

Success to translate the IM message triggers the callback.

### Translate IM message Error Callback (OnTranslateCallBackError)

```
public delegate void OnTranslateCallBackErrorHandler ();
public OnTranslateCallBackErrorHandler OnTranslateCallBackError;
```

Failure to translate the IM message triggers the callback.

### Voice to Text Success Callback (OnVoiceToTextSuccesss)

```
public delegate void OnVoiceToTextSuccesssHandler ();
public OnVoiceToTextSuccessHandler OnVoiceToTextSuccess;
```

Success to convert voice to text triggers the callback.

### Voice to Text Error Callback (OnVoiceToTextError)

```
public delegate void OnVoiceToTextErrorHandler ();
public OnVoiceToTextErrorHandler OnVoiceToTextError;
```

Failure to convert voice to text triggers the callback.

## Error Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).


