
---
title: Interactive Gaming API
description: 
platform: Objective-C
updatedAt: Thu Jan 10 2019 23:15:34 GMT+0000 (UTC)
---
# Interactive Gaming API
The Interactive Gaming API is composed of **Objective-C Interface** and **C++ Interface**, 

This page provides the Objective-C Interface, with which you can integrate the voice and video function into your app on the iOS platform.


## Basic Methods (AgoraRtcEngineKit)

`AgoraRtcEngineKit` is the basic interface class of Agora Native SDK. Creating an AgoraRtcEngineKit object and then calling the methods of this object enables the use of Agora Native SDK’s communication functionality.

### Initialize (sharedEngineWithAppId)

```
+ (instancetype _Nonnull)sharedEngineWithAppId:(NSString * _Nonnull)appId
                                      delegate:(id<AgoraRtcEngineDelegate> _Nullable)delegate;
```

This method initializes the `AgoraRtcEngineKit` class as a singleton instance. Call this method to initialize service before using `AgoraRtcEngineKit`.

The SDK uses `Delegate` to inform the application on the engine runtime events. All methods defined in Delegate are optional implementation methods.

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
<tr><td>appId</td>
<td>The App ID issued to the application developers by Agora. Apply for a new one from Agora if the key is missing in your kit.</td>
</tr>
<tr><td>delegate</td>
<td> </td>
</tr>
</tbody>
</table>

### Implement a Live Voice Broadcast

#### Set the Channel Profile (setChannelProfile)

```
- (int)setChannelProfile:(AgoraChannelProfile)profile;
```

This method configures the channel profile. The Agora RtcEngine needs to know what scenario the application is in to apply different methods for optimization.

The Agora Native SDK supports the following profiles:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Profile</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Communication</td>
<td>Default setting. This is used in one-on-one calls, where all users in the channel can talk.</td>
</tr>
<tr><td>Live Broadcast</td>
<td>Live Broadcast. Host and audience roles that can be set by calling setClientRole.  The host sends and receives voice, while the audience receives voice only with the sending function disabled.</td>
</tr>
<tr><td>Gaming</td>
<td>Gaming Mode. Any user in the channel can talk freely. This mode uses the codec with low-power consumption and low bitrate by default.</td>
</tr>
</tbody>
</table>

> -   Only one profile can be used at the same time in the same channel. If you want to switch to another profile, use `destroy` to destroy the current Engine and create a new one using `sharedEngineWithAppId` before calling this method to set the new channel profile.
> -   Make sure that different App IDs are used for different channel profiles.
> -   This method must be called and configured before a user joining a channel, because the channel profile cannot be configured when the channel is in use.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>profile</td>
<td><p>The channel profile. Choose one of the following:</p>
<div><ul>
<li>AgoraChannelProfileCommunication = 0: Communication (default)</li>
<li>AgoraChannelProfileLiveBroadcasting = 1: Live Broadcast</li>
<li>AgoraChannelProfileGame = 2: Gaming</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Set the User Role (setClientRole)

```
- (int)setClientRole:(AgoraClientRole)role;
```

In a live broadcasr channel, this method allows you:
* Set the user role as a host or an audience (default) before joining a channel;
* Switch the user role after joining a channel.

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
<tr><td>role</td>
<td>The user role in a live broadcast:</td>
</tr>
<tr><td><ul>
<li>AgoraClientRoleBroadcaster = 1; Host</li>
<li>AgoraClientRoleAudience = 2; Audience (default)</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: The Method is called successfully</li>
<li>&lt;0: Failed to call the method</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Enable the Audio Mode (enableAudio)

```
- (int)enableAudio;
```

This method enables the audio mode. The application can call this method either before joining a channel. This function is enabled by default.

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


> This method sets to enable the internal engine, and still works after `leaveChannel` is called.

#### Disables/Re-enables the Local Audio Function (enableLocalAudio)

```
- (int)enableLocalAudio:(BOOL)enabled;
```

When an App joins a channel, the audio function is enabled by default. This method disables or re-enables the local audio function, that is, to stop or restart local audio capturing and handling.
This method does not affect receiving or playing the remote audio streams, and is applicable to scenarios where the user wants to receive the remote audio streams without sending any audio stream to other users in the channel.
The `didMicrophoneEnabled` callback function will be triggered once the local audio function is disabled or re-enabled.

> - Call this method after `joinChannelByToken`.
> - This method is different from `muteLocalAudioStream`:
>   * `enableLocalAudio`: Disables/Re-enables the local audio capturing and handling.
>   * `muteLocalAudioStream`: Stops/Continues sending the local audio streams.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>enabled</td>
<td>
<ul>
<li>true: Re-enable the local audio function, that is, to start local audio capturing and handling.</li>
<li>false: Disable the local audio function, that is, to stop local audio capturing and handling.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success</li>
<li>&lt;0: Failure</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Disable the Audio Mode (disableAudio)

```
- (int)disableAudio;
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


> This method sets to disable the internal engine, and still works after `leaveChannel` is called.


#### Join a Channel (joinChannelByToken)

```
- (int)joinChannelByToken:(NSString * _Nullable)token
                channelId:(NSString * _Nonnull)channelId
                     info:(NSString * _Nullable)info
                      uid:(NSUInteger)uid
              joinSuccess:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))joinSuccessBlock;
```

This method allows a user to join a channel. Users in the same channel can talk to each other; and multiple users in the same channel can start a group chat. Users using different App IDs cannot call each other. Once in a call, the user must call the `leaveChannel` method to exit the current call before entering another channel. This method is called asynchronously; therefore, it can be called in the main user interface thread.

The SDK uses the OS X’s `AVAudioSession` shared object for audio recording and playing, so using this object may affect the SDK’s audio functions.

Once this method is called successfully, the SDK will trigger the callback. If both `joinChannelSuccessBlock` and `didJoinChannel` are implemented, the priority of `joinChannelSuccessBlock` is higher than `didJoinChannel`, thus `didJoinChannel` will be ignored. If you want to use `didJoinChannel`, set `joinChannelSuccessBlock` as nil.


> A channel does not accept duplicate UIDs, such as two users with the same UID. If your app supports logging in a user from different devices at the same time, ensure that you use different UIDs. For example, if you already used the same UID, make the UIDs different by adding the respective device ID to the UID. This is not applicable if your app does not support a user logging in from different devices at the same time. In this case, when you log in a new device, you will be logged out from the other device.

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
<tr><td>token</td>
<td><p>A Token generated by the application.</p>
<p>This parameter is optional if the user uses a static App ID. In this case, pass NIL as the parameter value.</p>
<p>If the user uses a Token, Agora issues an additional App Certificate to the application developers. The developers can then generate a user key using the algorithm and App Certificate provided by Agora for user authentication on the server.</p>
<p>In most circumstances, the static App ID will suffice. For added security, use a Token.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>channelName</td>
<td><p>A string providing the unique channel name for the AgoraRTC session. The length must be within 64 bytes.</p>
<p>The following is the supported scope:</p>
<ul>
<li>The 26 lowercase English letters from a to z</li>
<li>The 26 uppercase English letters from A to Z</li>
<li>The 10 numbers from 0 to 9</li>
<li>The space</li>
<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td>info</td>
<td>(Optional) Any additional information the developer wants to add. <sup>[1]</sup> It can be set as a NIL Sting or channel related information. Other users in the channel will not receive this information.</td>
</tr>
<tr><td>uid</td>
<td>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If not assigned (or set to 0), the SDK assigns one and returns it in the joinSuccessBlock callback. The app must record and maintain the returned value, as the SDK does not maintain it.</td>
</tr>
<tr><td>joinSuccessBlock</td>
<td>Callback on user successfully joined the channel.</td>
</tr>
</tbody>
</table>



> [1] For example, when a host wants to customize the resolution and bitrate for a live broadcast channel with CDN Live enabled, they can include them in this parameter in JSON format. For example, \{“owner”:true, …, “width”:300, “height”:400, “bitrate”:100\}. Only when neither `width`, `height`, and `bitrate` is 0 can the bitrate and resolution settings take effect.

> When joining a channel, the SDK calls `setCategory AVAudioSessionCategoryPlayAndRecord` to set `AVAudioSession` to `PlayAndRecord` mode. The application should not set it to any other mode. When setting to this mode, the sound being played (for example a ringtone) will be interrupted.

#### Leave a Channel (leaveChannel)

```
- (int)leaveChannel:(void(^ _Nullable)(AgoraChannelStats * _Nonnull stat))leaveChannelBlock;
```

This method allows a user to leave a channel, such as hanging up or exiting a call. After joining a channel, the user must call the leaveChannel method to end the call before joining another one.

The `leaveChannel` method releases all resources related to the call. The `leaveChannel` method is called asynchronously, and the user has not actually left the channel when the call returns. When the user leaves the channel, the SDK triggers the `didLeaveChannelWithstats` callback.

> If you call `destroy()` immediately after `leaveChannel`, the `leaveChannel` process will be interrupted, and the SDK will not trigger the `didLeaveChannelWithstats` callback.

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
<tr><td>leaveChannelBlock</td>
<td>Callback on user successfully left the channel.</td>
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


#### Set the Local Voice Pitch (setLocalVoicePitch)

```
- (int) setLocalVoicePitch:(double) pitch;
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
<tr><td>pitch</td>
<td>Voice frequency, in the range of [0.5, 2.0]. The default value is 1.0.</td>
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

#### Sets the Voice Position of the Remote user (setRemoteVoicePosition)

```
- (int)setRemoteVoicePosition:(int)uid pan:(double)pan gain:(double)gain;
```

This method sets the voice position of the remote user.

> This API is valid only for earphones, and is invalid when the speakerphone is enabled.

<table>
<colgroup>
<col>
</col>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td><p>User ID of the remote user</p>
</td></tr>
<tr><td>pan</td>
<td><p>Sets whether to change the spatial position of the audio effect. The range is [-1, 1]:</p>
<ul>
<li>0: The audio effect shows right ahead.</li>
<li>-1: The audio effect shows on the left.</li>
<li>1: The audio effect shows on the right.</li>
</ul>
</td>
</tr>
<tr><td>gain</td>
<td><p>Sets whether to change the volume of a single audio effect. The range is [0.0, 100.0]. The default value is 100.0. The smaller the number is, the lower the volume of the audio effect.</p>
</td></tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success</li>
<li>&lt; 0: Failure</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Sets to Voice-only Mode (setVoiceOnlyMode)

```
- (int)setVoiceOnlyMode:(bool)enable;
```

This method sets to voice-only mode (transmit the audio stream only), and the other streams will be ignored; for example the sound of the keyboard strokes.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>enable</td>
<td>
<ul>
<li>true: Enable voice-only mode.</li>
<li>false: Disable voice-only mode.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success</li>
<li>&lt; 0: Failure</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Set the Local Voice Equalization (setLocalVoiceEqualizationOfBandFrequency)

```
- (int)setLocalVoiceEqualizationOfBandFrequency:(AgoraAudioEqualizationBandFrequency)bandFrequency withGain:(NSInterger)gain;
```

This method sets the local voice equalization.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>bandFrequency</td>
<td>The band frequency ranging from 0 to 9; representing the respective 10-band center frequencies of the voice effects, including 31, 62, 125, 500, 1k, 2k, 4k, 8k, and 16k Hz.</td>
</tr>
<tr><td>gain</td>
<td>Gain of each band in dB; ranging from -15 to 15.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Set the Local Voice Reverberation (setLocalVoiceReverbOfType)

```
- (int)setLocalVoiceReverbOfType:(AgoraAudioReverbType)reverbType withValue:(NSInterger)value;
```

This method sets the local voice reverberation.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>reverbType</td>
<td>The reverberation types. This method contains five reverberation types. For details, see the description of each value:</td>
</tr>
<tr><td>value</td>
<td><ul>
<li>AgoraAudioReverbDryLevel = 0, (dB, [-20,10]), level of the dry signal</li>
<li>AgoraAudioReverbWetLevel = 1, (dB, [-20,10]), level of the early reflection signal (wet signal)</li>
<li>AgoraAudioReverbRoomSize = 2, ([0, 100]), room size of the reflection</li>
<li>AgoraAudioReverbWetDelay = 3, (ms, [0, 200]), length of the initial latency of the wet signal in ms</li>
<li>AgoraAudioReverbStrength = 4, ([0, 100]), length of the late reverberation</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>


