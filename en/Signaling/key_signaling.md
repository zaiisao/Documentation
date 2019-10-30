
---
title: Use Security Keys
description: 
platform: All Platforms
updatedAt: Wed Oct 30 2019 03:29:56 GMT+0800 (CST)
---
# Use Security Keys
## Introduction

The Agora Signaling SDK provides two different security keys for authentication: [App ID](#APPID) and [SignalingToken](#SignalingToken). The following figure shows the environments in which the security keys are used:

![](https://web-cdn.agora.io/docs-files/1555488293884)

Where:

-   An App ID can easily be obtained and is used in environments with low-security requirements, such as in a testing environment.

-   A SignalingToken adds security and is used in environments with high-security requirements, such as in a production environment.

-   An App Certificate is enabled for the sole purpose of generating a SignalingToken and cannot be used alone. Once an App Certificate is enabled, you can only use the SignalingToken for authentication.

<a name = "APPID"></a>

### App ID

After signing up at [Console](http://dashboard.agora.io), multiple projects can be created. Each project will be assigned a unique App ID. Anyone with your App ID can use it on any Agora SDK. Hence, it is prudent to safeguard the App IDs.

> To switch your App ID, you must first call the `destroy` method to destroy the current instance.

<a name = "SignalingToken"></a>

### SignalingToken

Agora recommends using a SignalingToken for added security.

## How to Get and Use an App ID

<a name = "Get-an-App-ID"></a>

### Get an App ID

Each Agora account can create multiple projects, and each project has a unique App ID.

1.  Sign up for a new account at [https://dashboard.agora.io/](https://dashboard.agora.io/).
2.  Click **Add New Project** on the **Projects** page in Console.
3.  Fill in the **Project Name** and click **Submit**.
    <img alt="../_images/create_project.png" src="https://web-cdn.agora.io/docs-files/en/create_project.png" style="width: 420.0px;"/>
4.  Find the App ID under the created project.


### Use an App ID

Access the Agora services by using your unique App ID:

1.  Enter the App ID in the start window to enable voice or video communication in the demo.
2.  Add the App ID to the code when developing the application.
3.  Set the `appId` parameter as the App ID when calling the API methods.


## How to Get and Use a SignalingToken

Each Agora account can create multiple projects, and each project has a unique App ID and App Certificate. You need to use both the App ID and App Certificate to generate a SignalingToken.

### Step 1: Get an App ID

[Get an App ID](#Get-an-App-ID).

### Step 2: Get an App Certificate

1.  Login to [https://dashboard.agora.io](https://dashboard.agora.io).
2.  Click **Add New Project** on the **Projects** page in Console.
3.  Fill in the **Project Name** and click **Submit**. Find the App ID under the created project.
     <img alt="../_images/create_project.png" src="https://web-cdn.agora.io/docs-files/en/create_project.png" style="width: 420.0px;"/>
4.  Enable the App Certificate for the project.
	-   Click **Edit** on the top-right of the project.
	-   Click **Enable** to the right of the App Certificate. Read **About App Certificate** before confirming the operation.
         <img alt="../_images/enable_app_cert.png" src="https://web-cdn.agora.io/docs-files/en/enable_app_cert.png" style="width: 420.0px;"/>
	-  Click the ‘eye’ icon to view the App Certificate. You can re-click this icon to hide the App Certificate.
        <img alt="../_images/view_app_certificate.png" src="https://web-cdn.agora.io/docs-files/en/view_app_certificate.png" style="width: 420.0px;"/>

> -   An App Certificate is enabled for the sole purpose of generating a token and cannot be used alone.
> 
> -   Keep the App Certificate on the server, never on any client machine.
> 
> -   It takes about an hour for the App Certificate to take effect after it is enabled.
> 
> -   Once the App Certificate is enabled for a project, a SignalingToken must be used. For example, before enabling the App Certificate, an App ID can be used to join a channel; but once an App Certificate is enabled, a SignalingToken must be used to join a channel.


5.  If your project integrates Agora’s Signaling SDK, you can use the Signaling Token Debugging Switch under App Certificate.

    -   If the App Certificate is not enabled, the Signaling Token Debugging Switch is disabled and you can set the SignalingToken to any value.

    -   If the App Certificate is enabled:

        -   The Signaling Token Debugging Switch is on if the App Certificate is enabled: You can set the SignalingToken or use `_no_need_token` to skip the setting.

        -   If you switch off the Signaling Token Debugging Switch, you can set the SignalingToken but cannot set it to `_no_need_token`. Otherwise, you will receive the Signaling error code 206.


### Step 3: Integrate the Schema

Use the following algorithm to generate a token (SignalingToken):

**Input**:

```
appId             = "C5D15F8FD394285DA5227B533302A518" // App ID
appCertificate    = "fe1a0437bf217bdd34cd65053fb0fe1d" // App Certificate
expiredTime       = "1546271999" // Authorized timestamp
account           = "test@agora.io" // The user ID defined by the client
```

Use the following field names in the sequence:

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Field Name</th>
<th>Type</th>
<th>Length</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Version</td>
<td>String</td>
<td> </td>
<td>SignalingToken version number, fixed as 1.</td>
</tr>
<tr><td>App ID</td>
<td>String</td>
<td>32</td>
<td>App ID provided by Agora, obtained at <a href="https://dashboard.agora.io">https://dashboard.agora.io</a>.</td>
</tr>
<tr><td>Authorized Timestamp</td>
<td>Number</td>
<td>10</td>
<td>The UTC timestamp represented by the number of seconds elapsed since 1/1/1970. Indicates the exact time when a user can no longer use the Agora service (for example, when a user is forced to leave an ongoing call).</td>
</tr>
<tr><td>Sign</td>
<td>String</td>
<td>32</td>
<td><p>Hex code for the signature. A string calculated by the MD5 algorithm based on inputs including the App Certificate and the following fields:</p>
<ul>
<li>account: User ID defined by the client.</li>
<li>appId:  A 32-character App ID string.</li>
<li>appCertificate: A 32-character App Certificate string.</li>
<li>expiredTime: The UTC timestamp indicating when a user cannot access the Agora Signaling System.</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



**Output**:

```
token       = "1:appId:expiredTime:md5(account + appId + appCertificate + expiredTime)"
            = "1:C5D15F8FD394285DA5227B533302A518:1546271999:md5(test@agora.ioC5D15F8FD394285DA5227B533302A518fe1a0437bf217bdd34cd65053fb0fe1d1546271999)"
            = "1:C5D15F8FD394285DA5227B533302A518:1546271999:5c0ee12fdf2020d0d0fdad04d6395473"
```

The generated token is the SignalingToken used to log in the Agora signaling system for [Step 4: Use a SignalingToken](#Use-a-SignalingToken).

> Agora provides sample code on GitHub for generating the token.
> -   [C++](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/src/generatorSignalToken.h)
> -   [Go](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/go/src/SignalingToken/SignalingToken.go)
> -   [Java](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/java/src/io/agora/signal/SignalingToken.java)
> -   [Node.js](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/nodejs/src/SignalingToken.js)
> -   [PHP](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/php/src/SignalingToken.php)
> -   [Python](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/python/src/SignalingToken.py)

<a name = "Use-a-SignalingToken"></a>

### Step 4: Use a SignalingToken

Before a user requests to log in the Agora signaling system:

1.  The client application requests authentication from your organization’s server.

2.  The server, upon receiving the request, uses the algorithm provided by Agora to generate a SignalingToken and then passes the SignalingToken back to the client application. The SignalingToken is based on the App Certificate, App ID, User ID defined by the client, and the Authorized Timestamp.

3.  The client application calls the `login` method and is required to set the token parameter as the generated SignalingToken.

4.  The Agora server receives the SignalingToken, confirms that the call comes from a legitimate user, then allows the user to access the Agora Signaling System.



