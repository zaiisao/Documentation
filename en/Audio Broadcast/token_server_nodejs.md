
---
title: Generate a Token from Your Server
description: 
platform: Node.js
updatedAt: Fri Jul 19 2019 12:36:52 GMT+0800 (CST)
---
# Generate a Token from Your Server

The token is used for joining a channel. This page shows how to generate a token on your server for the following SDKs

- Agora Native SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 

The following programming languages are covered. Choose the one that applies to you:

- Java
- C++
- Python
- Go
- PHP
- Node.js

> The Token Builder that we provide supports both int uid and string userAccount.




## Node.js

### Initializes the Token Builder

```javascript
initTokenBuilder = function (originToken);
```

This method uses the original token to reinitialize the token builder. Once called, this method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

> Do not call this method if you do not have an original token.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>originToken</code></td>
<td>The original token.</td>
</tr>
<tr><td>Return value</td>
<td><ul>
<li>0: Method call succeeded.</li>
<li>&lt;0: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>


**Struct of the TokenBuilder**

```javascript
var SimpleTokenBuilder = function (appID, appCertificate, channelName, uid);
```

This method is the struct of SimpleTokenBuilder.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td>App ID</td>
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Audio%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Audio%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td><li>User ID. A 32-bit unsigned integer with a value ranging from 1 to 2<sup>32</sup>-1. The uid must be unique. If a uid is not assigned (or set to 0), the SDK assigns one and returns it in the `onJoinChannelSuccess` callback function. Your app must record and maintain the returned value since the SDK does not do so.<li>The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: The 26 lowercase English letters: a to z. The 26 uppercase English letters: A to Z. The 10 numbers: 0 to 9. The space. "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",". </td>
</tr>
</tbody>
</table>


### Generates a Token \(buildToken\)

```javascript
this.buildToken = function ();
```

This method generates a token in the string format.




