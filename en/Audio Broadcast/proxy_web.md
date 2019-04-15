
---
title: Deploy the Enterprise Proxy
description: 
platform: Web
updatedAt: Fri Nov 09 2018 18:37:44 GMT+0800 (CST)
---
# Deploy the Enterprise Proxy
This page shows how enterprises can use the API methods provided by the Agora Web SDK to access Agora’s services through a company firewall.

> This page applies to the Agora Web SDK only. Proxy services by different service providers may result in slow performance if you use the Firefox browser. Therefore, Agora recommends using the same service provider for the proxy services. If you use different service providers, Agora recommends not using the Firefox browser.

## Introduction

Enterprises may restrict employees’ access to unauthorized websites through a company firewall. The firewall may also restrict access to Agora’s services. Agora provides two interfaces in the Web SDK. The interfaces allow requests to bypass the company firewall and reach the Agora SD-RTN directly through proxy servers. Enterprises can deploy the Nginx and TURN servers and call these interfaces to enable internal access to Agora’s services.

## Implementations

You need to deploy the Nginx and TURN servers before calling the methods in the Web SDK to enable the proxy service. Refer to the following steps to deploy the servers.

### 1.  Deploy the Nginx Server

> Ensure that you have acquired the domain name and certificate of the Nginx server before proceeding.

#### Install the Nginx Server

Refer to the steps in the [Nginx Installation Guide](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/) to deploy the Nginx server.

#### Configure the `conf` file of the Nginx Server

1. Run the following command line.

   ```
   sudo vim /etc/nginx/nginx.conf
   ```

2. Add the following configuration in the http domain of the `conf` file.

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

3. Run the following command line.

   ```
   sudo nginx -s reload
   ```

### 2. Deploy the TURN Server

Agora provides an open-source TURN server package for you to download and deploy.

1. Visit [https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-webrtc](https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-webrtc) and download the `turnServer` package. The complete package is composed of the following files: `README`, `turnserver`, `turnserver.conf`, and `turnserver.sh`.

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
   <td>The binding port of the UDP socket. The default value is 3478.</td>
   </tr>
   <tr><td><code>tcpport</code></td>
   <td>The binding port of the TCP socket. The default value is 3433.</td>
   </tr>
   <tr><td><code>realm</code></td>
   <td>Name of your company. For example. agora.io.</td>
   </tr>
   </tbody>
   </table>

1. Add the names of the users that will use the TURN server in the `turnserver.conf` file in the format of `username=key`. Run the following command line to get the value of `key`:

   ```
   echo -n "<username>:<realm>:<password>" | md5sum
   ```

2. Run the following command line to start the TURN server:

   ```
   usage: ./turnserver.sh [start/stop]
   ```

### 3. Call the Methods in the Web SDK to Set the Proxy

Choose either one of the following methods to set the proxy servers once you have finished deploying them:

**Method 1**: Set the `proxyServer` and `turnServer` parameters in the `createClient` method.

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
<li>proxyServer: (Optional) Enterprises with a company firewall can deploy the Nginx server and then use this parameter to pass signaling messages to Agora through the Nginx server.</li>
<li>turnServer: (Optional) Enterprises with a company firewall can deploy the TURN server and then use this parameter to send voice and video data to Agora through the TURN server.</li>
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

> Ensure that you set this parameter before calling the `client.join` method.

**Method 2**: Call the `setProxyServer` and `setTurnServer` methods in the Web SDK

- Deploy the Nginx server (setProxyServer).

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
	<td>Your Nginx server domain name.</td>
	</tr>
	</tbody>
	</table>

- Deploy the TURN server \(setTurnServer\).

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
	<td><p>Configure the Turn server:</p>
	<ul>
	<li><code>turnServerUrl</code>: Your TURN server URL address.</li>
			<li><code>username</code>: Your TURN server username.</li>
	<li><code>password</code>: Your TURN server password.</li>
	<li><code>udpport</code>: The UDP ports you want to add to the TURN server.</li>
	<li><code>forceturn</code>: Sets whether to force data transfer by the TURN server:<ul>
	<li><code>true</code>: Force data transfer.</li>
	<li><code>false</code>: (Default) Do not force data transfer.</li>
	</ul>
	</li>
	<li><code>tcpport</code>: (Optional) The TCP ports you want add to the TURN server.</li>
	</ul>
	</td>
	</tr>
	</tbody>
	</table>

	> Ensure that you call this method before the `client.join` method.

## Working Principles

The following figure shows how the proxy works:

<img alt="../_images/proxy_web_theory.jpg" src="https://web-cdn.agora.io/docs-files/en/proxy_web_theory.jpg" />

- The enterprise deploys the two proxy servers: Nginx and TURN servers.
- The enterprise users call the two methods in the Web SDK. Signaling messages are passed to Agora through the Nginx server, while voice and video data streams are sent to Agora through the TURN server, thus enabling access to Agora’s services.
