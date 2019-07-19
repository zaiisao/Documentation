
---
title: Generate a Token from the server
description: Guide on how to generate tokens on the server side
platform: C++
updatedAt: Fri Jul 19 2019 09:05:40 GMT+0800 (CST)
---
# Generate a Token from the server
This page shows how to generate a token on your server for Agora SDK versions 2.1.0+. The token is used for joining a channel.

The following programming languages are covered. Choose the one that applies to you:

- Java
- C++
- Python
- Go
- PHP
- Node.js

> The Token Builder that we provide supports both int uid and string userAccount. 


## Java

### Initializes the Token Builder

```java
public boolean initTokenBuilder(String originToken);
```

This method uses the original token to reinitialize the token builder. This method enables the token builder to inherit the App ID, App Certificate, Channel Name, uid, and Privilege of the original token.

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
<li>true: Method call succeeded.</li>
<li>false: Method call failed.</li>
</ul>
</td>
</tr>
</tbody>
</table>



**Struct of the TokenBuilder**

```java
public SimpleTokenBuilder(String appId, String appCertificate, String channelName, String uid);
```

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID. A 32-bit unsigned integer with a value ranging from 1 to 2<sup>32</sup>-1. The uid must be unique. If a uid is not assigned (or set to 0), the SDK assigns one and returns it in the `onJoinChannelSuccess` callback function. Your app must record and maintain the returned value since the SDK does not do so.</td>
</tr>
</tbody>
</table>

```java
public SimpleTokenBuilder(String appId, String appCertificate, String channelName, String userAccount);
```

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>userAccount</code></td>
<td>The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: <p><li>The 26 lowercase English letters: a to z. <li> The 26 uppercase English letters: A to Z. <li> The 10 numbers: 0 to 9. <li> The space. <li> "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",". </td>
</tr>
</tbody>
</table>


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
<li>True: Method call succeeded.</li>
<li>False: Method call failed.</li>
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
	<tr><td><code>uid</code></td>
<td>User ID. A 32-bit unsigned integer with a value ranging from 1 to 2<sup>32</sup>-1. The uid must be unique. If a uid is not assigned (or set to 0), the SDK assigns one and returns it in the `onJoinChannelSuccess` callback function. Your app must record and maintain the returned value since the SDK does not do so.</td>
</tr>
</tbody>
</table>

```
SimpleTokenBuilder(const std::string& appId, const std::string& appCertificate,
                   const std::string& channelName, const std::string& userAccount = "");
```

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
	<tr><td><code>userAccount</code></td>
<td>The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: <p><li>The 26 lowercase English letters: a to z. <li> The 26 uppercase English letters: A to Z. <li> The 10 numbers: 0 to 9. <li> The space. <li> "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",". </td>
</tr>
</tbody>
</table>

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

> Do not call this method if you do not have an original token.

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
	<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td><li>User ID. A 32-bit unsigned integer with a value ranging from 1 to 2<sup>32</sup>-1. The uid must be unique. If a uid is not assigned (or set to 0), the SDK assigns one and returns it in the `onJoinChannelSuccess` callback function. Your app must record and maintain the returned value since the SDK does not do so. <li>The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: The 26 lowercase English letters: a to z. The 26 uppercase English letters: A to Z. The 10 numbers: 0 to 9. The space. "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",". </td>
</tr>
</tbody>
</table>

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

```go
func CreateSimpleTokenBuilder(appID, appCertificate, channelName string, uid uint32) SimpleTokenBuilder
```

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>uid</code></td>
<td>User ID. A 32-bit unsigned integer with a value ranging from 1 to 2<sup>32</sup>-1. The uid must be unique. If a uid is not assigned (or set to 0), the SDK assigns one and returns it in the `onJoinChannelSuccess` callback function. Your app must record and maintain the returned value since the SDK does not do so.</td>
</tr>
</tbody>
</table>

```go
func CreateSimpleTokenBuilder2(appID, appCertificate, channelName string, userAccount string) SimpleTokenBuilder
```

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
</tr>
<tr><td><code>channelName</code></td>
<td>Name of the channel that the user wants to join.</td>
</tr>
<tr><td><code>userAccount</code></td>
<td>The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: <p><li>The 26 lowercase English letters: a to z. <li> The 26 uppercase English letters: A to Z. <li> The 10 numbers: 0 to 9. <li> The space. <li> "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",". </td>
</tr>
</tbody>
</table>

<a name = "initPrivilegesGo"></a>

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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
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
<td>ID of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span>Getting an App ID</span></a>.</td>
</tr>
<tr><td>App Certificate</td>
<td>Certificate of the application that you registered in the Agora Dashboard. See <a href="../../en/Interactive%20Broadcast/token.md"><span> Get an App Certificate</span></a>.</td>
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