### Implement a Live Video Broadcast

#### Enable the Video Mode (enableVideo)

```
- (int)enableVideo;
```

This method enables the video mode. The application can call this method either before entering a channel or during a call. If it is called before entering a channel, the service starts in the video mode; if it is called during a call, it switches from the audio to video mode. To disable the video mode, call the `disableVideo` method.

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


> This method sets to enable the internal engine, and still works after `leaveChannel` is called.

#### Disable the Video (disableVideo)

```
- (int)disableVideo;
```

This method disables the video mode. The application can call this method either before entering a channel or during a call. If it is called before entering a channel, the service starts in the audio mode; If it is called during a call, it switches from the video to audio mode. To enable the video mode, call the `enableVideo` method.

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


> This method sets to disable the internal engine, and still works after `leaveChannel` is called.

#### Enable the Local Video (enableLocalVideo)

```
- (int)enableLocalVideo:(BOOL)enabled;
```

This method enables/disables the local video, which is only applicable to the scenario when the user only wants to watch the remote video without sending any video stream to the other user.

-   Call this method after `enableVideo`, otherwise this method may not work properly.

-   After `enableVideo` is called, the local video will be enabled by default. This method is useds to enable and disable the local video while the remote video remains unaffected.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>enabled</td>
<td><p>Sets to enable the local video:</p>
<div><ul>
<li>YES: Enable the local video (default)</li>
<li>NO: Disable the local video. Once the local video is disabled, the remote users can no longer receive the video stream of this user, while this user can still receive the video streams of other remote users. When set to NO, this method does not require a local camera.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return value</td>
<td><ul>
<li>0: Method call succeeded</li>
<li>&lt; 0: Method call failed</li>
</ul>
</td>
</tr>
</tbody>
</table>

> This method sets to enable/disable the internal engine, and still works after `leaveChannel` is called.


#### Set the Video Profile (setVideoProfile)


> This method has been deprecated. If you hope to set the video encoder profile, Agora recommends using `setVideoEncoderConfiguration`.

```
- (int)setVideoProfile:(AgoraVideoProfile)profile
    swapWidthAndHeight:(BOOL)swapWidthAndHeight;
```

This method sets the video encoding profile. If the user does not need to set the video encoding profile after joining the channel, Agora recommends calling this method before `enableVideo` to minimize the time delay in receiving the first video frame.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>profile</td>
<td>The video profile. See the Video Profile Definition Table for details.</td>
</tr>
<tr><td>swapWidthAndHeight</td>
<td><p>The width and height of the output video is consistent with that of the video profile you set. This parameter sets whether to swap the width and height of the stream:</p>
<ul>
<li>True: Swap the width and height.</li>
<li>False: (Default) Do not swap the width and height.</li>
</ul>
<p>Since the landscape or portrait mode of the output video can be decided directly by the video profile, Agora recommends setting this parameter as default.</p>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>

**Video Profile Definition Table**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Video Profile</td>
<td>Resolution (Width x Height)</td>
<td>Frame Rate (fps)</td>
<td>Bitrate</td>
</tr>
<tr><td>120P</td>
<td>160 x 120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_3</td>
<td>120 x 120</td>
<td>15</td>
<td>50</td>
</tr>
<tr><td>180P</td>
<td>320 x 180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_3</td>
<td>180 x 180</td>
<td>15</td>
<td>100</td>
</tr>
<tr><td>180P_4</td>
<td>240 x 180</td>
<td>15</td>
<td>120</td>
</tr>
<tr><td>240P</td>
<td>320 x 240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_3</td>
<td>240 x 240</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>240P_4</td>
<td>424 x 240</td>
<td>15</td>
<td>220</td>
</tr>
<tr><td>360P</td>
<td>640 x 360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_3</td>
<td>360 x 360</td>
<td>15</td>
<td>260</td>
</tr>
<tr><td>360P_4</td>
<td>640 x 360</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>360P_6</td>
<td>360 x 360</td>
<td>30</td>
<td>400</td>
</tr>
<tr><td>360P_7</td>
<td>480 x 360</td>
<td>15</td>
<td>320</td>
</tr>
<tr><td>360P_8</td>
<td>480 x 360</td>
<td>30</td>
<td>490</td>
</tr>
<tr><td>360P_9</td>
<td>640 x 360</td>
<td>15</td>
<td>600</td>
</tr>
<tr><td>360P_10</td>
<td>640 x 360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>360P_11</td>
<td>640 x 360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>480P</td>
<td>640 x 480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_3</td>
<td>480 x 480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>640 x 480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>480 x 480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>848 x 480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>848 x 480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>480P_10</td>
<td>640 x 480</td>
<td>10</td>
<td>400</td>
</tr>
<tr><td>720P</td>
<td>1280 x 720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_3</td>
<td>1280 x 720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>960 x 720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>960 x 720</td>
<td>30</td>
<td>1380</td>
</tr>
</tbody>
</table>


#### Set the Video Resolution (setVideoResolution)


> This method has been deprecated. If you hope to set the video encoder profile, Agora recommends using `setVideoEncoderConfiguration`.

```
- (int)setVideoResolution: (CGSize)size andFrameRate: (NSInteger)frameRate bitrate: (NSInteger) bitrate;
```

If the video profiles listed in the table do not meet your needs, use this method to manually set the width, height, frame rate, and bitrate of the video. If the user does not need to set the video encoding profile after joining the channel, Agora recommends calling this method before `enableVideo` to minimize the time delay in receiving the first video frame.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>size</td>
<td>Size of the video that you want to set. The highest value is 1280 x 720.</td>
</tr>
<tr><td>frameRate</td>
<td>Frame rate of the video that you want to set. The highest value is 30. You can set it to 5, 10, 15, 24, 30, and so on.</td>
</tr>
<tr><td>bitrate</td>
<td><p>Bitrate of the video that you want to set. You need to manually work out the bitrate according to the width, height, and frame rate. With the same width and height, the bitrate varies with the change of the frame rate:</p>
<ul>
<li>If the frame rate is 5 fps, divide the recommended bitrate in the table by 2.</li>
<li>If the frame rate is 15 fps, use the recommended bitrate in the table.</li>
<li>If the frame rate is 30 fps, multiply the recommended bitrate in the table by 1.5.</li>
<li>Calculate your bitrate with the ratio if you choose other frame rates.</li>
</ul>
<p>If the bitrate you set is beyond the proper range, the SDK will automatically adjust it to a value within the range.</p>
</td>
</tr>
</tbody>
</table>



#### Set the Local Video View (setupLocalVideo)

```
- (int)setupLocalVideo:(AgoraRtcVideoCanvas * _Nullable)local;
```

This method configures the video display settings on local machine. The application calls this method to bind with the video window (view) of local video streams and configure the video display settings. Call this method after initialization to configure the local video display settings before entering a channel. After leaving the channel, the bind is still valid, which means the window still displays. To unbind the view, set the view value to NIL when calling `setupLocalVideo`.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>local</td>
<td><p>Video display settings. Class VideoCanvas:</p>
<ul>
<li>view: Video display window (view).</li>
<li>renderMode: Video display mode.<ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.</li>
</ul>
</li>
<li>Return value: The local user ID as set in the joinChannel method.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Set the Remote Video View (setupRemoteVideo)

```
- (int)setupRemoteVideo:(AgoraRtcVideoCanvas * _Nonnull)remote;
```

This method binds the remote user to the video display window, that is, sets the view for the user of the specified uid. Usually the application should specify the uid of the remote video in the method call before the user enters a channel. If the remote uid is unknown to the application, set it later when the application receives the onUserJoined event.

If the Video Recording function is enabled, the Video Recording Service joins the channel as a dumb client, which means other clients will also receive the `didJoinedOfUid` event. Your application should not bind it with the view, because it does not send any video stream. If your application cannot recognize the dumb client, bind it with the view when the firstRemoteVideoDecodedOfUid event is triggered. To unbind the user with the view, set the view to null. After the user has left the channel, the SDK unbinds the remote user.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>remote</td>
<td><p>Video display settings. Class VideoCanvas:</p>
<ul>
<li><p>view: Video display window (view).</p>
</li>
<li><p>renderMode: Video display mode.</p>
<div><ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.</li>
</ul>
</div>
</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Enable Dual-stream Mode (enableDualStreamMode)

```
- (int)enableDualStreamMode:(BOOL)enabled;
```

This method sets the stream mode to single- (default) or dual-stream mode. After it is enabled, the receiver can choose to receive the high stream, that is, high-resolution high-bitrate video stream, or low stream, that is, low-resolution low-bitrate video stream.

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
<tr><td>enabled</td>
<td><p>The mode is in single stream or dual stream:</p>
<ul>
<li>True: Dual stream</li>
<li>False: Single stream</li>
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



#### Set the Remote Video-stream Type (setRemoteVideoStream)

```
- (int)setRemoteVideoStream:(NSUInteger)uid
                       type:(AgoraVideoStreamType)streamType;
```

This method specifies the video-stream type of the remote user to be received by the local user when the remote user sends dual streams.

-   If dual-stream mode is enabled by calling `enableDualStreamMode`, you will receive the high-video stream by default. This method allows the application to adjust the corresponding video-stream type according to the size of the video windows to save the bandwidth and calculation resources.

-   If dual-stream mode is not enabled, you will receive the high-video stream by default.


The result after calling this method will be returned in `didApiCallExecute`. The Agora SDK receives the high-video stream by default to save the bandwidth. If needed, users can switch to the low-video stream using this method.

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
<td>User ID</td>
</tr>
<tr><td>streamType</td>
<td><p>Set the video-stream size.</p>
<p>The following lists the video-stream types:</p>
<ul>
<li>AgoraVideoStreamTypeHigh(0): High-video stream</li>
<li>AgoraVideoStreamTypeLow(1): Low-video stream</li>
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


#### Set the Default Remote Video Stream Type (setRemoteDefaultVideoStreamType)

```
- (int)setRemoteDefaultVideoStreamType:(AgoraVideoStreamType)streamType;
```

This method sets the default remote video stream type to high or low.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>streamType</td>
<td><p>The default remote video stream:</p>
<ul>
<li>AgoraVideoStreamTypeHigh = 0: The high video stream</li>
<li>AgoraVideoStreamTypeLow = 1: The low video stream</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Set High-quality Video Preferences (setVideoQualityParameters)

```
- (int)setVideoQualityParameters:(BOOL)preferFrameRateOverImageQuality;
```

This method allows users to set video preferences.

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
<tr><td>preferFrameRateOverImageQuality</td>
<td><p>The video preference to be set:</p>
<ul>
<li>True: Frame rate over image quality</li>
<li>False: Image quality over frame rate (default)</li>
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


#### Start Video Preview (startPreview)

```
- (int)startPreview;
```

This method starts the local video preview before joining the channel. Before using this method, you need to:

-   Call `setupLocalVideo` to set up the local preview window and configure the attributes

-   Call `enableVideo` to enable video


> Once the local video preview is enabled, the preview will not be shut down if you call `leaveChannel` to leave the channel. To shut down the preview, call `stopPreview`.

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



#### Stop Video Preview (stopPreview)

```
- (int)stopPreview;
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


#### Set the Local Video Display Mode (setLocalRenderMode)

```
- (int)setLocalRenderMode:(AgoraVideoRenderMode) mode;
```

This method configures the local video display mode. The application can call this method multiple times to change the display mode.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>mode</td>
<td><p>Configures the video display mode:</p>
<div><ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Set the Remote Video Display Mode (setRemoteRenderMode)

```
- (int)setRemoteRenderMode:(NSUInteger)uid
                      mode:(AgoraVideoRenderMode) mode;
