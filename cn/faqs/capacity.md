
---
title: Agora RTC SDK 最多支持多少人同时在线？
description: 
platform: All Platforms
updatedAt: Thu Apr 02 2020 16:33:17 GMT+0800 (CST)
---
# Agora RTC SDK 最多支持多少人同时在线？
<style> table th:first-of-type {     width: 80px; } th:third-of-type {     width: 170px; }</style>
<table>
  <tr>
    <th>场景</th>
    <th>平台</th>
    <th>规模</th>
  <tr>
    <td>通信</td>
    <td>Android、iOS、Windows、macOS、Linux、Web、Electron、Unity</td>
    <td><li>音频通话：单个频道支持百万人同时在线。<ul><ul><li>SDK 为 3.0.0 版本及以上，Agora 建议最多 17 人同时语音聊天。<li>SDK 为 3.0.0 版本以下，Agora 建议最多 7 人同时语音聊天。</li></ul></ul><li>视频通话：单个频道支持百万人同时在线。<ul><ul><li>SDK 为 3.0.0 版本及以上，Agora 建议最多 17 人同时视频聊天。<li>SDK 为 3.0.0 版本以下，Agora 建议最多 7 人同时视频聊天。</li></ul></ul><li>频道数：没有限制。</li></td>
  </tr>
  <tr>
    <td>直播</td>
    <td>Android、iOS、Windows、macOS、Linux、Web、Electron、Unity</td>
    <td><li>音频连麦：单个频道支持百万人同时在线，Agora 建议最多 17 人同时语音连麦。</li><li>视频连麦：单个频道支持百万人同时在线，Agora 建议最多 17 人同时视频连麦。</li><li>频道数：没有限制。</li></td>
  </tr>

</table>

<div class="alert note">如果同时发流人数超过 Agora 的建议值，你可能会遇到以下现象：<li>随机听不见或看不见某些频道内的发流用户。例如，SDK 为 3.0.0 版本，频道中有 18 人同时视频连麦，这 18 人均只能听见并看见随机 17 人的音视频，每人都会随机听不见并看不见某一人的音视频。<li>当下行带宽受限时，你还可能遇到卡顿、视频黑屏等未定义行为。</li></div>
