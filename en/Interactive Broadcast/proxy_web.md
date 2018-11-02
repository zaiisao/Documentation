
---
title: Deploy the Enterprise Proxy
description: 
platform: Web
updatedAt: Thu Nov 01 2018 09:29:47 GMT+0000 (UTC)
---
# Deploy the Enterprise Proxy
# Deploy the Enterprise Proxy

This page shows how enterprises can use the APIs provided by the Agora Web SDK to access Agora’s services through a company firewall.

> This page applies to the Agora Web SDK only. Proxy services by different service providers may result in slow performance if you are using the Firefox browser. Therefore, Agora recommends using the same service provider for the proxy services. If you use different service providers, Agora recommends not using the Firefox browser.

## Introduction

Enterprises may restrict employees’ access to unauthorized websites through a company firewall, which may restrict access to Agora’s services as well. Agora provides two interfaces in the Web SDK, which allows requests to bypass the company firewall and reach Agora SD-RTN directly through proxy servers. Enterprises can deploy Nginx and TURN Server and call these interfaces to enable internal access to Agora’s services.

## Implementations

You need to deploy the Nginx and TURN Server by yourself before calling the methods in the Web SDK to enable the Proxy service. Refer to the following steps to deploy the two servers.

### 1.  Deploy the Nginx Server

> Please ensure that you have acquired the domain name and certificate of the Nginx Server before proceeding.

#### Install the Nginx Server

Refer to the steps in [Nginx Installation Guide](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/) to deploy the Nginx Server.

#### Configure the **conf** file of the Nginx Server

1. Run the following command line:

   ```
   sudo vim /etc/nginx/nginx.conf
   ```

2. Add the following configuration in the http domain of the **conf** file

   ```javascript
   resolver 8.8.8.8;
   server {
           listen  80;
           listen  443;
           server_name <you dns>;
           ssl     on;
           ssl_certificate         <you certificate file absolute path>;
           ssl_certificate_key     <you certificate key file absolute path>;
           location /cs/ {
                   proxy_pass https://$arg_h:$arg_p/$arg_d;
           }
           location /rs/ {
                   proxy_pass https://$arg_h:$arg_p/$arg_d;
           }
           location /ws/ {
                   proxy_pass https://$arg_h:$arg_p;
                   proxy_http_version 1.1;
                   proxy_set_header Upgrade $http_upgrade;
                   proxy_set_header Connection "upgrade";
           }
   }
   ```

3. Run the following command line

   ```
   sudo nginx -s reload
   ```

### 2. Deploy the TURN Server

Agora provides an open-source TURN Server package for you to download and deploy.

1. Visit [https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-webrtc](https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-webrtc) and download the **turnServer** package. The complete package is composed of the following 4 files: **README**, **turnserver**, `turnserver.conf` and `turnserver.sh` .

2. Open `turnserver.sh` and set the following parameters:

   <table>
   <colgroup>
   <col/>
   <col/>
   </colgroup>
   <tbody>
   <tr><td><strong>Name</strong></td>
   <td><strong>Description</strong></td>
   </tr>
   <tr><td><code>extIP</code></td>
   <td>The external IP address</td>
   </tr>
   <tr><td><code>udpport</code></td>
   <td>The binding port of the UDP socket. The default value is “3478”</td>
   </tr>
   <tr><td><code>tcpport</code></td>
   <td>The binding port of the TCP socket. The default value is “3433”</td>
   </tr>
   <tr><td><code>realm</code></td>
   <td>Name of your company, or any name that stands for your company, like “agora.io”</td>
   </tr>
   </tbody>
   </table>

1. Add the name\(s\) of the user\(s\) that will use the TURN Server in the `turnserver.conf` file in the format of `username=key`. Run the following command line to get the value of `key`:

   ```
   echo -n "<username>:<realm>:<password>" | md5sum
   ```

2. Run the following command line to start the TRUN Server:

   ```
   usage: ./turnserver.sh [start/stop]
   ```

### 3. Call the Methods in the Web SDK to Set the Proxy

Choose either of the following two methods to set the Proxy servers once you have finished deploying them:

**Method 1**: Set the proxyServer and turnServer parameters in the createClient API

```javascript
createClient(config)
```

Set the `proxyServer` and `turnServer` parameters:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>config</code></td>
<td>Object</td>
<td><ul>
<li>proxyServer: (Optional) Enterprises with a company firewall can deploy the Nginx Server and then use this parameter to pass signaling messages to Agora through Nginx</li>
<li>turnServer: (Optional) Enterprises with a company firewall can deploy the TURN Server and then use this parameter to send voice and video data to Agora through TURN Server</li>
</ul>
</td>
</tr>
</tbody>
</table>

Sample Code

```javascript
var config = {
  mode: 'live',
  codec: 'vp8',
  proxyServer: 'YOUR NGINX PROXY SERVER IP',
  turnServer: {
      turnServerURL: 'YOUR TURNSERVER URL',
      username: 'YOUR USERNAME',
      password: 'YOUR PASSWORD',
      udpport: 'THE UDP PORT YOU WANT TO ADD',
      tcpport: 'THE TCP PORT YOU WANT TO ADD',
      forceturn: false
  }
}
var client = AgoraRTC.createClient(config);
```

> Please ensure that you set this parameter before calling `client.join` .

**Method 2**: Call the setProxyServer and setTurnServer APIs in the Web SDK

- Deploy the Nginx Server (setProxyServer)

	```javascript
	client.setProxyServer(domainName);
	```

	<table>
	<colgroup>
	<col/>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Name</strong></td>
	<td><strong>Type</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td><code>domainName</code></td>
	<td>String</td>
	<td>Your Nginx server domain name</td>
	</tr>
	</tbody>
	</table>

- Deploy the TURN Server \(setTurnServer\)

	```javascript
	that.setTurnServer = function (config);
	```

	<table>
	<colgroup>
	<col/>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>Name</strong></td>
	<td><strong>Type</strong></td>
	<td><strong>Description</strong></td>
	</tr>
	<tr><td><code>config</code></td>
	<td>Object</td>
	<td><p>Configure the TurnServer:</p>
	<ul>
	<li><code>turnServerUrl</code>: Your TURN Server URL address</li>
			<li><code>username</code>: Your TURN Server username</li>
	<li><code>password</code>: Your TURN Server password</li>
	<li><code>udpport</code>: The UDP port(s) you want to add to TURN Server</li>
	<li><code>forceturn</code>: Sets whether to force data transfer by TURN Server:<ul>
	<li><code>true</code>: Force data transfer</li>
	<li><code>false</code>: (default) Not to force data transfer</li>
	</ul>
	</li>
	<li><code>tcpport</code>: (Optional) The TCP port(s) you want add to TURN Server</li>
	</ul>
	</td>
	</tr>
	</tbody>
	</table>

	> Please ensure that you call this API before `client.join` .

## Working Principles

The following figure shows how the proxy works:

<img alt="../_images/proxy_web_theory.jpg" src="https://web-cdn.agora.io/docs-files/en/proxy_web_theory.jpg" style="width: 924.0px; height: 380.8px;"/>

- The enterprise deploys the two proxy servers: Nginx and TURN Server.
- The enterprise users call the two methods in the Web SDK. Signaling messages are passed to Agora through the Nginx Server, while voice and video data streams are sent to Agora through TURN Server, thus enabling access to Agora’s services.
