
---
title: 发版说明
description: 
platform: Web
updatedAt: Thu Sep 27 2018 18:58:42 GMT+0800 (CST)
---
# 发版说明
# 发版说明

本文提供 Agora Web SDK 的发版说明。

## 概览

Agora Web SDK 是通过 HTML 网页加载的 JavaScript 库。 Agora Web SDK 库在网页浏览器中调用 API 建立连接，控制音视频通话和直播服务。

**已知问题和局限性**

- 兼容性: 如需实现 Agora Native SDK 与 Agora Web SDK 的互通，必须将 Agora Native SDK 升级至 1.12 及以上版本。
- 如果在客户端安装了高清摄像头，则 Agora Web SDK 支持最大为 1080p 分辨率的视频流，但取决于不同的摄像头设备，最大分辨率也会受到影响。
- 在 Agora Native SDK 中提供的一些功能，如质量提示、测试服务、通话服务评分、录音和日志记录等，在 Agora Web SDK 中未包含。
- Agora Web SDK 暂不支持 IPv6 。
- Agora Web SDK 暂不支持代码二次混淆。

更多问题，详见 [Web 常见问题集](https://docs.agora.io/cn/2.4.1/faq/faq/web)。

## 2.4.1 版

该版本于 2018 年 9 月 19 日发布。修复问题列表详见下文。

**问题修复**

- 修复了本地 IP 地址变化后无法自动 publish ，导致对端接收不到本地视频流的问题。
- 修复了取消订阅远端流会触发 `stream-removed` 事件的问题。

## 2.4.0 版

该版本于 2018 年 8 月 24 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

#### 1. 支持 Token

详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

#### 2. 支持音频信号处理

在 `audioProcessing` 参数中新增声学回声消除属性（AEC）和自动噪声抑制属性（ANS）。

#### 3. 支持音视频前处理

在 `client.createStream` 方法中增加了 `audioSource` 和 `videoSource` 属性，可以指定创建的音视频流要使用的音视频 Track，因此在创建音视频流之前可以对音视频进行处理。

#### 4. 支持设置音频属性

新增 `stream.setAudioProfile` 接口，提供多组音频属性设置（采样率、单双声道、编码码率）。

#### 5. 支持弱网时音视频流回退

在网络环境不理想的情况下，为保证通话体验，新增 `client.setStreamFallbackOption` 接口支持在接收端选择订阅视频小流或者只订阅音频流。

#### 6. 新增通话相关延迟数据

在 `stream.getStats` 的回调信息中新增通话中的延迟数据，包括从发送端到 SD-RTN 的延迟、SD-RTN 到接收端的延迟、发送端到接收端的延迟，以及接收端播放音视频的延迟。

#### 7. 支持日志上传

新增 `AgoraRTC.Logger.enableLogUpload` 接口，支持将 SDK 的日志信息上传到 Agora 的服务器，方便调查问题。

**问题修复**

- 修复了不调用 `stream.setVideoProfile` 接口, `cameraId` 和 `microphoneId` 设置就不生效的问题。
- 修复在 Firefox 浏览器上调用 `setScreenProfile` 设置不生效的问题。
- 修复在 `client.createStream` 方法中同时开启 `audio` 和 `screen` 会出错的问题。
- 修复屏幕共享时设置 `microphoneId` 不生效的问题。
- 修复 `audioProcessing` 设置不生效的问题。

## **2.3.1 版**

该版本于 2018 年 6 月 7 日发布。新增特性与修复问题列表详见下文。

**问题修复**

- 修复浏览器安装了 Adblock Plus 插件之后偶发的推流失败的问题。
- 修复了多 IP 跳转不生效的问题。

## **2.3.0 版**

该版本于 2018 年 6 月 4 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

#### 1. 新通信模式

为增加 Web SDK 的适用场景，提升与 Native SDK 在通信和直播下的互通质量，在 `createClient` 方法中新增 `mode` 和 `codec` 参数，其中 `mode` 参数支持 rtc 和 live 两种场景，`codec` 参数支持 vp8 和 264 两种编解码方式。 
#### 2. 支持语音自动增益控制

为满足通话或直播中对语音响度进行控制和调整的需要，在 `createStream` 方法中新增 `audioProcessing` 参数。

#### 3. 网页端 Proxy 功能

为解决设有防火墙的企业用户无法直接使用 Agora 服务的问题，新增 2 个接口，通过代理服务器将用户的内网请求转到 Agora SD-RTN 上，从而实现内网访问。使用该功能，用户需要自行部署 Nginx 和 TURN 服务器， 并在加入频道之前就调用相关接口。详见 [进阶：企业部署代理服务器](../../cn/Interactive%20Broadcast/proxy_web.md) 中关于代理服务器工作原理、部署步骤以及调用 API 接口的描述。

#### 4. 加密功能

为提高通话或直播安全性，新增加密功能。用户只要在加入频道之前调用接口设置加密密码及加密方案即可。

**问题修复**

已修复：出现 p2plost 后重连一次或几次后不再重连。

## **2.2 版**

该版本于 2018 年 4 月 16 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

#### 1. 获取版本信息

支持获取当前使用的 SDK 版本信息。

#### 2. 设置小流参数

新增设置小流参数接口，允许对小流参数进行配置。

#### 3. Firefox 浏览器屏幕共享

新增 Firefox 浏览器屏幕共享功能，通过在` createStream` 方法中增加`mediaSource` 参数实现。详见 [Firefox 屏幕共享](../../cn/Interactive%20Broadcast/screensharing_web.md)。

#### 4. QQ 浏览器支持

新增 Android 设备对 QQ 浏览器的支持。

**问题修复**

解决了 iOS 端使用 Safari 浏览器时，audio-only 模式下没有声音的问题。

## **2.1.1 版**

该版本于 2018 年 3 月 19 日发布。

修复了 macOS 上 Firefox v59.01 浏览器使用 Web SDK 只能看到本地视频看不到对方视频的问题。

## **2.1 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>功能</td>
<td>描述</td>
</tr>
<tr><td>质量检测</td>
<td>用户在使用 Web SDK 进行远程通信或直播前，提供回调对当前的浏览器的兼容情况、麦克风及摄像头的使用情况、网络状况进行测试，以保证通话质量;</td>
</tr>
<tr><td>视频大小流</td>
<td>新增方便发送端控制发送大流或小流，以及接收端控制请求大流或小流，并允许对小流进行参数配置;</td>
</tr>
<tr><td>客户端踢人</td>
<td>提供回调通知被踢客户其已被禁止加入频道</td>
</tr>
<tr><td>RTMP 推流(直播)</td>
<td>新增接口支持 RTMP 推流</td>
</tr>
<tr><td>设置日志输出等级</td>
<td>新增接口对日志的输出等级进行设置</td>
</tr>
<tr><td>静音/取消静音</td>
<td>新增接口在通话或直播过程中静音或取消静音</td>
</tr>
</tbody>
</table>



**改进**

本次发版改进如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>功能</td>
<td>描述</td>
</tr>
<tr><td>创建音视频对象优化</td>
<td>[删除] 纯 web 客户端对象，方便客户集成</td>
</tr>
<tr><td>P2P 连接建立时间优化</td>
<td>从 1.8 秒缩短到 500 毫秒</td>
</tr>
<tr><td>优化丢包对抗</td>
<td>优化 FEC 对抗 20% 丢包和 ULP FEC 扛丢包</td>
</tr>
</tbody>
</table>



**问题修复**

- 修复了 Safari 上视频旋转 90 度的问题
- 修复了 macOS 上 Firefox v59.01 浏览器使用 Web SDK 只能看到本地视频看不到对方视频的问题

## **2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

**新增功能**

- 新增了支持 Safari 浏览器。
- 新增了支持 Firefox 浏览器。
- 新增了检查浏览器兼容性功能，在 `createClient`之前新增了 `checkSystemRequirements` ，以检查系统对浏览器是否支持。
- 新增了本地摄像头 / 麦克风权限已获取 microphone/camera permission granted 回调，通知应用程序已获取本地摄像头／麦克风使用权限。
- 新增视频自适应功能，如果客户使用的摄像头不满足配置要求，会自动适配到一个合理的配置要求，以使摄像头设备满足帧率和分辨率要求。

**问题修复**

- 修复了 iOS SDK 与 Web SDK 互通音量小的问题。
- 修复了使用 Chrome 浏览器长时间互通后掉线的问题。
- 修复了 Android 设备切换前后置摄像头时屏幕显示异常的问题。
- 修复了 Android 设备互通时，一方离开后，另一方屏幕显示异常的问题。
- 修复了 Chrome 浏览器未启用摄像头进入频道时，不提示需获取摄像头权限的问题。
- 修复了 iOS 端使用 Safari 浏览器互通时，退到后台后不显示对方画面的问题。

## **1.14 版**

该版本于 2017 年 10 月 20 日发布。

新增了屏幕共享功能，在 `createStream` 里修改了参数 `screen`, 并新增了参数 `extensionId` 。

## **1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

**新增功能**

- 支持 Web 端 CDN 推流，进行旁路支持，详见 `API configPublisher` 描述
- 在 Android 上支持最新版 Chrome

## **1.12 版**

该版本于 2017 年 7 月 25 日发布。。新增特性与修复问题列表详见下文。

新增和更新了以下 API:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>API</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>createClient</code></td>
<td>[新增] 创建纯 Web 端或 Web 互通版客户端对象，取决于你设置了哪种模式</td>
</tr>
<tr><td><code>renewChannelKey</code></td>
<td>[新增] 之前 Channel Key 过期后，需调用该方法更新 Channel Key</td>
</tr>
<tr><td><code>active-speaker<code></td>
<td>[新增] 提示当前频道内谁正在说话</td>
</tr>
<tr><td><code>setRemoteVideoStreamType</code></td>
<td>[新增] 当发送端发送了双流(视频大流和小流)时，本地用户调用该方法可以选择接收双流中的一路流</td>
</tr>
<tr><td><code>setVideoProfile</code></td>
<td>[更新] 设置视频属性，默认值为 480p_1</td>
</tr>
<tr><td><code>init</code></td>
<td>[更新] 初始化客户端对象</td>
</tr>
<tr><td><code>join</code></td>
<td>[更新] 允许用户加入 Agora 频道</td>
</tr>
</tbody>
</table>



更新了错误代码和解释。
