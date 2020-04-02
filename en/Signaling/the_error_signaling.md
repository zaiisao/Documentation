
---
title: Error Codes and Warning Codes
description: 
platform: All Platforms
updatedAt: Thu Apr 02 2020 05:47:49 GMT+0800 (CST)
---
# Error Codes and Warning Codes
The Agora Signaling SDK will return error or warning codes when calling APIs or during runtime:

-   **Error Codes** occur when the SDK encounters an error that cannot be recovered automatically without any application intervention. For example, the SDK returns an error if it fails to turn on the camera, and reminds the user not to use the camera.

-   **Warning Codes** occur when the SDK encounters an error that might be recovered automatically. These are only notifications, and can generally be ignored.


<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Error Code</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>SUCCESS</td>
<td>0</td>
<td>No error.</td>
</tr>
<tr><td>LOGOUT_E_OTHER</td>
<td>100</td>
<td>Logout due to reasons such as network issues.</td>
</tr>
<tr><td>LOGOUT_E_USER</td>
<td>101</td>
<td>A user logged out.</td>
</tr>
<tr><td>LOGOUT_E_NET</td>
<td>102</td>
<td>A network issue.</td>
</tr>
<tr><td>LOGOUT_E_KICKED</td>
<td>103</td>
<td>The account is logged in another place.</td>
</tr>
<tr><td>LOGOUT_E_PACKET</td>
<td>104</td>
<td>This error code is obsolete.</td>
</tr>
<tr><td>LOGOUT_E_TOKENEXPIRED</td>
<td>105</td>
<td>The SignalingToken expired.</td>
</tr>
<tr><td>LOGOUT_E_OLDVERSION</td>
<td>106</td>
<td>This error code is obsolete.</td>
</tr>
<tr><td>LOGOUT_E_TOKENWRONG</td>
<td>107</td>
<td>This error code is obsolete.</td>
</tr>
<tr><td>LOGOUT_E_ALREADY_LOGOUT</td>
<td>108</td>
<td>Called logout() when already logged out of the signaling system.</td>
</tr>
<tr><td>LOGIN_E_OTHER</td>
<td>200</td>
<td>An unknown reason.</td>
</tr>
<tr><td>LOGIN_E_NET</td>
<td>201</td>
<td>A network issue.</td>
</tr>
<tr><td>LOGIN_E_FAILED</td>
<td>202</td>
<td>The login was rejected by the server.</td>
</tr>
<tr><td>LOGIN_E_CANCEL</td>
<td>203</td>
<td>The user cancelled the login.</td>
</tr>
<tr><td>LOGIN_E_TOKENEXPIRED</td>
<td>204</td>
<td>The SignalingToken expired, and the login was rejected.</td>
</tr>
<tr><td>LOGIN_E_OLDVERSION</td>
<td>205</td>
<td>This error code is obsolete.</td>
</tr>
<tr><td>LOGIN_E_TOKENWRONG</td>
<td>206</td>
<td>The SignalingToken is invalid.</td>
</tr>
<tr><td>LOGIN_E_TOKEN_KICKED</td>
<td>207</td>
<td>The user used a renewed SignalingToken to login another place.</td>
</tr>
<tr><td>LOGIN_E_ALREADY_LOGIN</td>
<td>208</td>
<td>The user has already logged in. You can ignore this error.</td>
</tr>
<tr><td>LOGIN_E_INVALID_USER</td>
<td>209</td>
<td>The user ID is invalid.</td>
</tr>
<tr><td>JOINCHANNEL_E_OTHER</td>
<td>300</td>
<td>Failed to join a channel.</td>
</tr>
<tr><td>SENDMESSAGE_E_OTHER</td>
<td>400</td>
<td>Failed to send message.</td>
</tr>
<tr><td>SENDMESSAGE_E_TIMEOUT</td>
<td>401</td>
<td>A timeout in sending a message.</td>
</tr>
<tr><td>QUERYUSERNUM_E_OTHER</td>
<td>500</td>
<td>Failed to query the channel user number.</td>
</tr>
<tr><td>QUERYUSERNUM_E_TIMEOUT</td>
<td>501</td>
<td>A timeout in querying the channel user number.</td>
</tr>
<tr><td>QUERYUSERNUM_E_BYUSER</td>
<td>501</td>
<td>This error code is obsolete.</td>
</tr>
<tr><td>LEAVECHANNEL_E_OTHER</td>
<td>600</td>
<td>The user left the channel due to an unknown reason.</td>
</tr>
<tr><td>LEAVECHANNEL_E_KICKED</td>
<td>601</td>
<td>The user was kicked out by the administrator.</td>
</tr>
<tr><td>LEAVECHANNEL_E_BYUSER</td>
<td>602</td>
<td>The user has left the channel.</td>
</tr>
<tr><td>LEAVECHANNEL_E_LOGOUT</td>
<td>603</td>
<td>The user was kicked out of the channel upon logout.</td>
</tr>
<tr><td>LEAVECHANNEL_E_DISCONN</td>
<td>604</td>
<td>The user left the channel due to a network outage.</td>
</tr>
<tr><td>INVITE_E_OTHER</td>
<td>700</td>
<td>The call failed.</td>
</tr>
<tr><td>INVITE_E_REINVITE</td>
<td>701</td>
<td>Repeated calls.</td>
</tr>
<tr><td>INVITE_E_NET</td>
<td>702</td>
<td>A network issue.</td>
</tr>
<tr><td>INVITE_E_PEEROFFLINE</td>
<td>703</td>
<td>The other user is offline.</td>
</tr>
<tr><td>INVITE_E_TIMEOUT</td>
<td>704</td>
<td>A call timeout.</td>
</tr>
<tr><td>INVITE_E_CANTRECV</td>
<td>705</td>
<td>This error code is obsolete.</td>
</tr>
<tr><td>GENERAL_E</td>
<td>1000</td>
<td>A general error.</td>
</tr>
<tr><td>GENERAL_E_FAILED</td>
<td>1001</td>
<td>A general error: Failure.</td>
</tr>
<tr><td>GENERAL_E_UNKNOWN</td>
<td>1002</td>
<td>A general error: Unknown.</td>
</tr>
<tr><td>GENERAL_E_NOT_LOGIN</td>
<td>1003</td>
<td>A general error: An action before login.</td>
</tr>
<tr><td>GENERAL_E_WRONG_PARAM</td>
<td>1004</td>
<td>A general error: A parameter call error.</td>
</tr>
</tbody>
</table>




