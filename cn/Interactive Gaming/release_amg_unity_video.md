
---
title: 发版说明
description: 
platform: Unity
updatedAt: Mon Jan 28 2019 04:14:08 GMT+0000 (UTC)
---
# 发版说明
## 简介

该游戏软件包提供游戏语音和视频功能。点击 [游戏产品概述](https://docs.agora.io/cn/Interactive%20Gaming/product_gaming?platform=All%20Platforms) 了解关键特性。

## 2.1.0 版

该版本于 2018 年 2 月 27 日发布。新增特性与修复问题列表详见下文。

### 新增功能

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>功能</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>设置仅限语音模式</td>
<td>新增接口设置仅限语音模式，优化语音质量</td>
</tr>
<tr><td>设置远端音效位置</td>
<td>新增接口设置远端音效出现的位置和音量大小</td>
</tr>
<tr><td>设置本地说话音调</td>
<td>新增接口设置本地说话人声音的音调</td>
</tr>
<tr><td>提醒角色状态变化</td>
<td>指挥模式下，新增回调接口提醒频道内用户指挥与被指挥者角色状态发生变化</td>
</tr>
<tr><td>提示对方已关闭视频</td>
<td>新增回调，提示远端用户已关闭视频</td>
</tr>
<tr><td>API 名称变化</td>
<td>所有出现 <code>forGaming</code> 的 API 类和枚举值名称，<code>forGaming</code> 全部删除</td>
</tr>
</tbody>
</table>



### 优化

节省带宽:

-   v2.1.0 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

-   v2.1.0 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽;


### 修复问题

-   修复了一系列特定条件下偶现的崩溃;

-   修复了一些语音路由不符合预期的问题;


## 2.0 版

该版本于 2017 年 8 月 26 日发布。新增特性与修复问题列表详见下文。

**新增功能:**

-   支持 Unity 视频功能

-   支持禁用语音后可以通过音量键来调节背景音乐


**修复问题:**

-   修复了部分设备上音效播放相关的问题

-   修复了少数安卓手机录音不正常的问题


## 1.1 版

该版本于 2017 年 5 月 25 日发布。新增特性与修复问题列表详见下文。

<table>
<colgroup>
<col/>
</colgroup>
<tbody>
<tr><td><ul>
<li>Native-iOS: 将 API <code>joinChannel</code> 改名为 <code>joinChannelByKey</code></li>
<li>Native-iOS: 增加 API: <code>startAudioRecording</code> 和 <code>stopAudioRecording</code></li>
<li>Native-iOS: 增加录制声音小的 <code>onWarning</code> 消息</li>
<li>Native-Android: 增加 API: <code>startAudioRecording</code> 和 <code>stopAudioRecording</code></li>
<li>Android: 优化了原生开发接口: <code>pause</code>/<code>resume</code></li>
<li>优化了若干手机的音频问题</li>
<li>解决了 <code>startAudioRecording</code> 录音有杂音的问题</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



## 1.0 版

该版本于 2017 年 5 月 3 日发布。新增特性与修复问题列表详见下文。


该 SDK 首次发版，主要包含以下功能:

**1. 提供以下 API:**

<table>
<colgroup>
<col/>
</colgroup>
<tbody>
<tr><td><ul>
<li>Unity3D SDK:（C# APIs）</li>
<li>Cocos2d SDK (C++ APIs)</li>
<li>Native SDK: Android (Java APIs) 和 iOS(Objective-C APIs)</li>
<li>多音效播放功能: 包含预加载模式, 音效方位感</li>
<li>虚拟立体声功能: 不同 uid 不同方位的听感</li>
<li>语音变声功能</li>
<li>去杂音模式: 开启后只有人声被保留，其他杂音被滤除</li>
</ul>
</td>
</tr>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
<tr/>
</tbody>
</table>



**2. 压缩包大小**

**3. 性能优化**


