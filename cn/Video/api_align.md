
---
title: 核心方法对照表
description: List APIs of the key functions across the platforms
platform: All Platforms
updatedAt: Mon Feb 11 2019 08:01:50 GMT+0000 (UTC)
---
# 核心方法对照表
Agora SDK 支持多个平台，但是由于平台差异，不同平台在 API 的调用和实现上不完全一致，本文将 Android，iOS/macOS，Windows 和 Web 这几个平台的核心功能 API 对照列出，帮助你快速了解各个平台之间的差异。


<table>
  <tr>
    <th>核心功能</th>
    <th>Android</th>
    <th>iOS/macOS</th>
    <th>Web</th>
    <th>Windows</th>
  </tr>
  <tr>
    <td>初始化</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35466f690d0a9332f24ea8280021d5ed">create</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithAppId:delegate:">sharedEngineWithAppId</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/web/globals.html#createclient">AgoraRTC.createClient</a><br><a href="https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#init">Client.init</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/group__create_agora_rtc_engine.html">createAgoraRtcEngine</a><br><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac71db65e66942e4e0a0550e95c16890f">initialize</a></td>
  </tr>
  <tr>
    <td nowrap="nowrap">设置频道模式</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1bfb76eb4365b8b97648c3d1b69f2bd6">setChannelProfile</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setChannelProfile:">setChannelProfile</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/web/globals.html#createclient">AgoraRTC.createClient</a><sup>[1]</sup></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aab53788c74da25080bad61f0525d12ae">setChannelProfile</a></td>
  </tr>
  <tr>
    <td>设置用户角色</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa2affa28a23d44d18b6889fba03f47ec">setClientRole</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setClientRole:">setClientRole</a></td>
    <td>无<sup>[2]</sup></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a89ca6a15d5a388f3c82038e74bad4040">setClientRole</a></td>
  </tr>
  <tr>
    <td>加入频道</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c">joinChannel</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:">joinChannelByToken</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#join">Client.join</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c">joinChannel</a></td>
  </tr>
  <tr>
    <td>离开频道</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a2929e4a46d5342b68d0deb552c29d597">leaveChannel</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:">leaveChannel</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#leave">Client.leave</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a51c12d209373650638bfd82e28777081">leaveChannel</a></td>
  </tr>
  <tr>
    <td>更新 Token</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80">renewToken</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:">renewToken</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#renewtoken">Client.renewToken</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739">renewToken</a></td>
  </tr>
  <tr>
    <td>打开互通</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a49636ee063476d7c3da533668771fa03">enableWebSdkInteroperability</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableWebSdkInteroperability:">enableWebSdkInteroperability</a></td>
    <td>N/A<sup>[3]</sup></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5b82667e75a8f299a60b9b7968da48de">enableWebSdkInteroperability</a></td>
  </tr>
  <tr>
    <td>销毁实例</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#afb808cdc9025a77af7dd2bce98311bfe">destroy</a></td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/destroy">destroy</a></td>
    <td>N/A</td>
    <td><a href="https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afe4804c1f53bfee301c0960fda006c47">release</a></td>
  </tr>
</table>

> [1] Web 平台设置频道模式通过 `createClient` 中的 `ClientConfig` 的设置实现，详见 [ClientConfig](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.clientconfig.html)。

> [2] Web 平台目前没有用于设置用户角色的方法，但是你可以通过调用 `Client.publish`，`Client.unpublish` 和 `Client.subscribe` 来实现主播和观众的角色设置，详见[切换用户角色](../../cn/Video/role_web.md)。

> [3] Web 平台如需与其他各平台互通，需要将 [ClientConfig](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.clientconfig.html) 中的 `mode` 设置为 `live`。
