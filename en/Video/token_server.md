
---
title: Generate a Token
description: Guide on how to generate tokens on the server side
platform: Server
updatedAt: Tue Dec 11 2018 08:50:51 GMT+0000 (UTC)
---
# Generate a Token
This page shows how to generate a token on your server for Agora SDK versions 2.1.0+. The token is used for joining a channel.

The following programming languages are covered. Choose the one that applies to you:

- Java
- C++
- Python
- Go
- PHP
- Node.js

## Java

### Initializes the Token Builder

```java
public boolean initTokenBuilder(String originToken);
```

This method uses the original token to reinitialize the token builder. This method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

Use this method with the [Set the User Privilege \(setPrivilege\)](#setPrivilegeJava) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeJava) methods to add or delete privileges to the original token.

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

```java
public SimpleTokenBuilder(String appId, String appCertificate, String channelName, String uid);
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
</tbody>
</table>

<a name = "initPrivilegesJava"></a>

### Initializes the User Privileges \(initPrivileges\)

```java
public boolean initPrivileges(Role role);
```

This method initializes user privileges by specifying the user role.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>The user role. Each role is associated with a set of user privileges. Choose from the following roles:</p>
<ul>
<li>0: Attendee. Participants in a voice call or video call.</li>
<li>1: Publisher. Users (hosts) who publish video or/and voice streams in a live broadcast.</li>
<li>2: Subscriber. Users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin. Administrators.</li>
</ul>
</td>
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

<a name = "setPrivilegeJava"></a>

### Sets the User Privilege \(setPrivilege\)

```
public void setPrivilege(AccessToken.Privileges privilege, int expireTimestamp);
```

This method sets the user privilege according to the user role specified in [Initialize the User Privileges \(initPrivileges\)](#initPrivilegesJava).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
	<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>The user privilege according to the user role:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Publish the audio CDN.</li>
<li>Publish the video CDN.</li>
<li>Invite subscribers to publish the audio stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the video stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Subscriber Privileges:<ul>
<li>Join a channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Administrate the channel.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td>The privilege expiration time. The default value is 0 (the privilege never expires).</td>
</tr>
</tbody>
</table>



> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
>
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. For example, if you want to access the Agora Service within 10 minutes after the privilege is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegeJava"></a>

### Removes the User Privilege \(removePrivilege\)

```java
public void removePrivilege(AccessToken.Privileges privilege);
```

This method removes a previously added privilege.

### Generates a Token \(buildToken\)

```java
public String buildToken();
```

This method generates a token in the string format.

## C++

### Initializes the Token Builder

```c++
bool initTokenBuilder(const std::string& originToken);
```

This method uses the original token to reinitialize the token builder. This method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and privilege of the original token.

Use this method with the [Set the User Privilege \(setPrivilege\)](#setPrivilegeCpp) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeCpp) methods to add or delete privileges to the original token.

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

```c++
SimpleTokenBuilder();
SimpleTokenBuilder(const std::string& appId, const std::string& appCertificate,
                   const std::string& channelName, uint32_t uid = 0);
SimpleTokenBuilder(const std::string& appId, const std::string& appCertificate,
                   const std::string& channelName, const std::string& uid = "");
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
	<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
</tbody>
</table>

<a name="initPrivilegesCpp"></a>

### Initializes the User Privileges \(initPrivileges\)

```c++
bool initPrivileges(Role role);
```

This method initializes user privileges by specifying the user role.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>The user role. Each role is associated with a set of user privileges. Choose from the following roles:</p>
<ul>
<li>0: Attendee. Participants in a voice call or video call.</li>
<li>1: Publisher. Users (hosts) who publish video or/and voice streams in a live broadcast.</li>
<li>2: Subscriber. Users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin. Administrators.</li>
</ul>
</td>
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

<a name="setPrivilegeCpp"></a>

### Sets the User Privilege \(setPrivilege\)

```
void setPrivilege(AccessToken::Privileges privilege, uint32_t expireTimestamp = 0);
```

This method sets the user privilege according to the user role specified in [Initialize the User Privileges \(initPrivileges\)](#initPrivilegesCpp).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</stromg></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>The user privilege according to the user role:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Publish the audio CDN.</li>
<li>Publish the video CDN.</li>
<li>Invite subscribers to publish the audio stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the video stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Subscriber Privileges:<ul>
<li>Join a channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Administrate the channel.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td>The privilege expiration time. The default value is 0 (the privilege never expires).</td>
</tr>
</tbody>
</table>



> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. For example, you want to access the Agora Service within 10 minutes after the privilege is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name="removePrivilegeCpp"></a>

### Removes the User Privilege \(removePrivilege\)

```c++
void removePrivilege(AccessToken::Privileges privilege);
```

This method removes a previously added privilege.

### Generates a Token \(buildToken\)

```c++
std::string buildToken();
```

This method generates a token in the string format.

## Python

### Initializes the Token Builder

```python
def initTokenBuilder(self, originToken);
```

This method uses the original token to reinitialize the token builder. This method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

Use this method with the [Set the User Privilege \(setPrivilege\)](#setPrivilegePython) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegePython) methods to add or delete privileges to the original token.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong</td>
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

```python
def __init__(self, appID, appCertificate, channelName, uid);
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
	<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
</tbody>
</table>

<a name = "initPrivilegesPython"></a>

### Initializes the User Privileges \(initPrivileges\)

```python
def initPrivileges(self, role);
```

This method initializes the user privileges by specifying the user role.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
	<tr><td><strong>Name</strong></td>
	<td></strong>Description</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>User role. Each role is associated with a set of user privileges. Choose from the following roles:</p>
<ul>
<li>0: Attendee. Participants in a voice call or video call.</li>
<li>1: Publisher. Users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber. Users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin. Administrators.</li>
</ul>
</td>
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

<a name = "setPrivilegePython"></a>

### Sets the User Privilege \(setPrivilege\)

```python
def setPrivilege(self, privilege, expireTimestamp);
```

This method sets the user privilege according to the user role specified in [Initialize the User Privileges \(initPrivileges\)](#initPrivilegesPython).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>User privilege according to the user role:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Publish the audio CDN.</li>
<li>Publish the video CDN.</li>
<li>Invite subscribers to publish the audio stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the video stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Subscriber Privileges:<ul>
<li>Join a channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Administrate the channel.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td>The privilege expiration time. The default value is 0 (the privilege never expires).</td>
</tr>
</tbody>
</table>



> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
>
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. For example, if you want to access the Agora Service within 10 minutes after the privilege is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegePython"></a>

### Removes the User Privilege \(removePrivilege\)

```python
def removePrivilege(self, privilege);
```

This method removes a previously added privilege.

### Generates a Token \(buildToken\)

```python
def buildToken(self);
```

This method generates a token in the string format.

## Go

### Initializes the Token Builder

```go
func (builder SimpleTokenBuilder) InitTokenBuilder(originToken string);
```

This method uses the original token to reinitialize the token builder. This method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

Use this method with the [Set the User Privilege \(setPrivilege\)](#setPrivilegeGo) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeGo) methods to add or delete privileges to the original token.

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

```go
func CreateSimpleTokenBuilder(appID, appCertificate, channelName string, uid uint32) SimpleTokenBuilder;
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
</tbody>
</table>

<a name = "initPrivilegesGo"></a>

### Initializes the User Privileges \(initPrivileges\)

```go
func (builder SimpleTokenBuilder) InitPrivileges(role Role);
```

This method initializes the user privileges by specifying the user role.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>role</code.</td>
<td><p>User role. Each role is associated with a set of user privileges. Choose from the following roles:</p>
<ul>
<li>0: Attendee. Participants in a voice call or video call.</li>
<li>1: Publisher. Users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscribe. Users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin. Administrators.</li>
</ul>
</td>
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

<a name = "setPrivilegeGo"></a>

### Sets the User Privilege \(setPrivilege\)

```go
func (builder SimpleTokenBuilder) SetPrivilege(privilege AccessToken.Privileges, expireTimestamp uint32);
```

This method sets the user privilege according to the user role specified in [Initialize the User Privileges \(initPrivileges\)](#initPrivilegesGo).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>User privilege according to the user role:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Publish the audio CDN.</li>
<li>Publish the video CDN.</li>
<li>Invite subscribers to publish the audio stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the video stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Subscriber Privileges:<ul>
<li>Join a channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Administrate the channel.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td>The privilege expiration time. The default value is 0 (the privilege never expires).</td>
</tr>
</tbody>
</table>

> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
>
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. For example, if you want to access the Agora Service within 10 minutes after the privilege is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegeGo"></a>

### Removes the User Privilege \(removePrivilege\)

```go
func (builder SimpleTokenBuilder) RemovePrivilege(privilege AccessToken.Privileges);
```

This method removes a previously added privilege.

### Generates a Token \(buildToken\)

```go
func (builder SimpleTokenBuilder) BuildToken() (string,error);
```

This method generates a token in the string format.

## PHP

### Initializes the Token Builder

```php
function initTokenBuilder($originToken);
```

This method uses the original token to reinitialize the token builder. This method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

Use this method with the [Set the User Privilege \(setPrivilege\)](#setPrivilegePhp) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegePhp) methods to add or delete privileges to the original token.

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

```php
public function __construct($appID, $appCertificate, $channelName, $uid);
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
</tbody>
</table>

<a name = "initPrivilegesPhp"></a>

### Initializes the User Privileges \(initPrivileges\)

```php
public function initPrivilige($role);
```

This method initializes the user privileges by specifying the user role.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>User role. Each role is associated with a set of user privileges. Choose from the following roles:</p>
<ul>
<li>0: Attendee. Participants in a voice call or video call.</li>
<li>1: Publisher. Users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber. Users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin. Administrators.</li>
</ul>
</td>
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

<a name = "setPrivilegePhp"></a>

### Sets the User Privilege \(setPrivilege\)

```php
public function setPrivilege($privilege, $expireTimestamp)
```

This method sets the user privilege according to the user role specified in [Initialize the User Privileges \(initPrivileges\)](#initPrivilegesPhp).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>User privilege according to the user role:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Publish the audio CDN.</li>
<li>Publish the video CDN.</li>
<li>Invite subscribers to publish the audio stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the video stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Subscriber Privileges:<ul>
<li>Join a channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Administrate the channel.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td>The privilege expiration time. The default value is 0 (the privilege never expires).</td>
</tr>
</tbody>
</table>



[1]  Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.

[2]  The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. For example, if you want to access the Agora Service within 10 minutes after the privilege is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegePhp"></a>

### Removes the User Privilege \(removePrivilege\)

```php
public function removePriviledge($privilege);
```

This method removes a previously added privilege.

### Generates a Token \(buildToken\)

```php
public function buildToken();
```

This method generates a token in the string format.

## Node.js

### Initializes the Token Builder

```javascript
initTokenBuilder = function (originToken);
```

This method uses the original token to reinitialize the token builder. Once called, this method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

Use this method with the [Set the User Privilege \(setPrivilege\)](#setPrivilegeNode) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeNode) methods to add or delete privileges to the original token.

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Video/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
	<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>ID of the user who wants to join a channel.</td>
</tr>
</tbody>
</table>

<a name = "initPrivilegesNode"></a>

### Initializes the User Privileges \(initPrivileges\)

```javascript
this.initPrivileges = function (role);
```

This method initializes the user privileges by specifying the user role.

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>role</code.</td>
<td><p>User role. Each role is associated with a set of user privileges. Choose from the following roles:</p>
<ul>
<li>0: Attendee. Participants in a voice call or video call.</li>
<li>1: Publisher. Users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber. Users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin. Administrators.</li>
</ul>
</td>
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

<a name = "setPrivilegeNode"></a>

### Sets the User Privilege \(setPrivilege\)

```javascript
this.setPrivilege = function (privilege, expireTimestamp);
```

This method sets the user privilege according to the user role specified in [Initialize the User Privileges \(initPrivileges\)](#initPrivilegesNode).

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
	<tr><td><strong>Name</strong></td>
		<td><strong>Description</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>User privilege according to the user role:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Publish the audio CDN.</li>
<li>Publish the video CDN.</li>
<li>Invite subscribers to publish the audio stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the video stream. <sup>[1]</sup></li>
<li>Invite subscribers to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Subscriber Privileges:<ul>
<li>Join a channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join a channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
<li>Administrate the channel.</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td>The privilege expiration time. The default value is 0 (the privilege never expires).</td>
</tr>
</tbody>
</table>



[1]  Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.

[2]  The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970. For example, if you want to access the Agora Service within 10 minutes after the privilege is generated, set `expireTimestamp` as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegeNode"></a>

### Removes the User Privilege \(removePrivilege\)

```javascript
this.removePrivilege = function (privilege);
```

This method removes a previously added privilege.

### Generates a Token \(buildToken\)

```javascript
this.buildToken = function ();
```

This method generates a token in the string format.