```

This method configures the remote video display mode. The application can call this method multiple times to change the display mode.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>uid</td>
<td>The user ID of the user whose video streams are received.</td>
</tr>
<tr><td>mode</td>
<td><p>Configures the video display mode:</p>
<div><ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio will be filled with black.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Set the Local Video Mirror Mode (setLocalVideoMirrorMode)

```
- (int)setLocalVideoMirrorMode:(AgoraVideoMirrorMode) mode;
```

This method sets the local video mirror mode. Use this method before `startPreview`, or it does not take effect until you re-enable `startPreview`.

```
  typedef NS_ENUM(NSUInteger, AgoraVideoMirrorMode) {
    AgoraVideoMirrorModeAuto = 0,
    AgoraVideoMirrorModeEnabled = 1,
    AgoraVideoMirrorModeDisabled = 2,
};
```


### Manage the Camera

#### Switch Between Front and Back Cameras (switchCamera)

```
- (int)switchCamera;
```

This method switches between front and back cameras.

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



#### Check whether Camera Zoom is Supported (isCameraZoomSupported)

```
- (BOOL)isCameraZoomSupported;
```

It returns:

-   True: The device supports the camera zoom function

-   False: The device does not support the camera zoom function

#### Check whether Camera Flash is Supported (isCameraTorchSupported)

```
- (BOOL)isCameraTorchSupported;
```

It returns:

-   True: The device supports the camera flash function

-   False: The device does not support the camera flash function


> The App generally enables the front camera by default. If your front camera does not support front camera torch, this method will return false. If you want to check if the rear camera torch is supported, call `switchCamera` before using this method.

#### Check whether Manual Focus is Supported (isCameraFocusPositionInPreviewSupported)

```
- (BOOL)isCameraFocusPositionInPreviewSupported;
```

It returns:

-   True: The device supports the manual focus function

-   False: The device does not support the manual focus function


#### Check whether Autofocus is Supported (isCameraAutoFocusFaceModeSupported)

```
- (BOOL)isCameraAutoFocusFaceModeSupported;
```

It returns:

-   True: The device supports the autofocus function;

-   False: The device does not support the autofocus function;


#### Set the Camera Zoom Ratio (setCameraZoomFactor)

```
- (CGFloat)setCameraZoomFactor:(CGFloat)zoomFactor;
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
<tr><td>factor</td>
<td>The camera zoom factor ranging from 1.0 to the max zoom</td>
</tr>
</tbody>
</table>


#### Set the Manual Focus Position (setCameraFocusPositionInPreview)

```
- (BOOL)setCameraFocusPositionInPreview:(CGPoint)position;
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
<tr><td>positionX</td>
<td>Horizontal coordinate of the touch point in the view</td>
</tr>
<tr><td>positionY</td>
<td>Vertical coordinate of the touch point in the view</td>
</tr>
</tbody>
</table>



#### Enable the Camera Flash (setCameraTorchOn)

```
- (BOOL)setCameraTorchOn:(BOOL)isOn;
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
<tr><td>isOn</td>
<td>Whether to enable the camera flash function: True/False</td>
</tr>
</tbody>
</table>



#### Enable the Camera Autofocus (setCameraAutoFocusFaceModeEnabled)

```
- (BOOL)setCameraAutoFocusFaceModeEnabled:(BOOL)enable;
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
<tr><td>enabled</td>
<td>Whether to enable the camera autofocus function: True/False</td>
</tr>
</tbody>
</table>





### Enable Web SDK Interoperability (enableWebSdkInteroperability)

```
- (int)enableWebSdkInteroperability:(BOOL)enabled;
```

This method enables interoperability with the Agora Web SDK.

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
<tr><td>enabled</td>
<td><p>Whether to enable the interoperability with the Agora Web SDK is enabled:</p>
<ul>
<li>True: Enable the interoperability</li>
<li>False: Disable the interoperability</li>
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


### Set the Audio Route

#### Set the Default Audio Route (setDefaultAudioRouteToSpeakerPhone)

```
- (int)setDefaultAudioRouteToSpeakerphone:(BOOL)defaultToSpeaker;
```

This method sets whether the received audio is routed to the earpiece or speakerphone.


> -   Call this method only if you want to change the default settings.
> -   This method only works in audio mode.
> -   Call this method before `joinChannel`.


If the user does not call this method, the audio is routed to the earpiece by default.

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
<tr><td>defaultToSpeaker</td>
<td>YES: Audio is routed to the speakerphone</td>
</tr>
<tr><td>NO: Audio is routed to the earpiece</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



#### Enable the Speakerphone (setEnableSpeakerphone)

```
- (int)setEnableSpeakerphone:(BOOL)enableSpeaker;
```

This method enables the audio routing to the speaker. Ensure that the joinChannelByKey method has been executed successfully before calling this method.

The SDK calls setCategory AVAudioSessionCategoryPlayAndRecord with options to configure the headset/speaker, so any sound will be interrupted when calling this method.

After this method is called, the SDK returns the didAudioRouteChanged callback, indicating that the audio routing has changed.

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
<tr><td>enableSpeaker</td>
<td>Yes: Switches to the speaker.</td>
</tr>
<tr><td>No: Switches to the headset.</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



#### Check Whether the Speakerphone is Enabled (isSpeakerphoneEnabled)

```
- (BOOL)isSpeakerphoneEnabled;
```

This method checks whether the speakerphone is enabled.

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
<li>Yes: Yes, enabled.</li>
<li>No: No, not enabled.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Enable In-ear Monitoring (enableInEarMonitoring)

```
- (int)enableInEarMonitoring:(BOOL)enabled;
```

This method enables or disables the in-ear monitoring function.

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
<tr><td>enabled</td>
<td><ul>
<li>YES: Enable in-ear monitoring</li>
<li>NO: Disable in-ear monitoring (default)</li>
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



#### Set the Audio Session Operation Restriction (setAudioSessionOperationRestriction)

```
- (void)setAudioSessionOperationRestriction:(AgoraAudioSessionOperationRestriction)restriction;
```

The SDK and the app can both configure the audio session by default. The app may occassionally use other applications or third-party components to manipulate the audio session and restrict the SDK from doing so. This API allows the app to restrict the SDK’s manipulation of the audio session.


> -   You can call this method before or after joining the channel.
> -   This method restricts the SDK’s manipulation of the audio session. Any operation to the audio session relies solely on the app, other applications, or third-party components.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>restriction</td>
<td><p>The operational restriction of the SDK on the audio session. It comes as a bit mask.</p>
<div><ul>
<li>AgoraAudioSessionOperationRestrictionNone = 0: No restrictions. The SDK has full control over the audio session operation.</li>
<li>AgoraAudioSessionOperationRestrictionSetCategory = 1: The SDK cannot change the category of the audio session.</li>
<li>AgoraAudioSessionOperationRestrictionConfigurationSession = 1 &lt;&lt; 1: The SDK cannot change the category, mode or categoryOptions of the audio session.</li>
<li>AgoraAudioSessionOperationRestrictionDeactivateSession = 1 &lt;&lt; 2: The SDK cannot deactivate the session when it leaves the channel.</li>
<li>AgoraAudioSessionOperationRestrictionAll = 1 &lt;&lt; 7: Restricts the SDK from manipulating the audio session.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


### Set the Audio Volume

#### Enable the Audio Volume Indication (enableAudioVolumeIndication)

```
- (int)enableAudioVolumeIndication:(NSInteger)interval
                            smooth:(NSInteger)smooth;
```

This method allows the SDK to regularly report to the application on which user is speaking and the volume of the speaker. Once the method is enabled, the SDK returns the volume indications at the set time internal in the `reportAudioVolumeIndicationOfSpeakers` and `audioVolumeIndicationBlock` callback, regardless of whether anyone is speaking in the channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>interval</td>
<td><p>Time interval between two consecutive volume indications:</p>
<div><ul>
<li>&lt;= 0: Disables the volume indication</li>
<li>&gt; 0: The time interval between two consecutive volume indications in milliseconds. Agora recommends setting it to more than 200 ms. Do not set it lower than 10 ms, or the <code>reportAudioVolumeIndicationOfSpeakers</code> callback will not be triggered.</li>
</ul>
</div>
</td>
</tr>
<tr><td>smooth</td>
<td>The Smoothing factor that determines the sensitivity of this method. The range is [0-10] and Agora recommends setting it 3. The bigger the number, the more sensitive it is.</td>
</tr>
<tr><td>Return values</td>
<td><ul>
<li>0: Method call succeeded</li>
<li>&lt; 0: Method call failed</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Set the Volume of In-ear Monitor (setInEarMonitoringVolume)

```
- (int)setInEarMonitoringVolume:(NSInteger)volume;
```

This method sets the volume of the in-ear monitor.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>volume</td>
<td>Volume of the in-ear monitor, ranging from 0 to 100. The default value is 100.</td>
</tr>
<tr><td>Return values</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Get the Audio Effect Volume (getEffectsVolume)

```
- (double)getEffectsVolume;
```

This method gets the volume of the audio effects from 0.0 to 100.0.

#### Set the Audio Effect Volume (setEffectsVolume)

```
- (int)setEffectsVolume:(double)volume;
```

This method sets the volume of the audio effects.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>volume</td>
<td>Value ranging from 0.0 to 100.0. 100.0 is the default value.</td>
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



#### Adjust the Audio Effect Volume in Real Time (setVolumeOfEffect)

```
- (int)setVolumeOfEffect:(int)soundId
              withVolume:(double)volume;
```

This method adjusts the volume of the specified audio effect in real time.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>volume</td>
<td>Value ranging from 0.0 to 100.0. 100.0 is the default value.</td>
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



#### Play the Audio Effect (playEffect)

```
- (int)playEffect:(int)soundId
         filePath:(NSString * _Nullable)filePath
        loopCount:(int)loopCount
            pitch:(double)pitch
              pan:(double)pan
             gain:(double)gain
          publish:(BOOL)publish;
```

This method plays the specified audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
<td>ID of the specified audio effect. Each audio effect has a unique ID <sup>[4]</sup></td>
</tr>
<tr><td>filePath</td>
<td>The absolute path of the audio effect file</td>
</tr>
<tr><td>loopCount</td>
<td><p>Set the number of times looping the audio effect</p>
<ul>
<li>0: Play the audio effect once</li>
<li>1: Play the audio effect twice</li>
<li>-1: Play the audio effect in a loop indefinitely, until <code>stopEffect</code> or <code>stopAllEffects</code> is called</li>
</ul>
</td>
</tr>
<tr><td>pitch</td>
<td>Set whether to change the pitch of the audio effect. The range is [0.5, 2].
The default value is 1, which means no change to the pitch. The smaller the value, the lower the pitch.</td>
</tr>
<tr><td>pan</td>
<td><p>Spatial position of the local audio effect. The range is [-1.0, 1.0]</p>
<ul>
<li>0.0: The audio effect shows ahead</li>
<li>1.0: The audio effect shows on the right</li>
<li>-1.0: The audio effect shows on the left</li>
</ul>
</td>
</tr>
<tr><td>gain</td>
<td>Volume of the audio effect. The range is [0.0, 100,0]
The default value is 100.0. The smaller the value, the lower the volume of the audio effect</td>
</tr>
<tr><td>publish</td>
<td><p>Set whether to publish the specified audio effect to the remote stream:</p>
<ul>
<li>true: The audio effect, played locally, is published to the Agora Cloud and the remote users can hear it</li>
<li>false: The audio effect, played locally, is not published to the Agora Cloud and the remote users cannot hear it</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded</li>
<li>&lt; 0: Method call failed</li>
</ul>
</td>
</tr>
</tbody>
</table>



> [4] If you preloaded the audio effect into the memory through `preloadEffect`, ensure that the `soundID` value is set to the same value as in `preloadEffect`.

> In version v2.1.x, the following method, which is not recommended by Agora, is used.

```
- (int) playEffect: (int) soundId
          filePath: (NSString*) filePath
              loop: (BOOL) loop
             pitch: (double) pitch
               pan: (double) pan
              gain: (double) gain;
