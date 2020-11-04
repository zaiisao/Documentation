
---
title: Interactive Gaming API
description: 
platform: Cocos
updatedAt: Wed Nov 04 2020 08:24:00 GMT+0800 (CST)
---
# Interactive Gaming API
This page provides the **C++ Interface**, with which you can integrate the voice and video function into your app. 


## Basic Methods (IRtcEngine)

**agora::IRtcEngine** is the basic interface class of the Agora Native SDK. Creating an agora::IRtcEngine object and then calling the methods of this object enables the use of Agora Native SDK’s communication functionality. In previous versions, this class was named IAgoraAudio and is renamed to agora::IRtcEngine from v1.0.

### Creates an agora::IRtcEngine Object (create)

```
agora::rtc::IRtcEngine* AGORA_CALL createAgoraRtcEngine();
```

This method creates an IRtcEngine object. Unless otherwise specified, all called methods provided by the RtcEngine class are executed asynchronously. Agora recommends calling the methods in the same thread. Unless otherwise specified, the following rule applies to all API methods whose return values are integer types: 
- A return value of 0 means that the method call is successful
- A return value of less than 0 means that the method call fails.

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
<td>An agora::IRtcEngine object.</td>
</tr>
</tbody>
</table>


### Initializes the Agora SDK Service (initialize)

```
virtual int initialize(const RtcEngineContext& context) = 0;
```

This method initializes the Agora SDK service. Enter the App ID issued to you to start initialization. After creating an `agora::IRtcEngine` object, call this method to initialize the service before using any other methods. After initialization, the service is set to the audio mode by default. To enable video mode, call the `enableVideo` method after calling this method.

The definition of RTCEngineContext is：

```
struct RtcEngineContext
{
    IRtcEngineEventHandler* eventHandler;
    /** App ID issued to you by Agora. Apply for a new one from Agora if it is missing from your kit.
    */
    const char* appId;
    RtcEngineContext()
    :eventHandler(NULL)
    ,appId(NULL)
    {}
};
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
<tr><td>appID</td>
<td>App ID issued to you by Agora. Apply for a new one from Agora if it is missing from your kit. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a> on to how to get an App ID.</td>
</tr>
<tr><td>eventHandler</td>
<td><code>IRtcEngineEventHandler</code> is an abstract class that provides default implementations. The SDK uses this class to report to the app on SDK runtime events (callbacks).</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure:
	<ul>
		<li>ERR_INVALID_VENDOR_KEY(-101): The entered App ID is invalid.</li>
		</ul>
	</li>
</ul>
</td>
</tr>
</tbody>
</table>


### Implements a Live Voice Broadcast

#### Sets a Channel Profile (setChannelProfile)

```
virtual int setChannelProfile(CHANNEL_PROFILE_TYPE profile) = 0;
```

This method configures the channel profile. Agora RtcEngine needs to know what scenario the application is in to apply different methods for optimization.

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
<td>Default setting. This is used in one-on-one calls, where all users in the channel can talk freely.</td>
</tr>
<tr><td>Live Broadcast</td>
<td>Live Broadcast. Host and audience roles that can be set by calling the setClientRole method.  The host sends and receives voice, while the audience receives voice only with the sending function disabled.</td>
</tr>
<tr><td>Gaming</td>
<td>Gaming Mode. Any user in the channel can talk freely. This mode uses the codec with low-power consumption and low bitrate by default.</td>
</tr>
</tbody>
</table>


> -   Only one profile can be used at the same time in the same channel. If you want to switch to another profile, use `release` to destroy the current `IRtcEngine` and create a new one using `create` and `initialize` before calling this method to set the new channel profile.
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
<li>CHANNEL_PROFILE_COMMUNICATION = 0: (Default) Communication</li>
<li>CHANNEL_PROFILE_LIVE_BROADCASTING = 1: Live Broadcast</li>
<li>CHANNEL_PROFILE_GAME = 2: Gaming</li>
</ul>
</div>
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


#### Sets the User Role (setClientRole)

```
virtual int setClientRole(CLIENT_ROLE_TYPE role) = 0;
```

In the live-broadcast profile, this method allows you to:
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
<td><p>The user role in a live broadcast:</p>
<ul>
<li>CLIENT_ROLE_BROADCASTER = 1: Host</li>
<li>CLIENT_ROLE_AUDIENCE = 2: (Default) Audience</li>
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



#### Enables Audio Mode (enableAudio)

```
virtual int enableAudio() = 0;
```

This method enables audio mode. Audio mode is enabled by default.

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


> This method sets to enable the internal engine, and still works after you call the `leaveChannel` method.


#### Disables/Re-enables the Local Audio Function (enableLocalAudio)

```
virtual int enableLocalAudio(bool enabled) = 0;
```

When an app joins a channel, the audio function is enabled by default. This method disables or re-enables the local audio function, that is, to stop or restart local audio capturing and handling.
This method does not affect receiving or playing the remote audio streams, and is applicable to scenarios where the user wants to receive the remote audio streams without sending any audio stream to other users in the channel.
The SDK triggers the `onMicrophoneEnabled` callback once the local audio function is disabled or re-enabled.

> - Call this method after calling the `joinChannel` method.
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
<li>True: Re-enable the local audio function, that is, to start local audio capturing and processing.</li>
<li>False: Disable the local audio function, that is, to stop local audio capturing and processing.</li>
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


#### Disables Audio Mode (disableAudio)

```
virtual int disableAudio() = 0;
```

This method disables audio mode.

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


> This method sets to disable the internal engine, and still works after you call the `leaveChannel` method.


#### Allows a User to Join a Channel (joinChannel)

```
virtual int joinChannel(const char* token, const char* channelId, const char* info, uid_t uid) = 0;
```

This method allows a user to join a channel. Users in the same channel can talk to each other; and multiple users in the same channel can start a group chat. Users using different App IDs cannot call each other. Once in a call, the user must call the `leaveChannel` method to exit the current call before joining another channel.

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
<p>This parameter is optional if the user uses a static key or App ID. In this case, pass NULL as the parameter value.</p>
<p>If the user uses a Channel key, Agora issues an additional App Certificate to you. You can then generate a user key using the algorithm and App Certificate provided by Agora for user authentication on the server.</p>
<p>In most cases, the static App ID suffices. For added security, use the Channel Key.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>channel</td>
<td><p>A string providing the unique channel name for the AgoraRTC session. The length must be within 64 bytes.</p>
<p>The following is the supported scope:
	<ul>
		<li>The 26 lowercase English letters: a to z</li>
		<li>The 26 uppercase English letters: A to Z</li>
		<li>The 10 numbers: 0 to 9</li>
		<li>Space</li>
		<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","</li>
	</ul></p>
</td>
</tr>
<tr/>
<tr><td>info</td>
<td>(Optional) Additional information about the channel that you add. <sup>[1]</sup> It can be set as a NULL String or channel related information. Other users in the channel do not receive this message.</td>
</tr>
<tr><td>uid</td>
<td>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If not assigned (or set to 0), the SDK assigns one and returns it in the <code>onJoinChannelSuccess</code> callback. Your app must record and maintain the returned value, as the SDK does not maintain it.</td>
</tr>
<tr><td>Return Value</td>
<td>
	<ul>
<li>0: Success.</li>
<li>&lt; 0: Failure:
<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid.</li>
<li>ERR_NOT_READY (-3): Initialization fails.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>


> [1] For example, when a host wants to customize the resolution and bitrate for a live broadcast channel with CDN live enabled, you can include them in this parameter in JSON format. For example, \{“owner”:true, …, “width”:300, “height”:400, “bitrate”:100\}. Only when neither `width`, `height`, and `bitrate` is 0 can the bitrate and resolution settings take effect.

#### Allows a User to Leave a Channel (leaveChannel)

```
virtual int leaveChannel() = 0;
```

This method allows a user to leave a channel, such as hanging up or exiting a call.

After joining a channel, the user must call this method to end the call before joining another one. This method releases all resources related to the call and is called asynchronously. The user has not actually left the channel when the method call returns. Once the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.


> If you call `release()` immediately after you call `leaveChannel`, the SDK interrupts the `leaveChannel` process, and does not trigger the `onLeaveChannel` callback.

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
<li>ERR_REFUSED (-5): Fails to leave the channel. Reasons being that the user is not currently in a call or is in the process of leaving the channel.</li>
</ul>
	</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>

#### Sets the Local Voice Pitch (setLocalVoicePitch)

```
int setLocalVoicePitch(double pitch);
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

#### Sets the Voice Position of the Remote User (setRemoteVoicePosition)

```
virtual int setRemoteVoicePosition(int uid, double pan, double gain) = 0;
```

