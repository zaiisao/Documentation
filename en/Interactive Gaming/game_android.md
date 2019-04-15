
---
title: Interactive Gaming API
description: 
platform: Android
updatedAt: Fri Nov 02 2018 04:23:34 GMT+0800 (CST)
---
# Interactive Gaming API
This documentation is provided for the Java language with the following classes:

-   [RtcEngineForGaming Interface Class](#irtcengine): includes the main methods for the application to call.
-   [IAudioEffectManager Interface Class](#iaudioeffectmanager): includes the methods for the application to manage the audio effects.
-   [IRtcEngineEventHandlerForGaming Abstract Class](#irtcengineeventhandler): includes the callback methods.

<a id = "irtcengine"></a>
## RtcEngineForGaming Interface Class

### Create an RtcEngine Object (create)

```
public static RtcEngine create(Context context,
                               String appId,
                               IRtcEngineEventHandlerForGamingForGaming handler);
```

This method creates an RtcEngine object.

The Agora Native SDK supports one RtcEngine instance at a time, therefore the application should create one RtcEngine object only. Unless otherwise specified, all call methods provided by the RtcEngine class are executed asynchronously. It is recommended to call the interface methods in the same thread. Unless otherwise specified, the following rule applies to all APIs whose return values are integer types: A return value of 0 indicates that the call was successful, and a return value less than 0 indicates that the call failed.

### Set the Channel Profile (setChannelProfile)

```
public int setChannelProfile (int profile);
```

This method configures the channel profile. The Agora RtcEngine needs to know what scenario the application is in to apply different methods for optimization.

> -   Use one profile at the same time in the same channel.
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
<td><p>Channel profile. Choose from one of the following:</p>
<ul>
<li>CHANNEL_PROFILE_GAME_FREE_MODE = 2: Free Mode</li>
<li>CHANNEL_PROFILE_GAME_COMMAND_MODE = 3; Command Mode</li>
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



### Set the User Role (setClientRole)

```
public int setClientRole (int role);
```

This method allows you to set the user role before joining a channel or switch the user role after joining a channel.

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



### Leave a Channel (leaveChannel)

```
public int leaveChannel();
```

This method lets the user leave a channel (hang up or exit a call).

After joining a channel, the user must call the `leaveChannel` method to end the call before joining another one. The leaveChannel method releases all resources related to the call. The `leaveChannel` method is called asynchronously, and the user actually does not exit the channel when the call returns. Once the user exits the channel, the SDK triggers the `onLeaveChannel callback`.

<table>
<colgroup>
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



### Enable the Audio Mode (enableAudio)

```
public int enableAudio();
```

This method enables the audio mode.

<table>
<colgroup>
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



### Disable the Audio Mode (disableAudio)

```
public int disableAudio();
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



### Specify the Log File (setLogFile)

```
public int setLogFile (String filePath);
```

This method specifies the SDK output log file. The log file records all the log data of the SDK’s operation. Ensure that the saving directory in the application exists and is writable.

<table>
<colgroup>
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
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Set the Log Filter (setLogFilter)

```
public int setLogFilter (int filter);
```

This method sets the SDK output log filter. Use one or a combination of the filters.

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
	<li><code>LOG_FILTER_OFF</code> = 0;</li>
	<li><code>LOG_FILTER_CRITICAL</code> = 0x0008;</li>
	<li><code>LOG_FILTER_ERROR</code> = 0x000c;</li>
	<li><code>LOG_FILTER_WARN</code> = 0x000e;</li>
	<li><code>LOG_FILTER_INFO</code> = 0x000f;</li>
	<li><code>LOG_FILTER_CRITICAL</code> = 0x080f;</li>
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



### Set the Default Audio Route to Speakerphone (setDefaultAudioRouteToSpeakerphone)

```
public int setDefaultAudioRouteToSpeakerphone(boolean defaultToSpeaker);
```

This method sets the default audio route to `speakerphone` or `earpiece`.

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
<li>True: sets the default audio route to <code>speakerphone</code></li>
<li>False: sets the default audio route to <code>earpiece</code></li>
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
public int setEnableSpeakerphone( boolean enabled );
```

This method sets the audio route to `speakerphone`. The action, that a headset is plugged in or bluetooth is connected, does not affect the audio route.

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
<li>False: use the default audio route value. See <code>setDefaultAudioRouteToSpeakerphone</code> for details.</li>
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
public boolean isSpeakerphoneEnabled();
```

This method checks whether the audio route is set to speakerphone.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>isSpeakerphoneEnabled</code></td>
<td><ul>
<li>True: Speakerphone is enabled.</li>
<li>False: Speakerphone is disabled.</li>
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



### Enable the Audio Volume Indication (enableAudioVolumeIndication)

```
public int enableAudioVolumeIndication (int interval, int smooth);
```

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
<td><p>Specifies the time interval between two consecutive volume indications.</p>
<ul>
<li>&lt;=0 : Disables the volume indication.</li>
<li>&gt;0: Volume indication interval (ms). Recommended to set it to a minimum of 200 ms.</li>
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



### Mute the Local Audio Stream (muteLocalAudioStream)

```
public int muteLocalAudioStream (boolean muted);
```

This method enables/disables sending local audio streams to the network.

> When set to True, this method does not disable the microphone and thus does not affect any ongoing recording.

<table>
<colgroup>
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



### Mute all Remote Audio Streams (muteAllRemoteAudioStreams)

```
public int muteAllRemoteAudioStreams (boolean mute);
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



### Mute a Remote Audio Stream (muteRemoteAudioStream)

```
public int muteRemoteAudioStream (int uid, boolean mute);
```

This method mutes/unmutes a specified user’s audio stream.

> When set to True, this method mutes the audio stream without affecting the audio stream receiving process.

<table>
<colgroup>
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



### Get the SDK Version (getSdkVersion)

```
public static String getSdkVersion();
```

This method returns the string of the SDK version number.

### Get the Error Description (getErrorDescription)

```
public static String getErrorDescription(int error);
```

This method gets the error description.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>error</code></td>
<td>Warning code or error code returned in <code>onWarning</code> or <code>onError</code>.</td>
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



#### Join a Channel (joinChannel)

```
public abstract int joinChannel(String token,
                                String channelName,
                                String optionalInfo,
                                int optionalUid);
```

This method allows a user to join a channel. Users in the same channel can talk to each other; and multiple users in the same channel can start a group chat. Users using different App IDs cannot call each other. Once in a call, the user must call the `leaveChannel` method to exit the current call before entering another channel.

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
<tr><td><code>optionalInfo</code></td>
<td>(Optional) Additional information about the channel. Other users in the channel will not receive this information.</td>
</tr>
<tr><td><code>optionalUid</code></td>
<td><p>(Optional) User ID: A 32-bit unsigned integer ranging from 1 to (2^32-1). It must be unique. If not assigned (or set to 0), the SDK will assign one and return it in the onJoinChannelSuccess callback. The app needs to record and maintain the returned value as the SDK does not maintain it.</p>
<p>The UID is represented as a 32-bit unsigned integer in the SDK. Since unsigned integers are not supported by Java, the UID is handled as a 32-bit signed integer and larger numbers are interpreted as negative numbers in Java. If necessary, the UID can be converted to a 64-bit integer through “uid&amp;0xffffffffL”.</p>
</td>
</tr>
<tr/>
<tr><td>Return Value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
<ul>
	<li>ERR_INVALID_ARGUMENT = -2: The passed argument is invalid.</li>
	<li>>ERR_NOT_READY = -3: Initialization failed.</li>
</ul>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



#### Renew Token (renewToken)

```
public abstract int renewToken(String token);
```

This method updates the Token.

The token expires after a certain period of time once the Token schema is enabled when:

-   The `onError` callback reports the ERR_TOKEN_EXPIRED (109) error, or
-   The `onRequestToken` callback reports the ERR_TOKEN_EXPIRED (109) error, or
-   The user receives the `onTokenPrivilegeWillExpire` callback.


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
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Start an Audio Recording (startAudioRecording)

```
public int startAudioRecording( String filePath, int quality );
```

This method starts an audio recording. The SDK allows recording during a call, which supports either one of the following two formats:

-   .wav: Large file size with high sound fidelity
-   .aac: Small file size with low sound fidelity


Ensure that the directory to save the recording file exists and is writable. This method is usually called after the `joinChannel` method. The recording automatically stops when the `leaveChannel` method is called.

<table>
<colgroup>
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
<td>File path of the recording file. The string of the file name is in UTF-8 code.</td>
</tr>
<tr><td><code>quality</code></td>
<td><p>Audio recording quality:</p>
<ul>
	<li>AUDIO_RECORDING_QUALITY_LOW = 0: Low quality, file size is around 1.2 MB after 10 minutes of recording</li>
	<li>AUDIO_RECORDING_QUALITY_MEDIUM = 1: Medium quality, file size is around 2B M after 10 minutes of recording</li>
	<li>AUDIO_RECORDING_QUALITY_HIGH = 2: High quality, file size is around 3.75 MB after 10 minutes of recording</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
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



### Stop Audio Recording (stopAudioRecording)

```
public int stopAudioRecording();
```

This method stops recording on the client. Call this method before calling `leaveChannel`, otherwise the recording automatically stops when the `leaveChannel` method is called.

<table>
<colgroup>
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



### Start Audio Mixing (startAudioMixing)

```
public int startAudioMixing (String filePath,
                             boolean loopback,
                             boolean replace,
                             int cycle
                             int playTime);
```

This method mixes the specified local audio file with the audio stream from the microphone; or, it replaces the microphone’s audio stream with the specified local audio file. You can choose whether the other user can hear the local audio playback and specify the number of loop playbacks.

> -   If you want to use this API, ensure that Android device version is 4.2 or later, and the API version is 16 or later.
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
<td><p>Name and path of the local audio file to be mixed:</p>
<ul>
<li>If the path begins with <em>/assets/</em>, find the audio file in the <em>/assets/</em> directory.</li>
<li>Otherwise, find the audio file in the absolute path.</li>
</ul>
<p>Supported audio formats: mp3, aac, m4a, 3gp, wav, and flac.</p>
</td>
</tr>
<tr/>
<tr/>
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
<li>Positive integer: Number of loop playbacks</li>
<li>-1: Infinite loop</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>playTime</code></td>
<td><p>Start time (ms) of the audio file to play</p>
<p>0: Start from the beginning.</p>
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



### Stop Audio Mixing (stopAudioMixing)

```
public int stopAudioMixing();
```

This method stops audio mixing. Call this API when you are in the channel.

<table>
<colgroup>
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



### Pause Audio Mixing (pauseAudioMixing)

```
public int pauseAudioMixing();
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



### Resume Audio Mixing (resumeAudioMixing)

```
public int resumeAudioMixing();
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



### Adjust the Audio Mixing Volume (adjustAudioMixingVolume)

```
public int adjustAudioMixingVolume(int volume);
```

This method adjusts the audio volume during audio mixing. Call this API when you are in the channel.

<table>
<colgroup>
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



### Get the Audio Mixing Duration (getAudioMixingDuration)

```
public int getAudioMixingDuration();
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



### Get the Current Audio Position (getAudioMixingCurrentPosition)

```
public int getAudioMixingCurrentPosition();
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



### Adjust the Recording Volume (adjustRecordingSignalVolume)

```
public int adjustRecordingSignalVolume(int volume);
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
<td><p>Recording volume, ranging from 0 to 400:</p>
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



### Adjust the Playback Volume (adjustPlaybackSignalVolume)

```
public int adjustPlaybackSignalVolume(int volume);
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
<td><p>Playback volume, ranging from 0 to 400:</p>
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



### Get the Audio Effect Manager (getAudioEffectManager)

```
public IAudioEffectManager getAudioEffectManager();
```

This method gets the IAudioEffectManager object associated with the current RTC engine.

### Get the Media Engine Version (getMediaEngineVersion)

```
public static String getMediaEngineVersion();
```

This method returns the media engine version number.

<a id = "iaudioeffectmanager"></a>
## IAudioEffectManager Interface Class

### Set to Voice-only Mode (setVoiceOnlyMode)

```
public int setVoiceOnlyMode(boolean enable);
```

This method sets to voice-only mode (transmit the audio stream only), and the other streams will be ignored; for example the sound of the keyboard strokes.

> This API will be available starting in Agora Gaming SDK v2.1.

<table>
<colgroup>
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
<td><p>True: Enable voice-only mode.</p>
<p>False: Disable voice-only mode.</p>
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



### Set the Voice Position of the Remote User (setRemoteVoicePosition)

```
public int setRemoteVoicePosition(int uid, double pan, double gain);
```

Sets the voice position of the remote user.

This method enables the listener to tell the location of the source of the audio effect.

> -   This API will be available starting in Agora Gaming SDK v2.1.
> -   This API is valid only for earphones, and is invalid when the speakerphone is enabled.


<table>
<colgroup>
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
<li>0: The audio effect shows right ahead.</li>
<li>-1: The audio effect shows on the left.</li>
<li>1: The audio effect shows on the right.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr><td><code>gain</code></td>
<td><p>Sets whether to change the volume of a single audio effect. The range is [0.0, 100.0]</p>
<p>The default value is 100.0. The smaller the number is, the lower the volume of the audio effect.</p>
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



### Set the Local Voice Pitch (setLocalVoicePitch)

```
public int setLocalVoicePitch(double pitch);
```

This method changes the voice pitch of the local speaker.

> This API will be available starting in Agora Gaming SDK v2.1.

<table>
<colgroup>
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



### Get the Audio Effect Volume (getEffectsVolume)

```
double getEffectsVolume();
```

This method gets the volume of the audio effects from 0.0 to 1.0.

### Set the Audio Effect Volume (setEffectsVolume)

```
public int setEffectsVolume(double volume);
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
<td>Value ranges from 0.0 to 100.0. 100.0 is the default value.</td>
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



### Set the Volume of Audio Effect (setVolumeOfEffect)

```
public int setVolumeOfEffect(int soundId, double volume);
```

This method sets the volume of the specified audio effect.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>soundId</code></td>
<td>The ID of the specified audio effect. The ID for each effect is unique.</td>
</tr>
<tr><td><code>volume</code></td>
<td>The volume of the audio effect. The range is [0.0, 100.0]. The default value is 100.0 .</td>
</tr>
<tr><td>Return value</td>
<td><ul>
<li>Method call succeeded.</li>
<li>Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



### Play the Audio Effect (playEffect)

```
public int playEffect(int soundId, String filePath, boolean loop, double pitch, double pan, double gain);
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
<td>ID of the audio effect. Each audio effect has a unique ID. <sup>[1]</sup></td>
</tr>
<tr><td><code>filePath</code></td>
<td>Absolute path of the audio effect file.</td>
</tr>
<tr><td><code>loop</code></td>
<td><p>Sets whether to play the audio effect in loops:</p>
<ul>
<li>True: Yes</li>
<li>False: No (default)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>pitch</code></td>
<td><p>Sets whether to change the volume of the audio effect.</p>
<p>The range is [0.5, 2] with 1.0 as the default value, which means no need to change the volume.</p>
<p>The smaller the value is set, the lower the volume will be.</p>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>Apan</code></td>
<td><p>Sets whether to change the spatial position of the audio effect. The range is [-1, 1]:</p>
<ul>
<li>0: The audio effect shows right ahead.</li>
<li>-1: The audio effect shows on the left.</li>
<li>1: The audio effect shows on the right.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
	<tr><td><code>gain</code></td>
<td><p>Sets whether to change the volume of a single audio effect. The range is [0.0, 100.0].</p>
<p>The default value is 100.0. The smaller the number, the lower the volume of the audio effect.</p>
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

> [1] If the audio effect is loaded into the memory through `preloadEffect`, ensure that the soundId set is the same as in `preloadEffect`.

### Stop Playing the Audio Effect (stopEffect)

```
public int stopEffect(int soundId);
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
public int stopAllEffects();
```

This method stops playing all audio effects.

### Preload the Audio Effect (preloadEffect)

```
public int preloadEffect(int soundId, String filePath);
```

This method preloads a specific audio effect file (compressed audio file) in the memory.

<table>
<colgroup>
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



### Release the Audio Effect (unloadEffect)

```
public int unloadEffect(int soundId);
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



### Pause the Audio Effect (pauseEffect)

```
public int pauseEffect(int soundId);
```

This method pauses the specific audio effect.

<table>
<colgroup>
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
public int pauseAllEffects();
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



### Resume the Audio Effect (resumeEffect)

```
public int resumeEffect(int soundId);
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
public int resumeAllEffects();
```

This method resumes all audio effects.

<a id = "irtcengineeventhandler"></a>
## IRtcEngineEventHandlerForGaming Abstract Class

### Join Channel Callback (onJoinChannelSuccess)

```
public void onJoinChannelSuccess(String channel, int uid, int elapsed);
```

This callback indicates that the user has successfully joined the specified channel with a Channel Name and User ID assigned. The channel ID is assigned based on the channel name specified in the `joinChannel` API. If the User ID is not specified when `joinChannel` is called, the server assigns one automatically.

<table>
<colgroup>
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
<p>If the uid is specified in the joinChannel method, it returns the specified ID; if not, it returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling <code>joinChannel</code> until this event occurs.</td>
</tr>
</tbody>
</table>



### Rejoin Channel Callback (onRejoinChannelSuccess)

```
public void onRejoinChannelSuccess(String channel, int uid, int elapsed);
```

When the client loses connection with the server because of network problems, the SDK automatically attempts to reconnect, and then triggers this callback method upon reconnection.

<table>
<colgroup>
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
<p>If the UID is specified in the <code>joinChannel</code> method, it returns the specified ID;</p>
<p>If not, it returns an ID that is automatically assigned by the Agora server.</p>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>elapsed</code></td>
<td>Time elapsed (ms) from calling joinChannel until this event occurs.</td>
</tr>
</tbody>
</table>



### Warning Occurred Callback (onWarning)

```
public void onWarning(int warn);
```

This callback indicates that some warning occurred during SDK runtime. In most cases, the application can ignore the warnings reported by the SDK because the SDK can usually fix the issue and resume running. For instance, the SDK may report an ERR_OPEN_CHANNEL_TIMEOUT warning upon disconnection with the server and attempts to reconnect.

> See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).

<table>
<colgroup>
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
<td>Warning code.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Warning message.</td>
</tr>
</tbody>
</table>



### Error Occurred Callback (onError)

```
public void onError(int err);
```

This callback indicates that a network or media error occurred during SDK runtime.

In most cases, reporting an error means that the SDK cannot fix the issue and resume running, and therefore requires actions from the application or simply informs the user on the issue. For instance, the SDK reports an ERR_START_CALL error when failing to initialize a call. In this case, the application informs the user that the call initialization failed, and calls the `leaveChannel` method to exit the channel.

> See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).

<table>
<colgroup>
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
<td>Error codes.</td>
</tr>
</tbody>
</table>



### Audio Quality Callback (onAudioQuality)

```
public void onAudioQuality(int uid, int quality, short delay, short lost);
```

During a call, this callback is triggered once every two seconds to report on the audio quality of the current call. By default it is enabled.

<table>
<colgroup>
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
<td>User ID of the speaker</td>
</tr>
<tr><td><code>quality</code></td>
<td><p>Rating of the audio quality of the user:</p>
<ul>
<li>QUALITY_EXCELLENT( = 1)</li>
<li>QUALITY_GOOD( = 2)</li>
<li>QUALITY_POOR( = 3)</li>
<li>QUALITY_BAD( = 4)</li>
<li>QUALITY_VBAD( = 5)</li>
<li>QUALITY_DOWN( = 6)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td><code>delay</code></td>
<td>Time delay (ms).</td>
</tr>
<tr><td><code>lost</code></td>
<td>Packet loss rate (%).</td>
</tr>
</tbody>
</table>



### Leave Channel Callback (onLeaveChannel)

```
public void onLeaveChannel(IRtcEngineEventHandler.RtcStats stats);
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
	<li><code>totalDuration</code>: Call duration (s), represented by an aggregate value.</li>
	<li><code>txBytes</code>: Total number of bytes transmitted, represented by an aggregate value.</li>
	<li><code>rxBytes</code>: Total number of bytes received, represented by an aggregate value.</li>
	<li><code>txKBitRate</code>: Transmission bitrate (kbit/s), represented by an instantaneous value.</li>
	<li><code>rxKBitRate</code>: Receive bitrate (kbit/s), represented by an instantaneous value.</li>
	<li><code>lastmileQuality</code>: Network connection quality of the client, represented by an instantaneous value.</li>
	<li><code>cpuTotalQuality</code>: System CPU usage in percentage (%).</li>
	<li><code>cpuAppQuality</code>: Application CPU usage in percentage (%).</li>
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
<tr/>
</tbody>
</table>



```
struct SessionStat {
    int totalDuration;
    int txBytes;
    int rxBytes;
    int txKBitRate;
    int rxKBitRate;
    int lastmileQuality;
    int cpuTotalUsage;
    int cpuAppUsage;
    };
```

### Rtc Engine Statistics Callback (onRtcStats)

```
public void onRtcStats(IRtcEngineEventHandler.RtcStats stats);
```

The SDK updates the application on the statistics of the Rtc Engine runtime status once every two seconds.

<table>
<colgroup>
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
<td>See the description of <code>onLeaveChannel</code>.</td>
</tr>
</tbody>
</table>



### Audio Volume Indication Callback (onAudioVolumeIndication)

```
public void onAudioVolumeIndication(IRtcEngineEventHandler.AudioVolumeInfo[] speakers, unsigned int speakerNumber, int totalVolume);
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
<td><p>The speakers (array). Each speaker ():</p>
<ul>
<li><code>uid</code>: User ID of the speaker.</li>
<li><code>volume</code>: The volume of the speaker between 0 (lowest volume) to 255 (highest volume).</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr><td><code>speakerNumber</code></td>
<td>Total number of speakers.</td>
</tr>
<tr><td><code>totalVolume</code></td>
<td>Total volume after audio mixing between 0 (lowest volume) to 255 (highest volume).</td>
</tr>
</tbody>
</table>



### Channel Network Quality Callback (onNetworkQuality)

```
public void onNetworkQuality(int uid, int txQuality, int rxQuality);
```

This callback is triggered every 2 seconds to update the application on the network quality of each user in a communication or live broadcast channel.

<table>
<colgroup>
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
<td><p>User ID. The specific callback reports the network quality of the user with this UID.</p>
<p>If uid is 0, it reports the local network quality.</p>
</td>
</tr>
<tr/>
<tr><td><code>txQuality</code></td>
<td><p>The transmission quality of the user:</p>
<ul>
<li>QUALITY_EXCELLENT(1)</li>
<li>QUALITY_GOOD(2)</li>
<li>QUALITY_POOR(3)</li>
<li>QUALITY_BAD(4)</li>
<li>QUALITY_VBAD(5)</li>
<li>QUALITY_DOWN(6)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td><code>rxQuality</code></td>
<td><p>The receiving quality of the user:</p>
<ul>
<li>QUALITY_EXCELLENT(1)</li>
<li>QUALITY_GOOD(2)</li>
<li>QUALITY_POOR(3)</li>
<li>QUALITY_BAD(4)</li>
<li>QUALITY_VBAD(5)</li>
<li>QUALITY_DOWN(6)</li>
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



### User Joined the Channel Callback (onUserJoined)

```
public void onUserJoined (int uid, int elapsed);
```

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
<td>Time interval (m) from calling <code>joinChannel</code> until this callback is triggered.</td>
</tr>
</tbody>
</table>



### User has Gone Offline Callback (onUserOffline)

```
public void onUserOffline (int uid, int reason);
```

This callback notifies the application that a user has left the channel or gone offline.

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
<tr><td>reason</td>
<td><p>Reasons for the user going offline:</p>
<ul>
<li><code>USER_OFFLINE_QUIT</code>: User has quit the call.</li>
<li><code>USER_OFFLINE_DROPPED</code>: The SDK timed out and dropped offline because it had not received any data package for a long time.</li>
</ul>
<p>When a user quits the call but the message is not passed to the SDK due to an unreliable channel, the SDK may assume that the other user timed out and dropped offline.</p>
</td>
</tr>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



### Connection Lost Callback (onConnectionLost)

```
public void onConnectionLost();
```

This callback indicates that the SDK has lost connection with the network, and has remained unconnected for a period of time (10 s by default) despite attempts to reconnect. The SDK will keep trying to reconnect after this callback is triggered. Upon reconnection, an `onRejoinChannelSuccess` callback will be triggered.

### Connection Interrupted Callback (onConnectionInterrupted)

```
public void onConnectionInterrupted();
```

This method indicates that the SDK has lost connection with the server.

This method is triggered upon connection lost, while the `onConnectionLost` method (see above) is triggered when the SDK attempts to reconnect after losing connection. Once the connection is lost, and if the application does not call leaveChannel, the SDK automatically tries to reconnect repeatedly.

### User Muted the Audio Callback (onUserMuteAudio)

```
public void onUserMuteAudio (int uid, boolean muted);
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



### Audio Route Changed Callback (onAudioRouteChanged)

```
public void onAudioRouteChanged(int routing);
```

The SDK notifies the application that the audio route is changed to an earpiece, speakerphone, headset, or bluetooth.

### Client Role Changed Callback (onClientRoleChanged)

```
public void onClientRoleChanged(CLIENT_ROLE_TYPE oldRole, CLIENT_ROLE_TYPE newRole)
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
enum CLIENT_ROLE_TYPE
{
    CLIENT_ROLE_BROADCASTER = 1,
    CLIENT_ROLE_AUDIENCE = 2,
};
```

### Other User Paused/Resumed Video Callback (onUserMuteVideo)

```
public void onUserMuteVideo(int uid, boolean muted)；
```

This callback indicates that some other user has paused/resumed his/her video stream.

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
<td>User ID</td>
</tr>
<tr><td><code>muted</code></td>
<td><ul>
<li>true: User has paused his/her video.</li>
<li>false: User has resumed his/her video.</li>
</ul>
</td>
</tr>
<tr/>
</tbody>
</table>



### Rtc Engine Statistics Callback (onRtcStats)

```
public void onRtcStats(IRtcEngineEventHandler.RtcStats stats);
```

The SDK updates the application on the statistics of the Rtc Engine runtime status once every two seconds.

<table>
<colgroup>
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
<td>See the description of <code>onLeaveChannel</code>.</td>
</tr>
</tbody>
</table>



### Channel Network Quality Callback (onNetworkQuality)

```
public void onNetworkQuality(int uid, int txQuality, int rxQuality);
```

This callback is triggered every 2 seconds to update the application on the network quality in a communication or live broadcast channel.

<table>
<colgroup>
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
<td><p>User ID. The specific callback reports the network quality of the user with this UID.</p>
<p>If uid is 0, it reports the local network quality.</p>
</td>
</tr>
<tr/>
<tr><td><code>txQuality</code></td>
<td><p>The transmission quality of the user:</p>
<ul>
<li>QUALITY_EXCELLENT(1)</li>
<li>QUALITY_GOOD(2)</li>
<li>QUALITY_POOR(3)</li>
<li>QUALITY_BAD(4)</li>
<li>QUALITY_VBAD(5)</li>
<li>QUALITY_DOWN(6)</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr><td><code>rxQuality</code></td>
<td><p>The receiving quality of the user:</p>
<ul>
<li>QUALITY_EXCELLENT(1)</li>
<li>QUALITY_GOOD(2)</li>
<li>QUALITY_POOR(3)</li>
<li>QUALITY_BAD(4)</li>
<li>QUALITY_VBAD(5)</li>
<li>QUALITY_DOWN(6)</li>
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



### Audio Mixing Playback Finished Callback (onAudioMixingFinished)

```
public void onAudioMixingFinished();
```

This callback is triggered when the playback of the audio mixing file is finished.

### Token Expired Callback (onRequestToken)

```
public void onRequestToken();
```

When a Token is specified by calling `joinChannel`, if the SDK lost connection with the Agora server due to network issues, the Token may expire after a certain period of time and a new Token may be required to reconnect to the server.

This callback notifies the application the need to generate a new Token, and calls `renewToken` to renew the Token.

This function was previously provided when the callback reported `onError`: ERR_TOKEN_EXPIRED (109), ERR_INVALID_TOKEN (110). Starting from v1.7.3, the old method still works, but it is recommended to use this callback.

## Error Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_game.md).

