
---
title: Server SDK API
description: 
platform: Java
updatedAt: Wed Jul 31 2019 12:51:22 GMT+0800 (CST)
---
# Server SDK API
> Version: v1.4.0 BETA

The Server SDK API includes the following classes:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Class</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><a href="#signaling-class">Signaling Class</a></td>
<td>Basic Signaling class.</td>
</tr>
<tr><td><a href="#loginsession-class">LoginSession Class</a></td>
<td>An internal class of the Signaling Class that creates a <code>LoginSession</code> object when the <code>login</code> method of the Signaling Class is called.</td>
</tr>
<tr><td><a href="#channel-class">Channel Class</a></td>
<td>An internal class of the Signaling Class that creates a <code>Channel</code> object when the <code>channelJoind</code> method of the <code>LoginSession</code> object is called.</td>
</tr>
<tr><td><a href="#call-class">Call Class</a></td>
<td>An internal class of the Signaling Class that creates a <code>Call</code> object when the <code>channelInviteUser2</code> method of the <code>LoginSession</code> object is called.</td>
</tr>
<tr><td><a href="#userattrcallback-class">UserAttrCallback Class</a></td>
<td>An internal class of the Signaling Class that manages user-attribute-related callbacks.</td>
</tr>
<tr><td><a href="#logincallback-class">LoginCallback Class</a></td>
<td>An internal class of the Signaling Class that manages login callbacks.</td>
</tr>
<tr><td><a href="#messagecallback-class">MessageCallback Class</a></td>
<td>An internal class of the Signaling Class that manages message callbacks.</td>
</tr>
<tr><td><a href="#channelcallback-class">ChannelCallback Class</a></td>
<td>An internal class of the Signaling Class that manages channel callbacks.</td>
</tr>
<tr><td><a href="#callcallback-class">CallCallback Class</a></td>
<td>An internal class of the Signaling Class that manages call callbacks.</td>
</tr>
</tbody>
</table>



**Unlike other Signaling APIs**, the Server SDK APIs allow multiple signaling objects \(instances\). Although each signaling instance can include multiple sessions, for now, only one instance allows you to join one channel only.

> Avoid using synchronous or block callback operations. Otherwise, the signaling system may crash.

## Signaling Class

The Signaling Class is the basic class of signaling.

### Methods

#### Create a Signaling Object \(Signal\)

This method creates a signaling object.

```
public Signal(String appid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>appid</code></td>
<td>App account provided by Agora. See <a href="../../en/Agora%20Platform/key_signaling.md"><span>Get an App ID</span></a> for details.</td>
</tr>
</tbody>
</table>



#### Enable a Debug Log \(setDoLog\)

This method enables a debug log.

```
public void setDoLog(boolean doLog)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><Lcode>doLog</code></td>
<td><ul>
<li>True: Enable debug log</li>
<li>False: Disable debug log</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Login \(login\)

This method allows you to login Agora’s signaling system and it returns a LoginSession instance object. You must login before performing any action.

