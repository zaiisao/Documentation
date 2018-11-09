
---
title: Deploy the Enterprise Proxy
description: 
platform: Web
updatedAt: Fri Nov 09 2018 18:42:56 GMT+0000 (UTC)
---
# Deploy the Enterprise Proxy
This page shows how enterprises can use the API provided by the Agora Signaling SDK for Web to access Agora’s services through a company firewall.

> This page applies to the Agora Signaling SDK for Web only. Proxy services by different service providers may result in slow performance if you are using the Firefox browser. Therefore, Agora recommends using the same service provider for the proxy services. If you use different service providers, Agora recommends not using the Firefox browser.

## Introduction

Enterprises may restrict employees’ access to unauthorized websites through a company firewall, which may restrict access to Agora’s services as well. Agora provides one interface in the Signaling SDK for Web, which allows requests to bypass the company firewall and reach Agora SD-RTN directly through proxy servers. Enterprises can deploy Nginx Server and call the provided interface to enable internal access to Agora’s services.

## Implementations

You need to deploy the Nginx Server by yourself before calling the method in the Signaling SDK for Web to enable the Proxy service. 

### 1. Deploying the Nginx Server

> Please ensure that you have acquired the domain name and certificate of the Nginx Server before proceeding.

#### Install the Nginx Server

Refer to the steps in [Nginx Installation Guide](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/) to deploy the Nginx Server.

#### Configure the conf file of the Nginx Server

1.  Run the following command line:

	```
	sudo vim /etc/nginx/nginx.conf
	```

2.  Add the following configuration in the http domain of the *conf* file:

	```
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


3.  Run the following command line:

	```
	sudo nginx -s reload
	```

### 2. Call the Method in the Signaling SDK for Web to Set the Proxy

Set the `name` and `value` parameters in the `setupdebugging` API

```
setup_debugging(name, value)
```

Set the `proxyServer` and `turnServer` parameters:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Name</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>proxy_server: Enterprises with a company firewall can deploy the Nginx Server and then use this parameter to pass signaling messages to Agora through Nginx</td>
</tr>
<tr><td><code>value</code></td>
<td>Your server IP or URL</td>
</tr>
</tbody>
</table>


## Working Principles

The following figure shows how the proxy works:

<img alt="../_images/proxy_web_signaling.jpg" src="https://web-cdn.agora.io/docs-files/en/proxy_web_signaling.jpg" />


-   The enterprise deploys an Nginx proxy server.

-   The enterprise users call the method provided by the Signaling SDK for Web. Signaling messages are passed to Agora through the Nginx Server, thus enabling access to Agora’s services.

