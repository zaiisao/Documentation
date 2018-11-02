
---
title: 企业部署代理服务器
description: 
platform: Android,iOS,macOS,Windows
updatedAt: Fri Nov 02 2018 04:04:24 GMT+0000 (UTC)
---
# 企业部署代理服务器
本页为设有防火墙的企业用户展示如何使用 Agora 提供的代理服务器安装包，部署代理服务器来使用 Agora 的服务。

> 本文内容不适用于 Agora Web SDK。

## 功能简介

目前有部分企业用户通过设置防火墙，来限制员工访问外网或非认可的网站。 如果这些企业用户需要使用 Agora 的服务，则可以通过使用 Agora 提供的代理服务器安装包并进行部署，将 Agora 支持的 IP 地址和端口添加到防火墙上，实现内网访问。

## 实现方法
### 部署及配置 Agora 代理服务器

Agora 提供一个开源的代理服务器安装包，支持企业用户自行下载并部署代理服务器。具体步骤如下：

1.  访问网址 [https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-rtc-sdk](https://github.com/AgoraIO/Tools/tree/master/Proxy/proxy-for-rtc-sdk)，下载并安装 `install-ubuntu.sh` 脚本。

2.  在企业防火墙上添加 IP 地址和端口，允许内网访问。

3.  在 Agora SDK 内调用如下接口，并将其中的参数设置为添加的 IP 地址和端口。

	```
	setParameters("{\"rtc.proxy_server\":[0, \"IP\", port]}");
	```

	<table>
	<colgroup>
	<col/>
	<col/>
	</colgroup>
	<tbody>
	<tr><td><strong>名称</strong></td>
	<td><strong>描述</strong></td>
	</tr>
	<tr><td>IP</td>
	<td>代理服务器的 IP 地址，形式为字符串，如 127.0.0.1</td>
	</tr>
	<tr><td>port</td>
	<td>待添加到 Agora proxy 的端口数字，此处请直接填入 1080</td>
	</tr>
	</tbody>
	</table>


### 部署多台代理服务器

在负载大、单个服务器无法支撑，或发生故障不可用的情况下，企业也可以布置多台代理服务器。预计一台代理服务器可以支撑 100 台设备。

- DNS 配置下多台服务器轮询

	使用 DNS 配置多台服务器时需注意：

	-   部署 Shadowsocks 集群
	-   多台服务器使用相同的域名
	-   服务器使用轮询机制以保障负载均衡
	-   代理服务器发生故障时，需要更新 DNS 记录


- App 随机选择服务器

	App 可以从代理服务器列表中随机选择，然后设置给 SDK。

## 代理服务器工作原理

<img alt="../_images/proxy_theory.jpg" src="https://web-cdn.agora.io/docs-files/cn/proxy_theory.jpg" style="width: 742.4px; height: 440.0px;"/>


代理服务器的工作原理如下：

-   企业在企业网和公共互联网之间设置防火墙，以限制员工对未授权外网的访问
-   在公共互联网上部署代理服务器，当企业设备向企业防火墙发送访问请求时，防火墙向代理服务器，然后代理服务器再向服务器发送访问请求
-   服务器向代理服务器返回访问内容，代理服务器再通过企业防火墙将访问内容返回给企业设备

因此，企业 App 通过代理服务器访问 AgoraCloud 的大致步骤如下：

<img alt="../_images/proxy_agora.jpg" src="https://web-cdn.agora.io/docs-files/cn/proxy_agora.jpg" style="width: 651.2px; height: 156.8px;"/>
