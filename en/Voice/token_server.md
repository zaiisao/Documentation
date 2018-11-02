
---
title: Generate a Token
description: Guide on how to generate tokens on the server side
platform: Server
updatedAt: Fri Nov 02 2018 08:35:15 GMT+0000 (UTC)
---
# Generate a Token
This page shows how to generate a Token on your server for Agora SDK versions 2.1.0 and later which you can use for joining a chaneel.

We will cover the following languages, and you can choose one that applies to you:

- Java
- C++
- Python
- Go
- PHP
- Node.js

## Java

### Initialize the Token Builder

```java
public boolean initTokenBuilder(String originToken);
```

This method uses the original Token to reinitialize the Token Builder. Once called, this method enables the Token Builder to inherit the App ID, App Certificate, Channel Name, UID, and Privilege of the original Token.

Use this method with [Set the User Privilege \(setPrivilege\)](#setPrivilegeJava) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeJava) to add or delete privileges to the original Token.

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
<td>The original Token.</td>
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

This method is the struct of Simple Token Builder.

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
<td>ID of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span> Get an App Certificate</span></a>.</td>
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

### Initialize the User Privileges \(initPrivileges\)

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
<li>0: Attendee, that is, participants in a voice call or video call.</li>
<li>1: Publisher, that is, users (hosts) who publish video or/and voice streams in a live broadcast.</li>
<li>2: Subscriber, that is, users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin, that is, Administrators.</li>
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

### Set the User Privilege \(setPrivilege\)

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
<td><p>The user privilege in accordance with the user role, which includes:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join the channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join the channel.</li>
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
<li>Join the channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join the channel.</li>
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
<td>The privilege expiration time. The default value is 0 in which the privilege never expires.</td>
</tr>
</tbody>
</table>



> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
>
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970 till. If, for example, you want to access the Agora Service within 10 minutes after the privilege is generated, set it as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegeJava"></a>

### Remove the User Privilege \(removePrivilege\)

```java
public void removePrivilege(AccessToken.Privileges privilege);
```

This method removes a previously added privilege.

### Generate a Token \(buildToken\)

```java
public String buildToken();
```

This method generates a Token in the String format.

## C++

### Initialize the Token Builder

```c++
bool initTokenBuilder(const std::string& originToken);
```

This method uses the original Token to reinitialize the Token Builder. Once called, this method enables the Token Builder to inherit the App ID, App Certificate, Channel Name, UID, and Privilege of the original Token.

Use this method with [Set the User Privilege \(setPrivilege\)](#setPrivilegeCpp) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeCpp) to add or delete privileges to the original Token.

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
<td>The original Token.</td>
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

This method is the struct of Simple Token Builder.

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
<td>ID of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span> Get an App Certificate</span></a>.</td>
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

### Initialize the User Privileges \(initPrivileges\)

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
<li>0: Attendee, that is, participants in a voice call or video call.</li>
<li>1: Publisher, that is, users (hosts) who publish video or/and voice streams in a live broadcast.</li>
<li>2: Subscriber, that is, users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin, that is, Administrators.</li>
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

### Set the User Privilege \(setPrivilege\)

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
<td><p>The user privilege in accordance with the user role, which includes:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join the channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join the channel.</li>
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
<li>Join the channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join the channel.</li>
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
<td>The privilege expiration time. The default value is 0 in which the privilege never expires.</td>
</tr>
</tbody>
</table>



> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970 till. If, for example, you want to access the Agora Service within 10 minutes after the privilege is generated, set it as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name="removePrivilegeCpp"></a>

### Remove the User Privilege \(removePrivilege\)

```c++
void removePrivilege(AccessToken::Privileges privilege);
```

This method removes a previously added privilege.

### Generate a Token \(buildToken\)

```c++
std::string buildToken();
```

This method generates a Token in the String format.

## Python

### Initialize the Token Builder

```python
def initTokenBuilder(self, originToken);
```

This method uses the original Token to reinitialize the Token Builder. Once called, this method enables the Token Builder to inherit the App ID, App Certificate, Channel Name, UID, and Privilege of the original Token.

Use this method with [Set the User Privilege \(setPrivilege\)](#setPrivilegePython) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegePython) to add or delete privileges to the original Token.

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
<td>The original Token.</td>
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

This method is the struct of the Simple Token Builder.

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
<td>ID of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span> Get an App Certificate</span></a>.</td>
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

### Initialize the User Privileges \(initPrivileges\)

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
<li>0: Attendee; participants in a voice call or video call.</li>
<li>1: Publisher; users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber; users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin; administrators.</li>
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

### Set the User Privilege \(setPrivilege\)

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
<td><p>User privilege in accordance with the user role, which includes:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join the channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join the channel.</li>
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
<li>Join the channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join the channel.</li>
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
<td>The privilege expiration time. The default value is 0 in which the privilege never expires.</td>
</tr>
</tbody>
</table>



> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
>
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970 till. If, for example, you want to access the Agora Service within 10 minutes after the privilege is generated, set it as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegePython"></a>

### Remove the User Privilege \(removePrivilege\)

```python
def removePrivilege(self, privilege);
```

This method removes a previously added privilege.

### Generate a Token \(buildToken\)

```python
def buildToken(self);
```

This method generates a Token in the String format.

## Go

### Initialize the Token Builder

```go
func (builder SimpleTokenBuilder) InitTokenBuilder(originToken string);
```

This method uses the original Token to reinitialize the Token Builder. Once called, this method enables the Token Builder to inherit the App ID, App Certificate, Channel Name, UID, and Privilege of the original Token.

Use this method with [Set the User Privilege \(setPrivilege\)](#setPrivilegeGo) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeGo) to add or delete privileges to the original Token.

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
<td>The original Token.</td>
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

This method is the struct of the Simple Token Builder.

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
<td>ID of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span> Get an App Certificate</span></a>.</td>
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

### Initialize the User Privileges \(initPrivileges\)

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
<li>0: Attendee; participants in a voice call or video call.</li>
<li>1: Publisher; users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber; users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin; administrators.</li>
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

### Set the User Privilege \(setPrivilege\)

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
<td><p>User privilege in accordance with the user role, which includes:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join the channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join the channel.</li>
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
<li>Join the channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join the channel.</li>
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
<td>The privilege expiration time. The default value is 0 in which the privilege never expires.</td>
</tr>
</tbody>
</table>

> [1] Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.
>
> [2] The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970 till. If, for example, you want to access the Agora Service within 10 minutes after the privilege is generated, set it as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegeGo"></a>

### Remove the User Privilege \(removePrivilege\)

```go
func (builder SimpleTokenBuilder) RemovePrivilege(privilege AccessToken.Privileges);
```

This method removes a previously added privilege.

### Generate a Token \(buildToken\)

```go
func (builder SimpleTokenBuilder) BuildToken() (string,error);
```

This method generates a Token in the String format.

## PHP

### Initialize the Token Builder

```php
function initTokenBuilder($originToken);
```

This method uses the original Token to reinitialize the Token Builder. Once called, this method enables the Token Builder to inherit the App ID, App Certificate, Channel Name, UID, and Privilege of the original Token.

Use this method with [Set the User Privilege \(setPrivilege\)](#setPrivilegePhp) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegePhp) to add or delete privileges to the original Token.

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
<td>The original Token.</td>
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

This method is the struct of the Simple Token Builder.

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
<td>ID of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span> Get an App Certificate</span></a>.</td>
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

### Initialize the User Privileges \(initPrivileges\)

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
<li>0: Attendee; participants in a voice call or video call.</li>
<li>1: Publisher; users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber; users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin; administrators.</li>
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

### Set the User Privilege \(setPrivilege\)

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
<td><p>User privilege in accordance with the user role, which includes:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join the channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join the channel.</li>
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
<li>Join the channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join the channel.</li>
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
<td>The privilege expiration time. The default value is 0 in which the privilege never expires.</td>
</tr>
</tbody>
</table>



[1]  Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.

[2]  The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970 till. If, for example, you want to access the Agora Service within 10 minutes after the privilege is generated, set it as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegePhp"></a>

### Remove the User Privilege \(removePrivilege\)

```php
public function removePriviledge($privilege);
```

This method removes a previously added privilege.

### Generate a Token \(buildToken\)

```php
public function buildToken();
```

This method generates a Token in the String format.

## Node.js

### Initialize the Token Builder

```javascript
initTokenBuilder = function (originToken);
```

This method uses the original Token to reinitialize the Token Builder. Once called, this method enables the Token Builder to inherit the App ID, App Certificate, Channel Name, UID, and Privilege of the original Token.

Use this method with [Set the User Privilege \(setPrivilege\)](#setPrivilegeNode) and [Remove the User Privilege \(removePrivilege\)](#removePrivilegeNode) to add or delete privileges to the original Token.

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
<td>The original Token.</td>
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

This method is the struct of the Simple Token Builder.

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
<td>ID of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the App that you registered on the Agora dashboard. See <a href="../../en/Voice/token.md"><span> Get an App Certificate</span></a>.</td>
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

### Initialize the User Privileges \(initPrivileges\)

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
<li>0: Attendee; participants in a voice call or video call.</li>
<li>1: Publisher; users (hosts) who publish video and/or voice streams in a live broadcast.</li>
<li>2: Subscriber; users (audience) who need to subscribe to the voice and video streams in a live broadcast.</li>
<li>101: Admin; administrators.</li>
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

### Set the User Privilege \(setPrivilege\)

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
<td><p>User privilege in accordance with the user role, which includes:</p>
<ul>
<li>Attendee Privileges:<ul>
<li>Join the channel.</li>
<li>Publish the audio stream.</li>
<li>Publish the video stream.</li>
<li>Publish the data stream.</li>
</ul>
</li>
<li>Publisher Privileges:<ul>
<li>Join the channel.</li>
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
<li>Join the channel.</li>
<li>Request to publish the audio stream. <sup>[1]</sup></li>
<li>Request to publish the video stream. <sup>[1]</sup></li>
<li>Request to publish the data stream. <sup>[1]</sup></li>
</ul>
</li>
<li>Admin Privileges:<ul>
<li>Join the channel.</li>
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
<td>The privilege expiration time. The default value is 0 in which the privilege never expires.</td>
</tr>
</tbody>
</table>



[1]  Agora does not support the publisher inviting users to publish a voice/video/data stream or the subscriber requesting to publish a voice/video/data stream.

[2]  The `expireTimestamp` is represented by the number of seconds elapsed since 1/1/1970 till. If, for example, you want to access the Agora Service within 10 minutes after the privilege is generated, set it as the current timestamp + 600 \(seconds\). The valid time for each privilege is independent.

<a name = "removePrivilegeNode"></a>

### Remove the User Privilege \(removePrivilege\)

```javascript
this.removePrivilege = function (privilege);
```

This method removes a previously added privilege.

### Generate a Token \(buildToken\)

```javascript
this.buildToken = function ();
```

This method generates a Token in the String format.


