
---
title: Interactive Gaming API
description: 
platform: iOS
updatedAt: Fri Nov 02 2018 04:23:41 GMT+0800 (CST)
---
# Interactive Gaming API
This document is provided for the Objective-C language with the following classes:

-   [AgoraRtcEngineKit Interface Class](#AgoraRtcEngineKitInterfaceClass): includes the main methods for the application to call.

-   [AgoraRtcEngineKitDelegate Interface Class](#AgoraRtcEngineKitDelegateInterfaceClass): includes the callback methods.

<a name = "AgoraRtcEngineKitInterfaceClass"></a>
## AgoraRtcEngineKit Interface Class

### Initialize \(sharedEngineWithappId\)

```
+ (instancetype)sharedEngineWithAppId:(NSString *)appId
delegate:(id<AgoraRtcEngineKitDelegate>)delegate;
```

This method initializes the **AgoraRtcEngineKit** class as a singleton instance. Call this method to initialize before using **AgoraRtcEngineKit**.

The SDK uses **Delegate** to inform the application on the engine runtime events. All methods defined in **Delegate** are optional implementation methods.

### Set the Channel Profile \(setChannelProfile\)

```
- (int)setChannelProfile (AgoraChannelProfile) profile;
```

This method configures the channel profile. The Agora RtcEngine needs to know what scenario the application is in to apply different methods for optimization.

> -   Only use one profile can be used at the same time in the same channel.
> 
> -   This method must be called and configured before a user joins a channel, because the channel profile cannot be configured when the channel is in use.


<table>
<colgroup>
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
<td><p>Channel profile. Choose from one of the following:</p>
<ul>
<li>AgoraChannelProfileGame_Free_Mode = 2: Free Mode</li>
<li>AgoraChannelProfileGame_Command_Mode = 3: Command Mode</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><p>0: Method call succeeded.</p>
<p>&lt;0: Method call failed.</p>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the User Role \(setClientRole\)

```
- (int)setClientRole:(AgoraRtcClientRole)role;
```

This method allows you to set the user role as a host or an audience \(default\) before joining a channel. This method also allows you to switch the user role after joining a channel.

> This method is only applicable when you set the channel profile as command mode when calling `setChannelProfile`.

<table>
<colgroup>
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
<li>CLIENT_ROLE_AUDIENCE = 2; Audience(default)</li>
</ul>
<p>Once set, only the host in the channel can talk, not the audience.</p>
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



#### Join a Channel \(joinChannelByToken\)

```
- (int)joinChannelByToken:(NSString * _Nullable)token
                channelId:(NSString * _Nonnull)channelId
                     info:(NSString * _Nullable)info
                      uid:(NSUInteger)uid
              joinSuccess:(void(^ _Nullable)(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed))joinSuccessBlock;
```

This method allows a user to join a channel.

Users in the same channel can talk to each other; and multiple users in the same channel can start a group chat. Users using different App IDs cannot call each other. Once in a call, the user must call the `leaveChannel` method to exit the current call before entering another channel. This method is called asynchronously; therefore, it can be called in the main user interface thread.

The SDK uses the OS X’s **AVAudioSession** shared object for audio recording and playing, and so operating on this object may affect the SDK’s audio functions.

Once this method is called successfully, the SDK will trigger the callback. If both `joinChannelSuccessBlock` and `didJoinChannel` are implemented, the priority of `joinChannelSuccessBlock` is higher than `didJoinChannel` , thus `didJoinChannel` will be ignored. If you want to use `didJoinChannel` , set `joinChannelSuccessBlock`as nil.

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
<tr><td><code>token</code></td>
<td><p>A Token generated by the application.</p>
<p>This parameter is optional if the user uses a static App ID. In this case, pass NIL as the parameter value.</p>
<p>If the user uses a Token, Agora issues an additional App Certificate to the application developers. The developers can then generate a user key using the algorithm and App Certificate provided by Agora for user authentication on the server.</p>
<p>In most circumstances, the static App ID will suffice. For added security, use a Token.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>channelName</code></td>
<td><p>A string providing a unique channel name for the AgoraRTC session. The length must be within 64 bytes.</p>
<p>The following is the supported scope: a-z, A-Z, 0-9, space, !</p>
<p>#$%&amp;, ()+,</p>
<p>-, :;&lt;=., &gt;?</p>
<p>@[], ^_, {|}, ~</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td><code>info</code></td>
<td>(Optional) Additional information. It can be set as a NIL Sting or channel related information. Other users in the channel will not receive this information.</td>
</tr>
<tr><td><code>uid</code></td>
<td>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If not assigned (or set to 0), the SDK assigns one and returns it in the joinSuccessBlock callback. The app must record and maintain the returned value, as the SDK does not maintain it.</td>
</tr>
<tr><td><code>joinSuccessBlock</code></td>
<td>Callback on user successfully joined the channel.</td>
</tr>
</tbody>
</table>

> When joining a channel, the SDK calls **setCategory AVAudioSessionCategoryPlayAndRecord** to set **AVAudioSession** to **PlayAndRecord** mode. The application should not set it to any other mode. When setting to this mode, the sound being played \(for example a ringtone\) will be interrupted.

### Leave a Channel \(leaveChannel\)

```
- (int)leaveChannel;
```

This method lets the user leave a channel \(hang up or exit a call\). After joining a channel, the user must call the `leaveChannel` method to end the call before joining another one.

<table>
<colgroup>
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



### Enable the Audio Mode \(enableAudio\)

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



### Disable the Audio Mode \(disableAudio\)

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



### Mute Local Audio Stream \(muteLocalAudioStream\)

```
- (int)muteLocalAudioStream:(BOOL)mute;
```

This method mutes/unmutes the local audio stream.

<table>
<colgroup>
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
<li>True: Mute the local audio stream.</li>
<li>False: Unmute the local audio stream.</li>
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



### Mute all Remote Audio Streams \(muteAllRemoteAudioStreams\)

```
- (int)muteAllRemoteAudioStreams:(BOOL)mute;
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
<tr><td><code>muted</code></td>
<td><ul>
<li>Yes: Mutes all received audio streams.</li>
<li>No: Unmutes all received audio streams.</li>
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



### Mute a Remote Audio Stream \(muteRemoteAudioStream\)

```
- (int)muteRemoteAudioStream:(NSUInteger)uid
                            :(BOOL)mute;
```

This method mutes/unmutes a specified user’s audio stream.

> When set to Yes, this method mutes the audio stream without affecting the audio stream receiving process.

<table>
<colgroup>
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
<tr><td><code>mute</code></td>
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



### Enable the Audio Volume Indication \(enableAudioVolumeIndication\)

```
- (int)enableAudioVolumeIndication:(NSInteger)interval
                            smooth:(NSInteger)smooth;
```

This method enables the SDK to regularly report to the application on who is talking and the volume of the speaker.

<table>
<colgroup>
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
<li>&lt;=0: Disables the volume indication.</li>
<li>&gt;0: Volume indication interval (ms)). Recommend setting it to a minimum of 200 ms.</li>
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



### Set Voice-only Mode \(setVoiceOnlyMode\)

```
- (int) setVoiceOnlyMode:(BOOL) enable;
```

This method sets to voice-only mode \(transmit the audio stream only\), and the other streams will be ignored; for example the sound of the keyboard strokes.

<table>
<colgroup>
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



### Set the Voice Position of the Remote User \(setRemoteVoicePosition\)

```
- (int) setRemoteVoicePosition:(NSUInteger) uid
                           pan:(double) pan
                          gain:(double) gain;
```

Sets the voice position of the remote user.

This method enables the listener to tell the location of the source of the audio effect.

> This API is valid only for earphones, and is invalid when the speakerphone is enabled.

<table>
<colgroup>
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
<li>0: Audio effect shows right ahead.</li>
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
<p>The default value is 100.0. The smaller the value, the lower the volume.</p>
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



### Set the Local Voice Pitch \(setLocalVoicePitch\)

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
<tr><td><code>pitch</code></td>
<td>Voice frequency in the range of [0.5, 2.0]. The default value is 1.0.</td>
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



### Set a Log File \(setLogFile\)

```
- (int)setLogFile:(NSString*)filePath;
```

This method specifies the SDK output log file. The log file records all the log data of the SDK’s operation. Ensure that the directory to save the log file exists and is writable.

<table>
<colgroup>
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
<td>Full file path of the log file. The string of the log file is in UTF-8 code.</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



### Set a Log Filter \(setLogFilter\)

```
- (int)setLogFilter:(NSUInteger)filter;
```

This method sets the SDK output log filter. You can use either one or a combination of the filters.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>filter</code></td>
<td><p>Set the levels of the filters:</p>
<div><ul>
<li>LOG_FILTER_OFF = 0;</li>
<li>LOG_FILTER_CRITICAL = 0x0008;</li>
<li>LOG_FILTER_ERROR = 0x000c;</li>
<li>LOG_FILTER_WARN = 0x000e;</li>
<li>LOG_FILTER_INFO = 0x000f;</li>
<li>LOG_FILTER_CRITICAL = 0x080f;</li>
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



### Get the SDK Version \(getSdkVersion\)

```
+ (NSString *)getSdkVersion;
```

This method returns the string of the SDK version number.

#### Renew a Token \(renewToken\)

```
- (int)renewToken:(NSString * _Nonnull)token;
```

This method updates the Token.

The token expires after a certain period of time once the Token schema is enabled when:

-   The `rtcEngine:didOccurError`: callback reports the ERR\_TOKEN\_EXPIRED\(109\) error, or

-   The` rtcEngineRequestToken`: callback reports the ERR\_TOKEN\_EXPIRED\(109\) error, or

-   The user receives the `rtcEngine:tokenPrivilegeWillExpire`: callback.


The application should retrieve a new token and then call this method to renew it. Failure to do so will result in the SDK disconnecting from the server.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>token</code></td>
<td>Token to be renewed.</td>
</tr>
<tr><td>Return Value</td>
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



### Start an Audio Recording \(startAudioRecording\)

```
- (int)startAudioRecording:(NSString*)filePath;
```

This method starts an audio recording. The SDK allows recording during a call, which supports either one of the following two formats:

-   `.wav`: Large file size with high sound fidelity **OR**

-   `.aac`: Small file size with low sound fidelity


Ensure that the directory to save the recording file exists and is writable. This method is usually called after the `joinChannelByToken` method. The recording automatically stops when the `leaveChannel` method is called.

<table>
<colgroup>
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
<tr><td><code>quality</code></td>
<td><p>Audio recording quality:</p>
<ul>
<li><em>AgoraRtc_AudioRecordingQuality_Low = 0</em>: Low quality, file size around 1.2 MB after 10 minutes of recording</li>
<li><em>AgoraRtc_AudioRecordingQuality_Low = 1</em>: Medium quality, file size around 2 MB after 10 minutes of recording</li>
<li><em>AgoraRtc_AudioRecordingQuality_Low = 2</em>: High quality, file size around 3.75 MB after 10 minutes of recording</li>
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



### Stop an Audio Recording \(stopAudioRecording\)

```
- (int)stopAudioRecording;
```

This method stops recording on the client. Call this method before calling `leaveChannel` ,otherwise the recording automatically stops when the `leaveChannel` method is called.

<table>
<colgroup>
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
<td>0: Method call succeeded.</td>
</tr>
<tr><td>&lt;0: Method call failed.</td>
</tr>
</tbody>
</table>



### Start Audio Mixing \(startAudioMixing\)

```
- (int) startAudioMixing: (NSString*) filePath
                loopback: (BOOL) loopback
                 replace: (BOOL) replace
                   cycle: (NSInteger) cycle
                playTime: (NSInteger) playTime;
```

This method mixes the specified local audio file with the audio stream from the microphone; or, it replaces the microphone’s audio stream with the specified local audio file. You can choose whether the other user can hear the local audio playback and specify the number of loop playbacks. This API also supports online music playback.

> -   If you want to use the `startAudioMixing` API, then ensure that iOS device is version 8.0 or later.
> 
> -   Call this API when you are in the channel, otherwise it may cause issues.


<table>
<colgroup>
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



### Stop Audio Mixing \(stopAudioMixing\)

```
- (int)stopAudioMixing;
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



### Pause Audio Mixing \(pauseAudioMixing\)

```
- (int)pauseAudioMixing;
```

This method pauses the mixing the specified local audio with the microphone input. Call this API when you are in the channel.

<table>
<colgroup>
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



### Resume Audio Mixing \(resumeAudioMixing\)

```
- (int)resumeAudioMixing;
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



### Adjust the Audio Mixing Volume \(adjustAudioMixingVolume\)

```
- (int)adjustAudioMixingVolume:(NSInteger) volume;
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



### Get the Audio Mixing Duration \(getAudioMixingDuration\)

```
- (int)getAudioMixingDuration;
```

This method gets the duration \(ms\) of the audio mixing. Call this API when you are in the channel.

<table>
<colgroup>
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



### Get the Current Audio Position \(getAudioMixingCurrentPosition\)

```
- (int)getAudioMixingCurrentPosition;
```

This method gets the playback position \(ms\) of the audio. Call this API when you are in the channel.

<table>
<colgroup>
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



### Adjust the Recording Volume \(adjustRecordingSignalVolume\)

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
<tr><td><code>volume</code></td>
<td><p>Recording volume, ranges from 0 to 400:</p>
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



### Adjust the Playback Volume \(adjustPlaybackSignalVolume\)

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



### Get the Audio Effect Volume \(getEffectsVolume\)

```
- (double) getEffectsVolume;
```

This method gets the volume of the audio effects from 0.0 to 1.0.

### Set the Audio Effect Volume \(setEffectsVolume\)

```
- (int) setEffectsVolume:(double) volume;
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



### Play the Audio Effect \(playEffect\)

```
- (int) playEffect: (int) soundId
          filePath: (NSString*) filePath
              loop: (BOOL) loop
             pitch: (double) pitch
               pan: (double) pan
              gain: (double) gain;
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
<td>The ID of the audio effect. Each audio effect has a unique ID. <sup>[1]</sup></td>
</tr>
<tr><td><code>filePath</code></td>
<td>Absolute path of the audio effect file.</td>
</tr>
<tr><td><code>loop</code></td>
<td><p>Sets whether to play the audio effect in loops:</p>
<ul>
<li>True: Yes, it will be played in loops</li>
<li>False: No (default)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pitch</code></td>
<td><p>Sets whether to change the volume of the audio effect.</p>
<p>The range is [0.5, 2] with 1.0 as the default value, which means no need to change the volume.</p>
<p>The smaller the value, the lower the volume.</p>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pan</code></td>
<td><p>Sets whether to change the spatial position of the audio effect. The range is [-1, 1]:</p>
<ul>
<li>0: Audio effect shows right ahead.</li>
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
<p>The default value is 100.0. The smaller the value, the lower the volume.</p>
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

>  [1]  If the audio effect is loaded into the memory through `preloadEffect`, ensure that the soundId set is the same as in `preloadEffect`.

### Stop Playing an Audio Effect \(stopEffect\)

```
- (int) stopEffect:(int) soundId;
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



### Stop Playing all Audio Effects \(stopAllEffects\)

```
- (int) stopAllEffects;
```

This method stops playing all audio effects.

### Preload an Audio Effect \(preloadEffect\)

```
- (int) preloadEffect:(int) soundId
             filePath:(NSString*) filePath;
```

This method preloads a specific audio effect file \(compressed audio file\) to the memory.

<table>
<colgroup>
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
<td>Absolute path of the audio effect file.</td>
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



### Release an Audio Effect \(unloadEffect\)

```
- (int) unloadEffect:(int) soundId;
```

This method releases the specific preloaded audio effect from the memory.

<table>
<colgroup>
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



### Pause an Audio Effect \(pauseEffect\)

```
- (int) pauseEffect:(int) soundId;
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



### Pause all Audio Effects \(pauseAllEffects\)

```
- (int)pauseAllEffects();
```

This method pauses all audio effects.

### Resume an Audio Effect \(resumeEffect\)

```
- (int) resumeEffect:(int) soundId;
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



### Resume all Audio Effects \(resumeAllEffects\)

```
- (int) resumeAllEffects;
```

This method resumes all audio effects.

<a name = "AgoraRtcEngineKitDelegateInterfaceClass"></a>

## AgoraRtcEngineKitDelegate Interface Class

### Warning Occurred Callback \(didOccurWarning\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
didOccurWarning:(AgoraErrorCode)warningCode;
```

This callback method indicates that some warning occurred during SDK runtime. In most cases, the application can ignore the warnings reported by the SDK, because the SDK can usually fix the issue and resume running.

For instance, the SDK may report an **AgoraRtc_Error_OpenChannelTimeout(106)** warning upon disconnection with the server and attempts to reconnect.

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
<tr><td><code>warningCode</code></td>
<td>The warning code.</td>
</tr>
</tbody>
</table>



### Error Occurred Callback \(didOccurError\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
didOccurError:(AgoraErrorCode)errorCode;
```

This callback indicates that a network or media error occurred during SDK runtime. In most cases, reporting an error means that the SDK cannot fix the issue and resume running, and therefore requires actions from the application or simply informs the user on the issue. For instance, the SDK reports an **AgoraRtc_Error_StartCall(1002)** error when failing to initialize a call. In this case, the application informs the user that the call initialization failed and calls the `leaveChannel` method to exit the channel.

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
<tr><td><code>engine</code></td>
<td>The AgoraRtcEngineKit Object.</td>
</tr>
<tr><td><code>errorCode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



### Audio Volume Indication Callback \(reportAudioVolumeIndicationOfSpeakers\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine reportAudioVolumeIndicationOfSpeakers:
(NSArray*)speakers totalVolume:(NSInteger)totalVolume;
```

By default this notification is disabled. To enable it, use the `enableAudioVolumeIndication` method to configure it.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>speakers</code></td>
<td><p>The speakers (array). Each speaker ():</p>
<ul>
<li>uid: User ID of the speaker.</li>
<li>volume: Volume of the speaker between 0 (lowest volume) to 255 (highest volume).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>totalVolume</code></td>
<td>Total volume after audio mixing between 0 (lowest volume) to 255 (highest volume).</td>
</tr>
</tbody>
</table>



### Join Channel Callback \(didJoinChannel\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didJoinChannel:
(NSString*)channel withUid:(NSUInteger)uid elapsed:(NSInteger) elapsed;
```

This callback indicates that the user has successfully joined the specified channel.

<table>
<colgroup>
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
<td>Channel name.</td>
</tr>
<tr><td><code>uid</code></td>
<td><p>User ID.</p>
<p>If the uid is specified in the <code>joinChannel</code> method, it returns the specified ID; if not, it returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling <code>joinChannel</code> until this event occurs.</td>
</tr>
</tbody>
</table>



### Rejoin Channel \(didRejoinChannel\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didRejoinChannel: (NSString*)channel
                                                                withUid: (NSUInteger)uid
                                                                elapsed: (NSInteger) elapsed;
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
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID.</td>
</tr>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling <code>joinChannel</code> until this event occurs.</td>
</tr>
</tbody>
</table>



### Leave Channel Callback \(didLeaveChannelWithStats\)

```
- (void)rtcKit:(AgoraRtcEngineKit *)rtcKit didLeaveChannelWithStats:(AgoraRtcStats*)stats;
```

When the application calls the `leaveChannel` method, the SDK uses this callback to notify the application that the user has successfully left the channel. With this callback function, the application retrieves information such as the call duration and the statistics of data received/transmitted by the SDK.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>stats</code></td>
<td><p>Statistics about the call.</p>
<ul>
<li><code>duration</code>: Call duration (s), represented by an aggregate value.</li>
<li><code>txBytes</code>: Total number of bytes transmitted, represented by an aggregate value.</li>
<li><code>rxBytes</code>: Total number of bytes received, represented by an aggregate value.</li>
<li><code>txAudioKBitRate</code>: Transmission bitrate (kbit/s), represented by an instantaneous value.</li>
<li><code>rxAudioKBitRate</code>: Receive bitrate (kbit/s), represented by an instantaneous value.</li>
<li>users: Users in the call.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Client Role Changed Callback \(didClientRoleChanged\)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didClientRoleChanged:(AgoraRtcClientRole)oldRole newRole:(AgoraRtcClientRole)newRole;
```

When you set the channel profile as Live Broadcast when calling `setChannelProfile`, this callback is triggered when there is any role change in the channel.

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



```
typedef NS_ENUM(NSInteger, AgoraRtcClientRole) {
      AgoraRtc_ClientRole_Broadcaster = 1,
      AgoraRtc_ClientRole_Audience = 2,
};
```

### Audio Route Changed Callback \(didAudioRouteChanged\)

```
- (void)rtcKit:(AgoraRtcEngineKit *)rtcKit didAudioRouteChanged:(AudioOutputRouting)routing;
```

The SDK notifies the application that the audio route has switched to an earpiece, speakerphone, headset, or bluetooth device.

### Other User Muted Audio Callback \(didAudioMuted\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
didAudioMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

This callback indicates that another user has muted/unmuted his/her audio stream.

<table>
<colgroup>
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



### Other User Joined Callback \(didJoinedOfUid\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didJoinedOfUid:
(NSUInteger)uid elapsed:(NSInteger)elapsed;
```

This callback indicates that a user has successfully joined the channel. If there are other users in the channel when this user joins, the SDK also reports to the application on the existing users who are already in the channel.

<table>
<colgroup>
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
<td><p>User ID.</p>
<p>If the uid is specified in the <code>joinChannel</code> method, it returns the specified ID; if not, it returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (s) from calling <code>joinChannel</code> until this callback is triggered.</td>
</tr>
</tbody>
</table>



### Other User Offline Callback \(didOfflineOfUid\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine didOfflineOfUid:
(NSUInteger)uid reason:(AgoraUserOfflineReason)reason;
```

This callback indicates that a user has left the call or gone offline.

The SDK reads the timeout data to determine if a user has left the channel \(or has gone offline\). If no data package is received from the user in 15 seconds, the SDK assumes the user is offline. Sometimes a weak network connection may lead to false detections; therefore, it is recommend to use signaling for reliable offline detection.

<table>
<colgroup>
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
<li><strong>AgoraRtc_UserOffline_Quit</strong>: User has quit the call.</li>
<li><strong>AgoraRtc_UserOffline_Dropped</strong>: The SDK timed out and has dropped offline because it has not received a data package within a certain period.</li>
</ul>
<p>If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the event has timed out.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Connection Interrupted Callback \(rtcEngineConnectionDidInterrupted\)

```
- (void)rtcEngineConnectionDidInterrupted:(AgoraRtcEngineKit *)rtcKit;
```

This callback indicates that the connection is lost between the SDK and the server. Once the connection is lost, the SDK tries to reconnect it repeatedly until the application calls leaveChannel.

### Connection Lost Callback \(rtcEngineConnectionDidLost\)

```
- (void)rtcEngineConnectionDidLost:(AgoraRtcEngineKit *)rtcKit;
```

When the connection between the SDK and the server is lost, the **rtcEngineConnectionDidInterrupted** callback is triggered and the SDK reconnects automatically. If the reconnection fails within a certain period of time \(10 s by default, the callback **rtcEngineConnectionDidLost** is triggered. The SDK continues to try reconnecting until the application calls `leaveChannel`.

### Other User Paused/Resumed Video Callback \(didVideoMuted\)

```
- (void)rtcEngine:(AgoraRtcEngineKit * _Nonnull)engine didVideoMuted:(BOOL)muted byUid:(NSUInteger)uid;
```

This callback indicates that another user has paused/resumed his/her video streams.

> Currently, this callback returns invalid when the number of users in a channel exceeds 20, which will be improved in the future.

<table>
<colgroup>
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
<td>Yes: User has paused his/her video.</td>
</tr>
<tr><td>No: User has resumed his/her video.</td>
</tr>
</tbody>
</table>



### RtcEngine Statistics Callback \(reportRtcStats\)

```
- (void)rtcEngine:(AgoraRtcEngineKit *)engine
reportRtcStats:(AgoraRtcStats*)stats;
```

The SDK updates the application on the statistics of the RtcEngine runtime status once every two seconds.

<table>
<colgroup>
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
<td>See the following table.</td>
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
<tr><td><code>duration</code></td>
<td>Call duration (s), represented by an aggregate value.</td>
</tr>
<tr><td><code>txBytes</code></td>
<td>The total number of bytes transmitted, represented by an aggregate value.</td>
</tr>
<tr><td><code>rxBytes</code></td>
<td>The total number of bytes received, represented by an aggregate value.</td>
</tr>
</tbody>
</table>



### Token Expired Callback \(rtcEngineRequestToken\)

```
- (void)rtcEngineRequestToken:(AgoraRtcEngineKit * _Nonnull)engine;
```

When a Token is specified by calling `joinChannelByToken`, if the SDK lost connection with the Agora server due to network issues, the Token may expire after a certain period of time and a new Token may be required to reconnect to the server.

This callback notifies the application the need to generate a new Token, and calls `renewToken` to renew the Token.

This function was previously provided when the callback reported didOccurError\(\): ERR\_TOKEN\_EXPIRED\(109\)、ERR\_INVALID\_TOKEN\(110\). Starting from v1.7.3, the old method still works, but it is recommended to use this callback.

## Error Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).


