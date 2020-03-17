
---
title: Signaling API
description: 
platform: Web
updatedAt: Wed Jul 31 2019 12:45:27 GMT+0800 (CST)
---
# Signaling API
> Version: v1.4.0 BETA

The Signaling Web API consists of the following objects, with each object having its own methods and callback functions:

-   [Signal](#signal)
-   [Session](#session)
-   [Call](#call)
-   [Channel](#channel)


>  Avoid using synchronous or block callback operations. Otherwise, the signaling system may crash.

## <a name="signal"></a>Signal

Obtain a signal object:

```
signal = Signal(appid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>appid</code></td>
<td>The App ID of the project. See <a href="../../en/Agora%20Platform/key_signaling.md">Get an App ID</a> for details on how to obtain an App ID.</td>
</tr>
</tbody>
</table>



### Methods

#### Log into Agora's Signaling System (login)

Use this method to log into Agora’s signaling system and it returns a session. You can execute message, channel, and call operations, as well as set callback functions in that session.

```
login(account, token, reconnect_count, reconnect_time) : Session
```

-   When login is successful, the <code>onLoginSuccess</code> callback function is triggered,
-   When login fails, the <code>onLoginFailed</code> callback function is triggered,
-   When the SDK loses connection with the server, the <code>onLogout</code> callback function is triggered.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client. Maximum length is 128 visible characters (space not allowed). Can be a uid, nickname, guid, or any other content that is unique.</td>
</tr>
<tr><td><code>token</code></td>
<td><code>SignalingToken</code> generated from the App ID and App Certificate. See <a href="../../en/Agora%20Platform/key_signaling.md">>How to Get and Use a SignalingToken</a> for details.</td>
</tr>
<tr><td><code>reconnect_count</code></td>
<td>The maximum attempts allowed to reconnect to the server (10 by default).</td>
</tr>
<tr><td><code>reconnect_time</code></td>
<td>The maximum time allowed to reconnect to the server (30 s by default).</td>
</tr>
</tbody>
</table>



>  You can skip generating the token in a test environment by setting *token* to `_no_need_token`. However, Agora does not recommend this in a production environment. By default, if you are already logged in, the `login` call will be ignored. To cancel the previous login, call `logout` first without having to wait until it is successfully executed. The system will stop reconnecting to the server, if either `reconnect_count` or `reconnect_time` is reached.

#### Sets the System Log（setDoLog）

```
setDoLog(isEnabled, level)
```

This method sets the system log.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>isEnabled</code></td>
<td><p>Whether to print the system log.</p>
<div><ul>
<li>true: Print the system log.</li>
<li>false: (Default) Do not print the system log.</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>level</code></td>
<td><p>Sets the level of the system log.</p>
<div><ul>
<li>DEBUG (default)</li>
<li>WARNING</li>
<li>INFO</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

#### Retrieves the SDK Version (getSDKVersion)

This method retrieves the SDK version and is synchronous. The return value is a string containing the SDK version, such as ‘1.3.0’.

```
getSDKVersion() : String
```

## <a name="session"></a>Session

### Methods

#### Remote Calling Process (invoke)

This method is used for the remote calling process and triggers the <code>onInvokeRet</code> callback function when called successfully.

```
invoke(func, args, cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Function</th>
<th>Parameter Settings and Returns</th>
</tr>
<tr><td>Queries the user status.</td>
<td><ul>
<li>name: io.agora.signal.user_query_user_status</li>
<li>req: {“account”:account of the user, must be a JSON string}</li>
<li>Returns: status(1:online; 0:offline)</li>
</ul>
</td>
</tr>
<tr><td>Sets a user attribute.</td>
<td><ul>
<li>name: io.agora.signal.user_set_attr</li>
<li>req: {“name”:attribute name,”value”:attribute value}</li>
</ul>
</td>
</tr>
<tr><td>Retrieves a user attribute.</td>
<td><ul>
<li>name: io.agora.signal.user_get_attr</li>
<li>req: {“account”:account of the user, must be a JSON string,”name”:attribute name}</li>
<li>Returns: {“value”:attribute value}</li>
</ul>
</td>
</tr>
<tr><td>Retrieves all attributes of a user.</td>
<td><ul>
<li>name: io.agora.signal.user_get_attr_all</li>
<li>req: {“account”:account of the user, must be a JSON string}</li>
<li>Returns: The JSON value of all attributes</li>
</ul>
</td>
</tr>
<tr><td>Retrieves the number of the users in a channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_query_num</li>
<li>req: {“name”:channel name}</li>
<li>Returns: All users in the channel</li>
</ul>
</td>
</tr>
<tr><td>Checks if a user is in a specific channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_query_user_isin</li>
<li>req: {“name”:channel name,”account”:account of the user, must be a JSON string}</li>
<li>Returns: isin（1: in the channel; 0:not in the channel）</li>
</ul>
</td>
</tr>
<tr><td>Retrieves a list of the users in a channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_query_userlist</li>
<li>req: {“name”:channel name}</li>
<li>Returns: {“num”:number of the users in the channel,”list”:the latest user}</li>
</ul>
</td>
</tr>
<tr><td><b>Deprecated</b> Retrieves a list of the latest users in a channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_query_userlist_all</li>
<li>req: {“name”:channel name,”num”:number (by default 1000*1000)}</li>
<li>Returns: {“num”:number of the users in the channel,”list”:the latest users}</li>
<li>Agora recommends using it on the server. If using it on the client, pay attention to the limit on the usage frequency.</li>
</ul>
</td>
</tr>
<tr><td>Clears all attributes of a specific channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_clear_attr</li>
<li>req: {“channel”:channel name}</li>
</ul>
</td>
</tr>
<tr><td>Deletes an attribute of a specific channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_del_attr</li>
<li>req: {“channel”:channel name,”name”:attribute name}</li>
</ul>
</td>
</tr>
<tr><td>Sets an attribute of a specific channel.</td>
<td><ul>
<li>name: io.agora.signal.channel_set_attr</li>
<li>req: {“channel”:channel name,”name”:attribute name,”value”:value name}</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Sends a Point-to-point Message (messageInstantSend)

This method sends a point-to-point message to a specified user.

```
messageInstantSend(peer, msg, cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>peer</code></td>
<td>Other user’s account.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Message. Maximum length is 8 k.</td>
</tr>
<tr><td><code>cb</code></td>
<td>Callback function upon successful execution of this method.</td>
</tr>
</tbody>
</table>



Sample code:

```
session.messageInstantSend = function(peer, msg, cb)
```

#### Log out of Agora's Signaling System (logout)

This method logs out of Agora’s signaling system.

```
logout()
```

#### Allows a User to Join a Channel (channelJoin)

This method allows a user to join a channel and returns a channel object.

```
channelJoin(channelID)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>channelID<code>/</td>
<td><p>Channel name. It can be up to 128 visible characters and include the following special channel names and attributes <b>Deprecated and NOT recommended</b>:</p>
<ul>
<li><code>_agora_user_online</code>: All user-online or user-offline events within the current appID will be sent to this channel.</li>
<li><code>_agora_channel_event</code>: All events about a user joining or leaving a channel within the current appID will be sent to this channel.</li>
</ul>
</td>
</tr>
</tbody>
</table>



>  Special channels are used by servers only to receive login and channel events.

Sample code:

```
var do_join = function(name){
    log('Joining channel' +  name);

    channel = session.channelJoin(name);
};
```

#### Initiates a Call (channelInviteUser2)

This method initiates a call; such as, to invite another user to join a specific channel.

```
channelInviteUser2(channelID, peer, extra) : Call
```

If the call fails, the user will receive the <code>onInviteFailed</code> callback function. Possible reasons include:

-   The other user is offline.
-   Local network issues.
-   Server errors.


<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>channelID</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>peer</code></td>
<td>The other user’s account.</td>
</tr>
<tr><td><code>extra</code></td>
<td><p>Extra information that the caller wants to send to the callee. It can be up to 8 KB visible characters and must be in JSON format. For example:</p>
<div><ul>
<li>{“_require_peer_online”:1}  If the counterpart is offline, the <code>onInviteFailed</code> callback function will be triggered immediately.</li>
<li>{“_require_peer_online”:0}  If the counterpart is offline for more than 20 seconds, the <code>onInviteFailed</code> callback function will be triggered (default).</li>
<li>{“destMediaUid” : 123}  Specify the <code>uid</code> for joining the corresponding media channel.</li>
<li>{“srcNum” : “+123456789”} Specify the phone number to be displayed on the screen of the remote phone.</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

>  You do not need to set the `_require_peer_online` field when making a SIP call.

#### Retrieves the Login Status (getStatus)

This method retrieves the login status and is synchronous. The return value indicates the current login status.

```
getStatus() : Integer
```

The following table shows the relationship between the return value and the login status.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Return Value</th>
<th>Login Status</th>
</tr>
<tr><td>0</td>
<td>Not logged in/Logged out.</td>
</tr>
<tr><td>1</td>
<td>Connecting/Logging in.</td>
</tr>
<tr><td>2</td>
<td>Logged in.</td>
</tr>
</tbody>
</table>

### Callback Functions

#### A User has Logged into the Agora’s Signaling System (onLoginSuccess)

This callback function is triggered when a user has logged into Agora’s signaling system.

```
onLoginSuccess(uid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>uid</code></td>
<td>Set as O.</td>
</tr>
</tbody>
</table>



Sample code:

```

```

### The Remote Calling Process has Succeeded (cb)

This callback function is triggered when the remote calling process has succeeded.

```
cb(err, ret);
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>callID</code></td>
<td>CallID introduced by invoke()</td>
</tr>
<tr><td><code>err</code></td>
<td>Error code</td>
</tr>
<tr><td><code>resp</code></td>
<td>Returned JSON value</td>
</tr>
</tbody>
</table>



#### A User has Failed to Log into Agora’s Signaling System (onLoginFailed)

This callback function is triggered when a user has failed to log into Agora’s signaling system.

```
onLoginFailed(ecode)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>ecode</code></td>
<td><p>The error code.</p>
<div><ul>
<li>SUCCESS = 0,</li>
<li>LOGOUT_E_OTHER = 100</li>
<li>LOGOUT_E_USER = 101,  // logout by the user</li>
<li>LOGOUT_E_NET = 102,  // network failure</li>
<li>LOGOUT_E_KICKED = 103,  // login another device</li>
<li>LOGOUT_E_PACKET = 104,</li>
<li>LOGOUT_E_TOKENEXPIRED = 105, // token expired</li>
<li>LOGOUT_E_OLDVERSION = 106,</li>
<li>LOGOUT_E_TOKENWRONG = 107,</li>
<li>LOGOUT_E_ALREADY_LOGOUT = 108,</li>
<li>LOGIN_E_OTHER = 200,</li>
<li>LOGIN_E_NET = 201,</li>
<li>LOGIN_E_FAILED = 202,</li>
<li>LOGIN_E_CANCEL = 203,</li>
<li>LOGIN_E_TOKENEXPIRED = 204,</li>
<li>LOGIN_E_OLDVERSION = 205,</li>
<li>LOGIN_E_TOKENWRONG = 206,</li>
<li>LOGIN_E_TOKEN_KICKED = 207,</li>
<li>LOGIN_E_ALREADY_LOGIN = 208,</li>
<li>JOINCHANNEL_E_OTHER = 300,</li>
<li>SENDMESSAGE_E_OTHER = 400,</li>
<li>SENDMESSAGE_E_TIMEOUT = 401,</li>
<li>QUERYUSERNUM_E_OTHER = 500,</li>
<li>QUERYUSERNUM_E_TIMEOUT = 501,</li>
<li>QUERYUSERNUM_E_BYUSER = 502,</li>
<li>LEAVECHANNEL_E_OTHER = 600,</li>
<li>LEAVECHANNEL_E_KICKED = 601,</li>
<li>LEAVECHANNEL_E_BYUSER = 602,</li>
<li>LEAVECHANNEL_E_LOGOUT = 603,</li>
<li>LEAVECHANNEL_E_DISCONN  = 604,</li>
<li>INVITE_E_OTHER = 700,</li>
<li>INVITE_E_REINVITE = 701,</li>
<li>INVITE_E_NET = 702,</li>
<li>INVITE_E_PEEROFFLINE = 703,</li>
<li>INVITE_E_TIMEOUT = 704,</li>
<li>INVITE_E_CANTRECV = 705,</li>
<li>GENERAL_E = 1000,</li>
<li>GENERAL_E_FAILED = 1001,</li>
<li>GENERAL_E_UNKNOWN = 1002,</li>
<li>GENERAL_E_NOT_LOGIN = 1003,</li>
<li>GENERAL_E_WRONG_PARAM = 1004,</li>
<li>GENERAL_E_LARGE_PARAM   = 1005</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### A User has Logged out of Agora’s Signaling System (onLogout)

This callback function is triggered when a user has logged out of the Agora signaling system.

```
onLogout(reason)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### A Message has been Received (onMessageInstantReceive)

This callback function notifies a user that a message has been received.

```
onMessageInstantReceive(account, uid, msg)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>account</code></td>
<td>Caller’s account.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Set as 0.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Received instant message.</td>
</tr>
</tbody>
</table>



#### A Call Request has been Received (onInviteReceived)

This callback function is received by the callee when the callee has received a call invitation or request.

```
onInviteReceived(call)
```

## <a name="call"></a>Call

### Methods

#### Accepts a Call Invitation (channelInviteAccept)

This method accepts a call invitation. Once this method is called, the caller will receive the <code>onInviteAcceptedByPeer</code> callback function.

```
channelInviteAccept(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string. Can be “”.</td>
</tr>
</tbody>
</table>



#### Rejects a Call (channelInviteRefuse)

This method to rejects a call invitation. Once this API is called, the other user will receive the <code>onInviteRefusedByPeer</code> event.

```
channelInviteRefuse(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or NULL.</td>
</tr>
</tbody>
</table>



#### Sends a DTMF (Dual-tone Multi-frequency) Message to the Other User (channelInviteDTMF)

This method sends a DTMF message to the other user by calling through the SIP gateway. The receiver will receive the <code>onInviteMsg</code> callback function.

```
channelInviteDTMF(dtmf)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>dtmf</code></td>
<td>Subsequent analog phone number (DTMF) to be entered after the call goes through.</td>
</tr>
</tbody>
</table>



#### Ends a Call (channelInviteEnd)

This method ends a call. The user receives the <code>onInviteEndByMyself</code> callback function, and the other user receives the <code>onInviteEndByPeer</code> callback function.

```
channelInviteEnd()
```

### Callback Functions

#### The Callee has Received the Call Request (onInviteReceivedByPeer)

This callback function is received by the caller when the callee has received the call invitation or request.

```
onInviteReceivedByPeer(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



#### The Callee has Accepted the Call Request (onInviteAcceptedByPeer)

This callback function is received by the caller when the callee has accepted the call invitation or request.

```
onInviteAcceptedByPeer(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



#### The Callee has Rejected the Call Request (onInviteRefusedByPeer)

This callback function is received by the caller when the callee rejects the call invitation or request.

```
onInviteRefusedByPeer(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



#### A Call has Failed (onInviteFailed)

This callback function is triggered when a call has failed.

```
onInviteFailed(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



#### The Callee has Ended the Call (onInviteEndByPeer)

This callback function is triggered when the callee has ended the call.

```
onInviteEndByPeer(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



#### The Caller has Ended the Call (onInviteEndByMyself)

This callback function is triggered when the caller has ended the call.

```
onInviteEndByMyself(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra<c/ode></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



Sample code:

#### A Message sent by the Callee has been Received (onInviteMsg)

This callback function is triggered when the caller has received the message sent by the callee.

```
onInviteMsg(extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information about the call. Use a string, a JSON string, or Null.</td>
</tr>
</tbody>
</table>



## <a name="channel"></a>Channel

### Methods

#### Sends a Channel Message (messageChannelSend)

This method sends a channel message, and all users in the same channel receive the <code>onMessageChannelReceive</code> callback function.

```
messageChannelSend(msg, cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td>msg</td>
<td>Channel message. Each channel message must not exceed 8 KB visible characters. Each user cannot send more than 60 messages per second, and the entire channel cannot send more than 200 messages per second.</td>
</tr>
<tr><td><code>cb</code></td>
<td>Callback function when this method is called successfully.</td>
</tr>
</tbody>
</table>



Sample code:

```
channel.messageChannelSend( "message", function(){
  // channel message was sent
});
```

#### Allows a User to Leave a Channel (channelLeave)

This method allows a user to leave the current channel. Once the user has left the channel, all other users in the channel receive the <code>onChannelUserLeaved</code> callback function.

```
channelLeave(cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>cb</code></td>
<td>Callback function when this method is called successfully.</td>
</tr>
</tbody>
</table>



Sample code:

```
channel.channelLeave(function(){
// left the channel
});
```

#### Sets the Channel Attributes (channelSetAttr)

This method sets the channel attributes. Whenever a change is made on an attribute, all users in the channel receive the <code>onChannelAttrUpdated</code> callback function.

```
channelSetAttr(k, v, cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>k</code></td>
<td>Attribute name.</td>
</tr>
<tr><td><code>v</code></td>
<td>Attribute value.</td>
</tr>
<tr><td><code>cb</code></td>
<td>Callback function when this method is called successfully.</td>
</tr>
</tbody>
</table>



The following table shows the built-in attributes of the parameter, <code>k</code>:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Attributes</th>
<th>Description</th>
</tr>
<tr><td><code>_userNotification</code><b>Deprecated</b></td>
<td><ul>
<li>1: (Default) The channel sends the callback function that a user has joined or left the channel.</li>
<li>0: The channel does not send the callback function that a user has joined or left the channel.</li>
</ul>
</td>
</tr>
<tr><td><code>_channel_ttl</code></td>
<td>Time (s) between the last user leaving a channel and the system destroying the channel. The default value is 7200. A special channel will never be destroyed.</td>
</tr>
<tr><td><code>_member_num</code></td>
<td>Number of the users currently in the channel. It is automatically updated by the system.</td>
</tr>
<tr><td><code>_auto_update_num</code></td>
<td><p>Indicates whether to automatically refresh the number of the users currently in the channel.</p>
<ul>
<li>0: (Default) Disabled</li>
<li>1 - n: Refreshing frequency (s)</li>
</ul>
</td>
</tr>
<tr><td><code>_total_member_num</code><b>Deprecated</b></td>
<td>Automatically indicates the accumulated number of the users in the channel. Similar to <code>_member_num</code> , <code>_auto_update_num</code> and <code>_auto_update_total_num</code> both need to be set to none zero.</td>
</tr>
<tr><td><code>_auto_update_total_num</code><b>Deprecated</b></td>
<td><p>Indicates whether to automatically refresh the number of the users currently in the channel.</p>
<ul>
<li>0: (Default) Disabled</li>
<li>1 - n: Refreshing frequency (s) (same as <code>_auto_update_num</code> )</li>
</ul>
</td>
</tr>
</tr>
<tr><td><code>_sendmsg_limit</code></td>
<td>Maximum messages allowed to be sent in the channel.</td>
</tr>
</tr>
<tr><td><code>_setattr_limit</code></td>
<td>Maximum times for updating a channel attribute in a second.</td>
</tr>
</tbody>
</table>



Sample code:

```
channel.channelSetAttr("name", "john", function(){
  // attr name was set
```

});

#### Deletes a Channel Attribute (channelDelAttr)

This method deletes a specific attribute of the current channel.

```
channelDelAttr(k, cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>k</code></td>
<td>Attribute name.</td>
</tr>
<tr><td><code>cb</code></td>
<td>Callback function when this method is called successfully.</td>
</tr>
</tbody>
</table>



Sample code:

```
channel.channelDelAttr("name", function(){
  // attr name was deleted
});
```

#### Deletes all Channel Attributes (channelClearAttr)

This method deletes all attributes of the current channel.

```
channelClearAttr(cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>cb</code></td>
<td>Callback function when this method is called successfully.</td>
</tr>
</tbody>
</table>



Sample code:

```
channel.channelClearAttr(function(){
  // attr was cleared
});
```

### Callback Functionss

#### A User has Joined a Channel (onChannelJoined)

This callback function is triggered when a user has joined a channel.

```
onChannelJoined()
```

#### A User has Failed to Join a Channel (onChannelJoinFailed)

This callback function is triggered when a user has failed to join a channel.

```
onChannelJoinFailed(ecode)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### A User has Left a Channel (onChannelLeaved)

This callback function is triggered when a user has left a channel.

```
onChannelLeaved(code)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### Another User has Joined the Channel (onChannelUserJoined)

This callback function is triggered when another user has joined the channel.

```
onChannelUserJoined(account, uid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Set as 0.</td>
</tr>
</tbody>
</table>



#### Another User has Left the Channel (onChannelUserLeaved)

This callback function is triggered when another user has left the channel.

```
onChannelUserLeaved(account, uid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Set as 0.</td>
</tr>
</tbody>
</table>



#### The User List in the Channel (onChannelUserList)

A user will receive this callback function after joining a channel.

>  The retrieved user list includes a maximum of the latest 200 users in the channel.

```
onChannelUserList(users)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>users</code></td>
<td>User list of the channel the user has joined, with the following format:  <code>[[account1, uid1], [account2, uid2], …, [accountn, uidn]]</code>.</td>
</tr>
</tbody>
</table>



#### The Channel Attribute has Changed (onChannelAttrUpdated)

This callback function is triggered when the channel attribute has changed.

```
onChannelAttrUpdated(k, v, type)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>k</code></td>
<td>Attribute name.</td>
</tr>
<tr><td><code>v</code></td>
<td>Attribute value.</td>
</tr>
<tr><td><code>type</code></td>
<td><p>The change types:</p>
<ul>
<li>“update”: Update</li>
<li>“del”: Delete</li>
<li>“clear”: Delete all</li>
<li>“set”: N/A</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### A Channel Message has been Received (onMessageChannelReceive)

This callback function is triggered when a channel message has been received.

```
onMessageChannelReceive(account, uid, msg)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Parameter</th>
<th>Description</th>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>System generated internal uid.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Message.</td>
</tr>
</tbody>
</table>



## Error Codes and Warning Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_signaling.md).