```
public Signal.LoginSession login(String account, String token, Signal.LoginCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client. Maximum length is 128 visible characters (space not permitted). Can be a uid, nickname, guid, or any other content that is unique.</td>
</tr>
<tr><td><code>token</code></td>
<td>SignalingToken generated from the App ID and App Certificate. See <a href="../../en/Agora%20Platform/key_signaling.md"><span>How to Get and Use a SignalingToken</span></a> for details.</td>
</tr>
<tr><td><code>cb</code></td>
<td>LoginCallback instance object.</td>
</tr>
</tbody>
</table>



> You can skip the generation of the token in a test environment by setting `token` to `_no_need_token`. Agora does not recommend doing so in a production environment. By default, if you are already logged in, the call of `login` will be ignored. To cancel the previous login, call `logout` beforehand without having to wait until it is successfully executed.

#### Set a User Attribute \(userSetAttr\)

This method sets a specified attribute of a user.

```
public void userSetAttr(String name, String value, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>Attribute name</td>
</tr>
<tr><td><code>value</code></td>
<td>Attribute value</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserAttrCallback instance</td>
</tr>
</tbody>
</table>



#### Get a User Attribute \(userGetAttr\)

This method gets a specified attribute of a user. A UserAttrCallback instance object is returned.

```
public void userGetAttr(String account, String name, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>User account</td>
</tr>
<tr><td><code>name</code></td>
<td>Attribute name</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserAttrCallback instance</td>
</tr>
</tbody>
</table>



#### Get all the User Attributes \(userGetAttrAll\)

This method gets all the attributes of a user. A UserAttrCallback instance object is returned.

```
public void userGetAttrAll(String account, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>User account</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserGetAttrCallback instance</td>
</tr>
</tbody>
</table>



#### Query the Number of Users in a Channel \(channelQueryUserNum\)

This method gets the number of users in a channel. A UserAttrCallback instance object is returned.

```
public void channelQueryUserNum(String name, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>Channel name</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserAttrCallback instance</td>
</tr>
</tbody>
</table>



#### Get the Current Status \(getStatus\)

This method gets the current login status. The following table shows the relationship between the return value and the login status:

```
public int getStatus()
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Return Value</strong></td>
<td><strong>Login Status</strong></td>
</tr>
<tr><td>0</td>
<td>Not logged in/Logged out</td>
</tr>
<tr><td>1</td>
<td>Connecting/Logging in</td>
</tr>
<tr><td>2</td>
<td>Logged in</td>
</tr>
<tr><td>3</td>
<td>Login failed</td>
</tr>
<tr><td>4</td>
<td>Initial state</td>
</tr>
</tbody>
</table>



## LoginSession Class

An internal class of the Signaling Class that creates a `LoginSession` object when the `login` method of the Signaling Class is called.

### Methods

#### Logout \(logout\)

This method triggers the `onLogout` callback upon a successful execution.

```
public void logout();
```

#### Send a Point-to-point Message \(messageInstantSend\)

This method sends a point-to-point message to a specified account.

```
public void messageInstantSend(String account, String msg, Signal.MessageCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client.</td>
</tr>
<tr><td><code>msg</code></td>
<td>The message body. Maximum length is 8-k visible characters.</td>
</tr>
<tr><td><code>cb</code></td>
<td><code>MessageCallback</code> instance object.</td>
</tr>
</tbody>
</table>



#### Send a Point-to-Point Message \(messageInstantSend\)

This method sends a point-to-point message to a specified account.

```
public void messageInstantSend(String account, String msg, int ttl, Signal.MessageCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Message body. Maximum length is 8-k visible characters.</td>
</tr>
<tr><td><code>ttl</code></td>
<td>Time-to-live.</td>
</tr>
<tr><td><code>cb</code></td>
<td><code>MessageCallback</code> instance object.</td>
</tr>
</tbody>
</table>



#### Initiate a Call \(channelInviteUser2\)

This method initiates a call, such as inviting another user to join a specific channel. Calling and joining a channel are two different processes.

```
public Signal.LoginSession.Call channelInviteUser2(String channel, String peer, String extra, final Signal.CallCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name. Maximum length is 128 visible characters.</td>
</tr>
<tr><td><code>peer</code></td>
<td>Account of the other user.</td>
</tr>
<tr><td><code>extra</code></td>
<td><p>Extra information that the caller wants to send to the callee. It can be up to 8 KB visible characters and must be in JSON format. For example:</p>
<div><ul>
<li>{“_require_peer_online”:1}  If the counterpart is offline, the <code>onInviteFailed</code> callback will be triggered immediately.</li>
<li>{“_require_peer_online”:0}  If the counterpart is offline for more than 20 seconds, the <code>onInviteFailed</code> callback will be triggered (default).</li>
<li>{“destMediaUid” : 123}  Specify the uid for joining the corresponding media channel.</li>
<li>{“srcNum” : “+123456789”} Specify the phone number to be displayed on the screen of the remote phone.</li>
</ul>
</div><
</td>
</tr>
<tr><td><code>cb</code></td>
<td><code>CallCallback</code> instance object.</td>
</tr>
</tbody>
</table>

> You do not need to set the `_require_peer_online` field when making a SIP call.

#### Join a Channel \(channelJoin\)

This method allows a user to join a specified channel. Whenever he/she joins a specified channel, he/she will be disconnected from any previously joined channel.

```
public Signal.LoginSession.Channel channelJoin(String name, Signal.ChannelCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>name</code></td>
<td><p>Channel name. It can be up to 128 visible characters and include the following special channel names and attributes <b>Deprecated and NOT recommended</b>:</p>
<ul>
<li><code>_agora_user_online</code>: All user-online or user-offline events within the current appID will be sent to this channel.</li>
<li><code>_agora_channel_event</code>: All events about a user joining or leaving a channel within the current appID will be sent to this channel.</li>
</ul>
</td>
</tr>
<tr><td><code>cb</code></td>
<td><code>ChannelCallback</code> instance object.</td>
</tr>
</tbody>
</table>

> Special channels are used by servers only to receive login and channel events.

## Channel Class

An internal class of the Signaling Class that creates a `Channel` object when the `channelJoin` method of the `LoginSession` object is called.

### Methods

#### Leave a Channel \(channelLeave\)

This method allows a user to leave a channel. Once the user has left the channel, all other users in the channel receive the `onChannelUserLeaved` callback.

```
public void channelLeave()
```

#### Set the Channel Attributes \(channelSetAttr\)

This method sets the channel attributes. Whenever a change is made on an attribute, all users in the channel receive the `onChannelAttrUpdated` callback.

```
public void channelSetAttr(String name, String value)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>Attribute name. Maximum length is 128 visible characters.</td>
</tr>
<tr><td><code>value</code></td>
<td>Attribute value. Maximum length is 8096 visible characters.</td>
</tr>
</tbody>
</table>



The following table shows the built-in attributes of the parameter, name:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Attributes</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><b>Deprecated</b> <code>_userNotification</code></td>
<td><ul>
<li>1: Default value. The channel sends callbacks that a user has joined or left the channel.</li>
<li>0: The channel does not send callbacks that a user has joined or left the channel.</li>
</ul>
</td>
</tr>
<tr><td><code>_channel_ttl</code></td>
<td>Time (s) between the last user leaving a channel and the system destroying the channel. The default value is 7200. A special channel will never be destroyed.</td>
</tr>
<tr><td><b>Deprecated</b> <code>_member_num</code></td>
<td>Number of the users currently in the channel. It is automatically updated by the system.</td>
</tr>
<tr><td><b>Deprecated</b> <code>_auto_update_num</code></td>
<td><p>Indicates whether to automatically refresh the number of the users currently in the channel.</p>
<ul>
<li>0: Disabled (default)</li>
<li>1 - n: Refreshing frequency (s)</li>
</ul>
</td>
</tr>
<tr><td><code>_total_member_num</code></td>
<td>Automatically indicates the accumulated number of the users in the channel. Similar to <code>_member_num</code> , <code>_auto_update_num</code> and <code>_auto_update_total_num</code> both need to be set to none zero.</td>
</tr>
<tr><td><code>_auto_update_total_num</code></td>
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



#### Delete a Channel Attribute \(channelDelAttr\)

This method deletes a specific attribute of the current channel.

```
public void channelDelAttr(String name)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>Attribute name. Maximum length is 128 visible characters.</td>
</tr>
</tbody>
</table>



#### Delete all Channel Attributes \(channelClearAttr\)

This method deletes all attributes of the current channel.

```
public void channelClearAttr()
```

#### Send a Channel Message \(messageChannelSend\)

This method sends a channel message to all users in the same channel.

```
public void messageChannelSend(String msg)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>msg</code></td>
<td>Message body. Each channel message must not exceed 8 k visible characters. A user cannot send more than 60 messages per second, and the entire channel can not send more than 200 messages per second.</td>
</tr>
</tbody>
</table>



## Call Class

An internal class of the Signaling Class that creates a `Call` object when the `channelInviteUser2` method of the `LoginSession` object is called.

### Methods

#### Accept a Call \(channelInviteAccept\)

This method accepts the received call request/invitation. The caller will receive the onInviteAcceptedByPeer callback.

```
public void channelInviteAccept()
```

#### Accept a Call \(channelInviteAccept\)

This method accepts the received call request/invitation.

```
public void channelInviteAccept(String extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format. Can be “”.</td>
</tr>
</tbody>
</table>



#### Reject a Call \(channelInviteRefuse\)

This method rejects the received call invitation/request. Once this API is called, the other user will receive the `onInviteRefusedByPeer` event.

```
public void channelInviteRefuse()
```

#### Reject a Call \(channelInviteRefuse\)

This method rejects the received call invitation/request. Once this API is called, the other user will receive the `onInviteRefusedByPeer` event.

```
public void channelInviteRefuse(String extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### End a Call \(channelInviteEnd\)

This method ends a call. The user receives the `onInviteEndByMyself` event, and the other user receives the `onInviteEndByPeer` event.

```
public void channelInviteEnd(String extra)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### End a Call \(channelInviteEnd\)

This method ends a call. The user receives the `onInviteEndByMyself` event, and the other user receives the `onInviteEndByPeer` event.

```
public void channelInviteEnd()
```

## UserAttrCallback Class

An internal class of the Signaling Class that manages user attribute callbacks.

### Callbacks

#### Get the User Attribute Callback \(onUserGetAttr\)

This callback is triggered when getting the user attribute.

```
public void onUserGetAttr(String err, JSONObject ret)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>Error information. An empty string means that there is no error.</td>
</tr>
<tr><td><code>ret</code></td>
<td><ul>
<li>Success: {“result”: “ok”, “account”: “user account”, “name”: “attr name”, “value”: “attr value”}</li>
<li>Failure: {“result”: “failed”, “reason”: “failed reason”}</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Get all the User Attributes Callback \(onUserGetAttrAll\)

This callback is triggered when getting all the attributes of a user.

```
public void onUserGetAttrAll(String err, JSONObject ret)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>Error information. An empty string means that there is no error.</td>
</tr>
<tr><td><code>ret</code></td>
<td><ul>
<li>Success: {“result”: “ok”, “account”: “user account”, “json”: {“attr 1” : “value 1”, “attr 2” : “value2”}</li>
<li>Failure: {“result”: “failed”, “reason”: “failed reason”}</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### Set the User Attribute Callback \(onUserSetAttr\)

This callback is triggered when setting an attribute of a user.

```
public void onUserSetAttr(String err, JSONObject ret)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>Error information. An empty string means that there is no error.</td>
</tr>
<tr><td><code>ret</code></td>
<td><ul>
<li>Success: {“result”: “ok”, “reason”: “”}</li>
<li>Failure: {“result”: “failed”, “reason”: “failed reason”}</li>
</ul>
</td>
</tr>
</tbody>
</table>



## LoginCallback Class

An internal class of the Signaling Class that manages login-related callbacks.

### Callbacks

#### User Online Callback \(onLoginSuccess\)

This callback is triggered when a user is logged in the Agora signaling system.

```
public void onLoginSuccess(Signal.LoginSession session, int uid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Internal User ID generated by the system.</td>
</tr>
</tbody>
</table>



#### User Offline Callback \(onLogout\)

This callback is triggered when a user has logged out of the Agora signaling system.

```
public void onLogout(Signal.LoginSession session, int ecode)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### User Login Failed Callback \(onLoginFailed\)

This callback is triggered when a user fails to login the Agora signaling system.

```
public void onLoginFailed(Signal.LoginSession session, int ecode)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### Message Received Callback \(onMessageInstantReceive\)

This callback notifies a user that he/she has received a message.

```
public void onMessageInstantReceive(Signal.LoginSession session, String account, int uid, String msg)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>account</code></td>
<td>User ID defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Internal User ID generated by the system.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Message body.</td>
</tr>
</tbody>
</table>



#### Call Invitation Received Callback \(onInviteReceived\)

This callback is triggered when a user has received a call invitation/request.

```
public void onInviteReceived(Signal.LoginSession session, Signal.LoginSession.Call call)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call instance object.</td>
</tr>
</tbody>
</table>



#### Error Reported Callback \(onError\)

This callback is triggered when an error is detected.

```
public void onError(Signal.LoginSession session, int ecode, String reason)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code. For more information, see <a href="../../en/API%20Reference/the_error_signaling.md"><span>Error Codes and Warning Codes</span></a>.</td>
</tr>
<tr><td><code>reason</code></td>
<td>Reason why the error occurred.</td>
</tr>
</tbody>
</table>



## MessageCallback Class

An internal class of the Signaling Class that manages message-related callbacks.

### Callbacks

#### Message Sent Callback \(onMessageSendSuccess\)

This callback is triggered when a user has sent a message.

```
public void onMessageSendSuccess(Signal.LoginSession session)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
</tbody>
</table>



#### Message Failure Callback \(onMessageSendError\)

This callback is triggered when a user fails to send a message.

```
public void onMessageSendError(Signal.LoginSession session, int ecode)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



## ChannelCallback Class

An internal class of the Signaling Class that manages channel-related callbacks.

### Callbacks

#### User Joined Channel Callback \(onChannelJoined\)

This callback is triggered when a user has joined a channel.

```
public void onChannelJoined(Signal.LoginSession session, Signal.LoginSession.Channel channelName)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
</tbody>
</table>



#### User Failed to Join Channel Callback \(onChannelJoinFailed\)

This callback is triggered when a user has failed to join a channel.

```
public void onChannelJoinFailed(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session instance.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel instance.</td>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### User Left Channel Callback \(onChannelLeaved\)

This callback is triggered when the user has left a channel.

```
public void onChannelLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>ecode</code></td>
<td>Error code.</td>
</tr>
</tbody>
</table>



#### Other User Joined Channel Callback \(onChannelUserJoined\)

This callback is triggered when another user has joined the channel.

```
public void onChannelUserJoined(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid);


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>The session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>The channel Name.</td>
</tr>
<tr><td><code>account</code></td>
<td>The account of the other user defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>The internal User ID generated automatically by the system.</td>
</tr>
</tbody>
</table>



#### Other User Left Channel Callback \(onChannelUserLeaved\)

This callback is triggered when a user has left the channel.

```
public void onChannelUserLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>account</code></td>
<td>Account of the other user defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Internal user ID generated by the system.</td>
</tr>
</tbody>
</table>



#### User List Received Callback \(onChannelUserList\)

A user will receive this callback when he/she has joined a channel.

**Note:** 

The retrieved user list includes a maximum of 200 users latest in the channel.

```
public void onChannelUserList(Signal.LoginSession session, Signal.LoginSession.Channel channel, List<String> users, List<Integer> uids)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>users</code></td>
<td>List of user accounts used to login the app.</td>
</tr>
<tr><td><code>uid</code></td>
<td>List of users in the channel.</td>
</tr>
</tbody>
</table>



#### Channel Attribute Changed Callback \(onChannelAttrUpdated\)

This callback is triggered when a channel attribute is changed.

```
public void onChannelAttrUpdated(Signal.LoginSession session, Signal.LoginSession.Channel channel, String name, String value, String type)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>name</code></td>
<td>Name of the attribute.</td>
</tr>
<tr><td><code>value</code></td>
<td>Value of the attribute.</td>
</tr>
<tr><td><code>type</code></td>
<td>Type of the attribute.</td>
</tr>
</tbody>
</table>



#### Query the Number of Users in a Channel Callback \(onChannelQueryUserNum\)

This callback is triggered when the number of users in a channel is queried.

```
public void onChannelQueryUserNum(Signal.LoginSession session, String err, int num)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>name</code></td>
<td>Name of the attribute.</td>
</tr>
<tr><td><code>value<code></td>
<td>Value of the attribute.</td>
</tr>
<tr><td><code>type</code></td>
<td>Type of the attribute.</td>
</tr>
</tbody>
</table>



#### Channel Message Received Callback \(onMessageChannelReceive\)

This callback is trigged upon receiving the channel message.

```
public void onMessageChannelReceive(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid, String msg)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>channel</code></td>
<td>Channel name.</td>
</tr>
<tr><td><code>account</code></td>
<td>Account of the other user defined by the client.</td>
</tr>
<tr><td><code>uid</code></td>
<td>Internal user ID generated by the system.</td>
</tr>
<tr><td><code>msg</code></td>
<td>Message body.</td>
</tr>
</tbody>
</table>



## CallCallback Class

An internal class of the Signaling Class that manages call-related callbacks.

### Callbacks

#### Other User Received Call Callback \(onInviteReceivedByPeer\)

This callback is triggered when the callee has received a call invitation/request.

```
public void onInviteReceivedByPeer(Signal.LoginSession session, Signal.LoginSession.Call call)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
</tbody>
</table>



#### Other User Accepted Call Callback \(onInviteAcceptedByPeer\)

This callback is triggered when the callee has accepted a call invitation/request.

```
public void onInviteAcceptedByPeer(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### Other User Rejected Call \(onInviteRefusedByPeer\)

This callback is triggered when the callee has rejected a call invitation/request.

```
public void onInviteRefusedByPeer(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### Call Failed Callback \(onInviteFailed\)

This callback is triggered when a call fails.

```
public void onInviteFailed(Signal.LoginSession session, Signal.LoginSession.Call call, int ecode)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### Other User Ended Call Callback \(onInviteEndByPeer\)

This callback is triggered when the callee has ended the call.

```
public void onInviteEndByPeer(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### Call Ended Callback \(onInviteEndByMyself\)

This callback is triggered when the user ended the call.

```
public void onInviteEndByMyself(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. For example, specifying if it is a video or voice call. Must be in JSON format.</td>
</tr>
</tbody>
</table>



#### Message Received Callback \(onInviteMsg）

This callback is triggered when the user has received a DTMF message from the other user.

```
public void onInviteMsg(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Parameter</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>Session name.</td>
</tr>
<tr><td><code>call</code></td>
<td>Call object.</td>
</tr>
<tr><td><code>extra</code></td>
<td>Extra information the caller wants to deliver to the callee. Must be in JSON format.</td>
</tr>
</tbody>
</table>



## Error Codes and Warning Codes

See [Error Codes and Warning Codes](../../en/API%20Reference/the_error_signaling.md).