```

#### Stop Playing an Audio Effect (stopEffect)

```
- (int)stopEffect:(int)soundId;
```

This method stops playing a specified audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
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



#### Stop Playing all Audio Effects (stopAllEffects)

```
- (int)stopAllEffects;
```

This method stops playing all the audio effects.

#### Preload an Audio Effect (preloadEffect)

```
- (int)preloadEffect:(int)soundId
            filePath:(NSString * _Nullable) filePath;
```

This method preloads a specific audio effect file (compressed audio file) to the memory.


> To ensure smooth communication, pay attention to the size of the audio effect file. Agora recommends using this method to preload the audio effect before calling `joinChannelByToken`.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>filePath</td>
<td>Absolute path of the audio effect file.</td>
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



#### Release an Audio Effect (unloadEffect)

```
- (int)unloadEffect:(int)soundId;
```

This method releases a specific preloaded audio effect from the memory.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
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



#### Pause an Audio Effect (pauseEffect)

```
- (int)pauseEffect:(int)soundId;
```

This method pauses a specific audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
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



#### Pause all Audio Effects (pauseAllEffects)

```
- (int)pauseAllEffects;
```

This method pauses all the audio effects.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
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



#### Resume an Audio Effect (resumeEffect)

```
- (int)resumeEffect:(int)soundId;
```

This method resumes playing a specific audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
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



#### Resume all Audio Effects (resumeAllEffects)

```
- (int) esumeAllEffects;
```

This method resumes all the audio effects.


### Mute the Audio and Video Stream

#### Mute the Local Audio Stream (muteLocalAudioStream)

```
- (int)muteLocalAudioStream:(BOOL)mute;
```

This method mutes/unmutes the local audio.

> This method takes effect only when the user is in the channel. After the user leaves the channel, all the mute states are reset.

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
<tr><td>mute</td>
<td><ul>
<li>True: Mutes the local audio.</li>
<li>False: Unmutes the local audio.</li>
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



#### Mute all Remote Audio Streams (muteAllRemoteAudioStreams)

```
- (int)muteAllRemoteAudioStreams:(BOOL)mute;
```

This method mutes/unmutes all remote users’ audio streams.

> This method takes effect only when the user is in the channel. After the user leaves the channel, all the mute states are reset.

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
<tr><td>muted</td>
<td><ul>
<li>True: Stops playing all received audio streams.</li>
<li>False: Resumes playing all received audio streams.</li>
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



#### Mute a Specified Remote Audio Stream (muteRemoteAudioStream)

```
- (int)muteRemoteAudioStream:(NSUInteger)uid muted:(BOOL)mute;
```

Mute/unmute a specified remote user’s audio stream.

> When set to **True**, this method stops playing audio streams without affecting the audio stream receiving process.
> This method takes effect only when the user is in the channel. After the user leaves the channel, all the mute states are reset.

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
<td>User ID whose audio streams the user intends to mute.</td>
</tr>
<tr><td>mute</td>
<td><ul>
<li>True: Stops playing a specified user’s audio streams.</li>
<li>False: Allows playing a specified user’s audio streams.</li>
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


#### Mute the Local Video Stream (muteLocalVideoStream)

```
- (int)muteLocalVideoStream:(BOOL)muted;
```

This method enables/disables sending a local video stream to the network.


> When set to YES, this method does not disable the camera and thus does not affect the retrieval of local video streams. This method responds faster than enableLocalVideo (false).
> This method takes effect only when the user is in the channel. After the user leaves the channel, all the mute states are reset.

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
<tr><td>mute</td>
<td><ul>
<li>Yes: Stops sending the local video stream to the network.</li>
<li>No: Allows sending the local video stream to the network.</li>
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



#### Mute all Remote Video Streams (muteAllRemoteVideoStreams)

```
- (int)muteAllRemoteVideoStreams:(BOOL)mute;
```

This method enables/disables playing all remote callers’ video streams.


> When set to Yes, this method stops playing video streams without affecting the video stream receiving process.
> This method takes effect only when the user is in the channel. After the user leaves the channel, all the mute states are reset.

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
<tr><td>mute</td>
<td><ul>
<li>Yes: Stops playing all received video streams.</li>
<li>No: Allows playing all received video streams.</li>
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



#### Mute a Specified Remote Video Stream (muteRemoteVideoStream)

```
- (int)muteRemoteVideoStream:(NSUInteger)uid
                        mute:(BOOL)mute;
```

This method pauses/resumes receiving a specified user’s video stream.

> When set to Yes, this method stops playing video streams without affecting the video stream receiving process.
> This method takes effect only when the user is in the channel. After the user leaves the channel, all the mute states are reset.

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
<td>User ID of the specified user.</td>
</tr>
<tr><td>mute</td>
<td><ul>
<li>Yes: Stops playing a specified user’s video stream.</li>
<li>No: Allows playing a specified user’s video stream.</li>
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



### Audio Mixing

#### Start Audio Mixing (startAudioMixing)

```
- (int)startAudioMixing:(NSString *  _Nonnull)filePath
               loopback:(BOOL)loopback
                replace:(BOOL)replace
                  cycle:(NSInteger)cycle;
```

This method mixes the specified local audio file with the audio stream from the microphone; or, it replaces the microphone’s audio stream with the specified local audio file. You can choose whether the other user can hear the local audio playback and specify the number of loop playbacks. This API also supports online music playback.


> -   To use the startAudioMixing API, ensure the iOS device version is 8.0 or later.
> -   Call this API when you are in a channel, otherwise it may cause issues.


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
<tr><td>filePath</td>
<td>Name and path of the local audio file to be mixed.</td>
</tr>
<tr><td>Supported audio formats: mp3, aac, m4a, 3gp, and wav.</td>
</tr>
<tr><td>loopback</td>
<td>True:  Only the local user can hear the remix or the replaced audio stream.</td>
</tr>
<tr><td>False:  Both users can hear the remix or the replaced audio stream.</td>
</tr>
<tr><td>replace</td>
<td>True:  The content of the local audio file replaces the audio stream from the microphone.</td>
</tr>
<tr><td>False:  Local audio file mixed with the audio stream from the microphone.</td>
</tr>
<tr><td>cycle</td>
<td>Number of loop playbacks:</td>
</tr>
<tr><td>Positive integer: Number of loop playbacks</td>
</tr>
<tr><td>-1：Infinite loop</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



#### Stop Audio Mixing (stopAudioMixing)

```
- (int)stopAudioMixing;
```

This method stops audio mixing. Call this API when you are in a channel.

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



#### Pause Audio Mixing (pauseAudioMixing)

```
- (int)pauseAudioMixing;
```

This method pauses audio mixing. Call this API when you are in a channel.

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



#### Resume Audio Mixing (resumeAudioMixing)

```
- (int)resumeAudioMixing;
```

This method resumes audio mixing. Call this API when you are in a channel.

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



#### Adjust the Audio Mixing Volume (adjustAudioMixingVolume)

```
- (int)adjustAudioMixingVolume:(NSInteger)volume;
```

This method adjusts the volume during audio mixing. Call this API when you are in a channel.

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
<tr><td>volume</td>
<td>Volume ranging from 0 to 100. By default, 100 is the original volume.</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



#### Get the Audio Mixing Duration (getAudioMixingDuration)

```
- (int)getAudioMixingDuration;
```

This method gets the duration (ms) of audio mixing. Call this API when you are in a channel.

#### Get Current Audio Position (getAudioMixingCurrentPosition)

```
- (int)getAudioMixingCurrentPosition;
```

This method gets the playback position (ms) of the audio. Call this API when you are in a channel.

#### Drag the Audio Progress Bar (setAudioMixingPosition)

```
- (int)setAudioMixingPosition:(NSInteger)pos;
```

This method drags the playback progress bar of the audio mixing file to where you want to play instead of playing it from the beginning.

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
<tr><td>pos</td>
<td>Integer. The position of the audio mixing file (ms).</td>
</tr>
</tbody>
</table>



### Recording

#### Start an Audio Recording (startAudioRecording)

```
- (int)startAudioRecording:(NSString * _Nonnull)filePath
                   quality:(AgoraAudioRecordingQuality)quality;
```

This method starts an audio recording. The SDK allows recording during a call, which supports either one of the following two formats:

-   *.wav*: Large file size with high sound fidelity **OR**

-   *.aac*: Small file size with low sound fidelity


Ensure that the saving directory in the application exists and is writable. This method is usually called after the `joinChannelByToken` method. The recording automatically stops when the `leaveChannel` method is called.

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
<tr><td>filePath</td>
<td>Full file path of the recording file. The string of the file name is in UTF-8 code.</td>
</tr>
<tr><td>quality</td>
<td><p>Audio recording quality:</p>
<ul>
<li><code>AgoraRtc_AudioRecordingQuality_Low = 0</code>: Low quality, file size around 1.2 MB after 10 minutes of recording</li>
<li><code>AgoraRtc_AudioRecordingQuality_Low = 1</code>: Medium quality, file size around 2 MB after 10 minutes of recording</li>
<li><code>AgoraRtc_AudioRecordingQuality_Low = 2</code>: High quality, file size around 3.75 MB after 10 minutes of recording</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stop an Audio Recording (stopAudioRecording)

```
- (int)stopAudioRecording;
```

This method stops recording on the client.


> Call this method before calling `leaveChannel`, otherwise the recording automatically stops when the `leaveChannel` method is called.

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



#### Adjust the Recording Volume (adjustRecordingSignalVolume)

```
- (int)adjustRecordingSignalVolume:(NSInteger)volume;
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
<tr><td>volume</td>
<td><p>Recording volume, ranges from 0 to 400:</p>
<ul>
<li>0: Mute</li>
<li>100: Original volume</li>
<li>400: (Maximum) Four times the original volume with signal clipping protection</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



#### Adjust the Playback Volume (adjustPlaybackSignalVolume)

```
- (int)adjustPlaybackSignalVolume:(NSInteger)volume;
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
<tr><td>volume</td>
<td><p>Playback volume, ranges from 0 to 400:</p>
<ul>
<li>0: Mute</li>
<li>100: Original volume</li>
<li>400: (Maximum) Four times the original volume with signal clipping protection</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>


#### Enable Built-in Encryption and Set the Encryption Secret (setEncryptionSecret)

```
- (int)setEncryptionSecret:(NSString * _Nullable)secret;
```

Use `setEncryptionSecret` to specify an encryption password to enable built-in encryption before joining a channel. All users in a channel must set the same encryption password. The encryption password is automatically cleared once a user has left the channel. If the encryption password is not specified or set to empty, the encryption function will be disabled.


> Do not use this function together with CDN.

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
<tr><td>secret</td>
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



### Encryption

#### Set the Built-in Encryption Mode (setEncryptionMode)

```
- (int)setEncryptionMode:(NSString * _Nullable)encryptionMode;
```

The Agora Native SDK supports built-in encryption, which is in AES-128-XTS mode by default. If you want to use other modes, call this API to set the encryption mode.

All users in the same channel must use the same encryption mode and password. Refer to information related to the AES encryption algorithm on the differences between encryption modes.

Call setEncryptionSecret to enable the built-in encryption function before calling this API.


> Do not use this function together with CDN.

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
<tr><td>encryptionMode</td>
<td><p>Encryption mode. The following modes are currently supported:</p>
<ul>
<li>“aes-128-xts”:128-bit AES encryption, XTS mode</li>
<li>“aes-128-ecb”:128-bit AES encryption, ECB mode</li>
<li>“aes-256-xts”: 256-bit AES encryption, XTS mode</li>
<li>“”: When it is set to NUL string, the encryption is in “aes-128-xts” by default.</li>
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



### Establish a Data Stream

#### Create a Data Stream (createDataStream)

```
- (int)createDataStream:(NSInteger * _Nonnull)streamId
               reliable:(BOOL)reliable
                ordered:(BOOL)ordered;
