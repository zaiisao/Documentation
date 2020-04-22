
---
title: Gaming API
description: 
platform: Cocos Creator
updatedAt: Tue Apr 21 2020 10:47:23 GMT+0800 (CST)
---
# Gaming API
Agora Cocos Creator js SDK contains the following APIs:

- [.init(appid)](#module_agora.init)
- [.setChannelProfile(profile)](#module_agora.setChannelProfile)
- [.setClientRole(role)](#module_agora.setClientRole)
- [.joinChannel(token, channelId, [info], [uid])](#module_agora.joinChannel)
- [.leaveChannel()](#module_agora.leaveChannel)
- [.enableAudio()](#module_agora.enableAudio)
- [.disableAudio()](#module_agora.disableAudio)
- [.muteLocalAudioStream(mute)](#module_agora.muteLocalAudioStream)
- [.enableLocalAudio(enabled)](#module_agora.enableLocalAudio)
- [.muteAllRemoteAudioStreams(mute)](#module_agora.muteAllRemoteAudioStreams)
- [.muteRemoteAudioStream(uid, mute)](#module_agora.muteRemoteAudioStream)
- [.enableAudioVolumeIndication(interval, smooth)](#module_agora.enableAudioVolumeIndication)
- [.adjustRecordingSignalVolume(volume)](#module_agora.adjustRecordingSignalVolume)
- [.adjustPlaybackSignalVolume(volume)](#module_agora.adjustPlaybackSignalVolume)
- [.setDefaultAudioRouteToSpeakerphone(bVal)](#module_agora.setDefaultAudioRouteToSpeakerphone)
- [.setParameters(profile)](#module_agora.setParameters)
- [.getVersion()](#module_agora.getVersion)
- [.setLogFile(filePath)](#module_agora.setLogFile)
- [.setLogFilter(filter)](#module_agora.setLogFilter)
- [.on(event, callback, target)](#module_agora.on)

<a id="module_agora.init"></a>

### Initialize Agora `agora.init(appid)`

| Param | Type                | Description  |
| ----- | ------------------- | ------------ |
| appid | <code>String</code> | Agora App ID |

**Examples:**

```
require("js-agora");
agora.init("AGORA APP ID");
```

Initialize Agora
Note: You need only initialize the Agora RtcEngine once in one app. All the callback methods that are called asynchronously (e.g., `onJoinChannelSuccess` and `onLeaveChannel`) are in the same object.

Agora real-time audio and video messaging service do not identify testing environment and production environment.


<a id="module_agora.setChannelProfile"></a>

### Set the Channel Profile `agora.setChannelProfile(profile)`

| Param   | Type                | Description                                   |
| ------- | ------------------- | --------------------------------------------- |
| profile | <code>Number</code> | <li> 0 : Communication profile (default)<br> <li> 1 : Live Broadcast profile |

**Examples:**

```
agora.setChannelProfile(0)
```

This method configures the channel profile. The Agora RtcEngine needs to know what scenario the application is in to apply different methods for optimization.

> Parameters should only be the specified integer values, and the same applies to all the APIs here.

	
Agora Native SDK supports the following profiles:

| Profile     | Description                                                         |
| -------- | ------------------------------------------------------------ |
| Communication     | (Default) This is used in one-on-one calls, where all users in the channel can talk. |
| Live Broadcast     | Host and audience roles that can be set by calling the setClientRole method. The host sends and receives voice, while the audience receives voice only with the sending function disabled. |

> - Only one profile can be used at the same time in the same channel. 
> - Ensure that different App IDs are used for different channel profiles.
> - This method must be called and configured before a user joins a channel because the channel profile cannot be configured when the channel is in use.

<a id="module_agora.setClientRole"></a>

### Sets the User Role `agora.setClientRole(role)`

```
agora.setClientRole(2);
```

| Param | Type                | Description                          |
| ----- | ------------------- | ------------------------------------ |
| role  | <code>Number</code> | <li>1 : Host <br> <li>2 : Audience(default) |

In the live broadcasr profile, this method allows you to set the user role as a host or an audience (default) before joining a channel, or switch the user role after joining a channel.

<a id="module_agora.joinChannel"></a>

### Join a Channel `agora.joinChannel(token, channelId, [info], [uid])`

```
agora.joinChannel("", "CHANNEL_ID");
```

| Param     | Type                | Description                                                  |
| --------- | ------------------- | ------------------------------------------------------------ |
| token     | <code>String</code> | <li>If the user uses a static App ID, token is optional and can be set as an empty string.<br><li>If the user uses a token, Agora issues an additional App Certificate for you to generate a token based on the algorithm and App Certificate for user authentication on the server. For more information about Token, see [Use Security Keys](https://docs.agora.io/en/Interactive%20Gaming/token?platform=All%20Platforms). |
| channelId | <code>String</code> | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^\_,{ |
| [info]    | <code>String</code> | (Optional) Additional information about the channel. This parameter can be set as null or contain channel related information. Other users in the channel do not receive this message. |
| [uid]     | <code>number</code> | (Optional) User ID. A 32-bit unsigned integer with a value ranging from 1 to (2<sup>32</sup>-1). Uid must be unique. If Uid is not assigned (or set to 0), the SDK assigns and returns uid in the <code>onJoinChannelSuccess</code> callback. Your app must record and maintain the returned uid since the SDK does not do so. |

This method allows a user to join a channel. Users in the same channel can talk to each other, and multiple users in the same channel can start a group chat. Users with different App IDs cannot call each other. You must call the leaveChannel method to exit the current call before joining another channel.

> A channel does not accept duplicate UIDs, such as two users with the same UID. If your app supports logging in a user from different devices at the same time, ensure that you use different UIDs. For example, if you already used the same UID, make the UIDs different by adding the respective device ID to the UID. This is not applicable if your app does not support a user logging in from different devices at the same time. In this case, when you log in a new device, you will be logged out from the other device.

<a id="module_agora.leaveChannel"></a>

### Leave a Channel `agora.leaveChannel()`

```
agora.leaveChannel();
```

After joining a channel, the user must call the `leaveChannel` method to end the call before joining another channel. This method returns 0 if the user leaves the channel and releases all resources related to the call. 
	This method call is asynchronous, and the user has not exited the channel when the method call returns. Once the user leaves the channel, the SDK triggers the `onLeaveChannel` callback.

<a id="module_agora.enableAudio"></a>

### Enable the Audio `agora.enableAudio()`

```
agora.enableAudio();
```

> This method affects the internal engine and can be called after calling the `leaveChannel` method.

<a id="module_agora.disableAudio"></a>

### Disbale the Audio `agora.disableAudio()`

```
agora.disableAudio();
```

> This method affects the internal engine and can be called after calling the `leaveChannel` method.

<a id="module_agora.muteLocalAudioStream"></a>

### Mute the Local Audio Stream `agora.muteLocalAudioStream(mute)`

```
agora.muteLocalAudioStream(true);
```

| Param | Type                 | Description                                       |
| ----- | -------------------- | ------------------------------------------------- |
| mute  | <code>Boolean</code> | <li>true: Stop sending the local audio stream.<br><li>false: Send the local audio stream. |

This method is used to send or stop sending the local audio stream.

<a id="module_agora.enableLocalAudio"></a>

### Disables/Re-enables the Local Audio `agora.enableLocalAudio(enabled)`

```
agora.enableLocalAudio(true);
```

| Param   | Type                 | Description                                                  |
| ------- | -------------------- | ------------------------------------------------------------ |
| enabled | <code>Boolean</code> | <li>true: Re-enable the local audio function, that is, to start local audio capture and processing.<br><li>false: Disable the local audio function, that is, to stop local audio capture and processing. |

The audio function is enabled by default. This method disables/re-enables the local audio function, that is, to stop or restart local audio capture and processing.

This method does not affect receiving or playing the remote audio streams, and is applicable to scenarios where the user wants to receive remote audio streams without sending any audio stream to other users in the channel.

> - Call this method after calling the `joinChannel` method.
> - This method is different from the `muteLocalAudioStream` method:
>   - `enableLocalAudio`: Disables/Re-enables the local audio capture and processing.
>   - `muteLocalAudioStream`: Stops/Continues sending the local audio streams.

<a id="module_agora.muteAllRemoteAudioStreams"></a>

### Mute All Remote Audio Streams `agora.muteAllRemoteAudioStreams(mute)`

| Param | Type                 | Description                                                  |
| ----- | -------------------- | ------------------------------------------------------------ |
| mute  | <code>Boolean</code> | <li>true: Stop receiving all remote audio streams.<br><li>false: Receive all remote audio streams. |

This method is used to receive or stop receiving all remote audio streams, that is, mute or not to mute all remote users.

<a id="module_agora.muteRemoteAudioStream"></a>

### Mute a Specific Remote Audio Stream `agora.muteRemoteAudioStream(uid, mute)`

```
agora.muteRemoteAudioStream("UID", true);
```

| Param | Type                 | Description                                                  |
| ----- | -------------------- | ------------------------------------------------------------ |
| uid   | <code>String</code>  | ID of the specified remote user.                                                  |
| mute  | <code>Boolean</code> | <li>true: Stop receiving the specified remote user’s audio stream.<br><li>talse: Receive the specified remote user’s audio stream. |

Receives/Stops receiving a specified audio stream.

> If you called the `muteAllRemoteAudioStreams` method and set `muted` as `true` to stop receiving all remote video streams, ensure that the `muteAllRemoteAudioStreams` method is called and set `muted` as `false` before calling this method. The `muteAllRemoteAudioStreams` method sets all remote audio streams, while the `muteRemoteAudioStream` method sets a specified remote user's audio stream.

<a id="module_agora.enableAudioVolumeIndication"></a>

### Enable the Speaker Volume Indication `agora.enableAudioVolumeIndication(interval, smooth)`

```
agora.enableAudioVolumeIndication(1000, 3)
```

| Param    | Type                | Description                                                  |
| -------- | ------------------- | ------------------------------------------------------------ |
| interval | <code>Number</code> | Sets the time interval between two consecutive volume indications:<br> <li>&lt;= 0: Disables the volume indication.<br><li>&gt; 0: Time interval (ms) between two consecutive volume indications. Agora recommends setting interval ≥ 200 ms. The value should be ≥ 10 ms or the <code>onAudioVolumeIndication</code> callback would fail.  |
| smooth   | <code>Number</code> | The smoothing factor sets the sensitivity of the audio volume indicator. The value ranges between 0 and 10. The greater the value, the more sensitive the indicator. The recommended value is 3. |

Enables this callback at a set time interval to report on which users are speaking and the speakers' volume.

Once this method is enabled, the SDK returns the volume indication in the `onAudioVolumeIndication` callback at the set time interval, regardless of whether any user is speaking in the channel.

<a id="module_agora.adjustRecordingSignalVolume"></a>

### Adjust the Recording Volume `agora.adjustRecordingSignalVolume(volume)`

```
agora.adjustRecordingSignalVolume(100)
```

| Param  | Type                | Description                              |
| ------ | ------------------- | ---------------------------------------- |
| volume | <code>Number</code> |  The value ranges between 0 and 400 |

<a id="module_agora.adjustPlaybackSignalVolume"></a>

### Adjust the Playback Volume `agora.adjustPlaybackSignalVolume(volume)`

```
agora.adjustPlaybackSignalVolume(100)
```

| Param  | Type                | Description                                                  |
| ------ | ------------------- | ------------------------------------------------------------ |
| volume | <code>Number</code> | The value ranges between 0 and 400:<li>0: Mute<br><li>100: Original volume<br><li>400: (Maximum)Four times the original volume with signal-clipping protection |

This method is used to adjust the playback volume.

<a id="module_agora.setDefaultAudioRouteToSpeakerphone"></a>

### Set the Default Audio Playback Route `agora.setDefaultAudioRouteToSpeakerphone(bVal)`

```
agora.setDefaultAudioRouteToSpeakerphone(true)
```

| Param | Type                 | Description                                                  |
| ----- | -------------------- | ------------------------------------------------------------ |
| bVal  | <code>Boolean</code> | <li>true: Speakerphone.<br><li>false: Earpiece<br>Regardless of whether the audio is routed to the speakerphone or earpiece by default, once a headset is plugged in or Bluetooth device is connected, the default audio route changes. The default audio route switches to the earpiece once removing the headset or disconnecting the Bluetooth device. |

- This method only works in audio mode.
- Call this method before calling the `joinChannel` method.

**If necessary**, change the settings according to the default settings for each profile as follows:

| Profile | Default Audio Route                                                 |
| -------- | ------------------------------------------------------------ |
| Communication     | <li>Audio call: earpiece<br><li>Video Call: speakerphone<br><li>Disable video in a video call: earpiece |
| Live Broadcast     | Speakerphone                                                         |

>  If a user who is in the Communication profile calls the `disableVideo` method or if the user calls the `muteLocalVideoStream` and `muteAllRemoteVideoStreams` methods, the default audio route switches back to the earpiece automatically.



<a id="module_agora.setParameters"></a>

### Set Parameters `agora.setParameters(profile)`

```
agora.setParameters('{"key": "value"}');
```

| Param   | Type               | Description           |
| ------- | ------------------ | --------------------- |
| profile | <code>String</code> | The parameter as a JSON string in the specified format |

This method provides technical preview functionalities or special customizations by configuring the SDK with JSON options.

The JSON options are not public by default. Agora is working on making commonly used JSON options public in a standard way.

<a id="module_agora.getVersion"></a>

### Get the SDK Version `agora.getVersion()`

```
agora.getVersion();
```

<a id="module_agora.setLogFile"></a>
This method returns version of the current SDK in the string format.

### Set the Log File `agora.setLogFile(filePath)`

```
agora.setLogFile(filePath);
```

| Param    | Type                | Description                                   |
| -------- | ------------------- | --------------------------------------------- |
| filePath | <code>String</code> | File path of the log file. The string of the log file is in UTF-8.  |

This method specifies an SDK output log file.

The log file records all log data for the SDK’s operation. Ensure that the directory for the log file exists and is writable.

<a id="module_agora.setLogFilter"></a>

### Set the Log File Filter `agora.setLogFilter(filter)`

```
agora.setLogFilter(0x80f);
```

| Param  | Type                | Description                                                  |
| ------ | ------------------- | ------------------------------------------------------------ |
| filter | <code>Number</code> | Sets the log filter level:<br><li>LOG_FILTER_OFF = 0: Do not output any log.<br><li>LOG_FILTER_DEBUG = 0x80f: Output all API logs.<br><li>LOG_FILTER_INFO = 0x0f: Output logs of the CRITICAL, ERROR, WARNING, and INFO level.<br><li>LOG_FILTER_WARNING = 0x0e: Output logs of the CRITICAL, ERROR, and WARNING level.<br><li>LOG_FILTER_ERROR = 0x0c: Output logs of the CRITICAL and ERROR level.<br><li>LOG_FILTER_CRITICAL = 0x08: Output logs of the CRITICAL level. |

This method sets the output log level of the SDK.You can use one or a combination of the filters. 
	
The log level follows the sequence of OFF, CRITICAL, ERROR, WARNING, INFO, and DEBUG. Choose a level to see the logs preceding that level. 
	
For example, if you set the log level to WARNING, you see the logs within levels CRITICAL, ERROR, and WARNING.

<a id="module_agora.on"></a>

### Event Listeners `agora.on(event, callback, target)`

Agora SDK use this method to add listeners for events, including the following events:

- [agora.on('join-channel-success', (channel, uid, elapsed) => {}, this);](#joinChannelSuccess)
- [agora.on('audio-volume-indication', (speakers, speakerNumber, totalVolume) => {}, this);](#audioVolumeIndication)
- [agora.on('error', (err, msg) => {}, this);](#error)
- [agora.on('leave-channel', (stat) => {}, this);](#leaveChannel)
- [agora.on('user-joined', (uid, elapsed) => {}, this);](#userJoined)
- [agora.on('user-offline', (uid, reason) => {}, this);](#userOffline)
- [agora.on('user-mute-audio', (uid, muted) => {}, this);](#userMuteAudio)
- [agora.on('connection-interrupted', () => {}, this);](#connectionInterrupted)
- [agora.on('request-token', () => {}, this);](#requestToken)
- [agora.on('client-role-changed', (oldRole, newRole) => {}, this);](#clientRoleChanged)
- [agora.on('rejoin-channel-success', (channel, uid, elapsed) => {}, this);](#rejoinChannelSuccess)
- [agora.on('audio-quality', (uid, quality, delay, lost) => {}, this);](#audioQuality)
- [agora.on('warning', (warn, msg) => {}, this);](#warning)
- [agora.on('network-quality', (uid, txQuality, rxQuality) => {}, this);](#networkQuality)
- [agora.on('audio-routing-changed', (routing) => {}, this);](#audioRoutingChanged)
- [agora.on('connection-lost', () => {}, this);](#connectionLost)
- [agora.on('connection-banned', () => {}, this);](#connectionBanned)
- [agora.on("init-success", () => {}, this);](#initSuccess)
- [agora.on('recording-device-changed', (state, device) => {}, this);](#recordingDeviceChanged)

<a name="joinChannelSuccess"></a>

#### Join Channel Callback `agora.on('join-channel-success', (channel, uid, elapsed) => {}, this);`

**Kind**: global function  

| Param   | Type                | Description                                                  |
| ------- | ------------------- | ------------------------------------------------------------ |
| channel | <code>String</code> | Channel name                                                     |
| uid     | <code>Number</code> | User ID. If the uid is not specified when joinChannel is called, the server automatically assigns a uid. |
| elapsed | <code>Number</code> | Time elapsed (ms) from the user calling `joinChannel` until this callback is triggered.                 |

<a name="audioVolumeIndication"></a>

#### Audio Volume Indication Callback `agora.on('audio-volume-indication', (speakers, speakerNumber, totalVolume) => {}, this);`

**Kind**: global function  

| Param         | Type                | Description               |
| ------------- | ------------------- | ------------------------- |
| speakers      | <code>Array</code>  | The speakers (array). Each speaker (): <li>uid: User ID of the speaker. The uid of the local user is 0.<li>volume: The volume of the speaker that ranges from 0 (lowest volume) to 255 (highest volume).    |
| speakerNumber | <code>Number</code> | The size of the speakers array      |
| totalVolume   | <code>Number</code> | Total volume after audio mixing that ranges from 0 (lowest volume) to 255 (highest volume). |

<a name="error"></a>

#### Error Occurred Callback  `agora.on('error', (err, msg) => {}, this);`

**Kind**: global function  

| Param | Type                | Description |
| ----- | ------------------- | ----------- |
| err   | <code>\*</code>     | Error code    |
| msg   | <code>String</code> | Error description    |

<a name="leaveChannel"></a>

#### Leave Channel Callback `agora.on('leave-channel', (stat) => {}, this);`

**Kind**: global function  

| Param | Type                | Description                                                  |
| ----- | ------------------- | ------------------------------------------------------------ |
| stat  | <code>Object</code> | Statistics of the call:<li>duration: Call duration in seconds. <li>txBytes: Total number of bytes transmitted. <li>rxBytes: Total number of bytes received. <li>txKBitRate: Transmission bitrate in kbit/s. <li>rxKBitRate: Receive bitrate in kbit/s. <li>rxAudioKBitRate: Audio receive bitrate in kbit/s. <li>txAudioKBitRate: Audio receive bitrate in kbit/s. <li>rxVideoKBitRate: Video receive bitrate in kbit/s. <li>txVideoKBitRate: Video transmission bitrate in kbit/s. <li>userCount: The instant number of users in the channel when the user leaves the channel. <li>cpuAppUsage: Application CPU usage (%). <li>cpuTotalUsage: System CPU usage (%). |

<a name="userJoined"></a>

#### Other User Joined the Channel Callback `agora.on('user-joined', (uid, elapsed) => {}, this);`

**Kind**: global function  

| Param   | Type                | Description                               |
| ------- | ------------------- | ----------------------------------------- |
| uid     | <code>Number</code> | User ID                                   |
| elapsed | <code>Number</code> | Time delay (ms) from calling `joinChannel` until this callback is triggered. |

<a name="userOffline"></a>

#### Other User is Offline Callback  `agora.on('user-offline', (uid, reason) => {}, this);`

**Kind**: global function  

| Param  | Type                | Description |
| ------ | ------------------- | ----------- |
| uid    | <code>Number</code> | User ID        |
| reason | <code>Number</code> | Reasons for the user going offline.    |

<a name="userMuteAudio"></a>

#### Other User Muted the Audio Callback `agora.on('user-mute-audio', (uid, muted) => {}, this);`

**Kind**: global function  

| Param | Type                 | Description |
| ----- | -------------------- | ----------- |
| uid   | <code>Number</code>  | User ID     |
| muted | <code>Boolean</code> | Whether the user mutes the audio.    |

<a name="connectionInterrupted"></a>

#### Connection Interrupted Callback  `agora.on('connection-interrupted', () => {}, this);`

**Kind**: global function  
<a name="requestToken"></a>

#### Token Expired Callback `agora.on('request-token', () => {}, this);`

**Kind**: global function  
<a name="clientRoleChanged"></a>

#### User Role Changed Callback `agora.on('client-role-changed', (oldRole, newRole) => {}, this);`

**Kind**: global function  

| Param   | Type            | Description  |
| ------- | --------------- | ------------ |
| oldRole | <code>\*</code> | Role that you switched from |
| newRole | <code>\*</code> | Role that you switched to |

<a name="rejoinChannelSuccess"></a>

#### Rejoin Channel Callback `agora.on('rejoin-channel-success', (channel, uid, elapsed) => {}, this);`1

**Kind**: global function  

| Param   | Type                | Description                                   |
| ------- | ------------------- | --------------------------------------------- |
| channel | <code>String</code> | Channel name                                    |
| uid     | <code>Number</code> | User ID                                      |
| elapsed | <code>Number</code> | Time elapsed (ms) from calling `joinChannel` until this event occurs. |

<a name="audioQuality"></a>

#### Audio Quality Callback `agora.on('audio-quality', (uid, quality, delay, lost) => {}, this);`

**Kind**: global function  

| Param   | Type            | Description          |
| ------- | --------------- | -------------------- |
| uid     | <code>\*</code> | User ID              |
| quality | <code>\*</code> | Rating of the audio quality            |
| delay   | <code>\*</code> | Time delay (ms)     |
| lost    | <code>\*</code> | The packet loss rate(%) |

<a name="warning"></a>

#### Warning Occurred Callback `agora.on('warning', (warn, msg) => {}, this);`

**Kind**: global function  

| Param | Type            | Description |
| ----- | --------------- | ----------- |
| warn  | <code>\*</code> | Warning code    |
| msg   | <code>\*</code> | Warning description   |

<a name="networkQuality"></a>

#### Channel Network Quality Callback `agora.on('network-quality', (uid, txQuality, rxQuality) => {}, this);`

**Kind**: global function  

| Param     | Type            | Description  |
| --------- | --------------- | ------------ |
| uid       | <code>\*</code> | User ID       |
| txQuality | <code>\*</code> | The transmission quality of the user |
| rxQuality | <code>\*</code> | The receiving quality of the user |

<a name="audioRoutingChanged"></a>

#### Audio Route Changed Callback agora.on('audio-routing-changed', (routing) => {}, this);`

**Kind**: global function  

| Param   | Type            | Description |
| ------- | --------------- | ----------- |
| routing | <code>\*</code> | Route settings    |

<a name="connectionLost"></a>

#### Connection Lost Callback  `agora.on('connection-lost', () => {}, this);`

**Kind**: global function  
<a name="connectionBanned"></a>

#### Connection Banned Callback `agora.on('connection-banned', () => {}, this);`

**Kind**: global function  
<a name="initSuccess"></a>

#### Agora Initialized Callback `agora.on("init-success", () => {}, this);`

**Kind**: global function  

<a name="recordingDeviceChanged"></a>

#### Recording Device Changed Callback `agora.on('recording-device-changed', (state, device) => {}, this);`

**Kind**: global function  

| Param  | Type            | Description |
| ------ | --------------- | ----------- |
| state  | <code>\*</code> | Device state    |
| device | <code>\*</code> | Device        |
