
---
title: Generate a Token
description: Guide on how to generate tokens on the server side
platform: All Platforms
updatedAt: Fri Sep 18 2020 04:30:27 GMT+0800 (CST)
---
# Generate a Token
The Token, also known as the dynamic key, is used for authenticating app users when a user joins a channel, or logs onto the service system.

Tokens are generated on your server. When an app user joins a channel or logs onto the service system, the app client interacts with the app server in the following way:

1. The app client sends a request for token to the app server.
2. The app server generates a token.
3. The app server passes the token back to the app client.
4. The app client then passes the token to the Agora server by calling the method provided by the SDK, such as `joinChannel`.

This article introduces how to generate a token on your server using the code provided by Agora.

<div class="alert note">Based on the Agora product that you use, you need to generate different tokens:
	<ul>
		<li>The RTC token: Refer to this article to generate an RTC token if you use the Agora RTC SDK, On-premise Recording SDK, Cloud Recording, or Interactive Gaming SDK.</li>
		<li>The RTM token: Refer to <a href="https://docs.agora.io/en/Real-time-Messaging/rtm_token?platform=All%20Platforms">Generate an RTM token</a> if you use the Agora RTM SDK.</li>
	</ul>
</div>

## Prerequisites

Before proceeding, ensure that your project and the Agora product that you use meet the following requirements:

- Your Agora project has enabled the App Certificate on Console.
- The version of the Agora product that you use meet the following requirements:

	| Product | Versions that support tokens |
	| ---------------- | ---------------- |
	| RTC SDK      |<ul><li>The Native SDK: v2.1.0 or later</li><li>The Web SDK: v2.4.0 or later</li><li>The Electron SDK: All versions</li><li>The Unity SDK: All versions</li><li>The React Native SDK: All versions</li></ul>      |
	| On-premise Recording SDK | v2.1.0 or later |
	| Cloud Recording  | No version requirement |
	| Interactive Gaming SDK  | v2.2.0 or later |

## Implementation

Agora provides an open-source [AgoraDynamicKey](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) repository on GitHub, which enables you to generate tokens on your server with programming languages such as C++, Java, Python, PHP, Ruby, Node.js, and Go. Take C++ as an example, the following picture shows the code structure:

