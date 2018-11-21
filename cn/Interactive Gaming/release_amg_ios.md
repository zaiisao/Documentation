
---
title: 发版说明
description: 
platform: iOS
updatedAt: Thu Sep 27 2018 20:12:05 GMT+0800 (CST)
---
# 发版说明
# 发版说明

## 简介

该游戏软件包提供游戏语音功能，如需视频功能，请联系 [sales@agora.io](mailto:sales@agora.io) 。

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
</tbody>
</table>



### 优化

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>优化</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>包裁减</td>
<td>支持按需编译和打包，从而减小包体积</td>
</tr>
<tr><td>节省带宽</td>
<td>v2.1.0 以前: 如果你选择不听某人的音频，音频流会继续发送流。v2.1.0 开始: 如果你选择不听某人的流，则不会下发，从而节省带宽;</td>
</tr>
</tbody>
</table>



### 修复问题

-   修复了一系列特定条件下偶现的崩溃;

-   修复了一些语音路由不符合预期的问题;


## 2.0 版

该版本于 2017 年 8 月 26 日发布。新增特性与修复问题列表详见下文。

**新增功能:**

支持禁用语音后可以通过音量键来调节背景音乐

**修复问题:**

-   修复了部分设备上音效播放相关的问题

-   修复了 iOS 上耳机下声音小的问题

-   修复了少数安卓手机录音不正常的问题


## 1.1 版

该版本于 2017 年 5 月 25 日发布。新增特性与修复问题列表详见下文。

**新增功能:**

-   增加 API: `startAudioRecording` 和 `stopAudioRecording`

-   增加录制声音小的 `onWarning` 消息


**改进:**

-   优化了原生开发接口: `pause`/`resume`

-   优化了若干手机的音频问题

-   解决了 `startAudioRecording` 录音有杂音的问题

-   将 API `joinChannel` 改名为 `joinChannelByKey`


## 1.0 版

该版本于 2017 年 5 月 3 日发布。新增特性与修复问题列表详见下文。

该 SDK 首次发版，主要包含以下功能:

-   提供一套 Objective-C API 支持游戏语音功能

-   多音效播放功能: 包含预加载模式, 音效方位感

-   虚拟立体声功能: 不同 uid 不同方位的听感

-   语音变声功能

-   去杂音模式: 开启后只有人声被保留，其他杂音被滤除

-   压缩包大小

-   性能优化