```

This method creates a data stream. Each user can only have up to five data channels at the same time.


> Set `reliable` and `ordered` both as True or both as False. Do not set one as *True* and the other as *False*.

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
<tr><td>reliable</td>
<td><ul>
<li>True: The recipients will receive data from the sender within 5 seconds. If the recipient does not receive the sent data within 5 seconds, the data channel will report an error to the application.</li>
<li>False: The recipients may not receive any data, and the SDK will not report any error upon data missing.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>ordered</td>
<td><ul>
<li>True: The recipients will receive data in the order of the sender.</li>
<li>False: The recipients will not receive data in the order of the sender.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>&lt;0: Returns an error code when it fails to create the data stream. <sup>[5]</sup></li>
<li>&gt;0: Returns the Stream ID when the data stream is created.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



> [5] The error code is related to the positive integer displayed in [Error Codes and Warning Codes](../../en/API%20Reference/the_error_native.md) for example, if it returns -2, then it indicates 2: ERR_INVALID_ARGUMENT in [Error Codes and Warning Codes](../../en/API%20Reference/the_error_native.md).

#### Send a Data Stream (sendStreamMessage)

```
- (int)sendStreamMessage:(NSInteger)streamId
                    data:(NSData * _Nonnull)data;
```

This method sends data stream messages to all users in a channel. Up to 30 packets can be sent per second in a channel with each packet having a maximum size of 1 kB. The API controls the data channel transfer rate. Each client can send up to 6 kB of data per second. Each user can have up to five data channels simultaneously.

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
<tr><td>streamId</td>
<td>Stream ID.</td>
</tr>
<tr><td>message</td>
<td>Data to be sent</td>
</tr>
<tr><td>Return Value</td>
<td>When it fails to send the message, the following error code will be returned: ERR_SIZE_TOO_LARGE/ERR_TOO_OFTEN/ERR_BITRATE_LIMIT</td>
</tr>
</tbody>
</table>



### Test and Detection

#### Start an Audio Call Test (startEchoTest)

```
- (int)startEchoTest:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))successBlock;
```

This method launches an audio call test to determine whether the audio devices (for example, headset and speaker) and the network connection are working properly. In the test, the user first speaks, and the recording is played back in 10 seconds. If the user can hear the recording in 10 seconds, it indicates that the audio devices and network connection work properly.


> -   After calling the startEchoTest method, call stopEchoTest to end the test; otherwise, the application cannot run the next echo test, nor can it call the `joinChannel` method to start a new call.
> -   This method is applicable only when the user role is broadcaster. If you switch the channel mode from communication to live broadcast, ensure that `setClientRole` is called to change the user role from audience(default in the live broadcast mode) to broadcaster before using this method.


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
<tr><td>successBlock</td>
<td>Callback on successfully starting the echo test. See  joinSuccessBlock in joinChannelByToken for a description of the callback parameters.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<li>ERR_REFUSED (-5): Failed to launch the echo test, for example, initialization failed.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Stop an Audio Call Test (stopEchoTest)

```
- (int)stopEchoTest;
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
<li>ERR_REFUSED(-5): Failed to stop the echo test. It could be that the echo test is not running.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Enable the Network Test (enableLastmileTest)

```
- (int)enableLastmileTest;
```

This method tests the quality of the user’s network connection and is disabled by default.

This method is mainly used in two scenarios:

-   Before users join a channel, this method can be called to check the uplink network quality.
-   Before the audience in a channel switch to a host role, this method can be called to check the uplink network quality.


In both scenarios, calling this method consumes extra network traffic, which affects the communication quality.

Call disableLastmileTest to disable it immediately once the users have received the onLastmileQuality callback before they join the channel or switch the user role.

> For current hosts, do not call this method.

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



#### Disable the Network Test (disableLastmileTest)

```
- (int)disableLastmileTest;
```

This method disables the network connection quality test.

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



### Feedback

#### Retrieve the Current Call ID (getCallId)

```
- (NSString * _Nullable)getCallId;
```

When a user joins a channel on a client, a CallId is generated to identify the call from the client. Some methods such as rate and complain need to be called after the call ends in order to submit feedback to the SDK. These methods require assigned values of the `CallId` parameters. To use these feedback methods, call the getCallId method to retrieve the `CallId` during the call, and then pass the value as an argument in the feedback methods after the call ends.

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



#### Rate the Call (rate)

```
- (int)rate:(NSString * _Nonnull)callId
     rating:(NSInteger)rating
description:(NSString * _Nullable)description;
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
<tr><td>callId</td>
<td>Call ID retrieved from the getCallId method.</td>
</tr>
<tr><td>rating</td>
<td>Rating for the call between 1 (lowest score) to 10 (highest score).</td>
</tr>
<tr><td>description</td>
<td>A given description for the call with a length less than 800 bytes.</td>
</tr>
<tr><td>This parameter is optional.</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
<tr><td>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid, for example, callId invalid.</td>
</tr>
<tr><td>ERR_NOT_READY (-3): The SDK status is incorrect, for example, initialization failed.</td>
</tr>
</tbody>
</table>



#### Complain about the Call Quality (complain)

```
- (int)complain:(NSString * _Nonnull)callId
    description:(NSString * _Nullable)description;
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
<tr><td>callId</td>
<td>Call ID retrieved from the getCallId method.</td>
</tr>
<tr><td>description</td>
<td>A given description of the call with a length less than 800 bytes.</td>
</tr>
<tr><td>This parameter is optional.</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
<tr><td>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid, for example, callId invalid.</td>
</tr>
<tr><td>ERR_NOT_READY (-3): The SDK status is incorrect, for example, initialization failed.</td>
</tr>
</tbody>
</table>



### Added Functions

#### Renew Token (renewToken)

```
- (int)renewToken:(NSString * _Nonnull)token;
```

This method updates the Token.

The key expires after a certain period of time once the Token schema is enabled when:

-   The `rtcEngine:didOccurError:` callback reports the ERR_TOKEN_EXPIRED(109) error, or
-   The `rtcEngineRequestToken:` callback reports the ERR_TOKEN_EXPIRED(109) error, or


The application should retrieve a new key and then call this method to renew it. Failure to do so will result in the SDK disconnecting from the server.

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
<tr><td>token</td>
<td>Token to be renewed.</td>
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



#### Specify a Log File (setLogFile)

```
- (int)setLogFile:(NSString * _Nonnull)filePath;
```

This method specifies an SDK output log file. The log file records all the log data of the SDK’s operation. Ensure that the directory to save the log file exists and is writable.

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
<tr><td>filePath</td>
<td>File path of the log file. The string of the log file is in UTF-8 code.</td>
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


> The default log file location is at: sdcard/<appname\>/agorasdk.log, where appname is the name of the application.

#### Set the Log Filter (setLogFilter)

```
- (int)setLogFilter:(NSUInteger)filter;
```

This method sets the output log level of the SDK. You can use either one or a combination of the filters.

The log level follows the sequence of *Off*, *Critical*, *Error*, *Warning*, *Info*, and *Debug*. Choose a level, and you can see logs that precede that level.

For example, if you set the log level as *Warning*, then you can see logs in levels *Critical*, *Error* and *Wwarning*.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>filter</td>
<td><p>Set the levels of the filters:</p>
<div><ul>
<li>AgoraLogFilterOff = 0: Output no log.</li>
<li>AgoraLogFilterDebug = 0x080f: Output all the API logs.</li>
<li>AgoraLogFilterInfo = 0x000f: Output logs of the CRITICAL, ERROR, WARNING and INFO level.</li>
<li>AgoraLogFilterWarning = 0x000e: Output logs of the CRITICAL, ERROR and WARNING level.</li>
<li>AgoraLogFilterError = 0x000c: Output logs of the CRITICAL and ERROR level.</li>
<li>AgoraLogFilterCritical = 0x0008: Output logs of the CRITICAL level.</li>
</ul>
</div>
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



### Destroy the Engine Instance (destroy)

```
+ (void)destroy;
```

This method releases all the resources used by the Agora SDK. This is useful for applications that occasionally make voice or video calls, to free up resources for other operations when not making calls.

Once the application has called destroy() to destroy the created RtcEngine instance, no other methods in the SDK can be used and no callbacks occur. To start communications again, initialize `sharedEngineWithappId` to establish a new `AgoraRtcEngineKit` instance.


> -   Use this method in the sub thread.
> -   This method is called synchronously. The result returns after the IRtcEngine object resources are released. The app should not call this interface in the callback generated by the SDK, otherwise the SDK must wait for the callback to return before it can reclaim the related object resources, causing a deadlock.


### Get the SDK Version (getSdkVersion)

```
+ (NSString * _Nonnull)getSdkVersion;
```

This method returns the string of the SDK version number.


<a id = "rtcenginedelegate"></a>
## Delegate Methods (AgoraRtcEngineDelegate)

The SDK uses Delegate methods `AgoraRtcEngineDelegate` to report runtime events to the application. From v1.1, some Block callbacks in the SDK are replaced with Delegate methods. The old Block callbacks are therefore being deprecated, but can still be used in the current version. However, we recommend replacing them with Delegate methods. The SDK calls the Block method if a callback is defined in both Block and Delegate.

#### Warning Occurred Callback (didOccurWarning)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOccurWarning:(AgoraRtcErrorCode)warningCode;
```

This callback method indicates that some warning occurred during SDK runtime. In most cases, the application can ignore the warnings reported by the SDK because the SDK can usually fix the issue and resume running.

For instance, the SDK may report an `AgoraRtc_Error_OpenChannelTimeout(106)` warning upon disconnection with the server and attempts to reconnect.

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
<tr><td>warningCode</td>
<td>The warning code.</td>
</tr>
</tbody>
</table>



#### Error Occurred Callback (didOccurError)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOccurError:(AgoraRtcErrorCode)errorCode;
```

This callback indicates that a network or media error occurred during SDK runtime. In most cases, reporting an error means that the SDK cannot fix the issue and resume running, and therefore requires actions from the application or simply informs the user on the issue. For instance, the SDK reports an `AgoraRtc_Error_StartCall(1002)` error when failing to initialize a call. In this case, the application informs the user that the call initialization failed and calls the leaveChannel method to exit the channel.

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
<tr><td>Engine</td>
<td>The AgoraRtcEngineKit Object.</td>
</tr>
<tr><td>errorCode</td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### API Call Executed Callback (didApiCallExecute)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didApiCallExecute:(NSInteger)error api:(NSString * _Nonnull)api result:(NSString * _Nonnull)result;
```

This callback is triggered when the API has been executed.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>error</td>
<td>The error code that the SDK returns when the method call fails. If the SDK returns o, then the method has been called successfully.</td>
</tr>
<tr><td>api</td>
<td>The API that the SDK executes</td>
</tr>
<tr><td>result</td>
<td>The result of calling the API</td>
</tr>
</tbody>
</table>



#### Join Channel Callback (didJoinChannel)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didJoinChannel:(NSString * _Nonnull)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
```

Same as joinSuccessBlock in the joinChannelByToken API. This callback indicates that the user has successfully joined the specified channel.

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
<tr><td>channel</td>
<td>Channel name.</td>
</tr>
<tr><td>uid</td>
<td><p>User ID.</p>
<p>If the uid is specified in the joinChannelByToken method, returns the specified ID; if not, returns an ID that is automatically allocated by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannelByToken until this event occurs.</td>
</tr>
</tbody>
</table>



#### Rejoin Channel (didRejoinChannel)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didRejoinChannel:(NSString * _Nonnull)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
```

If the client loses connection with the server because of network problems, the SDK automatically attempts to reconnect, and then triggers this callback method upon reconnection, indicating that the user has rejoined the channel with the assigned channel ID and user ID.

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
<tr><td>channel</td>
<td>Channel name.</td>
</tr>
<tr><td>uid</td>
<td>User ID.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannelByToken until this event occurs.</td>
</tr>
</tbody>
</table>



#### User Left Channel Callback (didLeaveChannelWithStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didLeaveChannelWithStats:(AgoraChannelStats * _Nonnull)stats;
```

When the user executes leaveChannel() successfully, SDK will trigger this callback. Same as `leaveChannelBlock`.

**Definition of AgoraChannelStats**

