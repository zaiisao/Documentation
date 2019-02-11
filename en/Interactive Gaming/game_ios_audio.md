
---
title: Interactive Gaming API
description: 
platform: Objective-C
updatedAt: Mon Feb 11 2019 03:52:54 GMT+0000 (UTC)
---
# Interactive Gaming API
The Interactive Gaming API is composed of the **Objective-C Interface** and **C++ Interface**, 

This page provides the Objective-C interface, with which you can integrate the voice and video function into your app on iOS. To refer to the C++ Interface, see [C++ Interface](../../en/Interactive%20Gaming/game_cpp.md).


## Basic Methods (AgoraRtcEngineKit)

`AgoraRtcEngineKit` is the basic interface class of the Agora Native SDK. Creating an `AgoraRtcEngineKit` object and then calling the methods of this object enables the use of the Agora Native SDK’s communication functionality.

### Initializes (sharedEngineWithAppId)

```
+ (instancetype _Nonnull)sharedEngineWithAppId:(NSString * _Nonnull)appId
                                      delegate:(id<AgoraRtcEngineDelegate> _Nullable)delegate;
```

This method initializes the `AgoraRtcEngineKit` class as a singleton instance. Call this method to initialize the service before using the `AgoraRtcEngineKit` object.

The SDK uses `Delegate` to inform the app on the engine runtime events. All methods defined in `Delegate` are optional implementation methods.

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
<td>The App ID issued to you by Agora. Apply for a new one from Agora if it is missing in your kit.</td>
</tr>
<tr><td>delegate</td>
<td>The AgoraRtcEngineDelegate Class</td>
</tr>
</tbody>
</table>

### Implements a Live Voice Broadcast

#### Sets the Channel Profile (setChannelProfile)

```
- (int)setChannelProfile:(AgoraChannelProfile)profile;
```

This method configures the channel profile. `RtcEngine` needs to know what scenario the application is in to apply different methods for optimization.

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
<td>Live Broadcast. Host and audience roles that can be set by calling the <code>setClientRole</code> method.  The host sends and receives voice, while the audience receives voice only with the sending function disabled.</td>
</tr>
<tr><td>Gaming</td>
<td>Gaming Mode. Any user in the channel can talk freely. This mode uses the codec with low-power consumption and low bitrate by default.</td>
</tr>
</tbody>
</table>

> -   Only one profile can be used at the same time in the same channel. If you want to switch to another profile, use `destroy` to destroy the current `RtcEngine` and create a new one using the `sharedEngineWithAppId` method before calling this method to set the new channel profile.
> -   Ensure that different App IDs are used for different channel profiles.
> -   This method must be called and configured before a user joins a channel because the channel profile cannot be configured when the channel is in use.


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
<li>AgoraChannelProfileCommunication = 0: (Default) Communication</li>
<li>AgoraChannelProfileLiveBroadcasting = 1: Live Broadcast</li>
<li>AgoraChannelProfileGame = 2: Gaming</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Sets the User Role (setClientRole)

```
- (int)setClientRole:(AgoraClientRole)role;
```

In a Live-broadcasr profile, this method allows you to:
* Set the user role as a host or an audience (default) before joining a channel.
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
<td>The user role in a live broadcast:
<ul>
<li>AgoraClientRoleBroadcaster = 1: Host.</li>
<li>AgoraClientRoleAudience = 2: (Default) Audience.)</li>
</ul>
</td>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Enables the Audio Mode (enableAudio)

```
- (int)enableAudio;
```

This method enables the audio mode. The app can call this method before joining a channel. This function is enabled by default.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> This method enables the internal engine and still works after calling the `leaveChannel` method.

#### Disables/Re-enables the Local Audio Function (enableLocalAudio)

```
- (int)enableLocalAudio:(BOOL)enabled;
```

When an app joins a channel, the audio function is enabled by default. This method disables or re-enables the local audio function, that is, to stop or restart local audio capturing and handling.
This method does not affect receiving or playing the remote audio streams, and is applicable to scenarios where the user wants to receive the remote audio streams without sending any audio stream to other users in the channel.
The SDK triggers the `didMicrophoneEnabled` callback once the local audio function is disabled or re-enabled.

> - Call this method after calling the `joinChannelByToken` method.
> - This method is different from the `muteLocalAudioStream` method:
>   * `enableLocalAudio`: Disables/Re-enables the local audio capturing and processing.
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
<li>true: Re-enable the local audio function, that is, to start local audio capturing and processing.</li>
<li>false: Disable the local audio function, that is, to stop local audio capturing and processing.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Disables the Audio Mode (disableAudio)

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> This method disables the internal engine and still works after you call the `leaveChannel` method.


#### Allows a User to Join a Channel (joinChannelByToken)

```
- (int)joinChannelByToken:(NSString * _Nullable)token
                channelId:(NSString * _Nonnull)channelId
                     info:(NSString * _Nullable)info
                      uid:(NSUInteger)uid
              joinSuccess:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))joinSuccessBlock;
```

This method allows a user to join a channel. Users in the same channel can talk to each other and multiple users in the same channel can start a group chat. Users using different App IDs cannot call each other. Once in a call, the user must call the `leaveChannel` method to exit the current call before joining another channel. This method call is asynchronous; therefore, it can be called in the main user interface thread.

The SDK uses the OS X’s `AVAudioSession` shared object for audio recording and playback, so using this object may affect the SDK’s audio functions.

Once this method call is successful, the SDK triggers a callback. If both the `joinChannelSuccessBlock` and `didJoinChannel` callbacks are implemented, the priority of `joinChannelSuccessBlock` is higher than `didJoinChannel`, thus `didJoinChannel` is ignored. If you want to use `didJoinChannel`, set `joinChannelSuccessBlock` as nil.


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
<td><p>A token generated by the application.</p>
<p>This parameter is optional if the user uses a static App ID. In this case, pass NIL as the parameter value.</p>
<p>If the user uses a token, Agora issues an additional App Certificate to the you. You can then generate a token using the algorithm and App Certificate provided by Agora for user authentication on the server.</p>
<p>In most cases, the static App ID suffices. For added security, use a token.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>channelName</td>
<td><p>A string providing the unique channel name for the AgoraRTC session. The length must be within 64 bytes.</p>
<p>The following is the supported scope:</p>
<ul>
<li>The 26 lowercase English letters: a to z</li>
<li>The 26 uppercase English letters: A to Z</li>
<li>The 10 numbers: 0 to 9</li>
<li>Space</li>
<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td>info</td>
<td>(Optional) Any additional information you want to add. <sup>[1]</sup> It can be set as a NIL Sting or channel related information. Other users in the channel do not receive this information.</td>
</tr>
<tr><td>uid</td>
<td>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If not assigned (or set to 0), the SDK assigns one and returns it in the <code>joinSuccessBlock</code> callback. The app must record and maintain the returned value, as the SDK does not maintain it.</td>
</tr>
<tr><td>joinSuccessBlock</td>
<td>Callback on the user successfully joining the channel.</td>
</tr>
</tbody>
</table>



