
---
title: 游戏软件包发版说明
description: 
platform: iOS
updatedAt: Tue Sep 11 2018 23:12:42 GMT+0800 (CST)
---
# 游戏软件包发版说明
# 游戏软件包发版说明

## 简介

该游戏软件包提供游戏语音功能。

## v2.1.0.21 裁剪包 \(发布于 2018 年 6 月 15 日\)

> 如果你当前使用的 SDK 是 2.1 之前的版本，并希望升级到 2.1 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

减小包的体积:

-   舍弃了视频，伴奏，录制，加密，音效等功能

-   包大小不超过 1.4MB ，适合对包体积敏感的客户


## v2.1.0 \(发布于 2018 年 2 月 27 日\)

以下新功能或改进的文档修改记录，详见: [文档修订记录](../../cn/Product%20Overview/revisionhistory_game_ios.md)

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


## v2.0 \(发布于 2017 年 8 月 26 日\)

**新增功能:**

支持禁用语音后可以通过音量键来调节背景音乐

**修复问题:**

-   修复了部分设备上音效播放相关的问题

-   修复了 iOS 上耳机下声音小的问题

-   修复了少数安卓手机录音不正常的问题


## v1.1 \(发布于 2017 年 5 月 25 日\)

**新增功能:**

-   增加 API: `startAudioRecording` 和 `stopAudioRecording`

-   增加录制声音小的 `onWarning` 消息


**改进:**

-   优化了原生开发接口: `pause`/`resume`

-   优化了若干手机的音频问题

-   解决了 `startAudioRecording` 录音有杂音的问题

-   将 API `joinChannel` 改名为 `joinChannelByKey`


## v1.0 \(发布于 2017 年 5 月 3 日\)

该 SDK 首次发版，主要包含以下功能:

-   提供一套 Objective-C API 支持游戏语音功能

-   多音效播放功能: 包含预加载模式, 音效方位感

-   虚拟立体声功能: 不同 uid 不同方位的听感

-   语音变声功能

-   去杂音模式: 开启后只有人声被保留，其他杂音被滤除

-   压缩包大小

-   性能优化