```
__attribute__((visibility("default"))) @interface AgoraChannelStats: NSObject
@property (assign, nonatomic) NSInteger duration;
@property (assign, nonatomic) NSInteger txBytes;
@property (assign, nonatomic) NSInteger rxBytes;
@property (assign, nonatomic) NSInteger txAudioKBitrate;
@property (assign, nonatomic) NSInteger rxAudioKBitrate;
@property (assign, nonatomic) NSInteger txVideoKBitrate;
@property (assign, nonatomic) NSInteger rxVideoKBitrate;
@property (assign, nonatomic) NSInteger lastmileDelay;
@property (assign, nonatomic) NSInteger userCount;
@property (assign, nonatomic) double cpuAppUsage;
@property (assign, nonatomic) double cpuTotalUsage;
@end
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
<tr><td>stats</td>
<td><p>Statistics of the call:</p>
<ul>
<li>duration: Call duration in seconds, represented by an aggregate value.</li>
<li>txBytes: Total number of bytes transmitted, represented by an aggregate value.</li>
<li>rxBytes: Total number of bytes received, represented by an aggregate value.</li>
<li>txAudioKBitrate: Audio transmission bitrate in kbit/s, represented by an instantaneous value.</li>
<li>rxAudioKBitRate: Audio receive bitrate in kbit/s, represented by an instantaneous value.</li>
<li>txVideoKBitRate: Video transmission bitrate in kbit/s, represented by an instantaneous value.</li>
<li>rxVideoKBitRate: Video receive bitrate in kbit/s, represented by an instantaneous value.</li>
<li>lastmileDelay: The time delay in milliseconds from the Client to the VO Server</li>
<li>users: The instant number of users in the channel when the user leaves the channel.</li>
<li>puTotalUsage: System CPU usage (%).</li>
<li>cpuAppUsage: Application CPU usage (%).</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Audio Route Changed Callback (didAudioRouteChanged)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioRouteChanged:(AgoraAudioOutputRouting)routing;
```

This callback is triggered when the audio routing is changed. The definition of AgoraAudioOutputRouting is listed as follows:

```
typedef NS_ENUM(NSInteger, AgoraAudioOutputRouting)
{
    AgoraAudioOutputRoutingDefault = -1,
    AgoraAudioOutputRoutingHeadset = 0,
    AgoraAudioOutputRoutingEarpiece = 1,
    AgoraAudioOutputRoutingHeadsetNoMic = 2,
    AgoraAudioOutputRoutingSpeakerphone = 3,
    AgoraAudioOutputRoutingLoudspeaker = 4,
    AgoraAudioOutputRoutingHeadsetBluetooth = 5
};
```

#### The State of the Microphone Has Changed Callback (didMicrophoneEnabled)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didMicrophoneEnabled:(BOOL)enabled;
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
<tr><td>enabled</td>
<td>
	<ul>
		<li>true: The microphone is enabled.</li>
		<li>false: The microphone is disabled.</li>
	</ul>
	</td>
</tr>
</tbody>
</table>


#### Audio Quality Callback (audioQualityOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine audioQualityOfUid:(NSUInteger)uid quality:(AgoraNetworkQuality)quality delay:(NSUInteger)delay lost:(NSUInteger)lost;
```

Same as `audioQualityBlock`. During a call, this callback is triggered once every two seconds to report on the audio quality of the current call.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>User ID of the speaker</td>
</tr>
<tr><td>quality</td>
<td><p>Rating of the audio quality:</p>
<ul>
<li>AgoraNetworkQualityUnknown(0): The network quality is unknown.</li>
<li>AgoraNetworkQualityExcellent(1): The network quality is excellent.</li>
<li>AgoraNetworkQualityGood(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>AgoraNetworkQualityPoor(3): Users can feel the communication slightly impaired.</li>
<li>AgoraNetworkQualityBad(4): Users can communicate only not very smoothly.</li>
<li>AgoraNetworkQualityVBad(5): The network is so bad that users can hardly communicate.</li>
<li>AgoraNetworkQualityDown(6): Users cannot communicate at all.</li>
</ul>
</td>
</tr>
<tr><td>delay</td>
<td>Time delay in milliseconds.</td>
</tr>
<tr><td>lost</td>
<td>The packet loss rate(%).</td>
</tr>
</tbody>
</table>



#### Audio Volume Indication Callback (reportAudioVolumeIndicationOfSpeakers)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine reportAudioVolumeIndicationOfSpeakers:(NSArray<AgoraRtcAudioVolumeInfo *> * _Nonnull)speakers totalVolume:(NSInteger)totalVolume;
```

Same as `audioVolumeIndicationBlock`. By default this notification is disabled. If you need it, use the `enableAudioVolumeIndication` method to configure it.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>speakers</td>
<td><p>The speakers (array). Each speaker ():</p>
<ul>
<li>uid: User ID of the speaker. By default, 0 means the local user.</li>
<li>volume: The volume of the speaker that ranges from 0 (lowest volume) to 255 (highest volume).</li>
</ul>
</td>
</tr>
<tr><td>totalVolume</td>
<td>Total volume after audio mixing that ranges from 0 (lowest volume) to 255 (highest volume).</td>
</tr>
</tbody>
</table>



In the returned speakers array:

-   If the uid is 0, that is, the local user is the speaker, the returned `volume` is the same as `totalVolumn`.
-   If the uid is not 0 and the volume is 0, it indicates that the user specified by the uid did not speak.
-   If a uid is contained in the previous speakers array but not in the present one, it indicates that the user specified by the uid did not speak.


#### Other User Joined Callback (didJoinedOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didJoinedOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

Same as `userJoinedBlock`. This callback method notifies the application the broadcaster has joined the channel. If the broadcaster is already in the channel when the application joins the channel, the SDK also reports to the application on the broadcaster that is already in the channel.


> In the live broadcast scenario:
> -   The broadcaster can receive the callback when another broadcaster joins the channel.
> -   All the audience in the channel can receive the callback when the new broadcaster joins the channel.
> -   When a web application joins the channel, this callback is triggered as long as the web application publishes streams.


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
<td><p>User ID of the host.</p>
<p>If the uid is specified in the joinChannelByToken method, returns the specified ID; if not, returns an ID that is automatically allocated by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannelByToken until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### Other User Offline Callback (didOfflineOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didOfflineOfUid:(NSUInteger)uid reason:(AgoraUserOfflineReason)reason;
```

Same as `userOfflineBlock`. This callback indicates that the broadcaster has left the call or gone offline.

The SDK reads the timeout data to determine if a user has left the channel (or has gone offline). If no data package is received from the user in 15 seconds, the SDK assumes the user is offline. A poor network connection may lead to false detections; therefore, use signaling for reliable offline detection.

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
<tr><td>reason</td>
<td><p>This callback is triggered when:</p>
<ul>
<li>AgoraRtc_UserOffline_Quit(0): A user has quit the call.</li>
<li>AgoraRtc_UserOffline_Dropped(1): The SDK timed out and the user dropped offline because it has not received any data package within a certain period of time. If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the event has timed out.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Other User Muted Audio Callback (didAudioMuted)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

Same as userMuteAudioBlock. This callback indicates that some other user has muted/unmuted his/her audio streams.

> Currently, this callback returns invalid when the number of broadcasters in a channel exceeds 20, which will be improved in the future.

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
<tr><td>muted</td>
<td>Yes: User has muted his/her audio.</td>
</tr>
<tr><td>No: User has unmuted his/her audio.</td>
</tr>
</tbody>
</table>



#### RtcEngine Statistics Callback (reportRtcStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
reportRtcStats:(AgoraRtcStats*)stats;
```

Same as rtcStatsBlock. The SDK updates the application on the statistics of the RtcEngine runtime status once every two seconds.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>stat</td>
<td>See descriptions in <a href="#didleavechannelwithstats-live-ios"><span>User Left Channel Callback (didLeaveChannelWithStats)</span></a> .</td>
</tr>
</tbody>
</table>



#### Channel Network Quality Callback (networkQuality)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine networkQuality:(NSUInteger)uid txQuality:(AgoraRtcQuality)txQuality rxQuality:(AgoraRtcQuality)rxQuality;
```

This callback is triggered every 2 seconds to update the application on the current network quality of each user in a communication or live broadcast channel.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>
<div>User ID. The network quality of the user with this UID will be reported.</div>
<p>If uid is 0, it reports the local network quality.</p>
</td>
</tr>
<tr><td>txQuality</td>
<td><p>The transmission quality of the user:</p>
<ul>
<li>AgoraNetworkQualityUnknown(0): The network quality is unknown.</li>
<li>AgoraNetworkQualityExcellent(1): The network quality is excellent.</li>
<li>AgoraNetworkQualityGood(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>AgoraNetworkQualityPoor(3): Users can feel the communication slightly impaired.</li>
<li>AgoraNetworkQualityBad(4): Users can communicate only not very smoothly.</li>
<li>AgoraNetworkQualityVBad(5): The network is so bad that users can hardly communicate.</li>
<li>AgoraNetworkQualityDown(6): Users cannot communicate at all.</li>
</ul>
</td>
</tr>
<tr><td>rxQuality</td>
<td><p>The receiving quality of the user:</p>
<ul>
<li>AgoraNetworkQualityUnknown(0): The network quality is unknown.</li>
<li>AgoraNetworkQualityExcellent(1): The network quality is excellent.</li>
<li>AgoraNetworkQualityGood(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>AgoraNetworkQualityPoor(3): Users can feel the communication slightly impaired.</li>
<li>AgoraNetworkQualityBad(4): Users can communicate only not very smoothly.</li>
<li>AgoraNetworkQualityVBad(5): The network is so bad that users can hardly communicate.</li>
<li>AgoraNetworkQualityDown(6): Users cannot communicate at all.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Remote Video State Changed Callback (remoteVideoStateChangedOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine remoteVideoStateChangedOfUid:(NSUInteger)uid state:(AgoraVideoRemoteState)state;
```

This callback indicates that the state of the remote video has changed.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>ID of the user whose video state has changed</td>
</tr>
<tr><td>state</td>
<td><p>The state of the remote video:</p>
<ul>
<li>Running: the remote video is normal</li>
<li>Frozen: the remote video is frozen</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Connection Interrupted Callback (rtcEngineConnectionDidInterrupted)

```
- (void)rtcEngineConnectionDidInterrupted:(AgoraRtcEngineKit * _Nonnull)engine;
```

This method indicates that the SDK has lost connection with the server.

This method is triggered upon connection lost, while the onConnectionLost method is triggered when the SDK attempts to reconnect after losing connection. Once the connection is lost, and if the application does not call leaveChannel, the SDK automatically tries to reconnect repeatedly.

#### Connection Lost Callback (rtcEngineConnectionDidLost)

```
- (void)rtcEngineConnectionDidLost:(AgoraRtcEngineKit * _Nonnull)engine;
```

This callback indicates that the SDK has lost connection with the network, and it has remained unconnected for a period of time (10 seconds by default) despite that it attempts to reconnect. It is also triggered when the SDK fails to join the channel 10 seconds after it calls joinChannel. The SDK will keep trying to reconnect after this callback is triggered. Upon reconnection, an onRejoinChannelSuccess callback will then be triggered.

#### Connection Banned Callback (rtcEngineConnectionDidBanned)

```
- (void)rtcEngineConnectionDidBanned:(AgoraRtcEngineKit * _Nonnull)engine;
```

This callback is triggered when your connection is banned by the Agora Server.


#### First Local Audio Frame Received Callback (firstLocalAudioFrame)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstLocalAudioFrame:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

This callback method is triggered when the first local audio frame is received.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>The UID of the remote user whose audio frame is received.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannel until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### First Remote Audio Frame Received Callback (firstRemoteAudioFrameOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstRemoteAudioFrameOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

This callback method is triggered when the first remote audio frame is received.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>The UID of the remote user whose audio frame is received.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannel until this callback is triggered.</td>
</tr>
</tbody>
</table>


#### First Local Video Frame Displayed Callback (firstLocalVideoFrameWithSize)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstLocalVideoFrameWithSize:(CGSize)size elapsed:(NSInteger)elapsed;
```

Same as `firstLocalVideoFrameBlock`. Indicates that the first local video frame is displayed on the video window.

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
<tr><td>engine</td>
<td>The AgoraRtcEngineKit Object</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannelByToken until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### First Remote Video Frame Received and Decoded Callback (firstRemoteVideoDecodedOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstRemoteVideoDecodedOfUid:(NSUInteger)uid size:(CGSize)size elapsed:(NSInteger)elapsed;
```