This method sets the voice position of the remote user.

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
<td><p>User ID of the remote user</p>
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
<td><p>Sets whether to change the volume of a single audio effect. The value ranges between 0.0 and 100.0. The default value is 100.0. The smaller the number is, the lower the volume of the audio effect.</p>
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
virtual int setVoiceOnlyMode(bool enable) = 0;
```

This method sets to voice-only mode (transmits the audio stream only) and other streams are ignored, for example, the sound of the keyboard strokes.

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
<li>True: Enables voice-only mode.</li>
<li>False: Disables voice-only mode.</li>
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


#### Sets the Local Voice Equalization (setLocalVoiceEqualization)

```
int setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_FREQUENCY bandFrequency, int bandGain);
```

This method sets the local voice equalization effect.

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
<td>The band frequency. The value ranges between 0 and 9, representing the respective 10-band center frequencies of the voice effects, including 31, 62, 125, 500, 1k, 2k, 4k, 8k, and 16k Hz.</td>
</tr>
<tr><td>bandGain</td>
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



#### Sets the Local Voice Reverberation (setLocalVoiceReverb)

```
int setLocalVoiceReverb(AUDIO_REVERB_TYPE reverbKey, int value)
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
<tr><td>reverbKey</td>
<td>The reverberation key. This method contains five reverberation keys. For details, see the description of each value:</td>
</tr>
<tr><td>value</td>
<td><ul>
<li>AUDIO_REVERB_DRY_LEVEL = 0, (dB, [-20,10]), level of the dry signal</li>
<li>AUDIO_REVERB_WET_LEVEL = 1, (dB, [-20,10]), level of the early reflection signal (wet signal)</li>
<li>AUDIO_REVERB_ROOM_SIZE = 2, ([0, 100]), room size of the reflection</li>
<li>AUDIO_REVERB_WET_DELAY = 3, (ms, [0, 200]), length of the initial latency of the wet signal in ms</li>
<li>AUDIO_REVERB_STRENGTH = 4, ([0, 100]), length of the late reverberation</li>
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

### Manages the Audio Effects

#### Gets the Audio Effect Volume (getEffectsVolume)

```
int getEffectsVolume();
```

This method gets the volume of the audio effects. The value ranges between 0.0 and 100.0.

#### Sets the Audio Effect Volume (setEffectsVolume)

```
int setEffectsVolume(int volume);
```

This method sets the volume of the audio effects. The value ranges between 0.0 and 100.0.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>volume</td>
<td>The value ranges from 0.0 to 100.0 (default).</td>
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



#### Adjusts the Audio Effect Volume in Real Time (setVolumeOfEffect)

```
int setVolumeOfEffect(int soundId, int volume);
```

This method adjusts the volume of the specified sound effect in real time.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
<tr><td>volume</td>
<td>The value ranges from 0.0 to 100.0 (default).</td>
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



#### Plays the Audio Effect (playEffect)

```
int playEffect(int soundId, const char* filePath, int loopCount, double pitch, double pan, int gain, bool publish)
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
<td>ID of the specified audio effect. Each audio effect has a unique ID. <sup>[2]</sup></td>
</tr>
<tr><td>filePath</td>
<td>The absolute path of the audio effect file.</td>
</tr>
<tr><td>loopCount</td>
<td><p>Set the number of times looping the audio effect:</p>
<div><ul>
<li>0: Plays the audio effect once.</li>
<li>1: Plays the audio effect twice.</li>
<li>-1: Plays the audio effect in a loop indefinitely, until you call the <code>stopEffect</code> or <code>stopAllEffects</code> method.</li>
</ul>
</div>
</td>
</tr>
<tr><td>pitch</td>
<td>Sets whether to change the pitch of the audio effect. The value ranges between 0.5 and 2. The default value is 1, which means no change to the pitch. The smaller the value, the lower the pitch.</td>
</tr>
<tr><td>pan</td>
<td><p>Spatial position of the local audio effect. The value ranges between -1.0 and 1.0.</p>
<div><ul>
<li>0.0: The audio effect shows ahead.</li>
<li>1.0: The audio effect shows on the right.</li>
<li>-1.0: The audio effect shows on the left.</li>
</ul>
</div>
</td>
</tr>
<tr><td>gain</td>
<td>Volume of the audio effect. The range is [0.0, 100,0]
The default value is 100.0. The smaller the value, the lower the volume of the audio effect</td>
</tr>
<tr><td>publish</td>
<td><p>Sets whether or not to publish the specified audio effect to the remote stream:</p>
<div><ul>
<li>true: The audio effect, played locally, is published to the Agora Cloud and remote users can hear it.</li>
<li>false: The audio effect, played locally, is not published to the Agora Cloud and remote users cannot hear it.</li>
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



> [2] If you preloaded the audio effect into the memory through the `preloadEffect` method, ensure that the `soundID` value is set as the same value as in the `preloadEffect` method. 

#### Stops Playing an Audio Effect (stopEffect)

```
int stopEffect(int soundId);
```

This method stops playing a specific audio effect.

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



#### Stop Playing all Audio Effects (stopAllEffects)

```
int stopAllEffects();
```

This method stops playing all audio effects.

#### Preload an Audio Effect (preloadEffect)

```
int preloadEffect(int soundId, String filePath);
```

This method preloads a specific audio effect file (compressed audio file) to the memory.


> To ensure smooth communication, pay attention to the size of the audio effect file. Agora recommends using this method to preload the audio effect before calling the `joinChannel` method.

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
int unloadEffect(int soundId);
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
int pauseEffect(int soundId);
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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Pause all Audio Effects (pauseAllEffects)

```
int pauseAllEffects();
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



#### Resume an Audio Effect (resumeEffect)

```
int resumeEffect(int soundId);
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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Resume Playing all Audio Effects (resumeAllEffects)

```
int resumeAllEffects();
```

This method resumes playing all audio effects.

### Implements a Video Live Broadcast

#### Enables the Video Mode (enableVideo)

```
virtual int enableVideo() = 0;
```

You can call this method either before joining a channel or during a call. If you call this method before joining a channel, the service starts in the video mode. If you call this method during a call, the service switches from the audio to video mode. To disable the video mode, call the `disableVideo` method.

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

> This method sets to enable the internal engine, and still works after you call the `leaveChannel` method.

#### Disables the Video Mode (disableVideo)

```
virtual int disableVideo() = 0;
```

This method disables the video mode. You can call this method either before joining a channel or during a call. If you call this method before joining a channel, the service starts in the audio mode. If you call this method during a call, the service switches from the video to audio mode. To enable the video mode, call the `enablevideo` method.

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

> This method sets to disable the internal engine, and still works after you call the `leaveChannel` method.

#### Enables the Local Video (enableLocalVideo)

```
int enableLocalVideo(bool enabled);
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
<li>true: (Default) Enable the local video.</li>
<li>false: Disable the local video. Once the local video is disabled, remote users can no longer receive the video stream of this user, while this user can still receive the video streams of other remote users. When you set <code>enabled</code> as <code>false</code>, this method does not require a local camera.</li>
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


> This method enables/disables the internal engine, and still works after you call the `leaveChannel` method.

#### Sets the Video Profile (setVideoProfile)

```
virtual int setVideoProfile(VIDEO_PROFILE_TYPE profile, bool swapWidthAndHeight) = 0;
```

This method sets the video encoding profile. Each profile includes a set of parameters, such as the resolution, frame rate, and bitrate. When the camera device does not support the specified resolution, the SDK automatically chooses a suitable camera resolution, while the encoder resolution is still the one specified by this method.

If the user does not need to set the video encoding profile after joining a channel, Agora recommends calling this method before calling the `enableVideo` method to minimize the time delay in receiving the first video frame.

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
<td>The video profile. See the following table for details.</td>
</tr>
<tr><td>swapWidthAndHeight</td>
<td><p>The width and height of the output video are consistent with that of the video profile you set. This parameter sets whether to swap the width and height of the stream:</p>
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