> [1] For example, when a host wants to customize the resolution and bitrate for a live broadcast channel with CDN live enabled, you can include them in this parameter in JSON format. For example, \{“owner”:true, …, “width”:300, “height”:400, “bitrate”:100\}. Only when neither `width`, `height`, and `bitrate` is 0 can the bitrate and resolution settings take effect.

> When joining a channel, the SDK calls `setCategory AVAudioSessionCategoryPlayAndRecord` to set `AVAudioSession` to `PlayAndRecord` mode. The app should not set it to any other mode. When setting to this mode, the sound being played (for example, a ringtone) is interrupted.

#### Allows a User to Leave a Channel (leaveChannel)

```
- (int)leaveChannel:(void(^ _Nullable)(AgoraChannelStats * _Nonnull stat))leaveChannelBlock;
```

This method allows a user to leave a channel, such as hanging up or exiting a call. After joining a channel, the user must call this method to end the call before joining another one.

This method releases all resources related to the call and asynchronous. The user has not actually left the channel when the method call returns. When the user leaves the channel, the SDK triggers the `didLeaveChannelWithstats` callback.

> If you call the `destroy` method immediately after calling the `leaveChannel` method, the `leaveChannel` process is interrupted, and the SDK does not trigger the `didLeaveChannelWithstats` callback.

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
<td>Callback on the user successfully leaving the channel.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Sets the Local Voice Pitch (setLocalVoicePitch)

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
<td>Voice frequency. The value ranges between 0.5 and 2.0. The default value is 1.0.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

#### Sets the Playback Position and Volume of the Audio Effects Sent from the Remote User (setRemoteVoicePosition)

```
- (int)setRemoteVoicePosition:(int)uid pan:(double)pan gain:(double)gain;
```

This method sets the playback position and volume of the audio effects sent from the remote user.

> This method is valid only for earphones and is invalid when the speakerphone is enabled.

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
<td><p>User ID of the remote user.</p>
</td></tr>
<tr><td>pan</td>
<td><p>Sets whether to change the spatial position of the audio effect. The value ranges between -1 and 1:</p>
<ul>
<li>0: The audio effect shows right ahead.</li>
<li>-1: The audio effect shows on the left.</li>
<li>1: The audio effect shows on the right.</li>
</ul>
</td>
</tr>
<tr><td>gain</td>
<td><p>Sets whether to change the volume of a single audio effect. The value ranges between 0.0 and 100.0. The default value is 100.0. The smaller the value is, the lower the volume of the audio effect.</p>
</td></tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

#### Sets to Voice-only Mode (setVoiceOnlyMode)

```
- (int)setVoiceOnlyMode:(bool)enable;
```

This method sets to voice-only mode (transmits the audio stream only) and the other streams are ignored; for example, the sound of keyboard strokes.

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
<li>True: Enable voice-only mode.</li>
<li>False: Disable voice-only mode.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Sets the Local Voice Equalization (setLocalVoiceEqualizationOfBandFrequency)

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
<td>The band frequency. The value ranges between 0 to 9; representing the respective 10-band center frequencies of the voice effects, including 31, 62, 125, 500, 1k, 2k, 4k, 8k, and 16k Hz.</td>
</tr>
<tr><td>gain</td>
<td>Gain of each band (dB). The value ranges between -15 and 15.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Sets the Local Voice Reverberation (setLocalVoiceReverbOfType)

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
<li>AgoraAudioReverbDryLevel = 0, (dB, [-20,10]), level of the dry signal.</li>
<li>AgoraAudioReverbWetLevel = 1, (dB, [-20,10]), level of the early reflection signal (wet signal).</li>
<li>AgoraAudioReverbRoomSize = 2, ([0, 100]), room size of the reflection.</li>
<li>AgoraAudioReverbWetDelay = 3, (ms, [0, 200]), length of the initial latency of the wet signal in ms.</li>
<li>AgoraAudioReverbStrength = 4, ([0, 100]), length of the late reverberation.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>


### Implements a Live Video Broadcast

#### Enables the Video Mode (enableVideo)

```
- (int)enableVideo;
```

This method enables the video mode. You can call this method either before joining a channel or during a call. If you call this method before entering a channel, the service starts in the video mode. If you call this method during a call, the service switches from the audio to video mode. To disable the video mode, call the `disableVideo` method.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> This method enables the internal engine and still works after you call the `leaveChannel` method.

#### Disables the Video (disableVideo)

```
- (int)disableVideo;
```

This method disables the video mode. You can call this method either before joining a channel or during a call. If you call this method before joining a channel, the service starts in the audio mode. If you call this method during a call, the service switches from the video to audio mode. To enable the video mode, call the `enableVideo` method.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> This method disables the internal engine and still works after you call the `leaveChannel` method.

#### Enables the Local Video (enableLocalVideo)

```
- (int)enableLocalVideo:(BOOL)enabled;
```

This method enables/disables the local video, which is only applicable to the scenario when the user only wants to watch the remote video without sending any video stream to the other user.

-   Call this method after calling the `enableVideo` method. Otherwise, this method may not work properly.

-   After you call the `enableVideo` method, the local video is enabled by default. This method is used to enable and disable the local video while the remote video remains unaffected.


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
<li>YES: (Default) Enable the local video.</li>
<li>NO: Disable the local video. Once the local video is disabled, the remote users can no longer receive the video stream of this user, while this user can still receive the video streams of other remote users. When <code>enabled</code> is set as <code>NO</code>, this method does not require a local camera.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

> This method enables/disables the internal engine and still works after you call the `leaveChannel` method.


#### Sets the Video Profile (setVideoProfile)


> This method is deprecated. To set the video encoder profile, Agora recommends calling the `setVideoEncoderConfiguration` method.

```
- (int)setVideoProfile:(AgoraVideoProfile)profile
    swapWidthAndHeight:(BOOL)swapWidthAndHeight;
```

This method sets the video encoding profile. If the user does not need to set the video encoding profile after joining the channel, Agora recommends calling this method before calling the `enableVideo` method to minimize the time delay in receiving the first video frame.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
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


#### Sets the Video Resolution (setVideoResolution)


> This method is deprecated. To set the video encoder profile, Agora recommends calling the `setVideoEncoderConfiguration` method.

```
- (int)setVideoResolution: (CGSize)size andFrameRate: (NSInteger)frameRate bitrate: (NSInteger) bitrate;
```

If the video profiles listed in the table do not meet your needs, use this method to manually set the width, height, frame rate, and bitrate of the video. If the user does not need to set the video encoding profile after joining a channel, Agora recommends calling this method before calling the `enableVideo` method to minimize the time delay in receiving the first video frame.

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
<p>If the bitrate you set is beyond the proper range, the SDK automatically adjusts it to a value within the range.</p>
</td>
</tr>
</tbody>
</table>



#### Sets the Local Video View (setupLocalVideo)

```
- (int)setupLocalVideo:(AgoraRtcVideoCanvas * _Nullable)local;
```