![](https://web-cdn.agora.io/docs-files/1600072461550)

Under the `cpp` directory:

- `./sample/RtcTokenBuilderSample.cpp` contains the sample code for generating a token.
- `./src/RtcTokenBuilder.h` contains the API used for generating a token.

### Generate a token with the sample code

Under the `sample` directory of each programming language, you can find the sample code for generating a token. Take C++ as an example:

```C++
int main(int argc, char const *argv[]) {
 
  // Fill in your App ID
  std::string appID  = "970Cxxxxxxxxxxxxxxxxxxxxxxx1b33";
  // Fill in your App Certificate
  std::string  appCertificate = "5CFdxxxxxxxxxxxxxxxxxxxxxxx5d3b";
  // Fill in the channel name
  std::string channelName= "7d72xxxxxxxxxxxxxxxxxxxxxxbdda";
  // Fill in the user ID. 0 means that the server does not authenticate user IDs.
  uint32_t uid = 2882341273;
  // The timestamp for the token to expire
  uint32_t expirationTimeInSeconds = 3600;
  uint32_t currentTimeStamp = time(NULL);
  uint32_t privilegeExpiredTs = currentTimeStamp + expirationTimeInSeconds;
  std::string result;
 
  result = RtcTokenBuilder::buildTokenWithUid(
      appID, appCertificate, channelName, uid, UserRole::Role_Publisher,
      privilegeExpiredTs);
  std::cout << "Token With Int Uid:" << result << std::endl;
```

Refer to the following steps to generate a token with the sample code above.

Before proceeding, ensure that you have installed OpenSSL.

1. Download or clone the [AgoraDynamicKey](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) repository.
2. Open the `AgoraDynamicKey/cpp/sample/RtcTokenBuilderSample.cpp` file, replace the value of `appID`, `appCertificate`, `channelName`, and `uid` with your own, and comment out the code snippets of `buildTokenWithUserAccount`.
3. Open your Terminal, navigate to the same directory that holds `RtcTokenBuilderSample.cpp`, and run the following command. After that, an executable file `RtcTokenBuilderSample` appears in the folder:

	```
	g++ -std=c++0x -O0 -I../../ -L. RtcTokenBuilderSample.cpp -lz -lcrypto -o RtcTokenBuilderSample
	```

4. Run the following command. You token is generated and printed in your Terminal window.

	```
	./RtcTokenBuilderSample
	```

Steps for generating a token with the sample code of different programming languages are as follows:

<details>
	<summary><font color="#3ab7f8">Java</font></summary>
Before proceeding, ensure that you have installed a Java IDE.
	<ol>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/java</code> file in your IDE.</li>
		<li>Open <code>AgoraDynamicKey/java/src/io/agora/sample/RtcTokenBuilderSample.java</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Run the sample project. Your token is generated and printed in your IDE.
</li>
	</ol>
</details>


<details>
	<summary><font color="#3ab7f8">Python</font></summary>
Before proceeding, ensure that you have Python 2 as the development environment. Use the following command to check your current Python version:
<pre><code>Python -V</code></pre>
	<ol>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/python/sample/RtcTokenBuilderSample.py</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Open Terminal, navigate to the same directory that holds <code>RtcTokenBuilderSample.py</code>, and run the following command. The token is generated and printed in your Terminal window.
			<pre><code>python RtcTokenBuilderSample.py</code></pre>
		</li>
	</ol>
</details>

<details>
	<summary><font color="#3ab7f8">Python3</font></summary>
Before proceeding, ensure that you have Python 3 as the development environment. Use the following command to check your current Python version:
<pre><code>Python -V</code></pre>
	<ol>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/python/sample/RtcTokenBuilderSample.py</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Open Terminal, navigate to the same directory that holds <code>RtcTokenBuilderSample.py</code>, and run the following command. The token is generated and printed in your Terminal window.
			<pre><code>python RtcTokenBuilderSample.py</code></pre>
		</li>
	</ol>
</details>

<details>
	<summary><font color="#3ab7f8">PHP</font></summary>
Before proceeding, ensure that you have installed the latest version of PHP.
	<ol>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/sample/RtcTokenBuilderSample.php</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Open Terminal, navigate to the same directory that holds <code>RtcTokenBuilderSample.php</code>, and run the following command. The token is generated and printed in your Terminal window.
			<pre><code>php RtcTokenBuilderSample.php</code></pre>
		</li>
	</ol>
</details>

<details>
	<summary><font color="#3ab7f8">Node.js</font></summary>
Before proceeding, ensure that you have installed the LTS version of Node.js.
	<ol>
		<li>Run the following command to install the Node.js dependencies:
			<pre><code>npm install</code></pre>
		</li>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/nodejs/sample/RtcTokenBuilderSample.js</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Open Terminal, navigate to the same directory that holds <code>RtcTokenBuilderSample.js</code>, and run the following command. The token is generated and printed in your Terminal window.
			<pre><code>node RtcTokenBuilderSample.js</code></pre>
		</li>
	</ol>
</details>

<details>
	<summary><font color="#3ab7f8">Go</font></summary>
Before proceeding, ensure that you have installed the latest version of Golang.
	<ol>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/go/sample/RtcTokenBuilder/sample.go</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Open Terminal, navigate to the same directory that holds <code>sample.go</code>, and run the following command. After that, an executable file <code>RtcTokenBuilder</code> appears in the folder
			<pre><code>go build</code></pre>
		</li>
		<li>Run the following command. The token is generated and printed in your Terminal window.
			<pre><code>./RtcTokenBuilder</code></pre>
		</li>
	</ol>
</details>

<details>
	<summary><font color="#3ab7f8">Ruby</font></summary>
Before proceeding, ensure that you have installed Ruby v1.9 or later. Run the following command to check your current Ruby version:
	<pre><code>ruby -version</code></pre>
	<ol>
		<li>Download or clone the <a href="https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey">AgoraDynamicKey</a> repository.</li>
		<li>Open the <code>AgoraDynamicKey/ruby/sample/rtc_token_builder_sample.rb</code> file, replace the value of <code>appID</code>, <code>appCertificate</code>, <code>channelName</code>, and <code>uid</code> with your own, and comment out the code snippets of <code>buildTokenWithUserAccount</code>.</li>
		<li>Open Terminal, navigate to the same directory that holds <code>rtc_token_builder_sample.rb</code>, and run the following command. The token is generated and printed in your Terminal window.
			<pre><code>ruby rtc_token_builder_sample.rb</code></pre>
		</li>
	</ol>
</details>

### API reference

This section introduces the method to generate a token. Take C++ as an example:

```C++
static std::string buildTokenWithUid(
    const std::string& appId,
    const std::string& appCertificate,
    const std::string& channelName,
    uint32_t uid,
    UserRole role,
    uint32_t privilegeExpiredTs = 0);
```

| Parameter | Description | 
| ---------------- | ---------------- | 
| `appId`      | The App ID of your Agora project.      | 
| `appCertificate` | The App Certificate of your Agora project.|
| `channelName` | The unique channel name for the Agora RTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are:</br><ul><li>All lowercase English letters: a to z.</li><li>All uppercase English letters: A to Z.</li><li>All numeric characters: 0 to 9.</li><li>The space character.</li><li>Punctuation characters and other symbols, including: "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",".</li></ul>|
| `uid` | The user ID. A 32-bit unsigned integer with a value range from 1 to (2<sup>32</sup> - 1). It must be unique. Set `uid` as 0, if you do not want to authenticate the user ID, that is, any uid from the app client can join the channel or log onto the service system.|
| `role` | The privilege of the user:<ul><li>`Role_Publisher`(1): (Default) The user has the privilege of publishing streams.<li>`Role_Subscriber`(2): The user does not have the privilege of publishing streams.</ul>If you have enabled co-host token authentication, whether a user can publish streams is determined by this parameter and the user role in the `setClientRole` method. For details, see FAQ: <a href="https://docs.agora.io/en/faq/token_cohost">How do I use co-host token authentication</a>.|
| `privilegeExpiredTs` | The Unix timestamp (s) when the token expires, represented by the sum of the current timestamp and the valid time of the token. For example, if you set `privilegeExpiredTs` as the current timestamp plus 600 seconds, the token expires in 10 minutes. A token is valid for 24 hours at most. If you set this parameter as 0 or a period longer than 24 hours, the token is valid for 24 hours.|
	
<div class="note alert">Agora also provides a <code>buildTokenWithUserAccount</code> method which enables you to generate a token with a user account in the string format, though we recommend not using it.</div>
	
## Considerations
	
### Channel name and user ID

The channel name and user ID that you use to generate the token must be the same with those that you use to join the channel.

### App Certificate and token

To use the token for authentication, you need to enable the App Certificate for your project on Console. Once a project has enabled the App Certificate, you must use tokens to authenticate its users.

### Token expiration

A token is valid for 24 hours at most. To ensure the experience of your app user, the Agora SDK that you use triggers the following callbacks when a token expires:

- `onTokenPrivilegeWillExpire`: Occurs when the token expires in 30 seconds. Upon receiving this callback, you need to generate a new token on your server, and call `renewToken` to pass the new token to the SDK.
- `onRequestToken` (`onTokenPrivilegeDidExpire` on the Web platform): Occurs when the token has expired. Upon receiving this callback, you need to generate a new token on your server, and re-join the channel.


## Reference

You can also refer to the following documents according to your needs:

- [How to solve token-related errors?](https://docs.agora.io/en/faq/token_error)
- [What causes the 101 error on Cloud Recording SDK?](https://docs.agora.io/en/faq/101_error)
- [How to use co-host token authentication?](https://docs.agora.io/en/faq/token_cohost)