Same as `firstRemoteVideoDecodedBlock`. Indicates that the first remote video frame is received and decoded.

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
<tr><td>engine</td>
<td>The AgoraRtcEngineKit Object</td>
</tr>
<tr><td>uid</td>
<td>User ID of the user whose video stream is received.</td>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannelByToken until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### First Remote Video Frame Displayed Callback (firstRemoteVideoFrameOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstRemoteVideoFrameOfUid:(NSUInteger)uid size:(CGSize)size elapsed:(NSInteger)elapsed;
```

Same as `firstRemoteVideoFrameBlock`. Indicates that the first remote video frame is displayed on the screen.

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
<td>The user ID of the user whose video stream is received.</td>
</tr>
<tr><td>size</td>
<td>The size of the video (width and height).</td>
</tr>
<tr><td>elapsed</td>
<td>The time elapsed (ms) from calling joinChannelByToken until this callback is triggered.</td>
</tr>
</tbody>
</table>


#### First Remote Video Frame Displayed Callback (firstRemoteVideoFrameOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine firstRemoteVideoFrameOfUid:(NSUInteger)uid size:(CGSize)size elapsed:(NSInteger)elapsed;
```

Same as `firstRemoteVideoFrameBlock`. Indicates that the first remote video frame is displayed on the screen.

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
<td>The user ID of the user whose video stream is received.</td>
</tr>
<tr><td>size</td>
<td>The size of the video (width and height).</td>
</tr>
<tr><td>elapsed</td>
<td>The time elapsed (ms) from calling joinChannelByToken until this callback is triggered.</td>
</tr>
</tbody>
</table>


#### Other User Muted Audio Callback (didAudioMuted)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

Same as userMuteAudioBlock. This callback indicates that some other user has muted/unmuted his/her audio streams.

> Currently, this callback returns invalid when the number of broadcasters in a channel exceeds 20, which will be improved in the future.

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
<tr><td>muted</td>
<td>Yes: User has muted his/her audio.</td>
</tr>
<tr><td>No: User has unmuted his/her audio.</td>
</tr>
</tbody>
</table>


#### Other User Paused/Resumed Video Callback (didVideoMuted)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didVideoMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

Same as userMuteVideoBlock. This callback indicates that another user has paused/resumed his/her video streams.

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
<tr><td>muted</td>
<td>Yes: User has paused his/her video.</td>
</tr>
<tr><td>No: User has resumed his/her video.</td>
</tr>
</tbody>
</table>



#### Other User Enabled/Disabled Video Callback (didVideoEnabled)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didVideoEnabled:(BOOL)enabled byUid:(NSUInteger)uid;
```

This callback indicates that some other user has enabled/disabled the video function. Disabling the video function means that the user can only use voice call, can neither show/send their own videos nor receive or display videos from other people.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>uid</td>
<td>User ID</td>
</tr>
<tr><td>enabled</td>
<td><ul>
<li>YES: The user has enabled the video function and can enter a video call or video live broadcast.</li>
<li>NO: The user has disabled the video function and can only enter a voice session where the user can neither send or receive any video streams.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Other User Enabled/Disabled Local Video Callback (didLocalVideoEnabled)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didLocalVideoEnabled:(BOOL)enabled byUid:(NSUInteger)uid;
```