This method configures the video display settings on the local machine. The app calls this method to bind with the video window (view) of the local video stream and configure the video display settings. Call this method after initialization to configure the local video display settings before joining a channel. After leaving the channel, the bind is still valid, which means the window still displays. To unbind the view, set the view value to NIL when calling the `setupLocalVideo` method.

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
<td><p>Video display settings. VideoCanvas class:</p>
<ul>
<li>view: Video display window (view).</li>
<li>renderMode: Video display mode.<ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
</ul>
</li>
<li>Return value: The local user ID as set in the <code>joinChannel</code> method.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Sets the Remote Video View (setupRemoteVideo)

```
- (int)setupRemoteVideo:(AgoraRtcVideoCanvas * _Nonnull)remote;
```

This method binds the remote user to the video display window, that is, sets the view for the user of the specified uid. Usually the app specifies the uid of the remote video in this method call before the user joins a channel. If the remote uid is unknown to the app, set it later when the app receives the `onUserJoined` callback.

If the Video Recording function is enabled, the Video Recording Service joins the channel as a dummy client, which means other clients also receive the `didJoinedOfUid` callback. Your app should not bind the dummy client with the view, because the dummy client does not send any video stream. If your app cannot recognize the dummy client, bind the dummy client with the view when the SDK triggers the `firstRemoteVideoDecodedOfUid` callback. To unbind the user with the view, set `view` as `null`. After the user leaves the channel, the SDK unbinds the remote user.

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
<td><p>Video display settings. VideoCanvas class:</p>
<ul>
<li><p>view: Video display window (view).</p>
</li>
<li><p>renderMode: Video display mode.</p>
<div><ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
</ul>
</div>
</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Enables Dual-stream Mode (enableDualStreamMode)

```
- (int)enableDualStreamMode:(BOOL)enabled;
```

This method sets the stream mode to single- (default) or dual-stream mode. After `enabled` is set as `YES`, the receiver can choose to receive the high-stream (high-resolution high-bitrate) or low-stream (low-resolution low-bitrate) video.

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
<li>True: Dual stream.</li>
<li>False: Single stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Sets the Remote Video-stream Type (setRemoteVideoStream)

```
- (int)setRemoteVideoStream:(NSUInteger)uid
                       type:(AgoraVideoStreamType)streamType;
```

This method specifies the video-stream type of the remote user to be received by the local user when the remote user sends dual streams.

-   If dual-stream mode is enabled by calling the `enableDualStreamMode` method, you receive the high-stream video by default. This method allows the app to adjust the corresponding video-stream type according to the size of the video window to save the bandwidth and resources.

-   If dual-stream mode is not enabled, you receive the high-stream video by default.


The result after calling this method is returned in the `didApiCallExecute` callback. The Agora SDK receives the high-stream video by default to save the bandwidth. If needed, users can switch to the low-stream video using this method.

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
<td><p>Sets the video-stream size.</p>
<p>The video-stream types:</p>
<ul>
<li>AgoraVideoStreamTypeHigh(0): High-stream video.</li>
<li>AgoraVideoStreamTypeLow(1): Low-stream video.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Sets the Default Remote Video Stream Type (setRemoteDefaultVideoStreamType)

```
- (int)setRemoteDefaultVideoStreamType:(AgoraVideoStreamType)streamType;
```

This method sets the default remote video stream type to high- or low-stream.

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
<td><p>The default remote video stream type:</p>
<ul>
<li>AgoraVideoStreamTypeHigh = 0: The high-stream video.</li>
<li>AgoraVideoStreamTypeLow = 1: The low-stream video.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Sets the High-quality Video Preferences (setVideoQualityParameters)

```
- (int)setVideoQualityParameters:(BOOL)preferFrameRateOverImageQuality;
```

This method allows you to set the video preferences.

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
<li>True: Frame rate over image quality.</li>
<li>False: (Default) Image quality over frame rate.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Starts a Video Preview (startPreview)

```
- (int)startPreview;
```

This method starts a local video preview before joining a channel. Before using this method, you need to:

-   Call the `setupLocalVideo` method to set up the local preview window and configure the attributes.
-   Call the `enableVideo` method to enable the video.

> Once the local video preview is enabled, the preview does not stop if you call the `leaveChannel` method to leave the channel. To stop the preview, call the `stopPreview` method.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stops the Video Preview (stopPreview)

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Sets the Local Video Display Mode (setLocalRenderMode)

```
- (int)setLocalRenderMode:(AgoraVideoRenderMode) mode;
```

This method configures the local video display mode. The app can call this method multiple times to change the display mode.

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
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Sets the Remote Video Display Mode (setRemoteRenderMode)

```
- (int)setRemoteRenderMode:(NSUInteger)uid
                      mode:(AgoraVideoRenderMode) mode;
```