**Video profile definition**

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Video Profile</td>
<td>Enumeration</td>
<td>Resolution (Width x Height)</td>
<td>Frame Rate（fps）</td>
<td>Bitrate</td>
</tr>
<tr><td>120P</td>
<td>0</td>
<td>160x120</td>
<td>15</td>
<td>65</td>
</tr>
<tr><td>120P_3</td>
<td>2</td>
<td>120x120</td>
<td>15</td>
<td>50</td>
</tr>
<tr><td>180P</td>
<td>10</td>
<td>320x180</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>180P_3</td>
<td>12</td>
<td>180x180</td>
<td>15</td>
<td>100</td>
</tr>
<tr><td>180P_4</td>
<td>13</td>
<td>240x180</td>
<td>15</td>
<td>120</td>
</tr>
<tr><td>240P</td>
<td>20</td>
<td>320x240</td>
<td>15</td>
<td>200</td>
</tr>
<tr><td>240P_3</td>
<td>22</td>
<td>240x240</td>
<td>15</td>
<td>140</td>
</tr>
<tr><td>240P_4</td>
<td>24</td>
<td>424x240</td>
<td>15</td>
<td>220</td>
</tr>
<tr><td>360P</td>
<td>30</td>
<td>640x360</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>360P_3</td>
<td>32</td>
<td>360x360</td>
<td>15</td>
<td>260</td>
</tr>
<tr><td>360P_4</td>
<td>33</td>
<td>640x360</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>360P_6</td>
<td>35</td>
<td>360x360</td>
<td>30</td>
<td>400</td>
</tr>
<tr><td>360P_7</td>
<td>36</td>
<td>480x360</td>
<td>15</td>
<td>320</td>
</tr>
<tr><td>360P_8</td>
<td>37</td>
<td>480x360</td>
<td>30</td>
<td>490</td>
</tr>
<tr><td>360P_9</td>
<td>38</td>
<td>640x360</td>
<td>15</td>
<td>800</td>
</tr>
<tr><td>360P_10</td>
<td>39</td>
<td>640x360</td>
<td>24</td>
<td>800</td>
</tr>
<tr><td>360P_11</td>
<td>100</td>
<td>640x360</td>
<td>24</td>
<td>1000</td>
</tr>
<tr><td>480P</td>
<td>40</td>
<td>640x480</td>
<td>15</td>
<td>500</td>
</tr>
<tr><td>480P_3</td>
<td>42</td>
<td>480x480</td>
<td>15</td>
<td>400</td>
</tr>
<tr><td>480P_4</td>
<td>43</td>
<td>640x480</td>
<td>30</td>
<td>750</td>
</tr>
<tr><td>480P_6</td>
<td>45</td>
<td>480x480</td>
<td>30</td>
<td>600</td>
</tr>
<tr><td>480P_8</td>
<td>47</td>
<td>848x480</td>
<td>15</td>
<td>610</td>
</tr>
<tr><td>480P_9</td>
<td>48</td>
<td>848x480</td>
<td>30</td>
<td>930</td>
</tr>
<tr><td>720P</td>
<td>50</td>
<td>1280x720</td>
<td>15</td>
<td>1130</td>
</tr>
<tr><td>720P_3</td>
<td>52</td>
<td>1280x720</td>
<td>30</td>
<td>1710</td>
</tr>
<tr><td>720P_5</td>
<td>54</td>
<td>960x720</td>
<td>15</td>
<td>910</td>
</tr>
<tr><td>720P_6</td>
<td>55</td>
<td>960x720</td>
<td>30</td>
<td>1380</td>
</tr>
<tr><td>1080P</td>
<td>60</td>
<td>1920x1080</td>
<td>15</td>
<td>2080</td>
</tr>
<tr><td>1080P_3</td>
<td>62</td>
<td>1920x1080</td>
<td>30</td>
<td>3150</td>
</tr>
<tr><td>1080P_5</td>
<td>64</td>
<td>1920x1080</td>
<td>60</td>
<td>4780</td>
</tr>
</tbody>
</table>

> Whether 720p or above can be supported depends on the device. If the device cannot support 1080p, the frame rate will be lower than the one listed in the table. 

#### Sets the Local Video View (setupLocalVideo)

```
virtual int setupLocalVideo(const VideoCanvas& canvas) = 0;
```

This method configures the video display settings on the local machine.

