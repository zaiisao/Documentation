
---
title: 企业部署代理服务器
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:23:32 GMT+0800 (CST)
---
# 企业部署代理服务器
本页为有防火墙的企业用户展示如何通过部署 Nginx 和 TurnServer 中转服务器，并调用 Agora 的接口，来使用 Agora Web SDK 的服务。

> - 本文内容适用于 Agora Web SDK。
> - 在使用 Firefox 浏览器时，跨运营商代理速度会比较慢。Agora 建议使用同运营商进行代理。如果必须使用跨运营商代理，则避免使用 Firefox 浏览器。

## 功能简介

目前有部分安全要求高的企业用户通过设置防火墙，限制员工访问外网或非认可的网站。 为避免这些企业用户因防火墙无法使用 Agora 的服务，Agora Web SDK 提供了 2 个接口，将内网请求绕过防火墙，通过代理服务器转到 Agora SD-RTN 上，实现内网访问。 用户通过部署 Nginx 和 TURN 服务器，然后调用 Agora 的接口，就可以从内网访问 Agora。

## 实现方法

企业用户需要先自行部署 Nginx 和 TURN 服务器，然后再调用 Agora Web SDK 的接口前，来实现 Proxy 功能。

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

3. 执行如下命令行，即完成部署。

   ```
   sudo nginx -s reload
   ```

### 2. 部署 TURN 服务器

Agora 提供一个开源的 TURN 服务器安装包，支持企业用户自行下载并部署。

1. 访问网址 [https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-webrtc](https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-webrtc) 并下载 turnServer 安装包。完整的安装包内含四个文件：`README`，`turnserver`, `turnserver.conf` 和 `turnserver.sh` 。

2. 打开 `turnserver.sh`，并配置如下参数：

   <table>
   <colgroup>
   <col/>
   <col/>
   </colgroup>
   <tbody>
   <tr><td><strong>参数名称</strong></td>
   <td><strong>描述</strong></td>
   </tr>
   <tr><td><code>extIP</code></td>
   <td>外部 IP 地址</td>
   </tr>
   <tr><td><code>udpport</code></td>
   <td>与 UDP 端口号绑定的端口，默认值为 “3478”</td>
   </tr>
   <tr><td><code>tcpport</code></td>
   <td>与 TCP 端口号绑定的端口，默认值为 “3433”</td>
   </tr>
   <tr><td><code>realm</code></td>
   <td>公司名，或其他能代表公司的名字，如 “agora.io”</td>
   </tr>
   </tbody>
   </table>

3. 在 `turnserver.conf` 文件中添加需要使用 TURN 服务器的用户名。格式为 `username=key`。其中 `key` 的值可以通过如下命令行获取：

   ```
   echo -n "<username>:<realm>:<password>" | md5sum
   ```

4. 执行如下命令行，启动 TURN 服务器，完成部署。

   ```
   usage: ./turnserver.sh [start/stop]
   ```

### 3. 调用 Agora Web SDK 的接口设置代理服务器

完成部署后，选择下面任意一种方法，调用接口对代理服务器进行设置：

**方法一**：在创建音视频流对象方法中，设置 `proxyServer` 和 `turnServer` 参数

```
createClient(config)
```

在该方法中设置 `proxyServer` 和 `turnServer` 参数：

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>config</td>
<td>Object</td>
<td><ul>
<li>proxyServer：（可选）设有企业防火墙的用户使用该参数，可以部署 Nginx 服务器，以将信令消息通过 Nginx 服务器传到 Agora SD-RTN，然后使用 Agora 的服务</li>
<li>turnServer：（可选）设有企业防火墙的用户使用该参数，可以部署 TURN 服务器，以将音视频数据通过 TURN 服务器传到 Agora SD-RTN，然后使用 Agora 的服务</li>
</ul>
</td>
</tr>
</tbody>
</table>

示例代码

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

> 请确保在加入频道（`client.join`） 之前设置该参数。

**方法二**：调用 Web SDK 接口实现部署

部署 Nginx 服务器 \(setProxyServer\)

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
<tr><td><strong>参数</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>domainName</code></td>
<td>String</td>
<td>你的 Nginx 服务器域名</td>
</tr>
</tbody>
</table>

部署 TURN 服务器 \(setTurnServer\)

```
that.setTurnServer = function (config);
```

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>类型</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>config</code></td>
<td>Object</td>
<td><p>TURN 服务器配置：</p>
<div><ul>
<li>turnServerURL：你的 TURN 服务器 URL 地址</li>
<li>username：你在 TURN 服务器上注册并使用的用户名</li>
<li>password：你在 TURN 服务器上使用的密码</li>
<li>udpport：你想要添加的 UDP 端口</li>
<li>forceturn：是否启用强制中转：<ul>
<li>true：强制所有流走中转</li>
<li>false：（默认值）不强制走中转</li>
</ul>
</li>
<li>tcpport：（可选）你想要添加的 TCP 端口</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

> 请确保在加入频道（`client.join`）之前调用如上两个方法。



## 代理服务器工作原理

<img alt="../_images/proxy_web_theory.jpg" src="https://web-cdn.agora.io/docs-files/cn/proxy_web_theory.jpg" />

代理服务器的工作原理：

- 企业自行部署 Nginx 和 TURN 服务器
- 企业用户调用 Web SDK 的接口，使信令消息和音视频数据绕过企业防火墙，并分别通过 Nginx 服务器和 TURN 服务器发送给 Agora SD-RTN。