This method configures the remote video display mode. The app can call this method multiple times to change the display mode.

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
<td>The user ID of the remote user sending the video stream.</td>
</tr>
<tr><td>mode</td>
<td><p>Configures the video display mode:</p>
<div><ul>
<li>AgoraVideoRenderModeHidden (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>AgoraVideoRenderModeFit (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Sets the Local Video Mirror Mode (setLocalVideoMirrorMode)

```
- (int)setLocalVideoMirrorMode:(AgoraVideoMirrorMode) mode;
```

This method sets the local video mirror mode. Use this method before calling the `startPreview` method or this method does not work until you call the `startPreview` method again.

```
  typedef NS_ENUM(NSUInteger, AgoraVideoMirrorMode) {
    AgoraVideoMirrorModeAuto = 0,
    AgoraVideoMirrorModeEnabled = 1,
    AgoraVideoMirrorModeDisabled = 2,
};
```


### Camera Management

#### Switches Between Front and Back Cameras (switchCamera)

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Checks whether the Camera Zoom Function is Supported (isCameraZoomSupported)

```
- (BOOL)isCameraZoomSupported;
```

It returns:

-   True: The device supports the camera zoom function.
-   False: The device does not support the camera zoom function.

#### Checks whether the Camera Flash Function is Supported (isCameraTorchSupported)

```
- (BOOL)isCameraTorchSupported;
```

It returns:

-   True: The device supports the camera flash function.
-   False: The device does not support the camera flash function.


> The app enables the front camera by default. If your front camera does not support front camera flash, this method returns `False`. To check if the rear camera flash is supported, call the `switchCamera` method before calling this method.

#### Checks whether the Camera Manual Focus Function is Supported (isCameraFocusPositionInPreviewSupported)

```
- (BOOL)isCameraFocusPositionInPreviewSupported;
```

It returns:

-   True: The device supports the manual focus function.
-   False: The device does not support the manual focus function.


#### Checks whether the Camera Auto Focus Function is Supported (isCameraAutoFocusFaceModeSupported)

```
- (BOOL)isCameraAutoFocusFaceModeSupported;
```

It returns:

-   True: The device supports the auto focus function.
-   False: The device does not support the auto focus function.


#### Sets the Camera Zoom Ratio (setCameraZoomFactor)

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
<td>The camera zoom factor. The value ranges between 1.0 to the maximum zoom supported by the device.</td>
</tr>
</tbody>
</table>


#### Sets the Manual Focus Position (setCameraFocusPositionInPreview)

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
<td>Horizontal coordinate of the touch point in the view.</td>
</tr>
<tr><td>positionY</td>
<td>Vertical coordinate of the touch point in the view.</td>
</tr>
</tbody>
</table>



#### Enables the Camera Flash (setCameraTorchOn)

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
<td>Whether or not to enable the camera flash function: True/False.</td>
</tr>
</tbody>
</table>



#### Enables the Camera Auto Focus Function (setCameraAutoFocusFaceModeEnabled)

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
<td>Whether or not to enable the camera auto focus function: True/False.</td>
</tr>
</tbody>
</table>





### Enables Web SDK Interoperability (enableWebSdkInteroperability)

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
<td><p>Whether or not to enable interoperability with the Agora Web SDK:</p>
<ul>
<li>True: Enable interoperability.</li>
<li>False: Disable interoperability.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


### Sets the Audio Route

#### Sets the Default Audio Route (setDefaultAudioRouteToSpeakerPhone)

```
- (int)setDefaultAudioRouteToSpeakerphone:(BOOL)defaultToSpeaker;
```

This method sets whether the received audio is routed to the earpiece or the speakerphone.


> -   Call this method only if you want to change the default settings.
> -   This method only works in the audio mode.
> -   Call this method before calling the `joinChannel` method.


If you do not call this method, the audio is routed to the earpiece by default.

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
<td>
<ul>
<li>YES: The audio is routed to the speakerphone.
</li>
<li>
NO: The audio is routed to the earpiece.
</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td>
<ul>
<li>
0: Method call succeeded.
</li>
<li>
&lt;0: Method call failed.
</li>
</ul>
</td>
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
<td><ul>
<li>Yes: Switches to the speaker.</li>
<li>No: Switches to the headset.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Checks Whether the Speakerphone is Enabled (isSpeakerphoneEnabled)

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
<li>Yes: Enabled.</li>
<li>No: Disabled.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Enables In-ear Monitoring (enableInEarMonitoring)

```
- (int)enableInEarMonitoring:(BOOL)enabled;
```

This method enables or disables in-ear monitoring.

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
<li>YES: Enables in-ear monitoring.</li>
<li>NO: (Default) Disables in-ear monitoring.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Sets the Audio Session Operational Restriction (setAudioSessionOperationRestriction)

```
- (void)setAudioSessionOperationRestriction:(AgoraAudioSessionOperationRestriction)restriction;
```

The SDK and the app can both configure the audio session by default. The app may occassionally use other applications or third-party components to manipulate the audio session and restrict the SDK from doing so. This method allows the app to restrict the SDK’s manipulation of the audio session.

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
<td><p>The operational restriction of the SDK on the audio session. It is a bit mask.</p>
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


### Sets the Audio Volume

#### Enables the Callback to Report on which Users are Speaking and the Speakers' Volume (enableAudioVolumeIndication)

```
- (int)enableAudioVolumeIndication:(NSInteger)interval
                            smooth:(NSInteger)smooth;
```

This method allows the SDK to regularly report to the app on which users are speaking and the volume of the speakers. Once the method is enabled, the SDK returns the volume indications at the set time internal in the `reportAudioVolumeIndicationOfSpeakers` and `audioVolumeIndicationBlock` callbacks, regardless of whether anyone is speaking in the channel.

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
<li>&lt;= 0: Disables the volume indication.</li>
<li>&gt; 0: The time interval (ms) between two consecutive volume indications. Agora recommends setting it to more than 200 ms. Do not set it lower than 10 ms, or the SDK does not trigger the <code>reportAudioVolumeIndicationOfSpeakers</code> callback.</li>
</ul>
</div>
</td>
</tr>
<tr><td>smooth</td>
<td>The smoothing factor that determines the sensitivity of this method. The value ranges between 0 and 10. Agora recommends setting it as 3. The bigger the value, the higher the sensitivity.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Sets the Volume of the In-ear Monitor (setInEarMonitoringVolume)

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
<td>Volume of the in-ear monitor. The value ranges between 0 and 100. The default value is 100.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Gets the Volume of the Audio Effect (getEffectsVolume)

```
- (double)getEffectsVolume;
```

This method gets the volume of the audio effect. The value ranges between 0.0 and 100.0.

#### Sets the Volume of the Audio Effect (setEffectsVolume)

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
<td>Volume of the audio effect. The value ranges between 0.0 and 100.0 (default).</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Adjusts the Audio Effect Volume in Real Time (setVolumeOfEffect)

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
<td>Volume of the specified audio effect. The value ranges between 0.0 and 100.0 (default).</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Plays an Audio Effect (playEffect)

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
<td>ID of the specified audio effect. Each audio effect has a unique ID <sup>[4]</sup>.</td>
</tr>
<tr><td>filePath</td>
<td>The absolute path of the audio effect file.</td>
</tr>
<tr><td>loopCount</td>
<td><p>Sets the number of times playing the audio effect.</p>
<ul>
<li>0: Plays the audio effect once.</li>
<li>1: Plays the audio effect twice.</li>
<li>-1: Plays the audio effect in a loop indefinitely until the SDK calls the <code>stopEffect</code> or <code>stopAllEffects</code> method.</li>
</ul>
</td>
</tr>
<tr><td>pitch</td>
<td>Sets whether to change the pitch of the audio effect. The value ranges between 0.5 and 2. The default value is 1 (no change to the pitch). The smaller the value, the lower the pitch.</td>
</tr>
<tr><td>pan</td>
<td><p>Spatial position of the local audio effect. The value ranges between -1.0 and 1.0.</p>
<ul>
<li>0.0: The audio effect shows ahead.</li>
<li>1.0: The audio effect shows on the right.</li>
<li>-1.0: The audio effect shows on the left.</li>
</ul>
</td>
</tr>
<tr><td>gain</td>
<td>Volume of the audio effect. The value ranges between 0.0 and 100,0. The default value is 100.0. The smaller the value, the lower the volume of the audio effect.</td>
</tr>
<tr><td>publish</td>
<td><p>Sets whether or not to publish the specified audio effect to the remote stream:</p>
<ul>
<li>True: The audio effect, played locally, is published to the Agora Cloud and the remote users can hear it.</li>
<li>False: The audio effect, played locally, is not published to the Agora Cloud and the remote users cannot hear it.</li>
</ul>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

> [4] If you preloaded the audio effect into the memory through the `preloadEffect` method, ensure that the `soundID` value is set as the same value as in the `preloadEffect` method.

> In v2.1.x, the following method (not recommended by Agora) is used.

```
- (int) playEffect: (int) soundId
          filePath: (NSString*) filePath
              loop: (BOOL) loop
             pitch: (double) pitch
               pan: (double) pan
              gain: (double) gain;
```

#### Stops Playing an Audio Effect (stopEffect)

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stops Playing all Audio Effects (stopAllEffects)

```
- (int)stopAllEffects;
```

This method stops playing all the audio effects.

#### Preload an Audio Effect (preloadEffect)

```
- (int)preloadEffect:(int)soundId
            filePath:(NSString * _Nullable) filePath;
```

This method preloads the specified audio effect file (compressed audio file) to the memory.


> To ensure smooth communication, note the size of the audio effect file. Agora recommends using this method to preload the audio effect before you call the  `joinChannelByToken` method.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Releases an Audio Effect (unloadEffect)

```
- (int)unloadEffect:(int)soundId;
```

This method releases the specified preloaded audio effect from the memory.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Pauses an Audio Effect (pauseEffect)

```
- (int)pauseEffect:(int)soundId;
```

This method pauses the specified audio effect.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Pauses all Audio Effects (pauseAllEffects)

```
- (int)pauseAllEffects;
```

This method pauses all audio effects.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Resumes Playing an Audio Effect (resumeEffect)

```
- (int)resumeEffect:(int)soundId;
```

This method resumes playing the specified audio effect.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Resumes Playing all Audio Effects (resumeAllEffects)

```
- (int) esumeAllEffects;
```

This method resumes playing all audio effects.


### Mutes the Audio and Video Streams

#### Mutes the Local Audio Stream (muteLocalAudioStream)

```
- (int)muteLocalAudioStream:(BOOL)mute;
```

This method mutes/unmutes the local audio stream.

> This method works only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mutes all Remote Audio Streams (muteAllRemoteAudioStreams)

```
- (int)muteAllRemoteAudioStreams:(BOOL)mute;
```

This method mutes/unmutes all remote audio streams.

> This method works only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<li>True: Stops playing all remote audio streams.</li>
<li>False: Plays all remote audio streams.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mutes the Specified Remote User's Audio Stream (muteRemoteAudioStream)

```
- (int)muteRemoteAudioStream:(NSUInteger)uid muted:(BOOL)mute;
```

The method mutes/unmutes the specified remote user’s audio stream.

> When `mute` is set as `True`, this method stops playing audio streams without affecting the audio stream receiving process.
> This method works only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<td>User ID of the remote user.</td>
</tr>
<tr><td>mute</td>
<td><ul>
<li>True: Stops playing the specified remote user’s audio stream.</li>
<li>False: Plays the specified remote user’s audio stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Mutes the Local Video Stream (muteLocalVideoStream)

```
- (int)muteLocalVideoStream:(BOOL)muted;
```

This method enables/disables sending a local video stream to the network.

> When `muted` is set as `YES`, this method does not disable the camera and thus does not affect the retrieval of the local video stream. This method responds faster than the `enableLocalVideo` method with `enabled` set as `YES`.
> This method works only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<li>No: Sends the local video stream to the network.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mutes all Remote Video Streams (muteAllRemoteVideoStreams)

```
- (int)muteAllRemoteVideoStreams:(BOOL)mute;
```

This method enables/disables playing all remote video streams.


> When `mute` is set as `Yes`, this method stops playing video streams without affecting the video stream receiving process.
> This method works only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<li>Yes: Stops playing all remote video streams.</li>
<li>No: Plays all remote video streams.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mutes the Specified Remote User's Video Stream (muteRemoteVideoStream)

```
- (int)muteRemoteVideoStream:(NSUInteger)uid
                        mute:(BOOL)mute;
```

This method pauses/resumes receiving the specified remote user’s video stream.

> When `mute` is set as `Yes`, this method stops playing video streams without affecting the video stream receiving process.
> This method works only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<li>No: Plays a specified user’s video stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Audio Mixing

#### Starts Audio Mixing (startAudioMixing)

```
- (int)startAudioMixing:(NSString *  _Nonnull)filePath
               loopback:(BOOL)loopback
                replace:(BOOL)replace
                  cycle:(NSInteger)cycle;
```

This method mixes the specified local audio file with the audio stream from the microphone; or, it replaces the microphone’s audio stream with the specified local audio file. You can choose whether the other user can hear the local audio playback and specify the number of loop playbacks. This method also supports online music playback.


> -   To use this method, ensure the iOS device is 8.0 or later.
> -   Call this method when you are in a channel. Otherwise, it may cause issues.


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
<td>Name and path of the local audio file to be mixed. <li>Supported audio formats: mp3, aac, m4a, 3gp, and wav.</li></td>
</tr>
<tr><td>loopback</td>
<td>
<ul><li>True: Only the local user can hear the remix or the replaced audio stream.</li>
<li>False: Both users can hear the remix or the replaced audio stream.</li></ul>
</td>
</tr>
<tr><td>replace</td>
<td>
<ul><li>True: The content of the local audio file replaces the audio stream from the microphone.</li>
<li>False: Mix the local audio file with the audio stream from the microphone.</li>
</tr>
<tr><td>cycle</td>
<td>Number of loop playbacks:
<ul><li>Positive integer: Number of loop playbacks.</li>
<li>-1：Infinite loop.</li></ul>
</td>
</tr>
<tr><td>Return Value</td>
<td>
<ul><li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li></ul>
</td>
</tr>
</tbody>
</table>



#### Stops Audio Mixing (stopAudioMixing)

```
- (int)stopAudioMixing;
```

This method stops audio mixing. Call this method when you are in a channel.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Pauses Audio Mixing (pauseAudioMixing)

```
- (int)pauseAudioMixing;
```

This method pauses audio mixing. Call this method when you are in a channel.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Resumes Audio Mixing (resumeAudioMixing)

```
- (int)resumeAudioMixing;
```

This method resumes audio mixing. Call this method when you are in a channel.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Adjusts the Audio Mixing Volume (adjustAudioMixingVolume)

```
- (int)adjustAudioMixingVolume:(NSInteger)volume;
```

This method adjusts the volume during audio mixing. Call this method when you are in a channel.

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
<td>The audio mixing volume. The value ranges between 0 and 100. By default, 100 is the original volume.</td>
</tr>
<tr><td>Return Value</td>
<td>
<ul><li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li></ul>
</td>
</tr>
</tr>
</tbody>
</table>



#### Gets the Audio Mixing Duration (getAudioMixingDuration)

```
- (int)getAudioMixingDuration;
```

This method gets the duration (ms) of audio mixing. Call this method when you are in a channel.

#### Gets Audio Mixing Playback Position (getAudioMixingCurrentPosition)

```
- (int)getAudioMixingCurrentPosition;
```

This method gets the playback position (ms) of the audio mixing. Call this method when you are in a channel.

#### Sets the Playback Starting Position of the Audio Mixing File (setAudioMixingPosition)

```
- (int)setAudioMixingPosition:(NSInteger)pos;
```

This method sets the playback starting position of the audio mixing file to where you want to play from instead of from the beginning.

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
<td>Integer. The playback starting position of the audio mixing file (ms).</td>
</tr>
</tbody>
</table>



### Recording

#### Starts an Audio Recording (startAudioRecording)

```
- (int)startAudioRecording:(NSString * _Nonnull)filePath
                   quality:(AgoraAudioRecordingQuality)quality;
```

This method starts an audio recording. The SDK allows recording during a call, which supports the following formats:

-   *.wav*: Large file size with high fidelity.
-   *.aac*: Small file size with low fidelity.


Ensure that the directory to save the recording exists and is writable. Call this method after the calling the `joinChannelByToken` method. The recording automatically stops when you call the `leaveChannel` method.

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
<td>Full file path of the recording file. The string of the file name is in UTF-8.</td>
</tr>
<tr><td>quality</td>
<td><p>Audio recording quality:</p>
<ul>
<li><code>AgoraRtc_AudioRecordingQuality_Low = 0</code>: Low quality, the file size is around 1.2 MB after 10 minutes of recording.</li>
<li><code>AgoraRtc_AudioRecordingQuality_Low = 1</code>: Medium quality, the file size is around 2 MB after 10 minutes of recording.</li>
<li><code>AgoraRtc_AudioRecordingQuality_Low = 2</code>: High quality, the file size is around 3.75 MB after 10 minutes of recording.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stops an Audio Recording (stopAudioRecording)

```
- (int)stopAudioRecording;
```

This method stops recording on the client.


> Call this method before calling the `leaveChannel` method. Otherwise, the recording automatically stops when you call the `leaveChannel` method.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Adjusts the Recording Volume (adjustRecordingSignalVolume)

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
<td><p>Recording volume. The value ranges between 0 and 400:</p>
<ul>
<li>0: Mute.</li>
<li>100: Original volume.</li>
<li>400: (Maximum) Four times the original volume with signal clipping protection.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Adjusts the Playback Volume (adjustPlaybackSignalVolume)

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
<td><p>Playback volume. The value ranges between 0 and 400:</p>
<ul>
<li>0: Mute.</li>
<li>100: Original volume.</li>
<li>400: (Maximum) Four times the original volume with signal clipping protection.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

### Encryption

#### Enables Built-in Encryption with an Encryption Password (setEncryptionSecret)

```
- (int)setEncryptionSecret:(NSString * _Nullable)secret;
```

This method specifies an encryption password to enable built-in encryption before joining a channel. All users in a channel must set the same encryption password. The encryption password is automatically cleared once a user leaves the channel. If the encryption password is not specified or set to empty, the encryption function is disabled.


> Do not use this function in CDN live.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Sets the Built-in Encryption Mode (setEncryptionMode)

```
- (int)setEncryptionMode:(NSString * _Nullable)encryptionMode;
```

The Agora Native SDK supports built-in encryption, which is in AES-128-XTS mode by default. If you want to use other modes, call this method to set the encryption mode.

All users in the same channel must use the same encryption mode and password. Refer to the information related to the AES encryption algorithm on the differences between encryption modes.

Call the `setEncryptionSecret` method to enable the built-in encryption function before calling this method.


> Do not use this function in CDN live.

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
<td><p>Encryption mode. The following modes are supported:</p>
<ul>
<li>“aes-128-xts”: 128-bit AES encryption, XTS mode.</li>
<li>“aes-128-ecb”: 128-bit AES encryption, ECB mode.</li>
<li>“aes-256-xts”: 256-bit AES encryption, XTS mode.</li>
<li>“”: When set as NUL string, the encryption is in “aes-128-xts” by default.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Establishes a Data Stream

#### Creates a Data Stream (createDataStream)

```
- (int)createDataStream:(NSInteger * _Nonnull)streamId
               reliable:(BOOL)reliable
                ordered:(BOOL)ordered;
```

This method creates a data stream. Each user can only have up to five data channels at the same time.


> Set the `reliable` and `ordered` parameters both as `True` or both as `False`. Do not set one as `True` and the other as `False`.

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
<li>True: The recipients receive the data from the sender within 5 seconds. If the recipient does not receive the sent data within 5 seconds, the data channel reports an error to the app.</li>
<li>False: The recipients may not receive any data, and the SDK does not report any error upon data missing.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>ordered</td>
<td><ul>
<li>True: The recipients receive data in the sent order.</li>
<li>False: The recipients do not receive data in the sent order.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>&lt;0 : Returns an error code when it fails to create the data stream. <sup>[5]</sup></li>
<li>&gt;0 : Returns the Stream ID when the data stream is created.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

> [5] The error code is related to the positive integer displayed in [Error Codes and Warning Codes](../../en/API%20Reference/the_error_native.md). For example, if it returns -2, then it indicates 2: ERR_INVALID_ARGUMENT in [Error Codes and Warning Codes](../../en/API%20Reference/the_error_native.md).

#### Sends a Data Stream (sendStreamMessage)

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
<td>Data to be sent.</td>
</tr>
<tr><td>Return Value</td>
<td>When it fails to send the message, the following error code is returned: ERR_SIZE_TOO_LARGE/ERR_TOO_OFTEN/ERR_BITRATE_LIMIT</td>
</tr>
</tbody>
</table>



### Test and Detection

#### Starts an Audio Call Test (startEchoTest)

```
- (int)startEchoTest:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))successBlock;
```

This method launches an audio call test to determine whether the audio devices (for example, the headset and speaker) and the network connection are working properly. In this test, the user first speaks and the recording is played back in 10 seconds. If the user can hear the recording in 10 seconds, the audio devices and network connection work properly.

> -   After calling this method, call the `stopEchoTest` method to end the test. Otherwise, the app cannot run the next echo test, nor can it call the `joinChannel` method to start a new call.
> -   This method is applicable only when the user role is `broadcaster`. If you switch the channel profile from Communication to Live Broadcast, call the `setClientRole` method to change the user role from `audience` (the default setting in the live broadcast profile) to `broadcaster` before using this method.


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
<td>Callback on successfully starting the echo test. See <code>joinSuccessBlock</code> in <code>joinChannelByToken</code> for a description of the callback parameters.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure:
	<ul>
		<li>ERR_REFUSED (-5): Failed to launch the echo test, for example, initialization failed.</li>
	</ul>
	</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Stops the Audio Call Test (stopEchoTest)

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
<li>0: Success.</li>
<li>&lt; 0: Failure:
	<ul>
		<li>ERR_REFUSED(-5): Fails to stop the echo test. The echo test may not be running.</li>
	</ul>
	</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Enables the Network Test (enableLastmileTest)

```
- (int)enableLastmileTest;
```

This method tests the quality of the user’s network connection and is disabled by default.

This method is mainly used in two scenarios:

-   Before users join a channel, this method can be called to check the uplink network quality.
-   Before the audience in a channel switch to a host role, this method can be called to check the uplink network quality.

In both scenarios, calling this method consumes extra network traffic, which affects the communication quality.

Call the `disableLastmileTest` method to disable this test immediately once the users have received the `onLastmileQuality` callback before they join the channel or switch user roles.

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
<tr><td>callId</td>
<td>Call ID retrieved from the <code>getCallId</code> method.</td>
</tr>
<tr><td>rating</td>
<td>Rating for the call. The value ranges between 1 (lowest score) and 10 (highest score).</td>
</tr>
<tr><td>description</td>
<td>(Optional) A description of the call with a length less of than 800 bytes.</td>
</tr>
<tr><td>Return Value</td>
<td>
<ul>
<li>0: Succeess.</li>
<li>&lt; 0: Failure.</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid. For example, callId is invalid.
</li>
<li>
ERR_NOT_READY (-3): The SDK status is incorrect. For example, initialization failed.
</li>
</ul>
</ul>
</td>
</tr>
</tbody>
</table>



#### Disables the Network Test (disableLastmileTest)

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Feedback

#### Retrieves the Current Call ID (getCallId)

```
- (NSString * _Nullable)getCallId;
```

When a user joins a channel on a client, `callId` is generated to identify the call from the client. You can call the `rate` and `complain` methods after a call ends to submit feedback to the SDK. These feedback methods require assigned values of the `callId` parameters. To use these feedback methods, call this method to retrieve the `callId` during the call, and then pass the value as an argument in the feedback methods after the call ends.

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



#### Allows a User to Rate a Call (rate)

```
- (int)rate:(NSString * _Nonnull)callId
     rating:(NSInteger)rating
description:(NSString * _Nullable)description;
```

This method allows a user rate a call and is called after the call ends.

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
<td>Call ID retrieved from the <code>getCallId</code> method.</td>
</tr>
<tr><td>rating</td>
<td>Rating for the call. The value ranges between 1 (lowest score) and 10 (highest score).</td>
</tr>
<tr><td>description</td>
<td>(Optional) A description of the call with a length less of than 800 bytes.</td>
</tr>
<tr><td>Return Value</td>
<td>
<ul><li>0: Succeess.</li>
<li>&lt; 0: Failure.</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid. For example, <code>callId</code> is invalid.</li>
<li>ERR_NOT_READY (-3): The SDK status is incorrect. For example, initialization failed.</li>
</ul>	
</ul>
</td>
</tr>
</tbody>
</table>



#### Allows a User to Complain about the Call Quality (complain)

```
- (int)complain:(NSString * _Nonnull)callId
    description:(NSString * _Nullable)description;
```

This method allows a user to complain about the call quality and is called after a call ends.

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
<td>Call ID retrieved from the <code>getCallId</code> method.</td>
</tr>
<tr><td>description</td>
<td>(Optional) A description of the call with a length of less than 800 bytes.</td>
</tr>
<tr><td>Return Value</td>
<td>
<ul>
<li>0: Succeess.</li>
<li>&lt; 0: Failure.</li>
<ul><li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid. For example, <code>callId</code> is invalid.</li>
<li>ERR_NOT_READY (-3): The SDK status is incorrect. For example, initialization failed.</</li></ul>
</ul>
</td>
</tr>
</tbody>
</table>



### Added Functions

#### Renews the Token (renewToken)

```
- (int)renewToken:(NSString * _Nonnull)token;
```

This method renews the token.

The token expires after a time period once the token schema is enabled when:

-   The `rtcEngine:didOccurError:` callback reports the ERR_TOKEN_EXPIRED(109) error, or
-   The `rtcEngineRequestToken:` callback reports the ERR_TOKEN_EXPIRED(109) error.

The app should retrieve a new token and then call this method to renew it. Failure to do so results in the SDK disconnecting from the server.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Specifies a Log File (setLogFile)

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
<td>File path of the log file. The string of the log file is in UTF-8.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


> The default log file location is at: sdcard/<appname\>/agorasdk.log, where `appname` is the name of the app.

#### Sets the Log Filter (setLogFilter)

```
- (int)setLogFilter:(NSUInteger)filter;
```

This method sets the output log levels of the SDK. You can use either one or a combination of the filters.

The log level follows the sequence of `Off`, `Critical`, `Error`, `Warning`, `Info`, and `Debug`. Choose a level, and you can see the logs that precede that level.

For example, if you set the log level as `Warning`, then you can see the logs in levels `Critical`, `Error`, and `Warning`.

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
<li>AgoraLogFilterDebug = 0x080f: Output all API logs.</li>
<li>AgoraLogFilterInfo = 0x000f: Output logs of the CRITICAL, ERROR, WARNING, and INFO levels.</li>
<li>AgoraLogFilterWarning = 0x000e: Output logs of the CRITICAL, ERROR, and WARNING levels.</li>
<li>AgoraLogFilterError = 0x000c: Output logs of the CRITICAL and ERROR levels.</li>
<li>AgoraLogFilterCritical = 0x0008: Output logs of the CRITICAL level.</li>
</ul>
</div>
</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### Destroys the Engine Instance (destroy)

```
+ (void)destroy;
```

This method releases all resources used by the Agora SDK. This is useful for apps that occasionally make voice or video calls, to free up resources for other operations when not making calls.

Once the app calls the `destroy` method to destroy the created `RtcEngine` instance, no other methods in the SDK can be used and no callbacks occur. To start communications again, initialize `sharedEngineWithappId` to establish a new `AgoraRtcEngineKit` instance.


> -   Use this method in the subthread.
> -   This method call is synchronous. The result returns after the `IRtcEngine` object resources are released. The app should not call this interface in the callback generated by the SDK. Otherwise, the SDK must wait for the callback to return before it can reclaim the related object resources, causing a deadlock.


### Gets the SDK Version (getSdkVersion)

```
+ (NSString * _Nonnull)getSdkVersion;
```

This method returns the string of the SDK version number.


<a id = "rtcenginedelegate"></a>
## Delegate Methods (AgoraRtcEngineDelegate)

The SDK uses Delegate methods `AgoraRtcEngineDelegate` to report callbacks to the app. From v1.1, some Block callbacks in the SDK are replaced with Delegate callbacks. The old Block callbacks are therefore deprecated, but can still be used in the current version. However, Agora recommends replacing them with Delegate callbacks. The SDK calls the Block callback if a callback is defined in both Block and Delegate.

The callbacks are returned in the main thread.

#### Reports a Warning during SDK Runtime (didOccurWarning)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOccurWarning:(AgoraRtcErrorCode)warningCode;
```

This callback reports a warning during SDK runtime. In most cases, the app can ignore the warning reported by the SDK because the SDK can usually fix the issue and resume running.

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



#### Reports an Error during SDK Runtime (didOccurError)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOccurError:(AgoraRtcErrorCode)errorCode;
```

This callback reports an error during SDK runtime. In most cases, reporting an error means that the SDK cannot fix the issue and resume running, and therefore requires actions from the app or simply informs the user on the issue. For instance, the SDK reports an `AgoraRtc_Error_StartCall(1002)` error when failing to initialize a call. In this case, the app informs the user that the call initialization failed and calls the `leaveChannel` method to exit the channel.

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
<td>The <code>AgoraRtcEngineKit</code> object.</td>
</tr>
<tr><td>errorCode</td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### Occurs when an API Method Executes (didApiCallExecute)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didApiCallExecute:(NSInteger)error api:(NSString * _Nonnull)api result:(NSString * _Nonnull)result;
```

The SDK triggers this callback when an API method executes.

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
<td>The error code that the SDK returns when the method call fails. If the SDK returns 0, then the method call is successful.</td>
</tr>
<tr><td>api</td>
<td>The API method that the SDK executes.</td>
</tr>
<tr><td>result</td>
<td>The result of calling the API method.</td>
</tr>
</tbody>
</table>



#### Occurs when a User Joins a Channel (didJoinChannel)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didJoinChannel:(NSString * _Nonnull)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
```

This callback is the same as `joinSuccessBlock` in the `joinChannelByToken` method. The SDK triggers this callback when a user has successfully joined a specified channel.

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
	<p>If the uid is specified in the <code>joinChannelByToken</code> method, the SDK returns the specified ID. If not, the SDK returns a uid that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td>elapsed</td>
	<td>Time elapsed (ms) from calling the <code>joinChannelByToken</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when a User Rejoins a Channel (didRejoinChannel)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didRejoinChannel:(NSString * _Nonnull)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
```

If the client loses connection with the server because of network problems, the SDK automatically attempts to reconnect, and triggers this callback upon reconnection, indicating that the user has rejoined the channel with the assigned channel ID and user ID.

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
<td>Time elapsed (ms) from calling the <code>joinChannelByToken</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when a User Leaves a Channel (didLeaveChannelWithStats)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didLeaveChannelWithStats:(AgoraChannelStats * _Nonnull)stats;
```

When a user calls the `leaveChannel` method, the SDK trigger this callback. This callback is the same as `leaveChannelBlock`.

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
<li>duration: Call duration (s), represented by an aggregate value.</li>
<li>txBytes: Total number of bytes transmitted, represented by an aggregate value.</li>
<li>rxBytes: Total number of bytes received, represented by an aggregate value.</li>
<li>txAudioKBitrate: Audio transmission bitrate in kbit/s, represented by an instantaneous value.</li>
<li>rxAudioKBitRate: Audio receiving bitrate in kbit/s, represented by an instantaneous value.</li>
<li>txVideoKBitRate: Video transmission bitrate in kbit/s, represented by an instantaneous value.</li>
<li>rxVideoKBitRate: Video receiving bitrate in kbit/s, represented by an instantaneous value.</li>
<li>lastmileDelay: The time delay (ms) from the client to the VO server.</li>
<li>users: The number of users in the channel when the user leaves the channel.</li>
<li>puTotalUsage: System CPU usage (%).</li>
<li>cpuAppUsage: Application CPU usage (%).</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Occurs when the Audio Route Changes (didAudioRouteChanged)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioRouteChanged:(AgoraAudioOutputRouting)routing;
```

The SDK triggers this callback when the audio route changes. Here is the definition of `AgoraAudioOutputRouting`:

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

#### Occurs when the Microphone is Enabled/Disabled (didMicrophoneEnabled)

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
		<li>True: The microphone is enabled.</li>
		<li>False: The microphone is disabled.</li>
	</ul>
	</td>
</tr>
</tbody>
</table>


#### Reports the Audio Quality of the Current Call (audioQualityOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine audioQualityOfUid:(NSUInteger)uid quality:(AgoraNetworkQuality)quality delay:(NSUInteger)delay lost:(NSUInteger)lost;
```

This callback is the same as `audioQualityBlock`. During a call, the SDK triggers this callback once every two seconds to report on the audio quality of the current call.

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
<td>User ID of the speaker.</td>
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
<td>Time delay (ms).</td>
</tr>
<tr><td>lost</td>
<td>The packet loss rate(%).</td>
</tr>
</tbody>
</table>



#### Reports which Users are Speaking and the Speakers' Volume (reportAudioVolumeIndicationOfSpeakers)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine reportAudioVolumeIndicationOfSpeakers:(NSArray<AgoraRtcAudioVolumeInfo *> * _Nonnull)speakers totalVolume:(NSInteger)totalVolume;
```

The callback is the same as `audioVolumeIndicationBlock`. By default this callback is disabled. You can use the `enableAudioVolumeIndication` method to enable/disable this callback.

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
<li>uid: User ID of the speaker. By default, 0 is the local user.</li>
<li>volume: The volume of the speaker. The value ranges between 0 (lowest volume) and 255 (highest volume).</li>
</ul>
</td>
</tr>
<tr><td>totalVolume</td>
<td>Total volume after audio mixing. The volume ranges between 0 (lowest volume) and 255 (highest volume).</td>
</tr>
</tbody>
</table>

In the returned speakers array:

-   If `uid` is 0 (the local user is the speaker) the returned `volume` is the same as `totalVolume`.
-   If `uid` is not 0 and `volume` is 0, the user specified by the uid did not speak.
-   If a uid is in the previous speakers array but not in the current one, the user specified by the uid did not speak.


#### Occurs when a Host Joins the Channel (didJoinedOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didJoinedOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

This callback is the same as `userJoinedBlock` and notifies the app the host has joined the channel. If the host is already in the channel when the app joins the channel, the SDK also reports to the app on the hosts who are already in the channel.


> In the live broadcast scenario:
> -   The host receives this callback when another host joins the channel.
> -   All the audience members in the channel can receive this callback when a new host joins the channel.
> -   When a Web application joins the channel, the SDK triggers this callback as long as the Web application publishes streams.


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
<p>If the uid is specified in the <code>joinChannelByToken</code> method, the SDK returns the specified ID. If not, the SDK returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannelByToken</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when a Host Leaves a Channel (didOfflineOfUid)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didOfflineOfUid:(NSUInteger)uid reason:(AgoraUserOfflineReason)reason;
```

This callback is the same as `userOfflineBlock` and reports that the host has left the call or gone offline.

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
<td><p>The SDK triggers this callback when:</p>
<ul>
<li>AgoraRtc_UserOffline_Quit(0): A user has quit the call.</li>
<li>AgoraRtc_UserOffline_Dropped(1): The SDK timed out and the user dropped offline because it has not received any data package within a time period. If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the event has timed out.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Occurs when a Remote User’s Audio Stream is Muted/Unmuted. (didAudioMuted)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

This callback is the same as `userMuteAudioBlock` and reports that a remote user's audio stream is muted/unmuted.

> This callback returns invalid when the number of hosts in a channel exceeds 20.

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
<td>
<ul>
<li>Yes: The remote user's audio is muted.</li>
<li>No: The remote user's audio is unmuted.</li>
</ul>
</td>
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
<td>
<ul>
<li>Yes: User has muted his/her audio.</li>
<li>No: User has unmuted his/her audio.</li>
</ul>
</td>
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
<td>
<ul>
<li>Yes: User has paused his/her video.</li>
<li>No: User has unmuted his/her video.</li>
</ul>
</td>
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
<td>User ID. If the uid is specified in the joinChannelByToken method, returns the specified ID; if not, returns an ID that is automatically allocated by the Agora server.</td>
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
<tr><td>uid</td>
<td>User ID of the speaker</td>
</tr>
<tr><td>volume</td>
<td>Volume of the speaker, between 0 (lowest volume) to 255 (highest volume).</td>
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
<td>
<ul><li>Yes: Muted audio.</li>
<li>No: Unmuted audio.</li></ul>
</td>
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
<td>
<ul><li>Yes: User has paused his/her video.</li>
<li>No: User has resumed his/her video.</li></ul>
</td>
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
<td>Rating of the network quality:
<ul>
<li>AgoraRtc_Quality_Unknown = 0</li>
<li>AgoraRtc_Quality_Excellent = 1</li>
<li>AgoraRtc_Quality_Good = 2</li>
<li>AgoraRtc_Quality_Poor = 3</li>
<li>AgoraRtc_Quality_Bad = 4</li>
<li>AgoraRtc_Quality_VBad = 5</li>
<li>AgoraRtc_Quality_Down = 6</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Connection Lost Callback (connectionLostBlock)

```
- (void)connectionLostBlock:(void(^)())connectionLostBlock;
```

This callback indicates that the SDK has lost network connection with the server.


