
---
title: 企业部署代理服务器
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:07:54 GMT+0800 (CST)
---
# 企业部署代理服务器
本页为有防火墙的企业用户展示如何通过部署 Nginx 中转服务器，并调用 Agora 的接口，来使用 Agora Signaling SDK for Web 的服务。

> - 本文内容适用于 Agora Signaling SDK for Web。
> - 在使用 Firefox 浏览器时，跨运营商代理速度会比较慢。Agora 建议使用同运营商进行代理。如果必须使用跨运营商代理，则避免使用 Firefox 浏览器。

## 简介

目前有部分安全要求高的企业用户通过设置防火墙，限制员工访问外网或非认可的网站。 为避免这些企业用户因防火墙无法使用 Agora 的服务，Agora Signaling SDK for Web 提供了 2 个接口，将内网请求绕过防火墙，通过代理服务器转到 Agora SD-RTN 上，实现内网访问。 用户通过部署 Nginx 服务器，然后调用 Agora 的接口，就可以从内网访问 Agora。

## 实现方法

企业用户需要先自行部署 Nginx 服务器，然后在调用 Agora Signaling SDK for Web 的接口前，来实现 Proxy 功能。

### 1. 部署 Nginx 服务器

> 部署前请确认你已申请 Nginx 服务器的域名和证书。

#### 安装 Nginx 服务器

参考 [Nginx 服务器安装教程](https://jingyan.baidu.com/article/bad08e1ec2adc709c85121aa.html)完成 Centos 或 Ubuntu 系统下的 Nginx 安装。

#### 配置 Nginx 服务器的 conf

1. 在 Nginx 中执行如下命令行：

	```
	sudo vim /etc/nginx/nginx.conf
	```

2. 在 conf 文件的 http 域中添加如下配置项：

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

3.  执行如下命令行，即完成部署。

	```
	sudo nginx -s reload
	```

### 2. 调用 Agora Signaling SDK for Web 的接口设置代理服务器

完成部署后，选择下面任意一种方法，调用接口对代理服务器进行设置：

在 `setupdebugging` API 中设置 `name` 和 `value` 参数：

```
setup_debugging(name, value)
```

设置 `proxyServer` 和 `turnServer` 参数：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>proxy_server：拥有防火墙的企业可以部署 Nginx 服务器，并通过设置该参数将信令消息传递给 Agora</td>
</tr>
<tr><td><code>value</code></td>
<td>Nginx服务器 IP 或 URL</td>
</tr>
</tbody>
</table>

## 代理服务器工作原理

![](https://web-cdn.agora.io/docs-files/1540958110465)

代理服务器的工作原理：

- 企业自行部署 Nginx 服务器
- 企业用户调用 Agora Signaling SDK for Web 的接口，使信令消息绕过企业防火墙，并通过 Nginx 服务器发送给 Agora SD-RTN。

