
---
title: Interactive Gaming API
description: 
platform: Unity
updatedAt: Sun Jun 07 2020 06:19:38 GMT+0800 (CST)
---
# Interactive Gaming API
<div class="alert note">Agora no longer maintains the Agora Unity SDK for Interactive Gaming, use the Agora RTC Unity SDK instead. See more details in <a href="https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/unity/index.html">API Reference</a >.</div>

This document is provided for the C\# programming language with the following classes:

-   [IRtcEngine Abstract Class](#irtcengine): includes the main methods for the application to call.

-   [IAudioEffectManager Interface Class](#iaudioeffectmanager): includes the methods for the application to manage the audio effects.

<a name = "irtcengine"></a>
## IRtcEngine Abstract Class

### Create an Engine Instance

#### Get an Engine Instance (GetEngine)

```
public static IRtcEngine GetEngine (string appId);
```

This method gets an engine instance. If none exists, it creates an instance. Once the API is executed successfully, it returns the IRtcEngineForGaming instance (not null).

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
<td>App ID, see <a href="../../en/Agora%20Platform/token.md"><span>Get an App ID</span></a></td>
</tr>
</tbody>
</table>

#### Query an Engine Instance (QueryEngine)

```
public static IRtcEngine QueryEngine();
```

This method queries an engine instance. Unlike `GetEngine`, `QueryEngine` will not create a new instance if none exists.

<a name = "voice"></a>
### Implement the Voice Function

#### Set the Channel Profile (SetChannelProfile)

```
public int SetChannelProfile(CHANNEL_PROFILE profile);
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
<li><code>CHANNEL_PROFILE_GAME_FREE_MODE</code> = 0: The Free mode. All users in the channel can talk freely.</li>
<li><code>CHANNEL_PROFILE_GAME_COMMAND_MODE</code> = 1: The Command Mode. In this mode, there are two user roles, that is, the boradcaster and the audience in a channel. The broadcaster sends and receives streams while the audience only receive them.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Set the Client Role (SetClientRole)

```
public int SetClientRole(CLIENT_ROLE role);
```

This method sets the user role before joining a channel, and allows you to switch the user role after joining a channel.

> This method is only applicable when you set the channel profile as command mode when calling `SetChannelProfile`.

<table>
<colgroup>
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
<li><code>CLIENT_ROLE_BROADCASTER</code> = 1: Broadcaster</li>
<li><code>CLIENT_ROLE_AUDIENCE</code> = 2: Audience (default)</li>
</ul>
<p>Once set, only the broadcaster in the channel can talk, not the audience.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

<a id = "joinChannel"></a>
#### Join a Channel (JoinChannel)

```
public int JoinChannel (string token, string channelName, string optionalInfo, uint optionalUid);
```

This method allows a user to join a channel. Users in the same channel can talk to each other; and multiple users in the same channel can start a group chat. Users using different App IDs cannot call each other. Once in a call, the user must call the `LeaveChannel` method to exit the current call before entering another channel.

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
<p>This parameter is optional if the user uses a static App ID. In this case, pass NULL as the parameter value.</p>
<p>If the user uses a Token, Agora issues an additional App Certificate to the application developers. The developers can then generate a user key using the algorithm and App Certificate provided by Agora for user authentication on the server.</p>
<p>In most circumstances, the static App ID will suffice. For added security, use a Token.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>channelName</code></td>
<td><p>A string providing the unique channel name for the AgoraRTC session. The length must be within 64 bytes.</p>
<p>The following is the supported scope: 
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
<tr><td><code>optionalInfo</code></td>
<td>(Optional) Additional information about the channel. Other users in the channel will not receive this information.</td>
</tr>
<tr><td><code>optionalUid</code></td>
<td><p>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If not assigned (or set to 0), the SDK will assign one and return it in the <code>onJoinChannelSuccess</code> callback. The app needs to record and maintain the returned value as the SDK does not maintain it.</p>
<p>The UID is represented as a 32-bit unsigned integer in the SDK. Since unsigned integers are not supported by Java, the UID is handled as a 32-bit signed integer and larger numbers are interpreted as negative numbers in Java. If necessary, the UID can be converted to a 64-bit integer through “uid&amp;0xffffffffL”.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<ul>
<li>ERR_INVALID_ARGUMENT (-2): Invalid argument</li>
<li>ERR_NOT_READY (-3): Initiazation failed</li>
</ul>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Enable the Audio Mode (EnableAudio)

```
public int EnableAudio();
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



#### Disable the Audio Mode \(DisableAudio\)

```
public int DisableAudio();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Leave a Channel (LeaveChannel)

```
public int LeaveChannel();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Audio Route

#### Set the Default Audio Route to Speakerphone (setDefaultAudioRouteToSpeakerphone)

```
public int SetDefaultAudioRouteToSpeakerphone(bool speakerphone);
```

This method sets the default audio route to `speakerphone` or `earpiece`.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>speakerphone</code></td>
<td><ul>
<li>True: sets the default audio route to <em>speakerphone</em></li>
<li>False: sets the default audio route to <em>earpiece</em></li>
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

> On Unity for iOS, this method only works in audio mode.
> Once a headset is plugged in or bluetooth is connected, the audio route will be changed accordingly. Once removing the headset or disconnecting bluetooth, the audio route will be the default value.

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
<td>Speakerphone</td>
</tr>
<tr><td>Command</td>
<td>Speakerphone</td>
</tr>
</tbody>
</table>



#### Set the Audio Route to Speakerphone (SetEnableSpeakerphone)

```
public int SetEnableSpeakerphone (bool speakerphone);
```

This method sets the audio route to *speakerphone*. The action, that a headset is plugged in or bluetooth is connected, does not affect the audio route.

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
<tr><td><code>speakerphone</code></td>
<td><ul>
<li>True:<ul>
<li>If this API is called after a user joins a channel, the audio route of the user will be the speakerphone.</li>
<li>If this API is called before a user joins a channel, when the user joins a channel, the audio route of the user will be the speakerphone.</li>
</ul>
</li>
<li>False: use the default audio route value. See <code>setDefaultAudioRouteToSpeakerphone</code> for details.</li>
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



#### Check Whether Audio Route is Set to Speakerphone (IsSpeakerphoneEnabled)

```
public bool IsSpeakerphoneEnabled();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Enable the Audio Volume Indication

#### Enable the Audio Volume Indication (EnableAudioVolumeIndication)

```
public int EnableAudioVolumeIndication (int interval, int smooth);
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
<li>&lt;= 0 : Disables the volume indication.</li>
<li>&gt; 0: Volume indication interval (ms). Recommend setting it to a minimum of 200 ms.</li>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Mute the Audio and Video Stream

<a name = "enableLocalAudio"></a>
#### Enable/Disable the Local Audio Module (EnableLocalAudio)

```
public int EnableLocalAudio (bool enabled);
```

When an app joins a channel, the audio module is enabled by default. This method disables or re-enables the local audio capture, that is, to stop or restart local audio capturing and processing.

The [OnMicrophoneEnabledHandler](#onMicrophoneEnabledHandler) callback is triggered once the local audio module is disabled or re-enabled.

This method does not affect receiving or playing the remote audio streams, and is applicable to scenarios where the user wants to receive remote audio streams without sending any audio stream to other users in the channel.

> - Call this method after [JoinChannel](#joinChannel).
> - This method is different from [muteLocalAudioStream](#muteLocalAudioStream) :
>   - [EnableLocalAudio](#enableLocalAudio): Disables/Re-enables the local audio capture and processing. 
>   - [MuteLocalAudioStream](#muteLocalAudioStream): Stops/Continues sending the local audio streams.

<table>
<thead>
<td>Name</td>
<td>Description</td>
</thead> 
<tbody>
<tr>
<td><code>enabled</code></td>
<td>
Sets whether to disable/re-enable the local audio function:
<ul>
<li>True: (Default) Re-enable the local audio function, that is, to start local audio capture and processing.</li>
<li>False: Disable the local audio function, that is, to stop local audio capture and processing.</li>
</ul>
</td>
</tr>
<tr><td>Returns</td>
<td>
<ul>
<li>0: Success</li>
<li>&lt; 0: Failure</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "muteLocalAudioStream"></a>
#### Mute the Local Audio Stream (MuteLocalAudioStream)

```
public int MuteLocalAudioStream (bool mute);
```

This method enables/disables sending local audio streams to the network.
> - This method is only valid in the channel. If you leave the channel, the mute settings are cleared. 

> - Difference between `EnableLocalAudio` and `MuteLocalAudioStream`:
 `EnableLocalAudio`: Disables/Re-enables the local audio capturing and processing.
 `MuteLocalAudioStream`: Stops/Continues sending the local audio streams.

<table>
<colgroup>
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
<li>True: Stops sending local audio streams to the network.</li>
<li>False: Allows sending local audio streams to the network.</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mute all Remote Audio Streams (MuteAllRemoteAudioStreams)

```
public int MuteAllRemoteAudioStreams (bool mute);
```

This method mutes/unmutes all remote users’ audio streams.
> This method is only valid in the channel. If you leave the channel, the mute settings are cleared. 

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mute a Remote Audio Stream (MuteRemoteAudioStream)

```
public int MuteRemoteAudioStream (uint uid, bool mute);
```

This method mutes/unmutes a specified remote user’s audio stream.
> This method is only valid in the channel. If you leave the channel, the mute settings are cleared. 

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

<a name = "setDefaultMuteAllRemoteAudioStreams"></a>
#### Receive the Audio Streams by Default (SetDefaultMuteAllRemoteAudioStreams)

```c#
public int SetDefaultMuteAllRemoteAudioStreams(bool mute);
```

Sets whether to receive the audio streams by default.

> Call this method either before or after joining a channel. If you call this method after joining the channel, the audio streams of any subsequent user are not received.

<table>
<thead>
<td>Name</td>
<td>Description</td>
</thead> 
<tbody>
<tr>
<td><code>mute</code></td>
<td>
Sets whether or not to receive/stop receiving all remote users' audio streams by default:
<ul>
<li>true: Stop receiving all remote users' audio streams by default.</li>
<li>false: (Default) Receive all remote users' audio streams by default.</li>
</ul>
</td>
</tr>
<tr><td>Returns</td>
<td>
<ul>
<li>0: Succes</li>
<li>&lt; 0: Failure</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Get the SDK Version (GetSdkVersion)

```
public static string GetSdkVersion ();
```

This method returns the string of the version number in character format.

#### Get the Error Description (GetErrorDescription)

```
public static string GetErrorDescription (int code);
```

This method returns the error code when an error is detected during SDK runtime.

### Audio Mixing

#### Start Audio Mixing (StartAudioMixing)

```
public int StartAudioMixing (string filePath, bool loopback, bool replace, int cycle, int playTime = 0);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stop Audio Mixing (StopAudioMixing)

```
public int StopAudioMixing();
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



#### Pause Audio Mixing (PauseAudioMixing)

```
public int PauseAudioMixing();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Resume Audio Mixing (ResumeAudioMixing)

```
public int ResumeAudioMixing();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Adjust the Audio Mixing Volume (AdjustAudioMixingVolume)

```
public int AdjustAudioMixingVolume (int volume);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Get the Audio Mixing Duration (GetAudioMixingDuration)

```
public int GetAudioMixingDuration();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Get the Current Audio Position (GetAudioMixingCurrentPosition)

```
public int GetAudioMixingCurrentPosition();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Recording

#### Start an Audio Recording (StartAudioRecording)

```
public int StartAudioRecording(string filePath);
```

This method starts an audio recording. The SDK allows recording during a call, which supports either one of the following two formats:

- .wav: Large file size with high sound fidelity
- .aac: Small file size with low sound fidelity


Ensure that the directory to save the recording file exists and is writable. Call this method after joining a channel. The recording automatically stops when the `LeaveChannel` method is called.

<table>
<colgroup>
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
<li>&lt; 0: Method call failed</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stop an Audio Recording (StopAudioRecording)

```
public int StopAudioRecording();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Adjust the Recording Volume (AdjustRecordingSignalVolume)

```
public int AdjustRecordingSignalVolume (int volume);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Adjust the Playback Volume (AdjustPlaybackSignalVolume)

```
public int AdjustPlaybackSignalVolume (int volume);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Start an Audio Call Test (StartEchoTest)

```
public int StartEchoTest();
```

This method launches an audio call test to determine whether the audio devices (for example, headset and speaker) and the network connection work properly. In the test, the user speaks first, and the recording is played back in 10 seconds. If the user can hear what he or she says in 10 seconds, it indicates that the audio devices and network connection work properly.

> After calling the `startEchoTest` method, call `stopEchoTest` to end the test; otherwise the application cannot run the next echo test, nor can it call the joinChannel method to start a new call.

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
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



#### Stop an Audio Call Test (StopEchoTest)

```
public int StopEchoTest();
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



#### Retrieve the Current Call ID (GetCallId)

```
public string GetCallId();
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



#### Rate the Call (Rate)

```
public int Rate(string callId, int rating, string desc);
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
<li>&lt; 0: Method call failed.</li>
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



#### Complain about the Call Quality (Complain)

```
public int Complain(string callId, string desc);
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
<td>Call ID retrieved from the getCallId method.</td>
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
<li>&lt; 0: Method call failed.</li>
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



#### Pause the Audio (Pause)

```
public void Pause ();
```

This method pauses the audio. Ffor example, when using background mode, you can call this API to pause the audio.

#### Resume the Audio (Resume)

```
public void Resume ();
```

This method resumes the paused audio.

> `Pause`/`Resume` have dependencies with the method `EnableLocalAudio`. For example, when EnableLocalAudio is set to False to disable local audio, calling `Resume` can enable local audio again.


#### Destroy an Engine Instance (Destroy)

```
public static void Destroy();
```

This method releases all the resources used by the Agora SDK.  This is useful for applications that occassionaly make voice or video calls, to free up resources for other operations when not making calls.

Once the application has called `Destroy` to destroy the created RtcEngine instance,
no other methods in the SDK can be used and no callbacks occur.

> - Use this method in the sub thread.
> - This method is called synchronously. The result returns after the IRtcEngine object resources are released. The app should not call this interface in the callback generated by the SDK, otherwise the SDK must wait for the callback to return before it can reclaim the related object resources, causing a deadlock.


### Implement the Video Function

Before you implement the video function, read [Implement the Voice Function](#voice). The basic video function is based on the APIs listed in [Implement the Voice Function](#voice).

#### Enable the Video Mode (EnableVideo)

```
public int EnableVideo ();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Disable the Video Mode (DisableVideo)

```
public int DisableVideo ();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Set the Video Profile (SetVideoProfile)

```
public int SetVideoProfile(int profile, bool swapWidthAndHeight);
```

This method sets the video encoding profile. Each profile includes a set of parameters such as resolution, frame rate, and bitrate. When the camera does not support the specified resolution, the SDK chooses a suitable camera resolution, but the encoder resolution still uses the one specified by `SetVideoProfile`.

The method only sets the attributes of the streams encoded by the encoder which may be inconsistent with the final display. For example, when the resolution of the encoded stream is 640 x 480, and the rotation attribute of the stream is 90 degrees, then the resolution of the final display will be in portrait mode.

> -   Always set the video profile after calling the `EnableVideo` method.
> -   Always set the video profile before calling the `JoinChannel`/`StartPreview` method.


<table>
<colgroup>
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
<td>Video profile. See Video Profile Definition for details.</td>
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
<li>&lt; 0: Method call failed.</li>
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
<td>1920 x 1080 <sup>[1]</sup></td>
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


> [1] Whether the video can achieve 1080p resolution depends on the performance and capabilities of the device. Devices with lower performance may experience low frame rates when using 1080p.

#### Enable Dual-stream Mode (EnableDualStreamMode)

```
public int EnableDualStreamMode(bool enabled);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Set the Video Stream Type (SetRemoteVideoStreamType)

```
public int SetRemoteVideoStreamType(uint uid, int streamType);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Set High-quality Video Preferences (SetVideoQualityParameters)

```
public int SetVideoQualityParameters(bool preferFrameRateOverImageQuality);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Disable the Local Video (EnableLocalVideo)

```
public int EnableLocalVideo (bool enabled);
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
<li>True: Enable the local video (default)</li>
<li>False: Disable the local video</li>
</ul>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Start the Video Preview (StartPreview)

```
public int StartPreview ();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Stop the Video Preview (StopPreview)

```
public int StopPreview ();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Switch Between Front and Back Cameras (SwitchCamera)

```
public int SwitchCamera();
```

This method switches between front and back cameras.

> The method is only valid on Unity for iOS and Unity for Android.

#### Enable the Video Observer (EnableVideoObserver)

```
public int EnableVideoObserver ();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Disable the Video Observer (DisableVideoObserver)

```
public int DisableVideoObserver ();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mute the Local Video Stream (MuteLocalVideoStream)

```
public int MuteLocalVideoStream(bool mute);
```

This method enables/disables sending local video streams to the network. When set to True, this method does not disable the camera and thus does not affect getting local video streams.
> This method is only valid in the channel. If you leave the channel, the mute settings are cleared. 

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>


#### Mute all Remote Video Streams (MuteAllRemoteVideoStreams)

```
public int MuteAllRemoteVideoStreams(bool mute);
```

This method enables/disables playing all remote users’ video streams. When set to True, this method stops playing video streams without affecting the video stream receiving process.
> This method is only valid in the channel. If you leave the channel, the mute settings are cleared. 

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Mute a Specified Remote Video Stream (MuteRemoteVideoStream)

```
public int MuteRemoteVideoStream(uint uid, bool mute);
```

This method pauses/resumes playing a specified user’s video, that is, enables/disables playing a specified user’s video stream.
> This method is only valid in the channel. If you leave the channel, the mute settings are cleared. 

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>

<a name = "setDefaultMuteAllRemoteVideoStreams"></a>

#### Receive the Video Streams by Default (SetDefaultMuteAllRemoteVideoStreams)

```c#
public int SetDefaultMuteAllRemoteVideoStreams(bool mute);
```

Sets whether to receive the video streams by default.

> Call this method either before or after joining a channel. If you call this method after joining the channel, the video streams of any subsequent user are not received.

<table>
<thead>
<td>Name</td>
<td>Description</td>
</thead> 
<tbody>
<tr>
<td><code>mute</code></td>
<td>
Sets whether or not to receive/stop receiving all remote users' video streams by default:
<ul>
<li>True：Stop receiving all remote video streams by default.</li>
<li>False: (Default) Receive all remote users' video streams by default.</li>
</ul>
</td>
</tr>
<tr><td>Returns</td>
<td>
<ul>
<li>0: Succes</li>
<li>&lt; 0: Failure</li>
</ul>
</td>
</tr>
</tbody>
</table>



### Encryption

#### Enable Built-in Encryption (SetEncryptionSecret)

```
public int SetEncryptionSecret(string secret)；
```

Call `setEncryptionSecret` to specify an encryption password to enable built-in encryption before joining a channel, otherwise the communication will be unencrypted. All users in a channel must set the same encryption password. The encryption password is automatically cleared once the user has left the channel. If the encryption password is not specified or set to empty, the encryption function will be disabled.

<div class="alert note">Unity SDK does not have an encryption library by default.</div>

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Set the Built-in Encryption Mode (SetEncryptionMode)

```
public int SetEncryptionMode(string encryptionMode)；
```

The Agora SDK supports built-in encryption in AES-128-XTS mode by default. To use other modes, call this API to set the encryption mode.

All users in the same channel must use the same encryption mode and password. Refer to information related to the AES encryption algorithm on the differences between encryption modes.

Call `SetEncryptionSecret` to enable the built-in encryption function before calling this API.

<table>
<colgroup>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Others

#### Set the Log Filter (SetLogFilter)

```
public int SetLogFilter (LOG_FILTER filter);
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
<li>LOG_FILTER_OFF = 0;</li>
<li>LOG_FILTER_DEBUG = 0x80f;</li>
<li>LOG_FILTER_INFO = 0x0f;</li>
<li>LOG_FILTER_WARNING = 0x0e;</li>
<li>LOG_FILTER_ERROR = 0x0c;</li>
<li>LOG_FILTER_CRITICAL = 0x08;</li>
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Set a Log File (SetLogFile)

```
public int SetLogFile (string filePath);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



#### Renew Token (RenewToken)

```
public override int RenewToken (string token);
```

This method updates the Token.

The token expires after a certain period of time once the Token schema is enabled when:

-   The `onError` callback reports the ERR_TOKEN_EXPIRED (109) error, or
-   The `onRequestToken` callback reports the ERR_TOKEN_EXPIRED (109) error, or

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
<td>Specifies the Token to be renewed.</td>
</tr>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Callbacks

> All callbacks are returned in the main thread.

#### Join Channel Callback (JoinChannelSuccessHandler)

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



#### Rejoin Channel Callback (ReJoinChannelSuccessHandler)

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
<td>Time elapsed (ms) from calling <em>JoinChannel()</em> until this event occurs.</td>
</tr>
</tbody>
</table>



#### Connection Interrupted Callback (ConnectionInterruptedHandler)

```
public delegate void ConnectionInterruptedHandler ();
```

This callback indicates that the connection is lost between the SDK and the server. Once the connection is lost, the SDK tries to reconnect it repeatedly until the application calls `LeaveChannel`.

#### Connection Lost Callback (ConnectionLostHandler)

```
public delegate void ConnectionLostHandler ();
```

When the connection between the SDK and server is lost, the `ConnectionInterruptedHandler` callback is triggered and the SDK reconnects automatically. If the reconnection fails within a certain period (10 seconds by default), the callback `ConnectionLostHandler` is triggered. The SDK continues to reconnect until the application calls `leaveChannel`.

#### Other User Joined Channel Callback (UserJoinedHandler)

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



#### Other User Offline Callback (UserOfflineHandler)

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
<li><code>USER_OFFLINE_QUIT</code>: User has quit the call.</li>
<li><code>USER_OFFLINE_DROPPED</code>: The SDK timed out and has dropped offline because it has not received a data package within a certain period.</li>
</ul>
<p>If a user quits the call and the message is not passed to the SDK (due to an unreliable channel), the SDK assumes the event has timed out.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



#### Leave Channel Callback (LeaveChannelHandler)

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

#### Audio Volume Indication Callback (VolumeIndicationHandler)

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
<tr><td><code>speakers</code></td>
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


<a name ="onFirstRemoteAudioFrameHandler"></a>
#### First Remote Audio Frame Received Callback (OnFirstRemoteAudioFrameHandler)

```c#
public delegate void OnFirstRemoteAudioFrameHandler (uint userId, int elapsed);
```

This callback is triggered when the first remote audio frame is received.

<table>
<thead>
<td>Name</td>
<td>Description</td>
</thead> 
<tbody>
<tr>
<td><code>userId</code></td>
<td>User ID of the remote user.</td>
</tr>
<tr>
<td><code>elapsed</code></td>
<td>Time elapsed (ms) from the remote user calling <a href="#joinChannel">JoinChannel</a> until this callback is triggered.</td>
</tr>
</tbody>
</table>

<a name ="onFirstLocalAudioFrameHandler"></a>

#### First Local Audio Frame Sent Callback (OnFirstLocalAudioFrameHandler)

```c#
public delegate void OnFirstLocalAudioFrameHandler (int elapsed);
```

This callback is triggered when the first local audio frame is sent.

<table>
<thead>
<td>Name</td>
<td>Description</td>
</thead> 
<tbody>
<tr>
<td><code>elapsed</code></td>
<td>Time elapsed (ms) from the local user calling <a href="#joinChannel">JoinChannel</a> until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### Other User Muted Audio Callback (UserMutedHandler)

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



#### Warning Occurred Callback (SDKWarningHandler)

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



#### Error Occurred Callback (SDKErrorHandler)

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



#### RtcEngine Statistics Callback (RtcStatsHandler)

```
public delegate void RtcStatsHandler (RtcStats stats);
```

The SDK updates the application on the statistics of the RtcEngine runtime status once every two seconds.

#### Audio Mixing Playback Finished (AudioMixingFinishedHandler)

```
public delegate void AudioMixingFinishedHandler ();
```

This callback is triggered when the playback of the audio mixing file is finished.

#### Audio Route Changed Callback (AudioRouteChangedHandler)

```
public delegate void AudioRouteChangedHandler (AUDIO_ROUTE route);
```

The SDK notifies the application that the audio route is changed, and is switched to an earpiece, speakerphone, headset, or bluetooth device.


<a name = "onApiExecutedHandler"></a>

#### API Executed Callback (OnApiExecutedHandler)

```c#
public delegate void OnApiExecutedHandler (int err, string api, string result);
```

This callback is triggered when an API method is executed.

<table>
<thead>
<td>Name</td>
<td>Description</td>
</thead> 
<tbody>
<tr>
<td><code>err</code></td>
<td><a href="https://docs.agora.io/en/Interactive%20Gaming/the_error_game?platform=All%20Platforms#errorcode">Error code</a> that the SDK returns when the method call fails. If the SDK returns 0, then the method call was successful.</td>
</tr>
<tr>
<td><code>api</code></td>
<td>The method executed by the SDK.</td>
</tr>
<tr>
<td><code>result</code></td>
<td>The result of the method call.</td>
</tr>
</tbody>
</table>



#### Token Expired Callback (RequestTokenHandler)

```
public delegate void RequestTokenHandler ();
```

When a Token is specified by calling `joinChannel`, if the SDK lost connection with the Agora server due to network issues, the Token may expire after a certain period of time and a new Token may be required to reconnect to the server.

This callback notifies the application the need to generate a new Token, and calls `renewToken`to renew the Token.

This function was previously provided when the callback reported `onError`: ERR_TOKEN_EXPIRED (109), ERR_INVALID_TOKEN (110). Starting from v1.7.3, the old method still works, but it is recommended to use this callback.

#### First Remote Video Frame Received and Decoded Callback (OnFirstRemoteVideoDecodedHandler)

```
public delegate void OnFirstRemoteVideoDecodedHandler (uint uid, int width, int height, int elapsed);
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
<td>Time elapsed (ms) from the local user calling <code>joinChannel</code> until this callback is triggered.</td>
</tr>
</tbody>
</table>



#### Video Size Changed Callback (OnVideoSizeChangedHandler)

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

<a name = onMicrophoneEnabledHandler></a>
#### Microphone Enabled Callback (OnMicrophoneEnabledHandler)

```
public delegate void OnMicrophoneEnabledHandler (bool isEnabled);
```

This callback is triggered when the microphone is enabled or disabled.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>isEnabled</code></td>
<td>
<div><ul>
<li>True: The microphone is enabled.</li>
<li>False: The microphone is disabled.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

#### Client Role Changed Callback (OnClientRoleChangedHandler)

```
public delegate void OnClientRoleChangedHandler(int oldRole, int newRole)
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



#### User Video Muted Callback (OnUserMuteVideoHandler)

```
public delegate void OnUserMuteVideoHandler (uint uid, bool muted);
```

This callback indicates that some other user has paused/resumed his/her video streams.

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

### Set Voice-only Mode (SetVoiceOnlyMode)

```
int SetVoiceOnlyMode(bool enable);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Get the Audio Effect Volume (GetEffectsVolume)

```
double GetEffectsVolume();
```

This method gets the volume of the effects from 0.0 to 100.0.

### Set the Audio Effect Volume (SetEffectsVolume)

```
int SetEffectsVolume(double volume);
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
<li>&lt; 0: Method call failed.</li>
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


> If the audio effect is loaded into the memory through `preloadEffect`, ensure that the soundId set is the same as in `preloadEffect`.

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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Stop Playing all Audio Effects (StopAllEffects)

```
int StopAllEffects();
```

This method stops playing all the audio effects.

### Preload an Audio Effect (PreloadEffect)

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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Release an Audio Effect (UnloadEffect)

```
int UnloadEffect(int soundId);
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Pause an Audio Effect (PauseEffect)

```
virtual int PauseEffect(int soundId);
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



### Pause all Audio Effects (PauseAllEffects)

```
int PauseAllEffects();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Resume an Audio Effect (ResumeEffect)

```
int ResumeEffect(int soundId);
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



### Resume all Audio Effects (ResumeAllEffects)

```
int ResumeAllEffects();
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
<li>&lt; 0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



## Error Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).