This method binds the local user to the video window (view) of the local video stream and configures the video display settings. Call this method after initialization to configure the local video display settings before joining a channel. After leaving the channel, the bind is still valid, which means the window still displays. To unbind the window, set `view` as `NULL` when calling the `setupLocalVideo` method.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>canvas</td>
<td><p>Video display settings. Class VideoCanvas:</p>
<ul>
<li>view: Video display window (view).</li>
<li>renderMode: Video display mode.<ul>
<li>RENDER_MODE_HIDDEN (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>RENDER_MODE_FIT (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
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



```
struct VideoCanvas {
view_t view;
int renderMode;
uid_t uid;
};
```

#### Sets the Remote Video View (setupRemoteVideo)

```
virtual int setupRemoteVideo(const VideoCanvas& canvas) = 0;
```

This method binds the remote user to the video display window, that is, sets the view for the user of the specified uid. Usually, the app should specify the uid of the remote user sending the video in this method before the remote user joins a channel.

If the remote uid is unknown to the application, set it later when the app receives the `onUserJoined` callback. If the Video Recording function is enabled, the Video Recording Service joins the channel as a dummy client, which means other clients also receive this `onUserJoined` callback. Your app should not bind the dummy client with the view, because the dummy client does not send any video stream. If your app cannot recognize the dummy client, bind the dummy client with the view when the SDK triggers the `onFirstRemoteVideoDecoded` callback. To unbind the user with the view, set `view` as `null`. Once the user has left the channel, the SDK unbinds the remote user.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>canvas</td>
<td><p>Video display settings. VideoCanvas class:</p>
<ul>
<li>view: Video display window (view).</li>
<li>renderMode: Video display mode.<ul>
<li>RENDER_MODE_HIDDEN (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>RENDER_MODE_FIT (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
</ul>
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



```
struct VideoCanvas {
view_t view;
int renderMode;
uid_t uid;
};
```

#### Enables Dual-stream Mode (enableDualStreamMode)

```
int enableDualStreamMode(boolean enabled);
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
<td><p>The video mode is in single stream or dual stream:</p>
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



#### Sets the Remote Video-stream Type (setRemoteVideoStreamType)

```
int setRemoteVideoStreamType(uid_t uid, REMOTE_VIDEO_STREAM_TYPE streamType);
```

This method specifies the video-stream type of the remote user to be received by the local user when the remote user sends dual streams.

-   If dual-stream mode is enabled by calling the `enableDualStreamMode` method, you receive the high-video stream by default. This method allows the app to adjust the corresponding video-stream type according to the size of the video windows to save the bandwidth and resources.

-   If dual-stream mode is not enabled, you receive the high-video stream by default.


The `onApiCallExecuted` callback returns the results of this method call. The Agora SDK receives the high-stream video by default to save the bandwidth. If needed, users can switch to the low-stream video using this method.

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
<li>REMOTE_VIDEO_STREAM_HIGH(0): High-stream video.</li>
<li>REMOTE_VIDEO_STREAM_LOW(1): Low-stream video.</li>
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
int setRemoteDefaultVideoStreamType(REMOTE_VIDEO_STREAM_TYPE streamType);
```

This method sets the default remote video stream type to a high- or low-stream video.

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
<li>REMOTE_VIDEO_STREAM_HIGH(0): High-stream video.</li>
<li>REMOTE_VIDEO_STREAM_LOW(1): Low-stream video.</li>
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
int setVideoQualityParameters(bool preferFrameRateOverImageQuality);
```

This method allows users to set the video preferences.

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
virtual int startPreview() = 0;
```

This method starts the local video preview before joining a channel. Before using this method, you need to:

-   Call the `setupLocalVideo` method to set up the local preview window and configure the attributes.
-   Call the `enableVideo` method to enable video.

> Once you call this method to start the local video preview, if you call the `stopPreview` method before leaving a channel, call the `setRemoteVideo` method the next time you join a channel, or you will not see the remote video view.

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
virtual int stopPreview() = 0;
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
int setLocalRenderMode(RENDER_MODE_TYPE renderMode);
```

This method configures the local video display mode. You can call this method multiple times during a call to change the display mode.

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
<li>RENDER_MODE_HIDDEN (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>RENDER_MODE_FIT (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
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
int setRemoteRenderMode(uid_t uid, RENDER_MODE_TYPE renderMode);
```

This method configures the remote video display mode. You can call this method multiple times during a call to change the display mode.

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
<ul>
<li>RENDER_MODE_HIDDEN (1): Uniformly scale the video until it fills the visible boundaries. One dimension of the video may have clipped contents.</li>
<li>RENDER_MODE_FIT (2): Uniformly scale the video until one of its dimension fits the boundary. Areas that are not filled due to the disparity in the aspect ratio are filled with black.</li>
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


#### Sets the Local Video Mirror Mode (setLocalVideoMirrorMode)

```
int setLocalVideoMirrorMode(VIDEO_MIRROR_MODE_TYPE mirrorMode);
```

This method sets the local video mirror mode. Use this method before calling the `startPreview` method, or this method does not take effect until you call the `startPreview` method again.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>mode</td>
<td><p>Sets the local video mirror mode:</p>
<div><ul>
<li>VIDEO_MIRROR_MODE_AUTO = 0: The default mirror mode, that is, the mode is set by the SDK.</li>
<li>VIDEO_MIRROR_MODE_ENABLED = 1: Enable the mirror mode.</li>
<li>VIDEO_MIRROR_MODE_DISABLED = 2: Disable the mirror mode.</li>
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


### Enables Agora Web SDK Interoperability (enableWebSdkInteroperability)

```
int enableWebSdkInteroperability(bool enabled);
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


### Set the Audio Route

#### Set the Default Audio Route (setDefaultAudioRouteToSpeakerphone)

```
int setDefaultAudioRouteToSpeakerphone(bool defaultToSpeaker) = 0;
```

This method modifies the default audio route if necessary.

> -   Call this method only if you want to change the default settings.
> -   This method only works in audio mode.
> -   Call this method before `joinChannel`.


The default audio routes are listed in the following table:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Channel Mode</th>
<th>Default Audio Route</th>
</tr>
</thead>
<tbody>
<tr><td>Communication</td>
<td><p>Voice Communication: Earpiece</p>
<p>Video Communication: Speakerphone</p>
<p>If the user in a communication channel has called <code>disableVideo</code> or if the user has called <code>muteLocalVideoStream</code> and <code>muteAllRemoteVideoStreams</code>, the audio route is switched back to the earpiece automatically.</p>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Live Broadcast</td>
<td>Speakerphone</td>
</tr>
<tr><td>Game Voice</td>
<td>Speakerphone</td>
</tr>
</tbody>
</table>



Modify the default audio route **if necessary** according to the following table:

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
<td><ul>
<li>True: Speakerphone.</li>
<li>False: Earpiece.</li>
</ul>
<p>Whether the audio is routed to the speakerphone or earpiece, once a headset is plugged in or Bluetooth is connected,</p>
<p>the audio route will be changed. The audio route will be switched to default once removing the headset or disconnecting Bluetooth.</p>
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



#### Enable the Speakerphone (setEnableSpeakerphone)

```
int setEnableSpeakerphone(bool speakerOn) = 0;
```

This method enables the audio routing to the speakerphone.

After calling this method, the SDK will return the `onAudioRouteChanged` callback to indicate the changes.

> Read the default audio route explanation according to `setDefaultAudioRouteToSpeakerphone` and check whether it is necessary to call this method.

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
<li>True:<ul>
<li>If this API is called after joining a channel, whether the audio was routed to the headset, Bluetooth device, or earpiece, it will be routed to the speaker.</li>
<li>If this API is called before joining a channel, when joining a channel, the audio will be routed to the speaker whether the user uses a headset or Bluetooth device.</li>
</ul>
</li>
<li>False: The audio will follow the default audio route mentioned in <a href="#setdefaultroutetospeakerphone-live"><span>Set the Default Audio Route (setDefaultAudioRouteToSpeakerphone)</span></a>.</li>
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





### Encryption

#### Enables Built-in Encryption with an Encryption Password (setEncryptionSecret)

```
virtual int setEncryptionSecret(const char* secret) = 0;
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
virtual int setEncryptionMode(const char* encryptionMode) = 0;
```

The Agora Native SDK supports built-in encryption, which is in AES-128-XTS mode by default. Call this method to set other encryption modes.

All users in the same channel must use the same encryption mode and password. Refer to information related to the AES encryption algorithm on the differences between encryption modes.

Call the `setEncryptionSecret` method to enable the built-in encryption before calling this method.


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
<li>“”: When set as a NUL string, the encryption is in “aes-128-xts” by default.</li>
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
virtual int createDataStream(int* streamId, bool reliable, bool ordered) = 0;
```

This method creates a data stream. Each user can have up to five data channels at the same time.


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
<li>True: The recipients receive the data from the sender within 5 seconds. If the recipient does not receive the sent data within 5 seconds, the data channel  reports an error to the app.</li>
<li>False: The recipients may not receive any data, and the SDK does not report any error upon any data missing.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>ordered</td>
<td><ul>
<li>True: The recipients receive the data in the sent order.</li>
<li>False: The recipients do not receive the data in the sent order.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>&lt;0: Returns an error code when it fails to create the data stream. <sup>[4]</sup></li>
<li>&gt;0: Returns the Stream ID when the data stream is created.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

> [4] The error code is related to the positive integer displayed in [Error Codes and Warning Codes](../../en/Interactive%20Gaming/the_error_game.md). For example, if it returns -2, then it indicates 2: ERR_INVALID_ARGUMENT in [Error Codes and Warning Codes](../../en/Interactive%20Gaming/the_error_game.md).

#### Sends a Data Stream (sendStreamMessage)

```
virtual int sendStreamMessage(int streamId, const char* data, size_t length) = 0;
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
<td>Stream ID returned by <code>createDataStream</code>.</td>
</tr>
<tr><td>data</td>
<td>Sent data.</td>
</tr>
<tr><td>Return Value</td>
<td><p>When it fails to send the message, the following error code is returned:</p>
<p>ERR_SIZE_TOO_LARGE/ERR_TOO_OFTEN/ERR_BITRATE_LIMIT</p>
</td>
</tr>
<tr/>
</tbody>
</table>

### Test and Detection

#### Starts an Audio Call Test (startEchoTest)

```
virtual int startEchoTest() = 0;
```

This method launches an audio call test to determine whether the audio devices (for example, the headset and speaker) and the network connection are working properly. In this test, the user first speaks and the recording is played back in 10 seconds. If the user can hear the recording in 10 seconds, the audio devices and network connection work properly.

> -   After calling this method, call the `stopEchoTest` method to end the test; otherwise, the app cannot run the next echo test, nor can it call the `joinChannel` method to start a new call.
> -   This method is applicable only when the user role is `broadcaster`. If you switch the channel profile from Communication to Live Broadcast, ensure that you call the `setClientRole` method to change the user role from `audience` (default in the live broadcast profile) to `broadcaster` before using this method.


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
<li>ERR_REFUSED (-5): Fails to launch the echo test. For example, initialization fails.</li>
	</ul>
	</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Stops an Audio Call Test (stopEchoTest)

```
virtual int stopEchoTest() = 0;
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
virtual int enableLastmileTest() = 0;
```

This method tests the quality of the user’s network connection and is disabled by default.

This method is mainly used in two scenarios:

-   Before users join a channel, you can call this method to check the uplink network quality.
-   Before the audience in a channel switches to a host role, you can call this method to check the uplink network quality.

In both scenarios, calling this method consumes extra network traffic, which affects the communication quality.

Call the `disableLastmileTest` method to disable the test immediately once the users have received the `onLastmileQuality` callback before they join the channel or switch user roles.

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
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Disables the Network Test (disableLastmileTest)

```
virtual int disableLastmileTest() = 0;
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
virtual int getCallId(agora::util::AString& callId) = 0;
```

When a user joins a channel on a client, `callId` is generated to identify the call from the client. You can call the `rate` and `complain` methods after a call ends in order to submit feedback to the SDK. These feedback methods require assigned values of the `callId` parameters. To use these feedback methods, call this method to retrieve `callId` during a call, and then pass it as an argument in the feedback methods after the call ends.

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
<td>The current call ID.</td>
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



#### Rates the Call (rate)

```
virtual int rate(const char* callId, int rating, const char* description) = 0;
```

This method allows a user to rate a call after a call ends.

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
<td>The rating for the call. The value ranges between 1 (lowest score) and 10 (highest score).</td>
</tr>
<tr><td>description</td>
<td><p>(Optional) A given description for the call with a length less of than 800 bytes.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure:
	<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid. For example, <code>callId</code> is invalid.</li>
<li>ERR_NOT_READY (-3): The SDK status is incorrect. For example, initialization fails.</li>
	</ul>
	</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



#### Complains about the Call Quality (complain)

```
virtual int complain(const char* callId, const char* description) = 0;
```

This method allows a user to complain about the call quality after a call ends.

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
<td><p>(Optional) A given description of the call with a length of less than 800 bytes.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure:
	<ul>
<li>ERR_INVALID_ARGUMENT (-2): The passed argument is invalid. For example, <code>callId</code> is invalid.</li>
<li>ERR_NOT_READY (-3): The SDK status is incorrect. For example, initialization fails.</li>
	</ul>
	</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>

### Added Functions

#### Renews the Token (renewtoken)

```
virtual int renewToken(const char* token) = 0;
```

This method updates the token.

The token expires after a time period once the token schema is enabled. When:

-   The `onError` callback reports the error ERR_TOKEN_EXPIRED(109), or
-   The `onRequestToken` callback reports the error ERR_TOKEN_EXPIRED(109).

The app should retrieve a new token and then call this method to renew it. Failure to do so results in the SDK disconnecting with the server.

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
<td>Specifies the token to be renewed.</td>
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
int setLogFile(const char* filePath);
```

This method specifies an SDK output log file. The log file records all log data of the SDK’s operation. Ensure that the directory to save the log file exists and is writable.

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
int setLogFilter(unsigned int filter);
```

This method sets the output log level of the SDK. You can use either one or a combination of the filters.

The log level follows the sequence of `OFF`, `CRITICAL`, `ERROR`, `WARNING`, `INFO`, and `DEBUG`. Choose a level to see the logs that precede that level.

For example, if you set the log level as `WARNING`, then you can see the logs in the `CRITICAL`, `ERROR`, and `WARNING` levels.

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
<td><p>Sets the levels of the filters:</p>
<div><ul>
<li>LOG_FILTER_OFF = 0: Output no log.</li>
<li>LOG_FILTER_DEBUG = 0x80f: Output all API logs.</li>
<li>LOG_FILTER_INFO = 0x0f: Output logs of the CRITICAL, ERROR, WARNING, and INFO levels.</li>
<li>LOG_FILTER_WARNING = 0x0e: Output logs of the CRITICAL, ERROR, and WARNING levels.</li>
<li>LOG_FILTER_ERROR = 0x0c: Output logs of the CRITICAL and ERROR levels.</li>
<li>LOG_FILTER_CRITICAL = 0x08: Output logs of the CRITICAL level.</li>
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



### Releases the IRtcEngine Object (release)

```
virtual void release() = 0;
```

This method releases the `IRtcEngine` object.

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
<tr><td>sync</td>
<td><p>Indicates whether this method is called synchronously or asynchronously.</p>
<ul>
<li>True: Synchronous call. The result returns after the <code>IRtcEngine</code> object resources are released. The app should not call this interface in the callback generated by the SDK, otherwise the SDK must wait for the callback to return in order to recover the associated object resources, resulting in a deadlock. The SDK automatically detects the deadlock and turns it into an asynchronous call, but the test itself takes extra time.</li>
<li>False: Asynchronous call. The result returns immediately even when the <code>IRtcEngine</code> object resources are not released, and the SDK releases all resources on its own. Do not immediately uninstall the SDK’s dynamic library after a call, otherwise, it may crash because the SDK clean-up thread has not quit.</li>
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



### Gets the SDK Version (getVersion)

```
virtual const char* getVersion(int* build) = 0;
```

This method returns the string of the SDK version number in char format.

<a id = "RtcEngineParameters"></a>
### Parameter Methods (RtcEngineParameters)

This is an auxiliary class that sets the parameters for the SDK. The following section describes each of the methods in this class.

#### Mutes the Local Audio Stream (muteLocalAudioStream)

```
int muteLocalAudioStream(bool mute);
```

This method mutes/unmutes the local audio stream and enables/disables sending the local audio streams to the network.

> This method does not disable the microphone and thus does not affect any recording process.
> This method takes effect only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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


#### Mutes All Remote Audio Streams (muteAllRemoteAudioStreams)

```
int muteAllRemoteAudioStreams(bool mute);
```

This method enables/disables playing all remote audio streams.


> When `mute` is set as `True`, this method stops playing audio streams without affecting the audio stream receiving process.
> This method takes effect only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<li>True: Stops playing all received audio streams.</li>
<li>False: Plays all received audio streams.</li>
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


#### Mutes a Specified Remote User's Audio Stream (muteRemoteAudioStream)

```
int muteRemoteAudioStream(uid_t uid, bool mute);
```

This method mutes/unmutes the audio stream of a specified remote user and enables/disables playing the user's audio streams.


> When `mute` is set as `True`, this method stops playing the audio stream without affecting the audio stream receiving process.
> This method takes effect only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<td>User ID of the specified remote user.</td>
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
int muteLocalVideoStream (bool mute);
```

This method enables/disables sending the local video stream to the network.


> When `mute` is set as `True`, this method does not disable the camera and thus does not affect the retrieval of the local video stream. This method responds faster than the `enableLocalVideo` method with `enabled` set as `NO`.
> This method takes effect only when a user is in a channel. After the user leaves the channel, all mute states are reset.


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
<li>True: Stops sending the local video stream to the network.</li>
<li>False: Sends the local video stream to the network.</li>
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


#### Mutes All Remote Video Streams (muteAllRemoteVideoStreams)

```
int muteAllRemoteVideoStreams(bool mute);
```

This method enables/disables playing all remote video streams.


> When `mute` is set as `True`, this method stops playing all remote video streams without affecting the video stream receiving process.
> This method takes effect only when a user is in a channel. After the user leaves the channel, all mute states are reset.


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
<li>True: Stops playing all remote video streams.</li>
<li>False: Plays all remote video streams.</li>
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


#### Mutes a Specified Remote User's Video Stream (muteRemoteVideoStream)

```
int muteRemoteVideoStream(uid_t uid, bool mute);
```

This method pauses/resumes receiving a specified remote user’s video stream.

> This method takes effect only when a user is in a channel. After the user leaves the channel, all mute states are reset.

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
<td>User ID of the specified remote user.</td>
</tr>
<tr><td>mute</td>
<td><ul>
<li>True: Stops playing a specified remote user’s video stream.</li>
<li>False: Plays a specified remote user’s video stream.</li>
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

### Sets the Audio Volume

#### Reports on Which Users are Speaking and the Speakers' Volume. (enableAudioVolumeIndication)

```
int enableAudioVolumeIndication(int interval, int smooth);
```

This method enables the SDK to regularly report to the app on which users are speaking and the volume of the speakers. Once this method is enabled, the SDK returns the volume indications at the set time interval returned in the [Audio Volume Indication Callback `onAudioVolumeIndication`] callback, regardless of whether anyone is speaking in the channel.

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
<li>&gt; 0: The time interval (ms) between two consecutive volume indications. Agora recommends setting it to more than 200 ms. Do not set it lower than 10 ms, or the SDK does not trigger the <code>onAudioVolumeIndication</code> callback.</li>
</ul>
</div>
</td>
</tr>
<tr><td>smooth</td>
<td>Smoothing factor. The default value is 3.</td>
</tr>
<tr><td>Return values</td>
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
</tbody>
</table>

### Audio Mixing

#### Starts Audio Mixing (startAudioMixing)

```
int startAudioMixing(const char* filePath, bool loopback, bool replace, int cycle);
```

This method mixes the specified local audio file with the audio stream from the microphone; or, it replaces the microphone’s audio stream with the specified local audio file. You can choose whether the other user can hear the local audio playback and specify the number of loop playbacks. This method also supports online music playback.


> Call this method when you are in a channel, otherwise, it may cause issues.

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
<td><p>Name and path of the local audio file to be mixed.</p>
<p>Supported audio formats: mp3, aac, m4a, 3gp, and wav.</p>
</td>
</tr>
<tr/>
<tr><td>loopback</td>
<td><ul>
<li>True: Only the local user can hear the remix or the replaced audio stream.</li>
<li>False: Both users can hear the remix or the replaced audio stream.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>replace</td>
<td><ul>
<li>True: The content of the local audio file replaces the audio stream from the microphone.</li>
<li>False: Mix the local audio file with the audio stream from the microphone.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>cycle</td>
<td><p>Number of loop playbacks:</p>
<ul>
<li>Positive integer: Number of loop playbacks.</li>
<li>-1: Infinite loop.</li>
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



#### Stops Audio Mixing (stopAudioMixing)

```
int stopAudioMixing();
```

This method stops the audio mixing. Call this method when you are in a channel.

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
int pauseAudioMixing();
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
int resumeAudioMixing();
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
int adjustAudioMixingVolume(int volume);
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
<td><ul>
<li>0: Success.</li>
<li>&lt; 0: Failure.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Gets the Audio Mixing Duration (getAudioMixingDuration)

```
int getAudioMixingDuration();
```

This method gets the duration (ms) of audio mixing. Call this method when you are in a channel.

#### Gets the Audio Playback Position (getAudioMixingCurrentPosition)

```
int getAudioMixingCurrentPosition();
```

This method gets the playback position (ms) of the audio. Call this method when you are in a channel.

#### Sets the Audio Playback Position (setAudioMixingPosition)

```
int setAudioMixingPosition(int pos /*in ms*/);
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
<td>Integer. The playback position of the audio mixing file (ms).</td>
</tr>
</tbody>
</table>

### Recording

#### Starts an Audio Recording (startAudioRecording)

```
int startAudioRecording(const char* filePath, AUDIO_RECORDING_QUALITY_TYPE quality);
```

This method starts an audio recording. The SDK allows recording during a call, which supports one of the following formats:

-   *.wav*: Large file size with high fidelity.
-   *.aac*: Small file size with low fidelity.


Ensure that the directory to save the recording file exists and is writable. Call this method after calling the `joinChannel` method. The recording automatically stops when you call the `leaveChannel` method.

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
int stopAudioRecording();
```

This method stops recording on the client.

Call this method before calling the `leaveChannel` method; otherwise, the recording automatically stops when you call the `leaveChannel` method.

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
int adjustRecordingSignalVolume(int volume);
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
<td><p>The recording volume. The value ranges between 0 and 400:</p>
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
<tr/>
</tbody>
</table>



#### Adjusts the Playback Volume (adjustPlaybackSignalVolume)

```
int adjustPlaybackSignalVolume(int volume);
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
<td><p>The playback volume. The value ranges between 0 and 400:</p>
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
<tr/>
</tbody>
</table>



<a id = "irtcengineeventhandler"></a>
## Callbacks (IRtcEngineEventHandler)

The SDK uses the `agora::IRtcEngineEventHandler` interface class to send callbacks to the app, and the app inherits the methods of this interface class to retrieve these callbacks. All callbacks in this interface class have (empty) default implementations, and the app can inherit only some of the required callbacks instead of all of them. In the callbacks, the app should avoid time-consuming tasks or call blocking APIs; for example, SendMessage, otherwise, the SDK may not work properly.

The callbakcs are returned in the main thread.

#### Occurs when a User Joins a Channel (onJoinChannelSuccess)

```
virtual void onJoinChannelSuccess (const char* channel, uid_t uid, int elapsed);
```

This callback reports that a user has successfully joined a specified channel.

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
<td>The channel name.</td>
</tr>
<tr><td>uid</td>
<td><p>User ID.</p>
<p>If the uid is specified in the <code>joinChannel</code> method, it returns the specified ID; if not, it returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when a User Rejoins a Channel (onRejoinChannelSuccess)

```
virtual void onRejoinChannelSuccess(const char* channel, uid_t uid, int elapsed);
```

When the client loses connection with the server because of network problems, the SDK automatically attempts to reconnect and then triggers this callback upon reconnection.

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
<td>The channel name.</td>
</tr>
<tr><td>uid</td>
<td>User ID.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Reports a Warning during SDK Runtime (onWarning)

```
virtual void onWarning(int warn, const char* msg);
```

This callback reports a warning during SDK runtime. In most cases, the app can ignore the warnings reported by the SDK, because the SDK can usually fix the issue and resume running. For instance, the SDK may report an ERR_OPEN_CHANNEL_TIMEOUT warning upon disconnection with the server and attempts to reconnect.

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
<tr><td>warn</td>l
<td>The warning code.</td>
</tr>
<tr><td>msg</td>
<td>The warning message.</td>
</tr>
</tbody>
</table>



#### Reports an Error During SDK Runtime (onError)

```
virtual void onError(int err, const char* msg);
```

This callback reports an error during SDK runtime. In most cases, reporting an error means that the SDK cannot fix the issue and resume running, and therefore requires actions from the app or simply informs the user on the issue. For instance, the SDK reports an ERR_START_CALL error when failing to initialize a call. In this case, the app informs the user that the call initialization fails and calls the `leaveChannel` method to exit the channel.

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
<tr><td>err</td>
<td><p>Error code:</p>
<ul>
<li>ERR_INVALID_VENDOR_KEY(101): Invalid App ID.</li>
<li>ERR_INVALID_CHANNEL_NAME(102): Invalid channel name.</li>
<li>ERR_LOOKUP_CHANNEL_REJECTED(105): Fails to look up the channel, because the server rejects the request.</li>
<li>ERR_OPEN_CHANNEL_REJECTED(107): Fails to join the channel, because the media server rejects the request.</li>
<li>ERR_LOAD_MEDIA_ENGINE(1001): Fails to load the media engine.</li>
<li>ERR_START_CALL(1002): Fails to turn on the local audio or video devices and thus fails to start a call.</li>
<li>ERR_START_CAMERA(1003): Fails to turn on the local camera.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td>msg</td>
<td>The error message.</td>
</tr>
</tbody>
</table>



#### Occurs When an API Method Executes (onApiCallExecuted)

```
virtual void onApiCallExecuted(int err, const char* api, const char* result);
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
<td>The method that the SDK executes.</td>
</tr>
<tr><td>result</td>
<td>The result of calling the method.</td>
</tr>
</tbody>
</table>

#### Occurs when the State of the Microphone Changes (onMicrophoneEnabled)

```
virtual void onMicrophoneEnabled(bool enabled)
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


#### Reports the Audio Quality of the Current Call (onAudioQuality)

```
virtual void onAudioQuality(uid_t uid, int quality, unsigned short delay, unsigned short lost);
```

During a call, the SDK triggers this callback once every two seconds to report on the audio quality of the current call.

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
<li>QUALITY_UNKNOWN(0): The network quality is unknown.</li>
<li>QUALITY_EXCELLENT(1): The network quality is excellent.</li>
<li>QUALITY_GOOD(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>QUALITY_POOR(3): Users can feel the communication slightly impaired.</li>
<li>QUALITY_BAD(4): Users can communicate only not very smoothly.</li>
<li>QUALITY_VBAD(5): The network is so bad that users can hardly communicate.</li>
<li>QUALITY_DOWN(6): Users cannot communicate at all.</li>
</ul>
</td>
</tr>
<tr><td>delay</td>
<td>Time delay (ms).</td>
</tr>
<tr><td>lost</td>
<td>The packet loss rate (%).</td>
</tr>
</tbody>
</table>



#### Reports Which Users are Speaking and the Speakers' Volume (onAudioVolumeIndication)

```
virtual void onAudioVolumeIndication (const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume);
```

This callback reports which users are speaking and the speakers' volume. By default, this callback is disabled. You can use the `enableAudioVolumeIndication` method to enable/disable this callback.

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
<td><p>The speakers (array). Each speaker ():</p>
<ul>
<li>uid: User ID of the speaker. The uid of the local user is 0.</li>
<li>volume: The volume of the speaker. The value ranges between 0 (lowest volume) and 255 (highest volume).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>speakerNumber</td>
<td>Total number of speakers.</td>
</tr>
<tr><td>totalVolume</td>
<td>Total volume after audio mixing. The value ranges between 0 (lowest volume) and 255 (highest volume).</td>
</tr>
</tbody>
</table>

In the returned speakers array:

-   If `uid` is 0 (the local user is the speaker), the returned `volume` is the same as `totalVolume`.
-   If `uid` is not 0 and `volume` is 0, the user specified by the uid did not speak.
-   If a uid is in the previous speakers array but not in the current one, the user specified by the uid did not speak.


#### Occurs when a User Leaves a Channel (onLeaveChannel)

```
virtual void onLeaveChannel(const RtcStats& stat);
```

When the app calls the `leaveChannel` method, the SDK uses this callback to notify the app that a user successfully leaves a channel. With this callback, the app retrieves information, such as the call duration and the statistics of the data received/transmitted by the SDK.

**Definition of RtcStats**

```
struct RtcStats
{
    unsigned int duration;
    unsigned int txBytes;
    unsigned int rxBytes;
    unsigned short txKBitRate;
    unsigned short rxKBitRate;

    unsigned short rxAudioKBitRate;
    unsigned short txAudioKBitRate;

    unsigned short rxVideoKBitRate;
    unsigned short txVideoKBitRate;
    unsigned int userCount;
    double cpuAppUsage;
    double cpuTotalUsage;
};
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
<li>txKBitRate: Transmission bitrate (Kbps), represented by an instantaneous value.</li>
<li>rxKBitRate: Receiving bitrate (Kbps), represented by an instantaneous value.</li>
<li>txAudioKBitrate: Audio transmission bitrate (Kbps), represented by an instantaneous value.</li>
<li>rxAudioKBitRate: Audio receiving bitrate (Kbps), represented by an instantaneous value.</li>
<li>txVideoKBitRate: Video transmission bitrate (Kbps), represented by an instantaneous value.</li>
<li>rxVideoKBitRate: Video receiving bitrate (Kbps), represented by an instantaneous value.</li>
<li>users: The number of users in the channel when the user leaves the channel.</li>
<li>puTotalUsage: System CPU usage (%).</li>
<li>cpuAppUsage: Application CPU usage (%).</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Occurs when a Host Joins a Channel (onUserJoined)

```
virtual void onUserJoined(uid_t uid, int elapsed);
```

This callback notifies the app when a host joins a channel. If the host is already in the channel when the app joins the channel, the SDK also reports to the app on the hosts who are already in the channel.


> In the Live-broadcast profile:
> -   The host receives the callback when another host joins the channel.
> -   All the audience members in the channel can receive the callback when the new host joins the channel.
> -   When a Web app joins the channel, the SDK triggers this callback as long as the Web app publishes streams.


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
<tr><td>elapsed</td>
<td>Time delay (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when a Host Leaves a Channel or Goes Offline (onUserOffline)

```
virtual void onUserOffline(uid_t uid, USER_OFFLINE_REASON_TYPE reason);
```

This callback notifies the app that a host leaves a channel or goes offline.

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
<li><strong>USER_OFFLINE_QUIT(0)</strong>: A user has quit the call.</li>
<li><strong>USER_OFFLINE_DROPPED(1)</strong>: The SDK timed out and the user dropped offline because it has not received any data package within a certain period of time. If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the event has timed out.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>


#### Occurs when the Video Frame Size Changes (onVideoSizeChanged)

```
virtual void onVideoSizeChanged(uid_t uid, int width, int height, int rotation);
```

This callback reports when the size of a video frame changes.

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
<tr><td>width</td>
<td>Width (pixels) of the video frame.</td>
</tr>
<tr><td>height</td>
<td>Height (pixels) of the video frame.</td>
</tr>
<tr><td>rotation</td>
<td>New rotational angle of the video. Can be 0 (default), 90, 180, or 270.</td>
</tr>
</tbody>
</table>



#### Occurs when a Video Display Window is Destroyed (onViewDestroyed)

```
virtual void onViewDestroyed() = 0;
```

The SDK triggers this callback when a video display window is destroyed.


#### Reports the Statistics of the Current Call Session (onRtcStats)

```
virtual void onRtcStats(const RtcStats& stats);
```

The SDK reports to the app on the statistics of the call session once every two seconds.

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
<td>See the descriptions in the <a href="#onleavechannel-live-windows"><span>leaveChannel callback (onLeaveChannel)</span></a> .</td>
</tr>
</tbody>
</table>


#### Reports the Statistics of the Uploading Local Video Streams (onLocalVideoStats)

```
virtual void onLocalVideoStats(const LocalVideoStats& stats);
```

The SDK reports to the app on the statistics of the uploading local video streams once every two seconds. Here is the definition of `LocalVideoStats`：

```
struct LocalVideoStats
{
    int sentBitrate;
    int sentFrameRate;
};
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
<tr><td>stats</td>
<td><p>The statistics of the local video, including the:</p>
<ul>
<li>sentBitrate: Transmission bitrate (Kbps) since last count.</li>
<li>sentFrameRate: Transmission frame rate (fps) since last count.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
</tbody>
</table>



#### Reports the Statistics of the Receiving Remote Video Streams Sent from each Host (onRemoteVideoStats)

```
virtual void onRemoteVideoStats(const RemoteVideoStats& stats);
```

The SDK reports to the app on the statistics of the receiving remote video streams sent for each host once every two seconds. If there are multiple remote hosts, the SDK triggers this callback as many times.

**Definition of RemoteVideoStats**

```
struct RemoteVideoStats
{
    uid_t uid;
    int delay;  // obsolete
  int width;
  int height;
  int receivedBitrate;
  int receivedFrameRate;
    REMOTE_VIDEO_STREAM_TYPE rxStreamType;
};
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
<tr><td>stats</td>
<td><p>The statistics of the remote video, including the:</p>
<ul>
<li>uid: User ID of the remote user sending the video stream.</li>
<li>delay: Time delay (ms).</li>
<li>width: Width (pixels) of the video stream.</li>
<li>height: Height (pixels) of the video stream.</li>
<li>receivedBitrate: The data receive bitrate (Kbps) since the last count.</li>
<li>receivedFrameRate: The data receive frame rate (fps) since the last count.</li>
<li>rxStreamType: The stream type. High-stream or low-stream video.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>

#### Occurs when the Engine Sends the First Local Audio Frame (onFirstLocalAudioFrame)

```
virtual void onFirstLocalAudioFrame(int elapsed);
```

The SDK triggers this callback when the engine sends the first local audio frame.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>

#### Occurs when the Engine Receives the First Audio Frame From a Specific Remote User (onFirstRemoteAudioFrame)

```
virtual void onFirstRemoteAudioFrame(uid_t uid, int elapsed)
```

The SDK triggers this callback when the engine receives the first remote audio frame.

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
<td>The UID of the remote user sending the audio frame.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>

#### Occurs when the Engine Renders the First Local Video Frame on the Video Window (onFirstLocalVideoFrame)

```
virtual void onFirstLocalVideoFrame(int width, int height, int elapsed);
```

The SDK triggers this callback when the engine renders the first local video frame on the video window.

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
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>

#### Occurs when the Engine Decodes the First Video Frame from the Specified Remote User (onFirstRemoteVideoDecoded)

```
virtual void onFirstRemoteVideoDecoded(uid_t uid, int width, int height, int elapsed);
```

Occurs when the first remote video frame is received and decoded.

This callback is triggered in either of the following scenarios:

- The remote user joins the channel and sends the video stream.
- The remote user stops sending the video stream and re-sends it after 15 seconds. Possible reasons include:

	- The remote user leaves channel.
	- The remote user drops offline.
	- The remote user calls the `muteLocalVideoStream` method.
	- The remote user calls the `disableVideo` method.

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
<td>User ID of the remote user sending the video stream.</td>
</tr>
<tr><td>width</td>
<td>Width (pixels) of the video stream.</td>
</tr>
<tr><td>height</td>
<td>Height (pixels) of the video stream.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when the First Remote Video Frame is Rendered.(onFirstRemoteVideoFrame)

```
virtual void onFirstRemoteVideoFrame(uid_t uid, int width, int height, int elapsed, int elapsed);
```

The SDK triggers this callback when the first remote video frame is rendered on the user’s video window. The app can retrieve the time elapsed from a user joining the channel until the first video frame is displayed.

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
<td>User ID of the remote user sending the video stream.</td>
</tr>
<tr><td>width</td>
<td>Width (pixels) of the video stream.</td>
</tr>
<tr><td>height</td>
<td>Height (pixels) of the video stream.</td>
</tr>
<tr><td>elapsed</td>
<td>Time elapsed (ms) from calling the <code>joinChannel</code> method until the SDK triggers this callback.</td>
</tr>
</tbody>
</table>



#### Occurs when the Remote Video State Changes (onRemoteVideoStateChanged)

```
virtual void onRemoteVideoStateChanged(uid_t uid, REMOTE_VIDEO_STATE state);
```

The SDK triggers this callback when the remote video state changes.

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
<td>ID of the remote user whose video state changes.</td>
</tr>
<tr><td>state</td>
<td><p>The state of the remote video:</p>
<ul>
<li>REMOTE_VIDEO_STATE_RUNNING = 1: The remote video is normal.</li>
<li>REMOTE_VIDEO_STATE_FROZEN = 2: The remote video is frozen.</li>
</ul>
</td>
</tr>
</tbody>
</table>


#### Reports the Last Mile Network Quality of the Local User (onLastmileQuality)

```
virtual void onLastmileQuality(int quality);
```

This callback reports on the network quality once every two seconds after you call the `enableLastmileTest` method. When not in a call and the network test is enabled (by calling the `enableLastmileTest` method), the SDK triggers this callback irregularly to update the app on the network connection quality of the local user.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>quality</td>
<td><p>Quality of the last-mile network:</p>
<div><ul>
<li>QUALITY_UNKNOWN(0): The network quality is unknown.</li>
<li>QUALITY_EXCELLENT(1): The network quality is excellent.</li>
<li>QUALITY_GOOD(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>QUALITY_POOR(3): Users can feel the communication slightly impaired.</li>
<li>QUALITY_BAD(4): Users can communicate only not very smoothly.</li>
<li>QUALITY_VBAD(5): The network is so bad that users can hardly communicate.</li>
<li>QUALITY_DOWN(6): Users cannot communicate at all.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### Reports the Network Quality of Each User (onNetworkQuality)

```
virtual void onNetworkQuality(uid_t uid, int txQuality, int rxQuality);
```

The SDK triggers this callback once every two seconds to report to the app on the network quality of each user in a Communication or Live-broadcast profile.

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
<td>User ID. The network quality of the user with this UID is reported.
If <code>uid</code> is 0, it reports the local network quality.</td>
</tr>
<tr><td>txQuality</td>
<td><p>The transmission quality of the user:</p>
<div><ul>
<li>QUALITY_UNKNOWN(0): The network quality is unknown.</li>
<li>QUALITY_EXCELLENT(1): The network quality is excellent.</li>
<li>QUALITY_GOOD(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>QUALITY_POOR(3): Users can feel the communication slightly impaired.</li>
<li>QUALITY_BAD(4): Users can communicate only not very smoothly.</li>
<li>QUALITY_VBAD(5): The network is so bad that users can hardly communicate.</li>
<li>QUALITY_DOWN(6): Users cannot communicate at all.</li>
</ul>
</div>
</td>
</tr>
<tr><td>rxQuality</td>
<td><p>The receiving quality of the user:</p>
<div><ul>
<li>QUALITY_UNKNOWN(0): The network quality is unknown.</li>
<li>QUALITY_EXCELLENT(1): The network quality is excellent.</li>
<li>QUALITY_GOOD(2): The network quality is quite good, but the bitrate may be slightly lower than excellent.</li>
<li>QUALITY_POOR(3): Users can feel the communication slightly impaired.</li>
<li>QUALITY_BAD(4): Users can communicate only not very smoothly.</li>
<li>QUALITY_VBAD(5): The network is so bad that users can hardly communicate.</li>
<li>QUALITY_DOWN(6): Users cannot communicate at all.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


#### Occurs when a Remote User's Audio Stream is Muted/Unmuted. (onUserMuteAudio)

```
virtual void onUserMuteAudio(uid_t uid, bool muted);
```

The SDK triggers this callback when a remote user's audio stream has muted/unmuted.

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
<td>User ID of the remote user.</td>
</tr>
<tr><td>muted</td>
<td><ul>
<li>True: The remote user's audio is muted.</li>
<li>False: The remote user's audio is unmuted.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

#### Occurs when a Remote User's Video Stream Playback Pauses/Resumes. (onUserMuteVideo)

```
virtual void onUserMuteVideo(uid_t uid, bool muted);
```

The SDK triggers this callback when a remote user's video stream playback pauses/resumes. 

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
<tr><td>muted</td>
<td><ul>
<li>True: The remote user's video stream playback has paused.</li>
<li>False: The remote user's video stream playback has resumed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Occurs when the Specified Remote User Enables/Disables the Video Module (onUserEnableVideo)

```
virtual void onUserEnableVideo(uid_t uid, bool enabled);
```

The SDK triggers this callback when the specified remote user enables/disables the video module. Disabling the video function means that the user can only use a voice call, and can neither send their own video nor receive or display video from other users.

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
<td>User ID of the remote user.</td>
</tr>
<tr><td>enabled</td>
<td><ul>
<li>True: The remote user has enabled the video function and can enter a video call or a video live broadcast.</li>
<li>False: The user has disabled the video function and can only enter a voice session where the user can neither send or receive any video streams.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Occurs when the Specified Remote User Enables/Disables the Local Video Capturing Function (onUserEnableLocalVideo)

```
virtual void onUserEnableLocalVideo(uid_t uid, bool enabled);
```

The SDK triggers this callback indicates when the specified remote user enables/disables the local video capturing function.

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
<li>True: The user has enabled the local video capturing function. Other users in the channel can see the video of this user.</li>
<li>False: The user has disabled the local video capturing function. Other users in the channel can no longer receive the video stream from this user, while this user can still receive the video streams from other users.</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Occurs when the Camera Turns on and is Ready to Capture the Video (onCameraReady)

```
virtual void onCameraReady();
```

The SDK triggers this callback when the camera turns on and is ready to capture the video. If the camera fails to turn on, an error reports in the `onError` callback.

#### Occurs when the Video Stops Playing (onVideoStopped)

```
virtual void onVideoStopped();
```

The SDK triggers this callback when the video stops playing. The app can use this callback to change the configuration of the view (for example, display other pictures in the view) after the video stops playing.


#### Occurs when the Connection between the SDK and the Server is Interrupted (onConnectionInterrupted)

```
virtual void onConnectionInterrupted();
```

The SDK triggers this callback when the SDK loses connection with the server for more than four seconds after the connection is established.

The SDK triggers the `onConnectionLost` callback when the SDK attempts to reconnect after losing connection. Once the connection is lost, and if the app does not call the `leaveChannel` method, the SDK automatically tries to reconnect repeatedly.

#### Occurs when the Connection between the SDK and the Server is Lost (onConnectionLost)

```
virtual void onConnectionLost();
```

The SDK triggers this callback when the SDK has lost connection with the network and remained unconnected for a period of time (10 seconds by default) despite attempts to reconnect. The SDK also triggers this callback when the SDK fails to join the channel 10 seconds after calling the `joinChannel` method. The SDK keeps trying to reconnect after triggering this callback. Upon reconnection, the SDK triggers the `onRejoinChannelSuccess` callback.

#### Occurs when your Connection is Banned by the Agora Server (onConnectionBanned)

```
virtual void onConnectionBanned();
```

The SDK triggers this callback when your connection is banned by the Agora Server.


#### Occurs when the Local User Receives the Data Stream from the Remote User within Five Seconds (onStreamMessage)

```
virtual void onStreamMessage(uid_t uid, int streamID, const char* data, size_t length);
```

The SDK triggers this callback when the local user receives the data stream from the remote user within five seconds.

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
<tr><td>data</td>
<td>Data sent from the remote user.</td>
</tr>
<tr><td>length</td>
<td>Length of the data.</td>
</tr>
</tbody>
</table>


#### Occurs when the Local User does not Receive the Data Stream from the Remote User within Five Seconds (onStreamMessageError)

```
virtual void onStreamMessageError(uid_t uid, int streamId, int code, int missed, int cached);
```

The SDK triggers this callback when the local user receives the data stream from the remote user within five seconds.

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
<td>Stream ID.</td>
</tr>
<tr><td>code</td>
<td><ul>
<li>ERR_OK = 0: No error.</li>
<li>ERR_NOT_IN_CHANNEL=113: The user is not in a channel.</li>
<li>ERR_BITRATE_LIMIT=115: Limited bitrate.</li>
</ul>
<p>For more error code descriptions, see <a href="../../en/Interactive%20Gaming/the_error_game.md"><span>Error Codes and Warning Codes</span></a>.</p>
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


#### Occurs when the Token Expires (onRequestToken)

```
virtual void onRequestToken();
```

When a token is specified by calling the `joinChannel` method, if the SDK loses connection with the Agora server due to network issues, the token may expire after a time period and a new token may be required to reconnect to the server.

The SDK triggers this callback for the app to generate a new token in order to  call the `renewToken` method to specify the newly generated token.

This function was previously reported in the `onError` callback in ERR_TOEKN_EXPIRED(109) and ERR_INVALID_TOKEN(110). Starting from v1.7.3, the old method still works, but Agora recommends using this callback instead.

#### Occurs when the Audio Mixing File Playback Finishes (onAudioMixingFinished)

```
virtual void onAudioMixingFinished();
```

The SDK triggers this callback when the audio mixing file playback finishes after calling the `startAudioMixing` method. If you do not call the `startAudioMixing` method, an error code returns in the `onError` callback.

#### Occurs when the Local Audio Effect Playback Finishes (onAudioEffectFinished)

```
virtual void onAudioEffectFinished(int soundId)
```

The SDK triggers this callback when the local audio effect playback finishes.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>Name</td>
<td>Description</td>
</tr>
<tr><td>soundId</td>
<td>ID of the audio effect. Each audio effect has a unique ID.</td>
</tr>
</tbody>
</table>



#### Reports which User is the Loudest Speaker (onActiveSpeaker)

```
virtual void onActiveSpeaker(uid_t uid);
```

If you called the `enableAudioVolumeIndication` method, this callback returns the uid of the active speaker detected by the audio volume detection module of the SDK.

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
<td>The uid of the active speaker. By default, 0 is the local user. If needed, you can also add relative functions in your app, for example, the active speaker, once detected, will have his/her head portrait zoomed in.</td>
</tr>
</tbody>
</table>

> -   You need to call the `enableAudioVolumeIndication` method to trigger this callback.
>-   The active speaker is the speaker who speaks at the highest volume during a certain period of time.


#### Occurs when the User Role Switches in a Live Broadcast (onClientRoleChanged)

```
virtual void onClientRoleChanged(CLIENT_ROLE_TYPE oldRole, CLIENT_ROLE_TYPE newRole);
```

The SDK triggers this callback when the user role switches, for example, from a host to an audience or vice versa.

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


### Error Codes

See [Error Codes and Warning Codes](../../en/Interactive%20Gaming/error_rtc.md).