This callback indicates that some other user has enabled/disabled the local video function.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>uid</td>
<td>User ID</td>
</tr>
<tr><td>enabled</td>
<td><ul>
<li>YES: The user has enabled the local video function. Other users in the channel can see the video of this user.</li>
<li>NO: the user has disabled the local video function. Other users in the channel can no longer receive the video stream from this user, while this user can still receive the video streams from other users.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Local Video Statistics Callback (localVideoStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine localVideoStats:(AgoraRtcLocalVideoStats * _Nonnull)stats;
```

Same as `localVideoStatBlock`.The SDK updates the application on local video streams uploading statistics once every two seconds.

**Definition of AgoraRtcLocalVideoStats**

```
__attribute__((visibility("default"))) @interface AgoraRtcLocalVideoStats : NSObject
@property (assign, nonatomic) NSUInteger sentBitrate;
@property (assign, nonatomic) NSUInteger sentFrameRate;
@end
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
<tr><td>engine</td>
<td>Engine kit.</td>
</tr>
<tr><td>stats</td>
<td><p>Statistics of the local video, including:</p>
<ul>
<li>sentBytes: Total bytes sent since last count.</li>
<li>sentFrames: Total frames sent since last count.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>





#### Remote Video Statistics Callback (remoteVideoStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine remoteVideoStats:(AgoraRtcRemoteVideoStats * _Nonnull)stats;
```

Same as `remoteVideoStatBlock`. The SDK updates the application on the statistics of receiving remote video streams for each host once every 2 seconds. If there are muptiple remote hosts, this callback is triggered for multiple times every 2 seconds.

**Definition of AgoraRtcLocalVideoStats**

```
__attribute__((visibility("default"))) @interface AgoraRtcRemoteVideoStats : NSObject
@property (assign, nonatomic) NSUInteger uid;
@property (assign, nonatomic) NSUInteger delay __deprecated;
@property (assign, nonatomic) NSUInteger width;
@property (assign, nonatomic) NSUInteger height;
@property (assign, nonatomic) NSUInteger receivedBitrate;
@property (assign, nonatomic) NSUInteger receivedFrameRate;
@property (assign, nonatomic) AgoraVideoStreamType rxStreamType;
@end
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
<tr><td>stats</td>
<td><p>The statistics of the remote video, including:</p>
<div><ul>
<li>uid: User ID of the user whose video streams are received.</li>
<li>delay: Time delay (ms).</li>
<li>width: Width of the remote video.</li>
<li>height: Height of the remote video.</li>
<li>receivedBitrate: Data receiving bitrate (kbit/s).</li>
<li>receivedFrameRate: Data receiving frame rate (fps).</li>
<li>rxStreamType: High Video Stream or Low Video Stream.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### Camera Ready Callback (rtcEngineCameraDidReady)

```
- (void)rtcEngineCameraDidReady:(AgoraRtcEngineKit * _Nonnull)engine;
```

Same as `cameraReadyBlock`. This callback indicates that the camera is turned on and ready to capture video.

#### Video Stopped Callback (rtcEngineVideoDidStop)

```
- (void)rtcEngineVideoDidStop:(AgoraRtcEngineKit * _Nonnull)engine;
```

This callback indicates that the video is stopped. The application can use this callback to change the configuration of the view (for example, displaying other pictures in the view) after the video stops.

#### Data Stream Received Callback (receiveStreamMessageFromUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine receiveStreamMessageFromUid:(NSUInteger)uid streamId:(NSInteger)streamId data:(NSData * _Nonnull)data;
```

This callback indicates that the local user has received the data stream from the other user within five seconds.

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
<td>User ID</td>
</tr>
<tr><td>streamId</td>
<td>Stream ID</td>
</tr>
<tr><td>data</td>
<td>Data received by the recipients.</td>
</tr>
</tbody>
</table>



#### Data Stream Sent Failure Callback (didOccurStreamMessageErrorFromUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didOccurStreamMessageErrorFromUid:(NSUInteger)uid streamId:(NSInteger)streamId error:(NSInteger)error missed:(NSInteger)missed cached:(NSInteger)cached;
```

This callback indicates that the local user has not received the data stream from the other user within five seconds.

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
<td>User ID</td>
</tr>
<tr><td>streamId</td>
<td>Stream ID</td>
</tr>
<tr><td>error</td>
<td><ul>
<li>ERR_OK = 0, No error</li>
<li>ERR_NOT_IN_CHANNEL=113, the user is not in a channel</li>
<li>ERR_BITRATE_LIMIT=115, limited bitrate</li>
</ul>
<p>For more error code descriptions, see <a href="../../en/Interactive%20Gaming/the_error_native.md"><span>Error Codes and Warning Codes</span></a>.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>missed</td>
<td>The number of lost messages</td>
</tr>
<tr><td>cached</td>
<td>The number of incoming cached messages when the data stream is interrupted</td>
</tr>
</tbody>
</table>




#### Active Speaker Detected Callback (ActiveSpeaker)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine activeSpeaker:(NSUInteger)speakerUid;
```

If you have used the `enableAudioVolumeIndication` API, this callback is triggered then the volume detecting unit has detected active speaker in the channel. Also returns with the uid of the active speaker.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>speakerUid</td>
<td>The UID of the active speaker. By default, 0 means the local user. If needed, you can also add relative functions on your App, for example, the active speaker, once detected, will have his/her head portrait zoomed in.</td>
</tr>
</tbody>
</table>

> -   You need to call `enableAudioVolumeIndication` to receive this callback.
> -   The active speaker means the uid of the speaker who speaks at the highest volume during a certain period of time.


Refer to the following code for the logic:

```
 - (void)rtcEngine:(AgoraRtcEngineKit *)engine activeSpeaker:(NSUInteger)speakerUid {
[self switchToMainViewOfSpeaker:speakerUid];
}
```

#### User Role Changed Callback (didClientRoleChanged)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didClientRoleChanged:(AgoraClientRole)oldRole newRole:(AgoraClientRole)newRole;
```

This callback is triggered when the user role is switched, for example, from a host to an audience or vice versa.

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
<tr><td>oldRole</td>
<td>Role that you switched from.</td>
</tr>
<tr><td>newRole</td>
<td>Role that you switched to.</td>
</tr>
</tbody>
</table>


#### Camera Focus Changed Callback (cameraFocusDidChanged)

```
(void)rtcEngine:(AgoraRtcEngineKit* _Nonnull)engine cameraFocusDidChangedToRect:(CGRect)rect;
```

This callback is triggered when the camera focus area has changed.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>rect</td>
<td>The rectangle area in the camera zoom that specifies the focus area</td>
</tr>
</tbody>
</table>



#### Video Size Changed Callback (videoSizeChangedOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine videoSizeChangedOfUid:(NSUInteger)uid size:(CGSize)size rotation:(NSInteger)rotation;
```

This callback indicates that the size of the video frame has changed.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>User ID.</td>
</tr>
<tr><td>size</td>
<td>Size (pixels) of the video frame.</td>
</tr>
<tr><td>rotation</td>
<td>New rotational angle of the video. Can be 0, 90, 180, or 270. It is set as 0 by default.</td>
</tr>
</tbody>
</table>



#### Token Expired Callback (rtcEngineRequestToken)

```
- (void)rtcEngineRequestToken:(AgoraRtcEngineKit * _Nonnull)engine;
```

If a Token is specified when calling joinChannelByToken, and due to Token will be expired after a certain amount of time, and if SDK lost the connection with the Agora server out of network issues, it might need a new Token to reconnect to the server.

This callback notifies the application of generating a new Token, and calling renewtoken to specify the newly generated Token.

This function was previously provided in the callback report of didOccurError(): ERR_TOKEN_EXPIRED(109), ERR_INVALID_TOKEN(110). Starting from v1.7.3, the old method is still working, but it is recommended for you to put the related logic in this callback.

This function was previously provided when the callback reported didOccurError(): ERR_CHANNEL_KEY_EXPIRED(109)、ERR_INVALID_CHANNEL_KEY(110). Starting from v1.7.3, the old method still works, but it is recommended to use this callback.

#### Local Audio Mixing Playback Finished Callback (rtcEngineLocalAudioMixingDidFinish)

```
- (void)rtcEngineLocalAudioMixingDidFinish:(AgoraRtcEngineKit * _Nonnull)engine;
```

This callback is triggered when the audio mixing file playback is finished after calling `startAudioMixing`. If you failed to execute the `startAudioMixing` method, it returns the error code in the `didOccurError` callback.

#### Local Audio Effect Playback Finished Callback (rtcEngineDidAudioEffectFinish)

```
- (void)rtcEngineDidAudioEffectFinish:(AgoraRtcEngineKit * _Nonnull)engine soundId:(NSInteger)soundId;
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
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
</tbody>
</table>



#### Remote Audio Mixing Started Callback (rtcEngineRemoteAudioMixingDidStart)

```
- (void)rtcEngineRemoteAudioMixingDidStart:(AgoraRtcEngineKit * _Nonnull)engine;
```

This callback is triggered when a remote host calls `startAudioMixing`.

#### Remote Audio Mixing Stopped Callback (rtcEngineRemoteAudioMixingDidFinish)

```
- (void)rtcEngineRemoteAudioMixingDidFinish:(AgoraRtcEngineKit * _Nonnull)engine;
```

This callback is triggered when a remote host stops calling `startAudioMixing`.

<a id = "block"></a>
## Block Callbacks

The SDK supports Block callbacks to report runtime events to the application. From version 1.1, some Block callbacks in the SDK are replaced with Delegate methods. The old Block callbacks are therefore being deprecated, but can still be used in current versions. However, we recommend replacing them with Delegate methods. The SDK calls the Block method if a callback is defined in both Block and Delegate. If you want to use the Delegate methods, set the Block attribute of the method as nil. The following section describes each of the callback methods in the SDK.

#### Other User Joined Callback (userJoinedBlock)

```
- (void)userJoinedBlock:(void(^)
(NSUInteger uid, NSInteger elapsed))userJoinedBlock;
```

This callback method notifies the application the broadcaster has joined the channel. If the broadcaster is already in the channel when the application joins the channel, the SDK also reports to the application on the broadcaster that is already in the channel.

> In the live broadcast scenario:
> -   The broadcaster can receive the callback when another broadcaster joins the channel.
> -   All the audience in the channel can receive the callback when the new broadcaster joins the channel.
> -   When a web application joins the channel, this callback is triggered as long as the web application publishes streams.


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
<tr><td>If the uid is specified in the joinChannelByToken method, returns the specified ID; if not, returns an ID that is automatically allocated by the Agora server.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling joinChannelByToken until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### Other User offline Callback (userOfflineBlock)

```
- (void)userOfflineBlock:(void(^)
(NSUInteger uid))userOfflineBlock;
```

This callback indicates that a user has left the call or gone offline.

The SDK reads the timeout data to determine if a user has left the channel (or has gone offline). If no data package is received from the user in 15 seconds, the SDK assumes the user is offline. Sometimes a weak network connection may lead to false detections, therefore we recommend using signaling for reliable offline detection.

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
</tbody>
</table>



#### Rejoin Channel Callback (rejoinChannelSuccessBlock)

```
 - (void)rejoinChannelSuccessBlock:(void(^)(NSString* channel,
NSUInteger uid, NSInteger elapsed))rejoinChannelSuccessBlock;
```

When the local machine loses connection with the server because of network problems, the SDK automatically attempts to reconnect, and then triggers this callback method upon reconnection. The callback indicates that the user has rejoined the channel with an assigned channel ID and user ID.

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
<tr><td>channel</td>
<td>Channel name.</td>
</tr>
<tr><td>uid</td>
<td>User ID.</td>
</tr>
<tr><td>elapsed</td>
<td>Time delay (ms) in rejoining the channel.</td>
</tr>
</tbody>
</table>



#### User Left Channel Callback (leaveChannelBlock)

```
- (void)leaveChannelBlock:(void(^)
(AgoraRtcStats* stat))leaveChannelBlock;
```

This callback indicates that a user has left the channel, and it also provides call session statistics including the duration of the call and tx/rx bytes.

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
<tr><td>stat</td>
<td>See the table below.</td>
</tr>
</tbody>
</table>



<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>AgoraRtcStats</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>duration</td>
<td>Call duration (s), represented by an aggregate value.</td>
</tr>
<tr><td>txBytes</td>
<td>Total number of bytes transmitted, represented by an aggregate value.</td>
</tr>
<tr><td>rxBytes</td>
<td>Total number of bytes received, represented by an aggregate value.</td>
</tr>
</tbody>
</table>



#### RtcEngine Statistics Callback (rtcStatsBlock)

```
- (void)rtcStatsBlock:(void(^)
(AgoraRtcStats* stat))rtcStatsBlock;
```

This callback is triggered once every two seconds to update the application on RtcEngine runtime statistics.

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
<tr><td>stat</td>
<td>See the table below.</td>
</tr>
</tbody>
</table>



<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>AgoraRtcStats</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>duration</td>
<td>Call duration (s), represented by an aggregate value.</td>
</tr>
<tr><td>txBytes</td>
<td>Total number of bytes transmitted, represented by an aggregate value.</td>
</tr>
<tr><td>rxBytes</td>
<td>Total number of bytes received, represented by an aggregate value.</td>
</tr>
</tbody>
</table>



#### Audio Volume Indication Callback (audioVolumeIndicationBlock)

```
- (void)audioVolumeIndicationBlock:(void(^)(NSArray *speakers,
NSInteger totalVolume))audioVolumeIndicationBlock;
```

This callback indicates who is talking and the speaker’s volume. By default it is disabled. If needed, use the enableAudioVolumeIndication method to configure it.

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
<tr><td>speakers</td>
<td>The speakers (array). Each speaker ():</td>
</tr>
<tr><td>uid: User ID of the speaker.</td>
</tr>
<tr><td>volume：Volume of the speaker, between 0 (lowest volume) to 255 (highest volume).</td>
</tr>
<tr><td>totalVolume</td>
<td>The total volume after audio mixing between 0 (lowest volume) to 255 (highest volume).</td>
</tr>
</tbody>
</table>


#### First Local Video Frame Displayed Callback (firstLocalVideoFrameBlock)

```
 - (void)firstLocalVideoFrameBlock:(void(^)(NSInteger width,
NSInteger height, NSInteger elapsed))firstLocalVideoFrameBlock;
```

This callback indicates that the first local video frame is displayed on the screen.

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
<tr><td>width</td>
<td>Width (pixels) of the video stream.</td>
</tr>
<tr><td>height</td>
<td>Height (pixels) of the video stream.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from a user joining the channel until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### First Remote Video Frame Displayed Callback (firstRemoteVideoFrameBlock)

```
- (void)firstRemoteVideoFrameBlock:(void(^)(NSUInteger uid,
NSInteger width, NSInteger height,
NSInteger elapsed))firstRemoteVideoFrameBlock;
```

This callback indicates that the first remote video frame is displayed on the screen.

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
<td>User ID of the user whose video streams are received.</td>
</tr>
<tr><td>width</td>
<td>Width (pixels) of the video stream.</td>
</tr>
<tr><td>height</td>
<td>Height (pixels) of the video stream.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from the user joining the channel until this callback is triggered.</td>
</tr>
</tbody>
</table>


#### First Remote Video Frame Received and Decoded Callback (firstRemoteVideoDecodedBlock)

```
- (void)firstRemoteVideoDecodedBlock:(void(^)
(NSUInteger uid, NSInteger width, NSInteger height,
NSInteger elapsed))firstRemoteVideoDecodedBlock;
```

This callback indicates that the first remote video frame is received and decoded.

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
<td>User ID of the user whose video streams are received.</td>
</tr>
<tr><td>width</td>
<td>Width (pixels) of the video stream.</td>
</tr>
<tr><td>height</td>
<td>Height (pixels) of the video stream.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from the user joining the channel until this callback is triggered.</td>
</tr>
</tbody>
</table>


#### Other User Muted Audio Callback (userMuteAudioBlock)

```
- (void)userMuteAudioBlock:(void(^)
(NSUInteger uid, bool muted))userMuteAudioBlock;
```

This callback indicates that a user has muted/unmuted his/her audio stream.

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
<tr><td>muted</td>
<td>Yes: Muted audio.</td>
</tr>
<tr><td>No: Unmuted audio.</td>
</tr>
</tbody>
</table>


#### Other User Paused/Resumed Video Callback (userMuteVideoBlock)

```
- (void)userMuteVideoBlock:(void(^)
(NSUInteger uid, bool muted))userMuteVideoBlock;
```

This callback indicates that a user has paused/resumed his/her video stream.

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
<td>User ID</td>
</tr>
<tr><td>muted</td>
<td>Yes: User has paused his/her video.</td>
</tr>
<tr><td>No: User has resumed his/her video.</td>
</tr>
</tbody>
</table>



#### Local Video Statistics Callback (localVideoStatBlock)

```
- (void)localVideoStatBlock:(void(^)(NSInteger sentBytes,
NSInteger sentFrames))localVideoStatBlock;
```

The SDK updates the application about the statistics of uploading local video streams once every two seconds.

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
<tr><td>sentBytes</td>
<td>Total bytes sent since last count.</td>
</tr>
<tr><td>sentFrames</td>
<td>Total frames sent since last count.</td>
</tr>
</tbody>
</table>



#### Remote Video Statistics Callback (remoteVideoStatBlock)

```
- (void)remoteVideoStatBlock:(void(^)(NSUInteger uid,
NSInteger frameCount, NSInteger delay,
NSInteger receivedBytes))remoteVideoStatBlock;
```

The SDK updates the application about the statistics of receiving remote video streams once every two seconds.

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
<td>User ID that specifies whose video streams are received.</td>
</tr>
<tr><td>frameCount</td>
<td>Total frames received since last count.</td>
</tr>
<tr><td>delay</td>
<td>Time delay (ms)</td>
</tr>
<tr><td>receivedBytes</td>
<td>Total bytes received since last count.</td>
</tr>
</tbody>
</table>


#### Camera Ready Callback (cameraReadyBlock)

```
- (void)cameraReadyBlock:(void(^)())cameraReadyBlock;
```

This callback indicates that the camera is turned on and ready to capture video.

#### Audio Quality Callback (audioQualityBlock)

```
- (void)audioQualityBlock:(void(^)(NSUInteger uid,
AgoraNetworkQuality quality,  NSUInteger delay,
NSUInteger lost))audioQualityBlock;
```

During a conversation, this callback is triggered once every two seconds to report on the audio quality of the current call.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>uid</td>
<td>User ID of the speaker</td>
</tr>
<tr><td>quality</td>
<td><p>Rating of the audio quality:</p>
<ul>
<li>AgoraNetworkQualityUnknown(0): The network quality is unknown.</li>
<li>AgoraNetworkQualityExcellent(1): The network quality is excellent.</li>
<li>AgoraNetworkQualityGood(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>AgoraNetworkQualityPoor(3): Users can feel the communication slightly impaired.</li>
<li>AgoraNetworkQualityBad(4): Users can communicate only not very smoothly.</li>
<li>AgoraNetworkQualityVBad(5): The network is so bad that users can hardly communicate.</li>
<li>AgoraNetworkQualityDown(6): Users cannot communicate at all.</li>
</ul>
</td>
</tr>
<tr><td>delay</td>
<td>Time delay in milliseconds.</td>
</tr>
<tr><td>lost</td>
<td>The packet loss rate(%).</td>
</tr>
</tbody>
</table>



#### Network Quality Callback (lastmileQualityBlock)

```
- (void)lastmileQualityBlock:(void(^)
(AgoraNetworkQuality quality))lastmileQualityBlock;
```

This callback is triggered once every two seconds during a call to report on the network quality.

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
<tr><td>quality</td>
<td>Rating of the network quality:</td>
</tr>
<tr><td>AgoraRtc_Quality_Unknown = 0</td>
</tr>
<tr><td>AgoraRtc_Quality_Excellent = 1</td>
</tr>
<tr><td>AgoraRtc_Quality_Good = 2</td>
</tr>
<tr><td>AgoraRtc_Quality_Poor = 3</td>
</tr>
<tr><td>AgoraRtc_Quality_Bad = 4</td>
</tr>
<tr><td>AgoraRtc_Quality_VBad = 5</td>
</tr>
<tr><td>AgoraRtc_Quality_Down = 6</td>
</tr>
</tbody>
</table>



#### Connection Lost Callback (connectionLostBlock)

```
- (void)connectionLostBlock:(void(^)())connectionLostBlock;
```

This callback indicates that the SDK has lost network connection with the server.


